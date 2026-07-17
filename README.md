# Git-Flight-Rules
references
1. [git-flight-rules](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-TW.md)
2. [learnGitBranching](https://github.com/pcottle/learnGitBranching)

寫這份筆記主要想記錄一些自己在使用Git上遇到的問題以及遇到問題後最佳解法。與其去深讀那些Git大全，咀嚼那些可能一輩子都用不到的指令，不如把80%最常見的指令與情況練到極致，剩下進階的東西等有一天遇到再說吧。

順序應該是隨機的，想到就寫上去。每個rules我會確保在自己電腦上實驗過才commit上去。

- 設定錯的 Remote repo
    ```
    $ git remote set-url origin [new URL]
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
    $ git checkout <hash>
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
    - `<hash>`

    預設是 `--mixed`：清空暫存區但保留工作目錄下的變更。
    - `--soft`：保留暫存區與工作目錄下變更（當commit message寫錯很好用）。
    - `--hard`：完全不保留暫存區與工作目錄下變更。