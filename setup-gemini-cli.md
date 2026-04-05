# Gemini CLI セットアップ（簡易）

Gemini CLI をローカルで使うまでの最短手順です。詳細は [公式ドキュメント](https://google-gemini.github.io/gemini-cli/) を参照してください。

## 1. 前提

- **Node.js 20 以上**（必須。v18 未満では動きません）
- ターミナルで `node -v` を実行し、`v20.x` 以上であることを確認する

### Node が古い場合（例: v12）

[nvm](https://github.com/nvm-sh/nvm) で 20 を入れる例:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# シェルを開き直すか: source ~/.bashrc
nvm install 20
nvm alias default 20
node -v
```

## 2. インストールと起動

**毎回その場で試す（グローバルに入れない）:**

```bash
npx @google/gemini-cli
```

**グローバルに入れる:**

```bash
npm install -g @google/gemini-cli
gemini
```

## 3. 初回の認証

起動後の案内に従う。

- **Google アカウントでログイン**（Gemini Code Assist for individuals）: 無料枠が広い（目安: 公式の [Quotas and Pricing](https://google-gemini.github.io/gemini-cli/docs/quota-and-pricing.html) を参照）
- **Gemini API キー**: [Google AI Studio](https://aistudio.google.com/) などでキー取得。無料枠は Google ログインより狭い場合がある

## 4. プロジェクト用の文脈（任意）

リポジトリ直下の **`GEMINI.md`** に、このプロジェクト向けの指示を書いておくと、CLI が毎回その内容を読み込みます。本リポジトリには `GEMINI.md` を用意済みです。

- 読み込み確認: CLI 内で `/memory show`
- 再読み込み: `/memory refresh`

## 5. Cursor / IDE 連携（任意）

初回に「IDE に接続しますか？」と出た場合:

- **Yes**: 拡張で開いているファイルや diff 連携が使いやすくなる
- **No**: ターミナルだけで利用

「Skipping project agents due to untrusted folder」と出るときは、Cursor で **ワークスペースを Trusted** にするとプロジェクト向け機能が有効になりやすいです。

## 6. 便利コマンド（CLI 内）

| 入力 | 内容 |
|------|------|
| `/help` | ヘルプ |
| `?` | ショートカット一覧 |
| `/memory show` | 読み込んだ GEMINI.md などの確認 |

終了は **`Ctrl+C`**（必要なら 2 回）。

## 7. 無料枠・モデルについて

起動時に「無料ユーザー向けの制限・Pro は有料プラン」などの案内が出ることがあります。無料で試す分には **Flash 系モデル**で十分なことが多いです。詳細は [公式のお知らせ](https://goo.gle/geminicli-updates) および [Quotas and Pricing](https://google-gemini.github.io/gemini-cli/docs/quota-and-pricing.html) を確認してください。
