# update-001-from-lift2d

## 概要

`~/git/lift2d-to-3d-keypoints`（コミット d50569a 時点）で実績ができた運用ルールを `template/` に反映した案件。

- **反映元**: `/home/sakagawa/git/lift2d-to-3d-keypoints`
- **ステータス**: Closed（2026-07-27、コミット 31b7064）
- **注記**: 本案件は「テンプレート改訂フロー（update-XXX 案件）」の明文化前に実施したため、`design.md` はない。調査・計画は反映元リポジトリで作成された反映依頼書（`template-update-from-lift2d-20260727.md`、反映完了後に削除）が担った。本案件の実施経験をもとにフローを明文化した

## 反映内容（計画＝反映依頼書の要点）

1. 実装の Sonnet サブエージェント委任（指示6項目＋委任しない作業）
2. git 操作の Opus サブエージェント委任（指示5項目＋委任しない作業）
3. CHANGELOG.md の導入と完了ステップ8の拡充（README.md 更新義務を含む）
4. CLAUDE.md の肥大化防止方針（完了済み案件はポインタ1行、コマンド例は README.md を正とする）
5. 追加発見分: Codexレビューのバックグラウンド実行ルール、案件構成図への `reviews/` 追加、「テスト」セクション新設
6. 基準ドキュメントへのマージ: BUGFIX_STANDARD.md（仕様未定義は feat 扱い、要求ID対応、設計書整合性等）、REVIEW_CRITERIA.md（ドキュメント間の一貫性、技術スタックの整合性）

## Codexレビュー

`reviews/` に結果を保存（3サイクル: 高3・中3 → 高0・中0 に収束後、人レビューで承認）。

- codex-01: 高3（BACKLOG/TECH_STACK の payload 欠落、bug 案件の README 記録先の曖昧さ、不具合修正レビュー対象の不足）、中3（ドキュメント重要度分類の欠如、BUGFIX_STANDARD のレビュー順序不整合、payload に .gitignore がない）
- codex-02: 中2（実装サブエージェントの必読リスト未追従、グローバル gitignore による template/.gitignore の未追跡）
- codex-03: 高0・中0・低0（収束）
