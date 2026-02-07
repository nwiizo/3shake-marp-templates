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

## ビルド後の確認

ビルドが完了したら、出力ファイルの存在を確認してください。

```bash
ls -la slides/[file].html slides/[file].pdf
```

エラーが発生した場合の対処:
- `--no-stdin` の付け忘れ → stdin待ちでハング（Ctrl+Cで中断し再実行）
- `--allow-local-files` の付け忘れ → ローカル画像が表示されない
- 画像パスのミス → 相対パス `../../assets/images/` を確認

## プレビュー

- ビルド後に毎回 `open` しない
- 確認が必要な場合のみ、該当ページを指定して開く: `open slides/[file].html#5`

## 出力ファイルの使い分け

| 形式 | 用途 |
|-----|------|
| HTML | ローカルプレビュー、ブラウザで確認 |
| PDF | 配布用、印刷用 |
