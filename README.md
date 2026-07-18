# Git-Flight-Rules
references
1. [git-flight-rules](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-TW.md)
2. [learnGitBranching](https://github.com/pcottle/learnGitBranching)

寫這份筆記主要想記錄一些自己在使用Git上遇到的問題以及遇到問題後最佳解法。與其去深讀那些Git大全，咀嚼那些可能一輩子都用不到的指令，不如把80%最常見的指令與情況練到極致，剩下進階的東西等有一天遇到再說吧。

順序應該是隨機的，想到就寫上去。每個rules我會確保在自己電腦上實驗過才commit上去。

- 創建並切換分支
    ```
    $ git switch -c [branch_name]
    ```
- 切換分支
    ```
    $ git switch [branch_name]
    ```
- 想查看過去某個版本
    ```
    $ git checkout <commit_id>
    ```
    這個操作實際上是把 HEAD 參考從指向分支改成指向某個commit。
    此時想回到目前分支上，執行：
    ```
    $ git checkout <branch_name>
    ```
- 回退到之前的版本。
    ```
    $ git reset 
    ```
    - 加相對索引 e.g. `HEAD~1`
    - `<commit_id>`

    預設是 `--mixed`：清空暫存區但保留工作目錄下的變更。
    - `--soft`：保留暫存區與工作目錄下變更（當commit message寫錯很好用）。
    - `--hard`：完全不保留暫存區與工作目錄下變更。
- 解決分支衝突 [ref:1:46:00](https://www.youtube.com/watch?v=PN1k1CLXtlw)
    1. 手動修改產生衝突文件，合併衝突內容。
    2. `$ git add file`
    3. `$ git commit -m "message"`
    
    中止合併過程：`$ git merge --abort`
- 合併後發現有Bug：直接回退到上一版
    ```
    git reset --hard HEAD~1
    ```
- `git restore` 相關用法：工作區/暫存區 檔案恢復

    1. `git restore --source=<commit_id> <file>`：指定某個檔案回退到特定版本。常用`HEAD~1`代表上一版或直接指定commit id。
    2. `git restore <file>` 工作區所有修改消失。
    3. `git restore --staged <file>` 暫存區退回工作區。 
- `git rebase` [ref:1:46:17](https://www.youtube.com/watch?v=PN1k1CLXtlw)

    另一種合併方式，讓提交紀錄呈現線性歷史。記憶方式：你在 branch-A 上執行`git rebase <branch-B>`，就是把共同祖之後到 branch-A 的 HEAD 的所有提交，重新複製並重新應用到 branch-B 之上。

## Remote
- 設定錯的 Remote repo

    ```
    $ git remote set-url origin [new URL]
    ```
- Remote repo 有一些新的commits，你不想直接合併到 Local repo，可以先用 `git fetch` 看一下 diffs，沒問題之後再 `merge`/`rebase`
    ```
    $ git rebase o/main 
    ```
    🔖 side_note

    `git fetch` 可以想成把 remote branch (e.g. `origin/main`) 同步到 remote repo 的最新狀態；並且不會更新到目前分支的狀態。
- 解決 local repo 版本比 remote repo 舊的問題。想像一個你與你同事`git pull`某個 commit 後同時開發，但他比你早push幾個新的commits；此時你是無法push你自己local commits上去。

    Solution：

    先 `git pull` / `git pull --rebase` 現在remote repo 最新版本下來；前者是`git fetch` + `git merge`，後者是`git fetch` + `git rebase`，總之如果不想直接合併，就先 fetch 就好。

    [ref:learngitbranching - Diverged history](https://learngitbranching.js.org/) 做一遍大概就懂了
- 有時候在 github 開一個新的reop，創建新的README，並commit；並且本地也開新的專案跑了幾個commits，此時如果要push到這個新開的remote，就會跳出錯誤訊息：`fatal: refusing to merge unrelated histories`。解法：
    ```
    git pull origin main --allow-unrelated-histories
    ```

