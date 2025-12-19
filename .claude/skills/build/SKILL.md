# スライドビルド

Marpスライドのビルドとプレビューを行います。

## 使い方

`/build <ファイル名>` でスライドをビルドします。

## ビルドワークフロー

**重要**: `--no-stdin` を付けないとstdin待ちでハングします。必ず付与してください。

### 1. ファイルパスの確認

```bash
# slides/ 配下のmdファイルを確認
ls slides/**/*.md
```

### 2. HTML出力（プレビュー用）

```bash
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin
```

### 3. PDF出力（配布用）

```bash
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin
```

### 4. HTMLとPDFを連続生成

```bash
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin && \
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin
```

### 5. ライブプレビュー（編集しながら確認）

```bash
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files --watch
```

## 開発サーバー

| コマンド | 用途 |
|---------|------|
| `npm start` | ライブリロード付きローカルサーバー（ポート8080） |
| `npm run preview` | プレビューモード |

## 出力ファイルの使い分け

| 形式 | 用途 |
|-----|------|
| HTML | ローカルプレビュー、ブラウザで確認 |
| PDF | 配布用、印刷用 |
