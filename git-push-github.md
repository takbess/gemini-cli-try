# GitHub へ `git push` するまで（要点）

ローカルでコミットができていても、**GitHub への認証**が通っていないと `git push` はできません。流れと認証の用意だけ押さえれば足ります。

## 1. ローカル側の準備（初回）

まだ Git 管理していないフォルダなら、コミットまで進めます。

```bash
cd /path/to/your-project
git init -b main
git add .
git commit -m "Initial commit"
```

GitHub 上に空のリポジトリを作り、表示される URL（HTTPS か SSH）を控えます。

## 2. リモートを登録する

HTTPS の例（GitHub の「Code」からコピーした URL）:

```bash
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
```

すでに `origin` がある場合は:

```bash
git remote set-url origin https://github.com/<ユーザー名>/<リポジトリ名>.git
```

## 3. 認証を通す（ここがポイント）

`git push` は **GitHub に「あなた本人」だと証明する**必要があります。代表的には次のどちらかです。

### A. HTTPS + Personal Access Token（PAT）

1. GitHub → **Settings** → **Developer settings** → **Personal access tokens** でトークンを作成する（classic なら **`repo`** など必要なスコープを付与）。
2. `git push` のとき、ユーザー名は GitHub のユーザー名、**パスワードの代わりにトークン**を入力する。

コマンドラインだけで済ませたい場合は、[Git Credential Manager](https://github.com/git-ecosystem/git-credential-manager) の導入や、`git config credential.helper` の設定で毎回入力を減らせます。

### B. SSH 鍵

1. 鍵を作成: `ssh-keygen -t ed25519 -C "your_email@example.com"`
2. 公開鍵（`~/.ssh/id_ed25519.pub` の内容）を GitHub → **SSH and GPG keys** に登録。
3. リモートを SSH URL にする:

   ```bash
   git remote set-url origin git@github.com:<ユーザー名>/<リポジトリ名>.git
   ```

4. 接続確認: `ssh -T git@github.com`

## 4. プッシュする

初回は upstream を付けておくと、あとは `git push` だけで済みます。

```bash
git push -u origin main
```

ブランチ名が `master` など別名なら、その名前に読み替えます。

## 5. よくあるエラー

| 症状 | 想定原因 |
|------|----------|
| `could not read Username for 'https://github.com'` | HTTPS で認証情報が渡っていない（PAT 未設定・未入力、credential helper 未設定など）。 |
| `Permission denied (publickey)` | SSH を使っているのに鍵がない／GitHub に公開鍵が未登録／リモート URL が SSH になっていない。 |
| `rejected (non-fast-forward)` | リモートに自分にないコミットがある。`git pull --rebase origin main` などで取り込んでから再度 `push`。 |

---

**まとめ:** コミットまで終わったら、**リモート URL を正しく設定し、HTTPS なら PAT（＋必要なら credential）、SSH なら鍵登録**を済ませたうえで `git push -u origin main` を実行すればよいです。
