<!-- 2026-08-27 / 対象: update-005 README.md, design.md / session id: 01a0436f-04f4-72e1-9f66-4ba72d892e61 / 初回 -->
<!-- 対応方針: 高1件（On Hold 運用と feat フローの README.md 非必須の不整合）→ 凡例に「README.md 未作成の案件では作成して記録する」を追記し、README.md に適応の経緯を記録して対応（フロー側の README 必須化はスコープ外・別案件候補とした） -->

レビュー結果：高 1件、中・低 0件です。参照元の fmcs-utils パス・指定コミット・案件ファイルはいずれも実在し、フロー番号のずれもありません。

- **高 — On Hold 運用がテンプレートの案件フローと両立しない**

  [design.md](/Private/sakagawa/git/DEV_TEMPLATE/docs/issues/update-005-from-fmcs-onhold-status/design.md:39) は On Hold 時に「案件フォルダの README.md のステータスも On Hold に更新」と定めます。一方、反映対象外としている [template/CLAUDE.md](/Private/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:71) の feat フローは `requirements.md` と `design.md` しか作成を指示せず、README.md の作成・ステータス記録を要求しません。新規プロジェクトで feat を On Hold にすると、更新すべき README.md が存在せず、その場で運用を補完する必要があります。

  修正提案：`template/CLAUDE.md` も変更対象に含め、全案件種別で案件 README.md を作成し、ステータスを記録・更新することを明記してください。少なくとも feat フローの案件作成時に README.md 作成と初期ステータス記録を追加し、On Hold の定義と整合させる必要があります。