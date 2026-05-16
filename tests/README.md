# 個別の回帰テスト

比較 PDF とは別に、修正版の挙動を小さく確認するための TeX ファイルです。

- `ntheorem-amsthm.tex`
  - `\usepackage[amsthm]{ntheorem}` と併用してもコンパイルできることを確認します。
  - 元版では `Command \theoremstyle already defined` が出ます。
- `hyperref-before.tex`
  - 文書側が `statementsp` より先に `hyperref` を読み込む場合の確認です。
- `hyperref-after.tex`
  - 文書側が `statementsp` より後に `hyperref` を読み込む場合の確認です。
- `omitted-options.tex`
  - optional 引数を完全に省略しても `-NoValue-` が出ないことを確認します。
- `starred-ref.tex`
  - 星付き statement と Recall の確認です。
- `undefined-refcall.tex`
  - 未定義の `\refcallsp` に対するフォールバック確認です。
- `verbatim-body.tex`
  - `statementspverb` / `statementspverb*` で verbatim 本文を扱えることを確認します。
- `old-reference-behavior.tex`
  - 元版の参照挙動を切り分けるための最小ケースです。
  - 1回目のコンパイルでは `??` や undefined reference が出ます。
  - 複数回コンパイルすると多くは解消しますが、星付き環境の `\refsp` は番号部分が空になります。
