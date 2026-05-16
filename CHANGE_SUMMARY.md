# statementsp 修正内容の要約

## 対応した問題

1. `ntheorem[amsthm]` との競合

   元版の `statementsp` は内部で `amsthm` を読み込んでいました。
   文書側で次のように `ntheorem` を使うと、`amsthm` 互換の定理系コマンドが重複して定義されます。

   ```tex
   \usepackage[amsthm]{ntheorem}
   \usepackage{statementsp}
   ```

   その結果、次のエラーが出ます。

   ```text
   LaTeX Error: Command \theoremstyle already defined.
   ```

2. `.sty` 内でのパッケージ読み込み

   元版では `.sty` 内で `\usepackage` を使っていました。
   修正版ではパッケージ内部の作法に合わせて `\RequirePackage` に変更しました。

3. `tcolorbox` のオプション付き読み込み

   元版では `\usepackage[many]{tcolorbox}` を実行していました。
   修正版では `tcolorbox` 本体を通常読み込みし、必要なライブラリだけを
   `\tcbuselibrary{skins,breakable}` で指定しています。

4. `hyperref` オプションの強制

   元版は `hyperref` のオプションをパッケージ側から強制していました。
   その中には `pdftitle={}` や `pdfauthor={}` など、空の PDF メタデータ指定も含まれていました。
   修正版ではこれらを強制せず、文書側の設定を優先します。
   文書側が `hyperref` を読み込んでいない場合だけ、プリアンブル末尾で読み込みます。

5. 初回コンパイル時の参照値

   元版では、statement 名、Preview/Recall 用本文、個別名をその場で定義せず、
   aux ファイルに書いたものを次回コンパイルで読みます。
   そのため、1回目のコンパイルでは `\refcallsp`、`\refsp`、`\refnamesp` に
   `??` や undefined reference が出ます。

   修正版では、これらの値を aux ファイルに書くだけでなく、その場でも定義します。
   さらに、表示用の参照文字列を `statementsp` 側でも保持するようにしました。
   そのため、定義後の `\refsp`、`\refnamesp`、Recall のタイトルは初回コンパイルから表示できます。
   ただし、定義前 Preview は未来の statement 本文を必要とするため、初回だけは aux が必要です。

6. optional 引数を完全に省略した場合の `-NoValue-`

   元版では、角括弧のラベルと丸括弧の個別名を完全に省略すると、
   `xparse` の内部値 `-NoValue-` がタイトルなどに出る場合がありました。
   修正版では、完全省略を空文字として扱います。
   元版で安全だった `[]()` の明示的な空指定も、そのまま使えます。

   参照まわりでは、元版は `-NoValue-` をラベルにも使ってしまうため、
   同じ種類の statement で optional 引数を完全省略すると
   `Label 'th:-NoValue-' multiply defined` のような重複ラベル警告も出ます。

7. 星付き環境の参照番号

   元版の `statementsp*` は `\label` を置きますが、番号を持つ
   `\refstepcounter` を使っていません。
   そのため `\refsp{lem:star}` のような参照では、種類名だけが出て番号部分が空になります。

   修正版では、星付き statement は番号なし参照として扱い、
   `\refsp{lem:star}` は `Lemma` のように種類名を表示します。
   `\refnamesp{lem:star}` は `Lemma :(Zorn)` のように個別名も表示します。
   ハイパーリンク用には `\phantomsection` を置きます。

8. verbatim 本文

   既存の `statementsp` / `statementsp*` の挙動は維持したまま、
   verbatim 対応用の `statementspverb` / `statementspverb*` を追加しました。
   既存機能を壊さないため、通常の `statementsp` を verbatim 用に置き換える実装にはしていません。

## 個別テスト

- `tests/ntheorem-amsthm.tex`
- `tests/hyperref-before.tex`
- `tests/hyperref-after.tex`
- `tests/omitted-options.tex`
- `tests/starred-ref.tex`
- `tests/undefined-refcall.tex`
- `tests/verbatim-body.tex`
- `tests/old-reference-behavior.tex`
