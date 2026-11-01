# AGENTS.md

このファイルは、本リポジトリで作業するエージェント向けの基本情報をまとめたものです。

## プロジェクト概要

madr-manager は、Structured MADR v4.0.0 フォーマットに従って ADR（Architectural Decision Records）を対話的に作成・管理する Claude Code プラグインです。

- 実装はスキル定義と Markdown のリファレンスのみで構成され、ビルドやテストの対象となるコードはありません。
- プラグイン名は `madr-manager`、スキル名は `managing-madr` です。
- ライセンスは MIT です。

## サポートする操作

| 操作 | ワークフロー |
|---|---|
| ADR の新規作成 | `references/workflow/create-adr.md` |
| ADR の一覧表示 | `references/workflow/list-adrs.md` |
| ADR のステータス更新 | `references/workflow/update-adr-status.md` |
| ADR の内容表示 | `references/workflow/show-adr.md` |
| トピック別の決定履歴の要約 | `references/workflow/summarize-adr-history.md` |
| `index.md` の生成 | `references/workflow/generate-index.md` |

ワークフローのパスはいずれも `skills/managing-madr/` からの相対パスです。

## ディレクトリ構成

```txt
.
├── .claude-plugin/
│   └── plugin.json              # プラグインマニフェスト
├── skills/
│   └── managing-madr/
│       ├── SKILL.md             # スキル定義（操作の振り分けと読み込みルール）
│       └── references/
│           ├── spec/            # 仕様とテンプレート
│           └── workflow/        # 操作ごとの手順書
├── README.md                    # 英語版 README
├── README_ja.md                 # 日本語版 README
└── LICENSE
```

- `SKILL.md` は、ユーザーの依頼がどの操作に当たるかを判定し、対応するワークフローの読み込みを指示します。
- `references/spec/` には、Structured MADR の仕様書（`structured-madr-spec.md`）、ADR のテンプレート（`adr-template.md`）、一覧ファイルのテンプレート（`index-template.md`）があります。
- `references/workflow/` には、操作ごとの手順書があります。

## 設計上の前提

ワークフローやテンプレートを編集するときは、次の前提を崩さないでください。

- **1 つの ADR には 1 つの決定だけを記録する。** 複数の決定が含まれる場合は ADR を分割し、相互リンクで関係を表す。
- **ADR は削除しない。** rejected や deprecated になったものも履歴として残す。ステータス更新では、ステータス以外の内容を変更しない。
- **ファイル名は `NNNN-title-with-dashes.md`（4 桁の連番）とする。** 保存先は、ユーザーが指定しなければ確認したうえで、既定値として `docs/decisions/` を提案する。
- **ステータスは `proposed` / `accepted` / `rejected` / `deprecated` / `superseded by ADR-XXXX` のいずれか。** supersede するときは、新旧の ADR の `More Information` に相互リンクを追記する。
- **`index.md` を ADR の保存先に置き、作成・更新・ステータス変更のたびに同期する。** 存在しなければ `generate-index.md` で生成する。
- **ADR の本文は、ユーザーが会話している言語で書く。**
- **任意セクションに、プレースホルダーのテキストを残さない。** 内容がなければセクションごと省く。

## スキルの読み込みルール（SKILL.md）

- 同じファイルを 2 回読まない。読み込み済みかどうかを確認してから読む。
- `references/workflow/` 配下は先読みを禁止し、該当する操作を実行する直前に読む。
- `references/spec/` 配下は、必要に応じて先読みしてよい。
- 中間ファイルが必要なときだけ、OS の一時ディレクトリ（`mktemp -d -t madr-session-XXXXXX`）を使う。最終的な ADR ファイルはそこに書かない。
- `allowed-tools` で許可するツールは最小限に保つ。ツールを追加するときは、必要性を確認してから `SKILL.md` の frontmatter を更新する。

## 編集時の注意

- 新しいワークフローを追加するときは、次の 3 か所を揃えて更新する。
  - `references/workflow/` に手順書を追加する。
  - `SKILL.md` の「Operations」と「Resources」に追記する。
  - `README.md` と `README_ja.md` の「サポートする操作」に追記する。
- `README.md` と `README_ja.md` は同じ内容を保つ。片方だけを更新しない。
- `SKILL.md` とワークフロー、テンプレートは英語で書く。README は英語版と日本語版を用意する。
- フォーマットの根拠は `structured-madr-spec.md` にある。テンプレートやワークフローと仕様が食い違うときは、仕様を優先する。
- バージョンを上げるときは、`.claude-plugin/plugin.json` の `version` を更新する。

## 動作確認

自動テストはありません。変更後は、プラグインを Claude Code に読み込み、次の依頼を実際に試して確認します。

1. 議事録やメモから ADR を作成する（番号の採番、ファイル名、`index.md` の更新）。
2. 既存 ADR の一覧を表示する。
3. ADR を supersede し、新旧の ADR と `index.md` が揃って更新されることを確認する。
4. 特定トピックについて、決定の経緯を要約させる。
