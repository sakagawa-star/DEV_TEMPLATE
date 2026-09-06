# codex-01: update-011 初回レビュー（A）

- 日付: 2026-09-06
- 対象ファイル: `docs/issues/update-011-from-lift2d-milestone-layout/README.md`、`docs/issues/update-011-from-lift2d-milestone-layout/design.md`
- ストリーム: `rev-devtmpl-update-011`（ペイン w4:pB。新規起動）
- 依頼種別: A（初回レビュー）
- 直前に `/new` を送ったか: No（新規起動）
- ゲート状態: 未実施
- 指摘数: 高 2 / 中 0 / 低 0
- 収束判定: 未収束（次: 全件反映 → B）
- トークン実測（rollout jsonl の `total_token_usage` 累積）: input 448,576（うち cached 359,168）/ output 3,551（reasoning 2,046）/ total 452,127
- rollout jsonl: `~/.codex/sessions/2026/09/06/rollout-2026-09-06T19-08-48-01a07631-28ca-77d0-800c-adef4108d301.jsonl`

## 依頼文

> 以下のドキュメントをレビューせよ: docs/issues/update-011-from-lift2d-milestone-layout/README.md と docs/issues/update-011-from-lift2d-milestone-layout/design.md 。本案件は反映元リポジトリ /Private/sakagawa/git/lift2d-to-3d-keypoints（CLAUDE.md・AGENTS.md・docs/BUGFIX_STANDARD.md）の運用を template/ 配下へ取り込む反映設計であり、design.md の変更前テキストは現行の template/CLAUDE.md・template/AGENTS.md・template/docs/BUGFIX_STANDARD.md・template/docs/PROJECT_KNOWLEDGE.md・ルートの AGENTS.md・CLAUDE.md に一致している必要がある。レビュー観点は次の3点: (1) ドキュメント間の矛盾・参照切れ（存在しないファイル・セクションへの参照、フロー番号のずれ。design.md の変更後テキストを適用した後の template/CLAUDE.md 内部の整合を含む）(2) テンプレートとしての汎用性（プロジェクト固有記述の混入、プレースホルダの不足・不整合）(3) 新規プロジェクトへコピーした直後にそのまま運用できるか。

## レビュー結果

[AGENTS.md適用]

### 高

- template/AGENTS.md の新しい適合確認規定は、レビュー開始時に reviews/ ディレクトリが存在することを必須にしますが、フロー上は最初のレビュー結果を保存する段階で mkdir -p するだけです。空ディレクトリは Git 管理もされないため、新規案件の初回レビューで必ず不適合となり、自己矛盾します。ルート AGENTS.md の P15 も同じ問題を持ちます。
  修正案: 適合確認の必須対象から reviews/ を外す、または各レビュー工程の依頼前に reviews/ を作成することを明記してください。

- P13 の工程判定は feat では roadmap.md とマイルストーン文書、bug/update ではステップ4 だけを定義しています。しかし CLAUDE.md は案件フォルダ配下の criteria.md を独立に Codex レビューするよう規定しており、任意の補助文書も許容しています。これらをレビュー対象にすると工程・文書種別の対応が一意に決まらず、P13 自身の規定により「判定不能・高」となって criteria lock 等が収束不能になります。
  修正案: P13 に criteria 文書および許容する補助文書の工程・適合判定の対応を追加するか、それらを案件フォルダ／対象文書単位の適合確認の対象外として明記してください。

## 対応

- 高-1: P13・P15 の案件フォルダ単位の判定に「`reviews/` は結果の保存先であり存在の判定対象にしない」を明記。P15 の「存在すべき文書（README.md・design.md・reviews/）」から `reviews/` を除いた
- 高-2: P13 に「実験・検証の criteria 文書と案件直下の補助文書は本項目の対象外（これらだけが対象の依頼には本項目を適用せず、他の対象文書と同時指定の場合は対象文書単位の判定から除外）」を追加。反映元 lift2d の `AGENTS.md` にも同じ穴があるため、README に逆還元候補として記録
- 同じ会話で解消確認（codex-02）を依頼
