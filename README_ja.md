# madr-manager

[English](README.md) | 日本語

Structured MADR v4.0.0 形式の MADR（Markdown Architectural Decision Records）を、対話しながら作成・管理する Claude Code プラグインです。

## 概要

このプラグインは、設計や運用の意思決定を、会話しながら ADR（Architectural Decision Records）ファイルとして記録します。
議事録や会話の記録から ADR を作成できるほか、既存 ADR の一覧表示、ステータスの更新、内容や決定の経緯の確認もできます。

**サポートする操作:**

- **作成**: 対話しながら新しい ADR を作成する。1 つの ADR には 1 つの決定だけを記録し、複数の決定を含む議事録は ADR ごとに分ける
- **一覧**: プロジェクト内の既存 ADR を一覧表示する
- **ステータス更新**: 既存 ADR のステータスを変更する（proposed / accepted / rejected / deprecated / superseded）
- **表示**: 特定の ADR の全文を表示する
- **index.md の生成**: プロジェクト内の全 ADR をまとめた一覧ファイル（index.md）を生成する
- **決定履歴の確認**: index.md から関連する ADR をリンクでたどり、決定に至った議論や理由を要約する

**動作のルール:**

- ADR は削除しません。rejected や deprecated になったものも、履歴として残します。
- 新しい ADR が既存の ADR を置き換えるときは、旧 ADR のステータスを `superseded by ADR-XXXX` に更新し、新旧の ADR にお互いへのリンクを追記します。
- ADR の作成やステータス更新のたびに、index.md も更新します。index.md がなければ新しく生成します。
- ADR の本文は、会話と同じ言語で書きます。

## プロジェクト構成

```txt
madr-manager/
├── .claude-plugin/
│   └── plugin.json                  # プラグインマニフェスト
├── skills/
│   └── managing-madr/
│       ├── SKILL.md                 # スキル定義（操作の振り分けと読み込みルール）
│       └── references/
│           ├── spec/
│           │   ├── adr-template.md      # ADR テンプレート
│           │   ├── index-template.md    # index.md テンプレート
│           │   └── structured-madr-spec.md  # Structured MADR v4.0.0 仕様書
│           └── workflow/
│               ├── create-adr.md        # ADR 作成ワークフロー
│               ├── generate-index.md    # index.md 生成ワークフロー
│               ├── list-adrs.md         # ADR 一覧ワークフロー
│               ├── show-adr.md          # ADR 表示ワークフロー
│               ├── summarize-adr-history.md  # 決定履歴確認ワークフロー
│               └── update-adr-status.md # ADR ステータス更新ワークフロー
├── AGENTS.md                        # エージェント向けの基本情報
├── LICENSE
├── README.md
└── README_ja.md
```

## インストール

マーケットプレイスから madr-manager をインストールします。

```bash
# プラグインのインストール
/plugin install madr-manager@zv-plugins
```

## 使い方

インストール後は、Claude Code に ADR の管理を頼むだけで使えます。

```markdown
# 新規 ADR の作成
〇〇〇{議事録ファイルなど} から ADR をつくって
アーキテクチャ上の決定を記録したい
PostgreSQL を採用した理由を ADR に残したい

# 既存 ADR の一覧表示
既存の ADR を一覧表示して

# ADR ステータスの更新
ADR-0002 を deprecated に更新して
ADR-0003 は ADR-0005 に supersede されたのでステータスを更新して

# ADR の表示
ADR-0001 を表示して

# index.md の生成
ADR の一覧ファイル（index.md）を生成して

# 決定履歴の確認
〇〇〇について、現在の状況はどうなっている？
ADR を参照して、いままでの議論や決定の経緯を教えてほしい
```

ADR ファイルのファイル名は `NNNN-title-with-dashes.md` の形式で、NNNN は 4 桁の連番です。
保存先を指定しない場合は、作成時に保存先を確認します。このとき `{ProjectRoot}/docs/decisions/` を候補として提案します。
一覧表示や履歴の確認では、特に指定がなければ `docs/decisions/` を探します。
別のディレクトリで管理したい場合は、その場所を指定してください。
毎回指定しなくて済むように、CLAUDE.md や AGENTS.md などに ADR の保存先を書いておくことをおすすめします。

## MADR フォーマット

ADR ファイルは [Structured MADR v4.0.0](https://adr.github.io/madr/) 形式で記録されます。

## ライセンス

MIT
