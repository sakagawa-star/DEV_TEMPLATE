<!-- 2026-08-27 / 対象: update-004 README.md, design.md / ストリーム: rev-devtmpl-update-004 / フェーズ: ゲート（/new 実施後の全文レビュー） / 指摘: 高0・中1・低0 / トークン累計: total 403,516（input 400,939 うち cached 344,832 / output 2,577） / rollout: ~/.codex/sessions/2026/08/27/rollout-2026-08-27T23-09-35-01a0438e-0294-7f03-a5b2-f64e6b671c4e.jsonl -->
<!-- マーカー確認: 回答冒頭に「[AGENTS.md適用]」あり -->
<!-- 備考: 1回目のゲート実行は codex 側の容量エラー（Selected model is at capacity）で回答未生成。同一会話で同文を再送し成功（2回目） -->
<!-- 対応方針: 中1件 → design 2.1 の `herdr agent wait {ストリーム名} --timeout {MS}` の {MS} を具体値 1800000 に確定（レビュー依頼の待機と同値）。fmcs-utils 原文にも存在する未定義をテンプレート側で修正するもので、2.1 冒頭の差分注記にも追記 -->

[AGENTS.md適用]

## 中

- docs/issues/update-004-herdr-review/design.md:39 の herdr agent wait {ストリーム名} --timeout {MS} で、{MS} の値・選定基準が定義されていません。working 状態からの回復手順がそのまま実行できず、新規プロジェクトで待機運用が止まります。レビュー用の 1800000 と統一するなど、具体値を明記してください。

それ以外は、指定2文書の相互参照、対象ファイル、フロー番号、プレースホルダ、コピー後の配置計画に致命的な矛盾・参照切れは確認できませんでした。
