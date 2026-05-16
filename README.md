# statementsp 開発者向け確認セット

このフォルダは、`statementsp` の開発者に GitHub 上で見せるための最小セットです。
ローカルの作業ログ、ビルドキャッシュ、試行錯誤用ファイルは含めていません。

## 中身

- `patched/statementsp.sty`
  - 修正版の `statementsp.sty` です。
- `original/old-statementsp.sty`
  - TeX Live からコピーした元版です。人間が見比べやすいように `old-` を付けています。
- `original/statementsp.sty`
  - `old-statementsp.sty` と同じ内容です。
  - 元版 PDF を再ビルドするとき、TeX が `\usepackage{statementsp}` で読めるように、この名前でも置いています。
- `source/statementsp-maintainer-check-common.tex`
  - 元版・修正版で共通の確認本文です。
- `source/statementsp-maintainer-check-fixed.tex`
  - 修正版 PDF 用のラッパーです。タイトルだけを設定しています。
- `source/statementsp-maintainer-check-original.tex`
  - 元版 PDF 用のラッパーです。タイトルだけを設定しています。
- `pdf/statementsp-maintainer-check-fixed.pdf`
  - 修正版でコンパイルした確認 PDF です。
- `pdf/statementsp-maintainer-check-original.pdf`
  - 元版でコンパイルした確認 PDF です。
- `CHANGE_SUMMARY.md`
  - 問題点と修正内容の要約です。
- `tests/`
  - 個別の再現・回帰確認用 TeX ファイルです。

## 比較 PDF の再ビルド方法

リポジトリのルートから実行します。

```sh
TEXINPUTS="maintainer-review/patched:maintainer-review/source:.:" \
  latexmk -lualatex -interaction=nonstopmode -halt-on-error \
  -outdir=maintainer-review/build/fixed \
  maintainer-review/source/statementsp-maintainer-check-fixed.tex

TEXINPUTS="maintainer-review/original:maintainer-review/source:.:" \
  latexmk -lualatex -interaction=nonstopmode -halt-on-error \
  -outdir=maintainer-review/build/original \
  maintainer-review/source/statementsp-maintainer-check-original.tex
```

生成された PDF を `maintainer-review/pdf/` にコピーします。

## 比較の前提

確認本文は `statementsp-maintainer-check-common.tex` で共通化しています。
`fixed` と `original` のラッパーは、PDF のタイトルだけを変えています。
そのため、PDF 上の差分は基本的に読み込んだ `statementsp.sty` の違いによるものです。
