<!-- 2026-08-27 / 対象: update-006 README.md, design.md / session id: 01a04371-84d8-7561-901d-a97413f112b0 / 初回（初回実行はcodex側のモデル容量エラーで失敗し、同連番で再実行） -->
<!-- 対応方針: 中3件 → (1) マーカー欠落時の「未読込」断定を再実行→切り分け手順に変更 (2) コピー先相当の一時リポジトリでの実機確認を4章検証5として追加 (3) 反映元との diff を参考確認に格下げし設計書内全文との照合に変更。全件反映済み -->

致命的な問題はありません。指摘は中重要度3件です（低重要度は省略）。

- **中 — 適用マーカー欠落を「未読込」と断定している。**  
  [design.md](/Private/sakagawa/git/DEV_TEMPLATE/docs/issues/update-006-from-fmcs-agents-md/design.md:50) は、マーカーがない場合に `AGENTS.md` が未読込だと断定しています。しかし、指示は読まれていてもモデル出力が指示に従わない可能性はあります。原因を誤認し、有効なレビュー結果まで不必要に破棄します。  
  修正案: 「適用を確認できないため採用せず、新規セッションで再実行する。再発時は `AGENTS.md` の配置・起動ディレクトリ・CLI設定を確認する」とする。

- **中 — この変更の中心機能を実行検証しない設計になっている。**  
  [design.md](/Private/sakagawa/git/DEV_TEMPLATE/docs/issues/update-006-from-fmcs-agents-md/design.md:9) と [design.md](/Private/sakagawa/git/DEV_TEMPLATE/docs/issues/update-006-from-fmcs-agents-md/design.md:126) はテスト不要としていますが、今回追加するのは「新規プロジェクトで Codex が起動時に指示を読める」という運用上の契約です。静的な `diff` / `grep` だけでは、コピー後の配置・起動条件でマーカーが実際に出ることを保証できません。  
  修正案: 一時Gitリポジトリへ `template/` をコピーし、最小の `codex exec` レビューを1回実行して、`result.md` 先頭のマーカーを確認する手動検証を追加する。

- **中 — 設計書単体では検証を完遂できない。**  
  [design.md](/Private/sakagawa/git/DEV_TEMPLATE/docs/issues/update-006-from-fmcs-agents-md/design.md:102) は、検証に `/home/staff/sakagawa/git/fmcs-utils/AGENTS.md` を必須としています。これは反映元が同じ端末・パスに存在することを前提とし、テンプレート改訂フローが求める自己完結性に反します。しかも 2.1 に期待する全文は既に記載されています。  
  修正案: 外部ファイルとの `diff` を任意の参考確認に下げ、設計書内の全文との一致を確認する検証へ変更する。

参照切れ、セクション指定の不一致、フロー番号のずれは見当たりませんでした。`docs/REVIEW_CRITERIA.md` 等が現リポジトリ直下にない点も、反映対象が `template/docs/` であるため、この案件の参照切れには当たりません。