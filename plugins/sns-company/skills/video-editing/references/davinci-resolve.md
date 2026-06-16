# DaVinci Resolve 連携（高品質編集）＋クリップ品質スコアリング

> 用途：ffmpegでは作り込みにくい「トランジション/エフェクト/カラーグレ/音声ダッキング」を要する編集。
> 主に @mynaturescape の実写長尺。AIインサイトの定型章組みは `longform-pipeline.md`（ffmpeg）を使う。
> ⚠️ DaVinci実連携の実機検証は環境整備（macOS15＋DaVinci 21＋python.org版Python）完了後に行う。本書は接続手順とスコアリングの設計。

## 1. 接続の前提（macOS）

- **DaVinci Resolve を起動した状態**で外部スクリプトを実行する（API は起動中アプリに接続する）。
- Resolve 環境設定 → System → General → **「External scripting using」を Local（または Network）に設定**。
- **python.org 版 Python 3** を使う（Homebrew版だと fusionscript.so の読み込みに失敗することがある）。
- ⚠️ **無料版での外部スクリプト可否は版により異なる**。歴史的に外部スクリプト連携は Studio（有料）限定だった時期がある。**初回接続時に下記の最小スクリプトで疎通確認し、不可なら可否を一次情報で確認する**（憶測で進めない）。

### 環境変数（~/.zshrc 等に設定 or 実行前にexport）

```bash
export RESOLVE_SCRIPT_API="/Library/Application Support/Blackmagic Design/DaVinci Resolve/Developer/Scripting"
export RESOLVE_SCRIPT_LIB="/Applications/DaVinci Resolve/DaVinci Resolve.app/Contents/Libraries/Fusion/fusionscript.so"
export PYTHONPATH="$PYTHONPATH:$RESOLVE_SCRIPT_API/Modules/"
```

### 疎通確認（最小スクリプト）

```python
import DaVinciResolveScript as dvr
resolve = dvr.scriptapp("Resolve")
print(resolve)                      # None なら未接続（起動/権限/Python版を点検）
pm = resolve.GetProjectManager()
project = pm.GetCurrentProject()
print(project.GetName())
```

## 2. 主要 API の地図

```python
resolve   = dvr.scriptapp("Resolve")
pm        = resolve.GetProjectManager()
project   = pm.GetCurrentProject()          # or pm.CreateProject("name")
mediaPool = project.GetMediaPool()
mediaStorage = resolve.GetMediaStorage()

# 1) 素材取り込み
items = mediaStorage.AddItemListToMediaPool([ "/path/clip1.mov", "/path/clip2.mov" ])

# 2) タイムライン作成＋クリップ並べ
timeline = mediaPool.CreateEmptyTimeline("edit_v1")
mediaPool.AppendToTimeline(items)           # 採用したMediaPoolItemのリストを順に並べる

# 3) ページ遷移（カラー/Fairlight等）
resolve.OpenPage("color")                   # "media"|"cut"|"edit"|"fusion"|"color"|"fairlight"|"deliver"

# 4) 書き出し（Deliver）
project.LoadRenderPreset("H.264 Master")    # or SetRenderSettings(dict)
project.SetRenderSettings({ "TargetDir": "/path/out", "CustomName": "final" })
project.AddRenderJob()
project.StartRendering()
```

- トランジション・カラーノード・スピード調整などタイムライン単位の細かな操作は API カバレッジに濃淡がある。**APIで届かない部分は「テンプレートProject/PowerGrade/Renderプリセットを事前にUIで用意し、スクリプトはそれを適用する」**のが堅実（=人手で作った品質テンプレを自動適用）。
- カラー：LUT適用・PowerGradeのstill適用はノードAPI経由。確実なのは**プロジェクトにグレード済テンプレタイムラインを用意→新素材を流し込む**運用。
- 音声ダッキング：Fairlightでナレーション帯にBGMをサイドチェイン。API操作が難しければ**ffmpegの `sidechaincompress` で先に音声ミックスを作りDaVinciは映像主体**に分業してよい。

## 3. クリップ品質スコアリング（@mynaturescape 実写駆動の採否）

撮影クリップを機械基準で採否判定し、採用分だけ DaVinci に流す。**しきい値は初期ヒューリスティック＝素材で調整する**。

| 指標 | 計測法 | 除外基準（初期値） |
|---|---|---|
| ピント（鮮鋭度） | グレースケールのラプラシアン分散 | **< 100 で除外**（ボケ） |
| 輝度 | グレースケール平均 | **30未満 or 225超で除外**（黒つぶれ/白飛び） |
| 手ブレ | 連続フレーム間のオプティカルフロー平均量（フレーム幅で正規化） | **> 0.3 で除外**（ブレ大） |

```python
import cv2, numpy as np

def score_clip(path, sample_every=15):
    cap = cv2.VideoCapture(path)
    w = cap.get(cv2.CAP_PROP_FRAME_WIDTH) or 1920
    focus, bright, shake = [], [], []
    prev = None; i = 0
    while True:
        ok, frame = cap.read()
        if not ok: break
        if i % sample_every == 0:
            g = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            focus.append(cv2.Laplacian(g, cv2.CV_64F).var())   # 鮮鋭度
            bright.append(float(g.mean()))                     # 輝度
            if prev is not None:
                flow = cv2.calcOpticalFlowFarneback(prev, g, None,
                          0.5, 3, 15, 3, 5, 1.2, 0)
                mag = np.linalg.norm(flow, axis=2).mean() / w  # 幅で正規化
                shake.append(mag)
            prev = g
        i += 1
    cap.release()
    return {
        "focus":  float(np.median(focus))  if focus  else 0.0,
        "bright": float(np.median(bright)) if bright else 0.0,
        "shake":  float(np.median(shake))  if shake  else 0.0,
    }

def accept(s):
    return s["focus"] >= 100 and 30 <= s["bright"] <= 225 and s["shake"] <= 0.3
```

- 採用クリップを **focus 降順＋手ブレ昇順** で並べ、構成（冒頭フック→本編→締め）に割り当てる。
- 植物は静止前提（[[feedback_plant_video_direction]]）＝手ブレ基準は厳しめでよい。
- スコアの根拠（除外したクリップと理由）を task_log に残し、後でしきい値を調整する。

## 4. ffmpeg と DaVinci の分業指針

| 工程 | 使う |
|---|---|
| AIインサイトの章組み・字幕焼き込み・図解合成 | **ffmpeg**（確立済・`longform-pipeline.md`） |
| 実写クリップの採否スコアリング | **OpenCV**（本書3節） |
| トランジション/カラーグレ/音の作り込み | **DaVinci Resolve** |
| 音声ミックス（ナレ+BGM+SFX・ダッキング） | DaVinci Fairlight、難しければ ffmpeg `sidechaincompress`/`amix` |

> 原則：**APIで不確実な高度編集は「UIで作った品質テンプレをスクリプトで適用」**。自動化は再現性のある所から段階導入し、毎回 before/after を確認する。
