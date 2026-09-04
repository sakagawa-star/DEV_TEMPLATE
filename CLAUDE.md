# CLAUDE.md

このファイルはClaude Codeがこのリポジトリを理解するためのガイドです。

## このリポジトリについて

**新規開発プロジェクト用の開発ドキュメント・ワークフローをテンプレートとして管理する**リポジトリ。
`/home/sakagawa/git/ViTPose` で実践しているドキュメント駆動開発（実装前にドキュメントを作り、Subagent＋人でレビューしてから実装するワークフロー）を、他プロジェクトへ転用できるよう汎用化して保管する。

このリポジトリ自体は**テンプレートの置き場**であり、ここでアプリケーション開発は行わない。

## ディレクトリ構成

```
DEV_TEMPLATE/
├── CLAUDE.md                       # 本ファイル（このリポジトリの説明）
├── AGENTS.md                       # Codex が起動時に読む指示ファイル（レビュー定型指示。template/AGENTS.md と同一内容）
├── docs/
│   └── issues/                     # テンプレート改訂案件（update-XXX。README / design / reviews）
│       └── update-001-from-lift2d/ # 例: lift2d の運用実績を反映した案件
└── template/                       # 新規プロジェクトへコピーするテンプレート一式（payload）
    ├── CLAUDE.md                   # 新規プロジェクト用 CLAUDE.md 雛形（{{ }} プレースホルダ入り）
    ├── AGENTS.md                   # 新規プロジェクト用 Codex 指示ファイル（レビュー定型指示。そのまま転用可）
    ├── .gitignore                  # レビュー過程ログ等の除外設定（そのまま転用可）
    └── docs/
        ├── BACKLOG.md                # 案件一覧の空雛形（{{ }} プレースホルダ入り）
        ├── CHANGELOG.md              # リリース履歴の空雛形（そのまま転用可）
        ├── TECH_STACK.md             # 技術スタックの空雛形（{{ }} プレースホルダ入り）
        ├── HERDR_SETUP.md            # Herdr+codex レビュー環境のセットアップ手順（そのまま転用可）
        ├── REQUIREMENTS_STANDARD.md  # 要求仕様書 記述基準（そのまま転用可）
        ├── DESIGN_STANDARD.md        # 機能設計書 記述基準（そのまま転用可）
        ├── BUGFIX_STANDARD.md        # 不具合修正 記述基準（そのまま転用可）
        ├── REVIEW_CRITERIA.md        # レビュー基準（そのまま転用可）
        └── codex-exec-ubuntu24-bwrap-fix.md  # Ubuntu24でのcodex exec bwrap対策（環境依存メモ）
```

## テンプレートの使い方（新規プロジェクト立ち上げ手順）

1. `template/` 配下を新規プロジェクトのルートへコピーする（`.gitignore` を含む）
   - `template/CLAUDE.md` → プロジェクトルートの `CLAUDE.md`
   - `template/AGENTS.md` → プロジェクトルートの `AGENTS.md`
   - `template/.gitignore` → プロジェクトルートの `.gitignore`
   - `template/docs/*` → プロジェクトの `docs/`
2. コピーした `CLAUDE.md` / `docs/BACKLOG.md` / `docs/TECH_STACK.md` の `{{ }}` プレースホルダをプロジェクト固有の内容に置き換え、記入例コメントを削除する
3. `docs/` 直下の4基準（REQUIREMENTS / DESIGN / BUGFIX / REVIEW）は原則そのまま使う
4. `docs/issues/` は最初の案件作成時にフォルダを作る（空フォルダの事前作成は不要）

## テンプレート保守の方針

- 他プロジェクト（ViTPose、lift2d-to-3d-keypoints 等）での実践で得た改善は、汎用化してこのリポジトリの `template/` に反映する
- プロジェクト固有の記述（ドメイン知識・特定ライブラリ等）は `template/CLAUDE.md` に持ち込まず、プレースホルダまたは汎用例に留める
- 4基準ドキュメントはドメイン中立を保つ（特定分野の用語・例に依存しない）

## テンプレート改訂フロー（update-XXX 案件）

テンプレートの改良は、同一PC内の他リポジトリで実績ができた運用を取り込む形で行うことが多い。テンプレート（`template/` 配下）を編集する場合、以下のフローを**厳守**する。

