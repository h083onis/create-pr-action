# Create PR GitHub Action

## 1. 概要
この GitHub Action は、変更したファイルをコミットし、そのコミットに基づいてプルリクエスト（PR）を作成します。プルリクエストのタイトルと本文には、コミットメッセージがそのまま利用されます。

- 必要なパーミッション: `contents: write` と `pull-requests: write`。
- 入力としてコミットメッセージを受け取り、出力としてプルリクエストを作成したブランチを返します。

## 2. 使い方
このアクションを使用するには、以下のように `workflow` ファイルを作成します。

```yaml
name: Create PR
on:
  push:
    branches:
      - main

jobs:
  create-pr:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Create PR
        uses: <ユーザー名>/<リポジトリ名>@main
        with:
          message: "コミットメッセージ例"
入力:
message: コミットメッセージ（必須）
出力:
branch: プルリクエストを作成したブランチ名。
```

## 3. 必須要件
GitHub Actions が有効なリポジトリ。
プルリクエストの作成には、`contents: write` と `pull-requests: write` のパーミッションが必要です。

## 4. 入出力インターフェース
入力:
message: コミットメッセージとして使用される文字列。プルリクエストのタイトルと本文にも使用されます。
出力:
branch: プルリクエストを作成したブランチ名。

## 5. 環境変数
- `USERNAME`: コミットに使用するユーザー名（github-actions[bot]）
- `EMAIL`: コミットに使用するメールアドレス（41898282+github-actions[bot]@users.noreply.github.com）
- `MESSAGE`: コミットメッセージ
- `BRANCH`: 自動的に生成されるプルリクエスト用のブランチ名
- `GITHUB_TOKEN`: GitHub API を使ってプルリクエストを作成するための認証トークン

## 6. GITHUB_TOKEN パーミッション
GITHUB_TOKEN は GitHub Actions のデフォルトトークンで、リポジトリに対する認証情報を提供します。以下のパーミッションを設定する必要があります：

- `contents: write` - リポジトリに対する書き込み権限（コミットやプッシュを行うため）
- `pull-requests: write` - プルリクエストを作成するために必要な権限

GITHUB_TOKEN は GitHub Actions の自動生成トークンで、secrets.GITHUB_TOKEN としてアクセス可能です。

## 7. よくある質問
- Q1: プルリクエストが作成されない場合、どうすればよいですか？

  - GITHUB_TOKEN のパーミッションが正しく設定されていることを確認してください。contents: write と pull-requests: write が設定されていないと、プルリクエストの作成に失敗します。
- Q2: エラー Permission denied が発生しました。どうすればよいですか？
  - .github/scripts/bump.sh などのスクリプトに実行権限が設定されているか確認してください。また、リポジトリ内のアクセス権限やトークン設定が正しいことを確認してください。
- Q3: gh pr create コマンドが失敗します。原因は何ですか？
  - gh コマンドがインストールされているか、正しい権限で設定されているか確認してください。GitHub CLI (gh) が必要です。
- Q4: コミットメッセージとプルリクエストのタイトルはどのように決まりますか？
  - コミットメッセージがそのままプルリクエストのタイトルと本文に使われます。コミットメッセージを適切に設定することが重要です。