# codex-06: update-012 人レビュー指摘（D5・D6 追加）対応後の解消確認（B。C の後）

- 日付: 2026-09-07
- 対象ファイル: `docs/issues/update-012-import-provenance-rule/README.md`、`docs/issues/update-012-import-provenance-rule/design.md`
- ストリーム: `rev-devtmpl-update-012`（ペイン w4:pC。codex-03（全文ゲート）〜05 と同一会話。`/new` なし）
- 依頼種別: B（解消確認。人レビューの指摘〔`template/CLAUDE.md` ドキュメント更新フロー ステップ5 の但し書き削除 = D5・D6〕を反映した後）
- 直前に `/new` を送ったか: No
- ゲート状態: 実施済み: codex-03.result.md
- 指摘数: 高 1 / 中 0 / 低 0
- 収束判定: 未収束（次: 全件反映 → B。同じ会話、`/new` なし）
- トークン実測（rollout jsonl の `total_token_usage` 累積。codex-03〜06 の合計）: input 1,060,264（うち cached 818,944）/ output 6,536（reasoning 3,656）/ total 1,066,800。codex-06 単体の増分: total 288,867
- rollout jsonl: `~/.codex/sessions/2026/09/06/rollout-2026-09-06T21-46-24-01a076c1-73ce-76c3-a060-1313feb307e6.jsonl`
- 案件累計（codex-01〜06）: total 1,460,653

## 依頼文

> 人（ユーザー）レビューの指摘に対応してドキュメントを更新した（D5・D6 の追加。template/CLAUDE.md ドキュメント更新フロー ステップ5 の但し書きを削除して Sonnet 委任にし、「実装の実行方法」の適用範囲を広げる）。変更点に問題があれば指摘して。

## レビュー結果

[AGENTS.md適用]

高: D5 は update 案件にも「実装の実行方法」を適用しますが、同節の「想定外事象」の復帰先が requirements.md / design.md または investigation.md のままです。update 案件にはそれらが存在せず、想定外時に必ず戻るべき調査・設計・レビューの経路が実行不能になります。
修正案: D5 の読み替えに、同節「想定外事象の扱い」の復帰先を update 案件では「ステップ2 の調査 → README.md / design.md の更新 → ステップ4 のレビュー」と読む旨を追加してください。

## 対応

- 高: D5 の読み替えに復帰先の読み替えを追加。改訂履歴・README 実施記録を同期
- 同じ会話で解消確認（codex-07）を依頼