1. **案件作成** → `docs/issues/update-{number}-{slug}/` フォルダを作成する（例: `update-001-from-lift2d`。slug には反映元がわかる名前を付ける）
2. **調査・計画** → 反映元リポジトリ（ユーザーがパスを指定する）のドキュメントとテンプレートを比較し、差分を洗い出す。汎用的な内容のみを反映対象として選別し（プロジェクト固有の記述は除外）、反映元パス・調査結果・反映対象の選別と理由を案件フォルダの `README.md` に記録する。**この時点ではテンプレートを編集しない**
3. **設計・保存** → どのテンプレートファイルに・どのように反映するかを、案件フォルダの `design.md` に書いてファイル保存する（自己完結・曖昧表現禁止。/clear 後でも design.md だけ読めば反映作業ができるレベルまで具体化する）。**保存が完了するまでテンプレートの編集に進んではならない**
4. **レビュー（Codex → 人）** → `template/CLAUDE.md` の「Codexによるレビューの実行方法（Herdr 対話方式）」を以下の読み替えで適用する。前提環境の確認、ストリームのライフサイクル、「レビューの進め方」（「収束」の定義・依頼 A/B/C の遷移表・人レビューの前提・逸脱の禁止）、「結果の保存」の冒頭メタ（依頼種別・`/new` の有無・ゲート状態・収束判定）はそのまま従う
   - ストリーム命名: `{{プロジェクト略称}}` は `devtmpl` と読み替える（例: `rev-devtmpl-update-004`）
   - パスの読み替え: 節内の `docs/HERDR_SETUP.md` は `template/docs/HERDR_SETUP.md`、`docs/codex-exec-ubuntu24-bwrap-fix.md` は `template/docs/codex-exec-ubuntu24-bwrap-fix.md` と読み替える
   - `docs/REVIEW_CRITERIA.md` の基準は本リポジトリでは適用せず、下記のレビュー観点3点の明示で代替する（従来どおりの運用）
   - 保存先: `docs/issues/{案件フォルダ}/reviews/`（codex-NN.result.md を git 管理。full.log は生成しない。過去案件の full.log のための除外設定は `.gitignore` に残置）
   - レビュー対象: `README.md` と `design.md`
   - レビュー観点: (1) ドキュメント間の矛盾・参照切れ（存在しないファイル・セクションへの参照、フロー番号のずれ）(2) テンプレートとしての汎用性（プロジェクト固有記述の混入、プレースホルダの不足・不整合）(3) 新規プロジェクトへコピーした直後にそのまま運用できるか
5. **実装** → `template/CLAUDE.md` の「実装の実行方法（Sonnetサブエージェント）」に準じ、Sonnet サブエージェントに design.md 準拠でテンプレートを更新させる。読み替え: 必読ドキュメントは 本ファイル → 案件 `README.md` → `design.md` → 変更対象のテンプレートファイル。テスト実行は不要。厳密準拠・想定外事象での即中断報告・git 操作や本ファイル更新の禁止・報告形式のルールはそのまま従う
6. **完了** → Claude Code 本体が差分を検証し、本ファイルのディレクトリ構成に変更があれば更新して、git 操作（後述）でコミットする。案件フォルダは完了後も削除・移動しない

## Claude Code 運用ルール

- **git 操作**: コミット・プッシュは `template/CLAUDE.md` の「git 操作の実行方法（Opusサブエージェント）」に従い、Opus サブエージェントに委任する（本リポジトリにもそのまま適用する）
- **Bash 実行**: `template/CLAUDE.md` の「Bash 実行時のルール」に従う（`cd <path> && <command>` 連結禁止。別ディレクトリでの実行は `git -C <path> ...` 等のコマンド側オプションを使う）
- テンプレート本文とルールを二重管理しない。本リポジトリ向けの読み替え（保存先・レビュー観点等）だけを本ファイルに書き、手順の本文は `template/CLAUDE.md` を参照する

## 未テンプレート化（今後の拡張候補）

- `docs/issues/` の案件テンプレート（README / requirements / design / investigation の空雛形）
- グローバルスキル（`~/.claude/commands/handover.md` / `review.md`）の同梱・運用ルール
