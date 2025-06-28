# 🐱‍👤 M1 MacBook向け LabelImg 初心者ガイド  
### ― Python + LabelImg で画像アノテーションを始めよう ―

---

## ✅ 対象者

- M1/M2 MacBookユーザー
- ターミナルやPythonが初めての人でもOK
- 画像アノテーションをこれから始めたい初心者

---

## 📦 使用ツール

- **ターミナル.app**（Macに標準搭載）
- **VS Code（Visual Studio Code）** ※任意（コードの修正に便利）
- **Python 3.10（Homebrew経由でインストール）**
- **pipx**（Pythonアプリケーションの管理）
- **LabelImg 1.8.5**（アノテーションツール）

---

## 🗂️ 1. 環境のリセットとセットアップ

### ステップ1：ターミナル.appを開く

macOS の「Launchpad」または「Spotlight（⌘ + Space）」で `ターミナル` と検索して開いてください。

---

### ステップ2：Homebrew（パッケージ管理ツール）のインストール

**ターミナルに以下をコピー＆ペーストして実行します。**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

### ステップ3：Python 3.10 のインストール（ターミナルで実行）

```bash
brew install python@3.10
```

---

### ステップ4：pipx のインストール（ターミナルで実行）

```bash
brew install pipx
pipx ensurepath
exec $SHELL -l
```

---

### ステップ5：LabelImg 1.8.5 のインストール（ターミナルで実行）

```bash
pipx install --python $(brew --prefix python@3.10)/bin/python3.10 labelImg==1.8.5
```

---

## 🏷️ 2. アノテーション用画像の準備

### ステップ6：画像フォルダを作成（ターミナルで実行）

```bash
mkdir -p ~/annotations/images/raw
cd ~/annotations
```

---

### ステップ7：画像を保存

「猫5枚＋犬5枚」など、JPEG画像（例: `cat_1.jpg`, `dog_1.jpg`）を `~/annotations/images/raw/` フォルダに保存します。

---

## 🐍 3. LabelImg の起動と初期設定

### ステップ8：LabelImg を起動（ターミナルで実行）

```bash
labelImg ~/annotations/images/raw
```

---

## 🔧 4. LabelImg がクラッシュする場合の修正

### ステップ9：canvas.py の修正

#### ⬅️ ファイルを VS Code で開く

```bash
code ~/.local/pipx/venvs/labelimg/lib/python3.10/site-packages/libs/canvas.py
```

#### 🔧 修正内容（float → int）

以下のような箇所をすべて修正します。

**修正前：**

```python
p.drawRect(left_top.x(), left_top.y(), rect_width, rect_height)
```

**修正後：**

```python
p.drawRect(int(left_top.x()), int(left_top.y()), int(rect_width), int(rect_height))
```

他にも以下のように `int(...)` を使って float を明示的に変換します。

```python
p.drawLine(self.prev_point.x(), 0, self.prev_point.x(), self.pixmap.height())
↓
p.drawLine(int(self.prev_point.x()), 0, int(self.prev_point.x()), int(self.pixmap.height()))
```

---

## 🎨 5. LabelImg の使い方（かんたん解説）

1. **画像フォルダ**：`~/annotations/images/raw` を選択
2. **クラス名**：`cat`, `dog` などを入力
3. **矩形（バウンディングボックス）作成**：マウスでドラッグ
4. **保存形式**：PascalVOC (XML形式)
5. **保存先**：同じフォルダ or `annotations/labels/` を作って保存

---

## 🚀 終了と保存

- `Ctrl + S`：現在のアノテーションを保存
- `Cmd + Q` または画面上の×：LabelImgを終了

---

## 💡 よくあるエラーと対処法

| 症状 | 対処方法 |
|------|----------|
| 起動後すぐ落ちる | `canvas.py` を確認し、float→int の修正を再確認 |
| `labelImg: command not found` | `pipx ensurepath` 後に `exec $SHELL -l` を実行 |
| リソースファイルが無い | LabelImg 1.8.5 では同梱済みなので不要 |

---

## ✅ 完了！

お疲れさまでした！この手順を踏めば、M1 MacBook でも LabelImg を安定して使えるようになります。画像アノテーションの第一歩を踏み出しましょう！

---
