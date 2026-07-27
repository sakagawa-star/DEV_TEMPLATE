**高**
- [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:36) / [同:49](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:49) / [同:73](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:73) / [template/docs/REVIEW_CRITERIA.md](/home/sakagawa/git/DEV_TEMPLATE/template/docs/REVIEW_CRITERIA.md:41)  
  `docs/BACKLOG.md` と `docs/TECH_STACK.md` が必須運用ファイルとして参照されていますが、`template/` payload に存在しません。コピー直後に「BACKLOGへ追加」「TECH_STACK参照・更新」が実行不能になります。  
  修正案: `template/docs/BACKLOG.md` と `template/docs/TECH_STACK.md` の空テンプレートを追加し、`CLAUDE.md` のディレクトリ構成と実ファイルを一致させる。

- [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:89) / [同:116](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:116)  
  不具合修正フローの案件作成で「`README.md` に不具合概要と再現手順を記録」とありますが、案件ディレクトリ構成では `docs/issues/{案件}/README.md` がその役割です。ルート `README.md` も payload に存在せず、コピー直後に記録先が割れます。  
  修正案: line 89 を `docs/issues/bug-{number}-{slug}/README.md` に記録する指示へ変更。ルート `README.md` はユーザー向け仕様が変わる場合のみ更新対象にする。

- [template/docs/BUGFIX_STANDARD.md](/home/sakagawa/git/DEV_TEMPLATE/template/docs/BUGFIX_STANDARD.md:62) / [同:85](/home/sakagawa/git/DEV_TEMPLATE/template/docs/BUGFIX_STANDARD.md:85) / [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:160)  
  BUGFIX_STANDARD は必要に応じて `requirements.md` / `design.md` も修正・保存すると読めますが、不具合修正レビューの Codex プロンプトは `investigation.md` だけをレビュー対象にしています。設計書変更がレビューゲートを通らない可能性があります。  
  修正案: 不具合修正レビューの対象を「`investigation.md` と、変更した `requirements.md` / `design.md`」に拡張する。ステップ3にも「変更した関連ドキュメントをすべて保存」と明記する。

**中**
- [template/docs/REVIEW_CRITERIA.md](/home/sakagawa/git/DEV_TEMPLATE/template/docs/REVIEW_CRITERIA.md:4) / [同:47](/home/sakagawa/git/DEV_TEMPLATE/template/docs/REVIEW_CRITERIA.md:47) / [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:178)  
  レビュー対象はドキュメントとコードですが、重要度「高/中/低」の定義がコード向けだけです。一方で運用上はドキュメントレビューでも「高・中ゼロ」が終了条件なので、判定基準が不安定になります。  
  修正案: ドキュメント用の重要度定義を追加する。例: 高=実装不能/重大矛盾/参照切れ、中=誤実装につながる曖昧さや必須条件漏れ、低=運用に影響しない表記。

- [template/docs/BUGFIX_STANDARD.md](/home/sakagawa/git/DEV_TEMPLATE/template/docs/BUGFIX_STANDARD.md:86) / [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:92)  
  BUGFIX_STANDARD の差し戻し運用では「ユーザーとレビュー」と書かれていますが、CLAUDE.md 本体は「Codex再帰レビューで高・中ゼロ → 人レビュー」を必須化しています。手順解釈によって Codex レビューを飛ばせます。  
  修正案: BUGFIX_STANDARD 側も「Codexレビュー → 必要修正 → 人レビュー」に統一する。

- [template/CLAUDE.md](/home/sakagawa/git/DEV_TEMPLATE/template/CLAUDE.md:143)  
  `*.full.log` は `.gitignore` とされていますが、payload に `.gitignore` がありません。コピー直後は重いレビュー過程ログを誤って git 管理する可能性があります。  
  修正案: `template/.gitignore` を追加し、少なくとも `docs/issues/*/reviews/*.full.log` を入れる。

**低**
- 致命的な低重要度指摘はありません。

確認のみで、ファイル変更はしていません。