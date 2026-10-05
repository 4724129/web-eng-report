# 第2回 Webエンジニアリング演習 レポート
## 学籍番号
4724129
## コンフリクトが発生した理由
`practice/conflict-a`と`practice/conflict-b`の2つのブランチを、同じ`main`のコミットから作成した。その後、両方のブランチで`README.md`の同じ行を、それぞれ別の内容に変更した。
- conflict-a：`- ブランチを使って安全に変更する`
- conflict-b：`- コンフリクトを自分で解決できる`
conflict-aを先に`main`へマージしたため、`main`の該当行はconflict-aの内容になった。その後、conflict-bの変更を統合しようとしたとき、同じ行が異なる内容に変更されていたため、Gitはどちらを残すべきか自動で判断できず、コンフリクトが発生したと考えられる。
## 解決手順
1. `git pull --no-rebase origin practice/conflict-b`を実行し、`CONFLICT (content): Merge conflict in README.md`と表示されることを確認した。
2. `git status`で、`README.md`が`both modified`（未解決）になっていることを確認した。
3. `README.md`を開き、記号（`<<<<<<<`、`=======`、`>>>>>>>`）をすべて削除した。
4. 2つの内容をまとめた1行に編集した。
   `- ブランチを使い、コンフリクトも自分で解決する`
5. `git add README.md`で、解決した変更をステージングした。
6. `git commit -m "Resolve README conflict"`で、解決結果をコミットした。
7. `git push --set-upstream origin practice/conflict-b`でGitHubへ送信した。
8. GitHubでプルリクエストが競合なしになったことを確認し、`main`へマージして、ブランチを削除した。
9. ローカルで`git switch main`、`git pull origin main`を実行し、`main`を最新にした。
## 履歴
@4724129 ➜ /workspaces/web-eng-report (main) $ git log --oneline --graph --all | cat
*   b766c59 Merge pull request #3 from 4724129/practice/conflict-b
|\  
| * 51e6262 Tidy README after conflict resolution
| * 3e53954 Resolve README conflict
|/| 
| * dcc1d9c Update goal in conclift B
* |   abee5bf Merge pull request #2 from 4724129/practice/conflict-a
|\ \  
| |/  
|/|   
| * 7c01de2 Update goal in conclift A
|/  
*   f50e067 Merge pull request #1 from 4724129/feature/add-readme
|\  
| * 59e0b67 Add README
|/  
* a76f9a2 Add index.html
* 13f9620 Add index.html
* 880637d Add devcontainer configuration for Web Engineering
* 3dd75b5 Remove duplicate project name from README
* 9c9e9d0 Initial commit