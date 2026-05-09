# claude-ONE

GitHub Issues や Pull Request 上で [Claude Code](https://claude.com/claude-code) を呼び出して、コードの自動生成・レビュー・修正を行えるようにするリポジトリです。

## 概要

このリポジトリには、`@claude` メンションで Anthropic の Claude を起動するための GitHub Actions ワークフローが設定されています。Issue や PR のコメントで `@claude` をつけて指示を出すと、Claude が以下のような作業を自動で行います。

- 質問への回答
- コードレビュー
- バグ修正・機能追加などの実装
- ブランチへのコミット & プッシュ
- Pull Request 作成リンクの提示

## 仕組み

`.github/workflows/claude.yml` で [`anthropics/claude-code-action`](https://github.com/anthropics/claude-code-action) を呼び出しています。

### トリガーイベント

| イベント | 動作 |
| --- | --- |
| `issues` (`opened`, `assigned`) | Issue 作成・アサイン時に起動 |
| `issue_comment` (`created`) | Issue / PR コメントに `@claude` が含まれると起動 |
| `pull_request_review` (`submitted`) | PR レビュー送信時に起動 |
| `pull_request_review_comment` (`created`) | PR レビューコメント作成時に起動 |

### 必要な権限

ワークフローには以下の `permissions` が付与されています。

- `contents: write` — ファイル変更とブランチへのプッシュ
- `pull-requests: write` — PR コメントの投稿
- `issues: write` — Issue コメントの投稿
- `id-token: write` — OIDC 認証

## セットアップ

このリポジトリをフォーク、もしくは同様の設定を別リポジトリで再現する場合は次の手順を行ってください。

1. リポジトリの **Settings → Secrets and variables → Actions** に `ANTHROPIC_API_KEY` を登録します。
   - Anthropic の API キーは [Anthropic Console](https://console.anthropic.com/) から取得できます。
2. `.github/workflows/claude.yml` をリポジトリに配置します（このリポジトリの内容をそのまま利用できます）。
3. Issue または PR で `@claude <依頼内容>` のようにコメントすると Claude が応答します。

## 使い方の例

Issue やコメントで次のように依頼できます。

```text
@claude このリポジトリの README を作成してください
```

```text
@claude src/utils.ts の関数 formatDate にユニットテストを追加してください
```

```text
@claude この PR をレビューして、改善点をコメントしてください
```

Claude は単一のコメントを更新しながら進捗を表示し、変更が必要な場合は現在のブランチへコミットしたうえで PR 作成リンクを提示します。

## 参考リンク

- [Claude Code 公式ドキュメント](https://docs.claude.com/en/docs/claude-code/overview)
- [claude-code-action リポジトリ](https://github.com/anthropics/claude-code-action)
- [よくある質問 (FAQ)](https://github.com/anthropics/claude-code-action/blob/main/docs/faq.md)
