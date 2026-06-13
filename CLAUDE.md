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
└── template/                       # 新規プロジェクトへコピーするテンプレート一式（payload）
    ├── CLAUDE.md                   # 新規プロジェクト用 CLAUDE.md 雛形（{{ }} プレースホルダ入り）
    └── docs/
        ├── REQUIREMENTS_STANDARD.md  # 要求仕様書 記述基準（そのまま転用可）
        ├── DESIGN_STANDARD.md        # 機能設計書 記述基準（そのまま転用可）
        ├── BUGFIX_STANDARD.md        # 不具合修正 記述基準（そのまま転用可）
        ├── REVIEW_CRITERIA.md        # レビュー基準（そのまま転用可）
        └── codex-exec-ubuntu24-bwrap-fix.md  # Ubuntu24でのcodex exec bwrap対策（環境依存メモ）
```

## テンプレートの使い方（新規プロジェクト立ち上げ手順）

1. `template/` 配下を新規プロジェクトのルートへコピーする
   - `template/CLAUDE.md` → プロジェクトルートの `CLAUDE.md`
   - `template/docs/*` → プロジェクトの `docs/`
2. コピーした `CLAUDE.md` の `{{ }}` プレースホルダをプロジェクト固有の内容に置き換え、冒頭の「テンプレート利用ガイド」コメントを削除する
3. `docs/` 直下の4基準（REQUIREMENTS / DESIGN / BUGFIX / REVIEW）は原則そのまま使う
4. 必要に応じて `docs/TECH_STACK.md` / `docs/BACKLOG.md` / `docs/issues/` を作成する（未テンプレート化）

## テンプレート保守の方針

- ViTPose 等での実践で得た改善は、汎用化してこのリポジトリの `template/` に反映する
- プロジェクト固有の記述（ドメイン知識・特定ライブラリ等）は `template/CLAUDE.md` に持ち込まず、プレースホルダまたは汎用例に留める
- 4基準ドキュメントはドメイン中立を保つ（特定分野の用語・例に依存しない）

## 未テンプレート化（今後の拡張候補）

- `docs/TECH_STACK.md` の空テンプレート
- `docs/BACKLOG.md` の空テンプレート（ロードマップ＋案件表の雛形）
- `docs/issues/` の案件テンプレート（README / requirements / design / investigation の空雛形）
- グローバルスキル（`~/.claude/commands/handover.md` / `review.md`）の同梱・運用ルール
