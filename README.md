# Git Hands-on

## Gitワークフローの基本

このプロジェクトでのGitワークフローについて解説します。Issue作成からPull Requestのマージまでの一連の流れを説明します。

### 1. Issueの作成

新機能の追加やバグ修正を行う前に、まずIssueを作成します。

```bash
# GitHubのWebインターフェースでIssue作成、または
gh issue create --title "新機能: ユーザー認証機能の追加" --body "ユーザーログイン・ログアウト機能を実装する"
```

### 2. ブランチの作成

作業用のブランチを作成します。ブランチ名は分かりやすい名前にしましょう。

```bash
# 最新のmainブランチに移動
git checkout main
git pull origin main

# 新しいブランチを作成・切り替え
git checkout -b feature/user-authentication

# または以下でも同じ
git branch feature/user-authentication
git checkout feature/user-authentication
```

### 3. 作業の実行

ブランチで実際の開発作業を行います。

```bash
# ファイルを編集後、変更をステージング
git add .

# コミットメッセージを記述してコミット
git commit -m "feat: ユーザー認証機能のベース実装"

# 複数回のコミットを行う場合
git add specific-file.js
git commit -m "fix: バリデーション処理の修正"
```

### 4. リモートブランチへのプッシュ

作業したブランチをリモートリポジトリにプッシュします。

```bash
# 初回プッシュ時（上流ブランチの設定も同時に行う）
git push -u origin feature/user-authentication

# 2回目以降は
git push
```

### 5. Pull Requestの作成

GitHub上またはCLIでPull Requestを作成します。

```bash
# GitHub CLIを使用する場合
gh pr create --title "feat: ユーザー認証機能の追加" --body "Issue #1 の対応

## 変更内容
- ログイン画面の実装
- ユーザー認証API の追加
- セッション管理機能

## テスト
- [ ] ログイン機能の動作確認
- [ ] ログアウト機能の動作確認
- [ ] 認証エラーハンドリング"

# または GitHub の Web インターフェースで作成
```

### 6. コードレビューと修正

レビュワーからのフィードバックに基づいて修正を行います。

```bash
# レビューコメントに基づいて修正
git add .
git commit -m "review: レビューコメントに基づく修正"
git push

# 複数の小さなコミットをまとめたい場合
git rebase -i HEAD~3  # 直近3つのコミットを対象
```

### 7. Pull Requestのマージ

レビューが完了したらPull Requestをマージします。

```bash
# GitHub CLIでマージ
gh pr merge --merge  # マージコミットを作成
gh pr merge --squash # スカッシュマージ
gh pr merge --rebase # リベースマージ

# または GitHub の Web インターフェースでマージ
```

### 8. ローカル環境の後処理

マージ後はローカル環境を整理します。

```bash
# mainブランチに戻る
git checkout main

# 最新のmainブランチを取得
git pull origin main

# 作業完了したブランチを削除
git branch -d feature/user-authentication

# リモートブランチも削除（GitHub上で自動削除されない場合）
git push origin --delete feature/user-authentication
```

## よく使うGitコマンド

### 状況確認
```bash
git status          # 現在の状況を確認
git log --oneline   # コミット履歴を確認
git branch -a       # 全ブランチを確認
git diff            # 変更差分を確認
```

### やり直し・修正
```bash
git reset HEAD~1    # 直前のコミットを取り消し（変更は保持）
git reset --hard HEAD~1  # 直前のコミットを完全に取り消し
git checkout -- filename  # ファイルの変更を取り消し
git stash          # 一時的に変更を保存
git stash pop      # 保存した変更を復元
```

### ブランチ管理
```bash
git checkout -b new-branch    # 新ブランチ作成・切り替え
git merge feature-branch      # ブランチをマージ
git branch -d branch-name     # ローカルブランチを削除
```

## ベストプラクティス

1. **わかりやすいコミットメッセージ**: `feat:`, `fix:`, `docs:` などのプレフィックスを使用
2. **小さな単位でコミット**: 機能ごと、修正ごとに分けてコミット
3. **定期的なpush**: 作業内容は定期的にリモートにプッシュ
4. **コンフリクトの早期解決**: mainブランチの変更を定期的にマージ
5. **レビューの活用**: Pull Requestでのコードレビューを積極的に活用
