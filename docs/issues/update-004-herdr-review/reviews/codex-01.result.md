<!-- 2026-08-27 / 対象: update-004 README.md, design.md / ストリーム: rev-devtmpl-update-004 / フェーズ: 反復（初回。/new なし） / 指摘: 高0・中1・低0 / トークン累計: total 166,400（input 163,941 うち cached 125,696 / output 2,459） / rollout: ~/.codex/sessions/2026/08/27/rollout-2026-08-27T23-06-03-01a0438a-c685-7ee1-89bb-425b8ab2d92d.jsonl -->
<!-- マーカー確認: 回答冒頭に「[AGENTS.md適用]」あり（ルート AGENTS.md の読み込みを確認） -->
<!-- 対応方針: 中1件 → design 2.3-6 の残存参照 grep から full.log を分離し、「結果の保存」の意図的な1行のみ許容する2段構えの確認に変更。4章 検証3 も追従 -->

[AGENTS.md適用]

中: 設計どおりに反映すると、必須検証が必ず失敗します。

docs/issues/update-004-herdr-review/design.md:85 は新レビュー節に full.log を意図的に残しますが、docs/issues/update-004-herdr-review/design.md:133 は full.log を含む grep が0件でなければ中断する、としています。さらに docs/issues/update-004-herdr-review/design.md:174 もこれを合格条件にしています。そのため、完了処理へ進めません。

修正案: full.log をグローバルな残存禁止パターンから外し、旧コマンド・旧見出し・旧出力先などを個別に検査してください。full.log は「旧ログを保存しない」という新方式の説明および既存案件との互換説明で許容する、と明記するのが整合的です。
