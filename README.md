# 資料作成工場くん (Document Factory-kun)

ローカルの [Phuker/md2html](https://github.com/Phuker/md2html) と
[Kozea/WeasyPrint](https://github.com/Kozea/WeasyPrint) を組み合わせた、
**Markdown → 単一 HTML → PDF** を商用グレードの UX で回せる Web ダッシュボードです。

> 🟢 **ワンタッチ起動**: `start.bat` をダブルクリックするだけで完結します。
> 初回は venv 作成と依存パッケージのインストールが自動で行われます。

---

## ✨ 主な機能

- **編集ページ**: Markdown エディタ + ライブプレビュー + ツールバー (B / I / H / リンク / コード / 表 …) + 文字数 / 単語数 / 読了時間
- **一括変換ページ**: 複数 `.md` または `.zip` をドロップ → ZIP でダウンロード
- **履歴ページ**: 過去の変換を全件保持、フィルタ・検索・再ダウンロード・削除
- **環境設定ページ**: テーマ、既定値、サーバ情報 (light / dark / auto)
- **🎨 デザインシステム**: 8 テーマ (Stripe / Sakura / Ocean / Sunset / Neon / Forest / Minimal / Off) をワンクリックで切替。表紙・本文・図表すべてにテーマトークン (`--omd-primary` 等) を適用
- **🧩 装飾**: 28 ボックス + 22 リボン + 14 吹き出し + **16 箇条書き** をピッカー UI から挿入 (選択範囲を包む)
- **✒️ 見出しスタイル**: 13 プリセット (default / marker / ribbon / tag / box / arrow / corner / centerlines …) を選択中テーマに自動適用
- **📽️ スライドモード**: 16:9 ランドスケープ、見出しレベル (h1/h2/h3) でページ区切り、太字見出し + 上部カラーバー
- **📥 HTML への適用**: デザイン / 見出し / スライドを既存 HTML ファイルに後付け適用 (`/api/apply-design`)
- **📊 PPTX 出力**: スライドモード時に PowerPoint (`.pptx`) を生成。16:9 レイアウト、テーマカラーのトップバー、装飾（ボックス/リボン/吹き出し/箇条書き）がすべて反映。`make_pptx=true` で有効化
- **コマンドパレット**: `Ctrl/Cmd + K` で全操作を呼び出し
- **キーボードショートカット**: `E` / `B` / `H` でページ切替、`Cmd+B` / `Cmd+I` で書式編集
- **トースト通知**: 成功 / 警告 / エラーを画面右下に表示
- **モーダル確認**: 削除や下書きクリアなど取り消し不可な操作で確認
- **自動保存**: エディタの内容は下書きとしてブラウザに自動保存
- **レスポンシブ**: デスクトップ / タブレット / モバイルに対応
- **アクセシブル**: ARIA ラベル、キーボード操作、フォーカスリング

---

## クイックスタート (Windows)

1. `start.bat` を **ダブルクリック**
2. 初回は venv・依存パッケージ・md2html (editable) のセットアップが自動で走ります
3. ブラウザが自動で開きます (開かない場合は表示された URL をクリック)
4. 使い終わったら `stop.bat` をダブルクリック

その他のスクリプト:

| ファイル | 用途 |
| --- | --- |
| `start.bat` / `start.ps1` / `start.sh` | ダッシュボードを起動 (ダブルクリック可) |
| `stop.bat` / `stop.ps1` | サーバを停止 |
| `status.bat` | 起動中かどうかを確認 |
| `start.bat --no-wait --no-browser` | ランチャーを待機させない (CI 用) |

---

## 🧩 装飾 (Decorations) 記法

Markdown 内に短い構文を書くだけで、テーマに合った装飾を挿入できます。

### ボックス (28 種) — ブロック
```markdown
:::box-26 title="POINT"
これはボックス26の本文です。タイトル付きのデザイン。
:::
```

### リボン (22 種) — インライン
```markdown
本文テキスト [ribbon-7]リボン部分[/ribbon-7] 続きテキスト。
```

### 吹き出し (14 種) — インライン
```markdown
[bubble-3-r]右向き吹き出し[/bubble-3-r]
```

### 箇条書き (16 種) — ブロック
```markdown
:::list-9
- シェブロン
- シェブロン (›) アイコン
- おしゃれ
:::

:::list-13
1. 数字を丸で囲う
2. 番号付きリスト
3. 数字が円の中
:::
```

| 番号 | スタイル | 種別 |
| --- | --- | --- |
| 1 | モノクロ枠 | ul |
| 2 | 爽やか (青破線+水色背景) | ul |
| 3 | 上下ボーダー | ul |
| 4 | ドット囲み | ul |
| 5 | シャドウ | ul |
| 6 | ステッチ | ul |
| 7 | 付箋バー | ul |
| 8 | タグ風 | ul |
| 9 | シェブロン (›) | ul |
| 10 | チェック (✓) | ul |
| 11 | 指差し (☞) | ul |
| 12 | フラットバー | ul |
| 13 | 数字を丸で囲う | ol |
| 14 | 数字を四角で囲う | ol |
| 15 | POINT タブ付き | ul |
| 16 | 手書き風数字 | ol |

`1. ` で始まるリストは自動的に `<ol>` として展開されます。ピッカー UI (ツールバーの 📦 ボタン) からも挿入可能。**選択中のテキストを包む形で挿入** されます。

---

## ✒️ 見出しスタイル (13 プリセット)

デザインドロップダウン横の **「見出し」** で選択できます。テーマが切り替わってもアクセントカラーが連動します。

| グループ | プリセット |
| --- | --- |
| シンプル | default / marker / bubble / box |
| おしゃれ | ribbon / tag / stitch / stripe / arrow / corner / centerlines / double / gradient |

見出しは**全テーマで太字 (font-weight: 700)** になります。`centerlines` は左右に線が伸びるデザイン、`gradient` は線形グラデーションの背景、`marker` は蛍光マーカー風です。

---

## 📽️ スライドモード

編集画面右上の **「スライド」** チェックボックスを ON にすると、見出しレベルごとにページ区切りが挿入され、**16:9 ランドスケージ (297mm × 167mm)** で PDF が出力されます。境界レベルは H1 / H2 / H3 から選べます (既定は H2)。

スライドページには **テーマプライマリのカラーバー** が上部 4mm に表示され、見出しがスライドタイトルとして大きめにレイアウトされます。装飾・見出しプリセット・テーマはすべてスライドでも反映されます。

---

## 🎨 デザインシステム

| テーマ | アクセント | 雰囲気 |
| --- | --- | --- |
| **Stripe** (既定) | Indigo | プロフェッショナル・標準 |
| Sakura | 桜ピンク | 柔らかい・親しみ |
| Ocean | 群青 | クール・知的 |
| Sunset | 茜色 | 暖かい・元気 |
| Neon | ネオン蛍光 | ポップ・若手向け |
| Forest | 深緑 | 自然・落ち着き |
| Minimal | グレー | シンプル・印刷向け |
| Off | 素の md2html | 素の GitHub 風 |

CSS 変数 (`--omd-primary` / `--omd-heading` / `--omd-accent` 等) でトークン化されており、装飾・見出し・箇条書きがすべて同じトークンで連動します。

---

## ページ構成

- **編集 (`E`)** — Markdown を書いて、リアルタイムでプレビュー → HTML / PDF / **PPTX** を生成
- **一括変換 (`B`)** — 複数ファイル / `.zip` をまとめて変換、ZIP でダウンロード
- **履歴 (`H`)** — 過去の変換結果を検索 / フィルタ / 再ダウンロード
- **環境設定** — テーマ、ブラウザ起動、自動保存、サーバ情報を管理

各ページはサイドバー (`Ctrl+B` で折りたたみ) とパンくずリストで横断的に移動できます。

---

## ディレクトリ構成

```
資料作成工場くん/
├── md2html/          ← Phuker/md2html (clone)
├── WeasyPrint/       ← Kozea/WeasyPrint (clone, 参照用: 実体は pip wheel)
├── dashboard/        ← Flask 製 UI (今回の追加)
│   ├── app.py            … Flask サーバ + REST API
│   ├── converter.py      … md2html + WeasyPrint のブリッジ
│   ├── jobs.py           … 履歴の永続化
│   ├── templates/        … 画面テンプレート (base.html, index.html)
│   ├── static/css/       … デザインシステム (tokens / base / components / layout / pages)
│   ├── static/js/        … UI ロジック (icons / api / ui / editor / pages / app)
│   ├── samples/          … サンプル Markdown
│   ├── data/jobs.json    … 履歴 (自動生成)
│   └── outputs/<job>/    … 変換結果 (自動生成)
├── scripts/
│   ├── launcher.py       … ワンタッチ起動ランチャー
│   ├── stop.py           … 停止
│   └── status.py         … 状態表示
├── .venv/             ← 仮想環境 (start.bat が自動作成)
├── start.bat          ← ★ ワンタッチ起動
├── stop.bat           ← ★ 停止
├── status.bat         ← ★ 状態確認
├── start.ps1 / start.sh
├── requirements.txt
└── README.md
```

---

## 初回セットアップを手動でしたい場合

```powershell
cd "C:\Users\knsry\Desktop\資料作成工場くん"
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m pip install -e .\md2html
python -m dashboard
```

---

## WeasyPrint (PDF 生成) が無い環境では

Windows では WeasyPrint がネイティブ DLL (GTK3 / Pango / Cairo) を必要とします。`start.bat` の出力に
```
[!] WeasyPrint unavailable: PDF generation is disabled (HTML still works).
[!] To enable PDF: install MSYS2 + pango/cairo/gobject-introspection, then restart.
```
と出ても **HTML 変換は問題なく動きます**。
ステータスインジケータ (サイドバー下部) で現在の状態が確認できます。

PDF を有効化したい場合:
1. [MSYS2](https://www.msys2.org/) をインストール
2. MSYS2 ターミナルで:
   ```bash
   pacman -S mingw-w64-x86_64-pango mingw-w64-x86_64-cairo \
             mingw-w64-x86_64-gobject-introspection
   ```
3. `start.bat` を再実行 (ランチャーが `C:\msys64\mingw64\bin` を自動で見ます)

> ⚠ 200MB 以上の追加インストールが必要で、初回セットアップは `start.bat` で完結しますが、PDF を有効化するための GTK3 ランタイムだけは MSYS2 から別途導入が必要です。HTML だけでも普段の用途には十分です。

---

## REST API

| メソッド | パス | 説明 |
| --- | --- | --- |
| `GET`  | `/api/status` | md2html / WeasyPrint / Edge の可否とバージョン、装飾カウント (28/22/14/16)、見出しプリセット数 (13) |
| `POST` | `/api/preview` | `{markdown, design_system, heading_style, slide_mode, slide_level, cover, ...}` → HTML (永続化なし) |
| `POST` | `/api/convert` | 単体変換 (`multipart` or JSON `{name, markdown, design_system, make_pptx, ...}`) |
| `POST` | `/api/batch`   | 一括変換 (multipart: `files` または `zip`, `make_pptx` 対応) |
| `POST` | `/api/apply-design` | 既存 HTML に `{design_system, heading_style, slide_mode, slide_level}` を後付け適用 |
| `GET`  | `/api/jobs` | 履歴 |
| `GET`  | `/api/jobs/<id>/download?kind=html\|pdf\|pptx\|zip&name=...` | 成果物 |
| `DELETE` | `/api/jobs/<id>` | 履歴削除 |
| `GET`  | `/api/samples` | サンプル Markdown ファイル一覧 |
| `GET`  | `/api/samples/<name>` | サンプル Markdown の中身 |
| `GET`  | `/api/decorations` | 装飾レジストリ (boxes / ribbons / bubbles / lists + 構文) |
| `GET`  | `/api/heading-styles` | 見出しプリセット一覧 |

### 単体の例

```powershell
$body = @{ name = "hello.md"; markdown = "# Title`n`n**hi**" } | ConvertTo-Json
Invoke-RestMethod -Method Post -Uri http://127.0.0.1:8765/api/convert `
  -ContentType "application/json; charset=utf-8" -Body $body
```

### 一括の例

```powershell
curl -X POST http://127.0.0.1:8765/api/batch `
  -F "files=@a.md" -F "files=@b.md" -F "options={\"toc\":true}"
```

---

## キーボードショートカット

| キー | 動作 |
| --- | --- |
| `Ctrl/Cmd + K` | コマンドパレットを開く |
| `Ctrl/Cmd + B` | 太字 (エディタで) / サイドバー切替 |
| `Ctrl/Cmd + I` | 斜体 (エディタで) |
| `E` | 編集ページ |
| `B` | 一括変換ページ |
| `H` | 履歴ページ |
| `Ctrl/Cmd + Enter` | 変換を実行 (エディタで) |
| `Esc` | モーダル / コマンドパレットを閉じる |

---

## ライセンス

- 本ダッシュボードの追加コード: 親リポジトリの主旨に従い、プロジェクト内の `md2html` (GPLv3) と `WeasyPrint` (BSD) の条件を尊重します。
- 各ライブラリの権利表示は `WeasyPrint/LICENSE`, `md2html/LICENSE` を参照してください。
