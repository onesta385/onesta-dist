# WordPress 制作規約（AI・人 共通）— ブロックのみ・カスタム HTML 禁止

対象: ONEstar / ONEsta を導入したサイトで、固定ページ・投稿・テンプレート・パターンを新規作成または修正するすべての AI エージェントと人。テーマは問わない（ONEstar 以外のテーマでも同じ）。

この文書はサイトごとに次の場所で読める。作業前に必ず全文を読み、従えない指示を受けたら作業を止めて依頼者に確認する。

- 各サイト: `https://<サイト>/wp-content/plugins/onesta/docs/ai-authoring-policy.md`（WordPress をサブディレクトリに置いたサイトは `/wp/` 等が間に入る）
- 公開リポジトリ: `https://raw.githubusercontent.com/onesta385/onesta-dist/main/docs/ai-authoring-policy.md`
- 正本: `onesta-core/onesta/docs/ai-authoring-policy.md`

## AI に作業させるときの渡し方

依頼文の先頭に次の 1 行を入れる。

```
作業前に https://raw.githubusercontent.com/onesta385/onesta-dist/main/docs/ai-authoring-policy.md を読み、その規約に従うこと。カスタム HTML は使わない。
```

## 絶対ルール

1. コンテンツは WordPress のブロックだけで作る。既存の標準ブロックと ONEsta ブロックの組み合わせで表現する。
2. カスタム HTML ブロック（`core/html`）を新規に使わない。既存の `core/html` を見つけたら、そのままにせずブロックへ作り直す提案をする。
3. 本文に `style` 属性、`<style>`、`<script>`、`<iframe>`（埋め込みブロックで代替できるもの）を書かない。見た目はブロックの設定（配置・幅・色・余白・タイポグラフィのプリセット）で決める。
4. 区切り線（`<hr>`・`core/separator`）と空白ブロック（`core/spacer`）を使わない。区切りは見出し・背景色・カラム分割で、余白はブロックの余白設定で作る。
5. サイト固有の CSS・JS・PHP が必要になったら本文に書かず、サイト専用プラグイン（または ONEsta の `custom.css` / `custom.php`）へ入れる依頼として切り出す。テーマ・ONEsta 本体には手を入れない。
6. コードエディタ（HTML 直接編集）に AI が生成した HTML を貼り付けない。貼り付けは必ずビジュアルエディタで行い、標準ブロックへ変換された結果を確認する。
7. 本番サイトを直接編集しない。編集はローカルまたは検証環境で行い、確認後に反映する。やむを得ず本番を直接編集する場合は、事前に依頼者へ知らせ、更新前の状態（リビジョン・DB ダンプ）を残す。

## 作り方の順序

1. 同じサイトの既存ページを開き、使われているブロックとクラス名（`className`）を確認する。新しい見た目を発明する前に、既存の語彙で組む。
2. ONEsta のブロックカタログ（`docs/block-guide.md`・`docs/agent-onestar-onesta-block-authoring-manual.md`）から目的に合うブロックを選ぶ。
3. 標準ブロック（グループ・カラム・見出し・段落・画像・ボタン・リスト・テーブル・クエリループ）で組む。グループの「グリッド」レイアウト（最小列幅）でカード並びを作る。
4. どうしてもブロックで表現できない場合は、作らずに「必要なブロックまたはサイト専用 CSS」の実装依頼として書き出す。

## やりたいこと → 使うもの

| やりたいこと | 使うもの | 使わないもの |
|---|---|---|
| 全幅の背景付きセクション | グループ（配置: 全幅・背景色）または `onesta/section` | `<div style="background:...">` |
| 2〜4 列のカード | グループ（グリッド・最小列幅）またはカラム | flex を書いた `<div>` |
| 見出し＋本文の帯 | 見出しブロック＋段落ブロック | `<h2 style="...">` |
| 区切り | 見出し・背景色の切り替え・カラム | `<hr>`・区切りブロック |
| 余白 | ブロックの余白（padding / margin）設定 | スペーサー・空の段落 |
| ボタン | ボタンブロック | `<a style="...">` |
| 丸バッジ・装飾 | クラス名を付けたグループ＋サイト専用 CSS の依頼 | `clip-path` 等の inline style |
| 表 | テーブルブロック | `<table style="...">` |
| Q&A・手順・比較 | ONEsta の FAQ・ステップ・比較ブロック | 手書き HTML |
| 記事一覧 | クエリループ | 手書きのカード HTML |

## 完了前の検査（すべて 0 件で合格）

- エディタで保存→再読込→再保存し、「想定されていないか無効なコンテンツ」の警告が 0 件
- 本文中の `<!-- wp:html`、`style=`、`<style`、`<script`、`<hr`、`wp:separator`、`wp:spacer` が 0 件
- 320・375・390・600・768・781・782・1024・1280px で横溢れ 0、コンソールエラー 0
- 既存ページと同じ見出し階層（h1 は 1 つ、h2→h3 の順）

保存済み本文の機械検査の例（wp-cli）:

```
wp post get <ID> --field=post_content | grep -c -E 'wp:html|style=|<style|<script|<hr|wp:separator|wp:spacer'
```

## 参照資料（ONEsta 同梱）

- `skills/onestar-site-builder/SKILL.md` — 導入・検証・配布の手順（ONEsta 開発者向け）
- `docs/agent-onestar-onesta-block-authoring-manual.md` — 編集意図別のブロック選択
- `docs/block-guide.md` — ブロック一覧と仕様
- `docs/site-custom-layer-policy.md` — サイト固有コードの置き場所

## 違反した成果物の扱い

`core/html` や inline style を含む成果物は受け入れない。ブロックで作り直してから再提出する。
