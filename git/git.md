---
###### tags: `共同開發工具` `Git`
---
Git
===

:::warning
**前情提要:**
這張主要在介紹 git 是什麼，以及其的用法，並介紹 github 一個遠端數據庫。也會概述一下 git 分支，可以如何使用。
:::

## ![](https://i.imgur.com/M88iZWx.png =50x50) Git 是什麼?
Git 是一個軟體，可藉由它產生一個**數據庫（repository）**，並且做到**分散式版本控制**。
:::info
以下幾個功能讓協同開發變得容易，例如:
1. 多處放置同一份程式碼
2. 歷史紀錄追蹤與回朔
:::
> 版本控制系統(Version Control System)
> – 管理你的程式碼版本反悔了回得去、合併了能處理、多人協作互不干擾

## ![](https://i.imgur.com/coiVSBt.png =50x50) GitHub 是什麼？
GitHub 是一個遠端數據庫，可用於檔案存放，並紀錄檔案版本，將本地端的數據庫存於遠端，達到共同開發的目的。

## ![](https://i.imgur.com/zI98zX2.png =50x50) Git vs. GitHub?
一個是工具，一個是網站，GitHub 的本體是一個 Git Server。

## 沒有 Git?
![](https://i.imgur.com/ZGgUtDA.png =430x270)

當你好不容易寫完程式碼，卻被你的好同事 gank 。

## 安裝git - Windows
:::info
**git官網:** https://git-scm.com/download/win
然後開啟 Git Bash
:::
![](https://i.imgur.com/eUbZCcs.png)

## 設定 git 識別資料
在你安裝 Git 後首先應該做的事是設定使用者名稱及電子郵件。
這一點非常重要，因為每次 Git 的提交會使用這些資訊，而且提交後不能再被修改。
```git!
git config --global user.name "[使用者名字]"

git config --global user.email "[電子信箱]"
```
若你有傳遞 --global 參數，只需要做這工作一次，因為在此系統，不論 Git 做任何事都會採用此資訊。

## 檢查讀者的設定
```git!
git config --list

// or

git config -l
```

## 建立全新的儲存庫
```git!
echo "# test" >> README.md

git init
git add README.md
git commit –m “first commit”
git remote add origin [github_repository URL]
git push –u origin main
```

## 基本指令與觀念
![](https://i.imgur.com/AHgQEPd.png =x350)
提交訊息是查看其他人提交的修改內容或自己檢查歷史記錄時重要的資料。
所以要用心填寫讓人容易理解的提交訊息。

:::success
**Git的標準提交(commit)訊息：**

*第1行：提交時修改內容的摘要*
*第2行：空行*
*第3行以後：修改的理由*
:::

建議以這種形式填寫提交訊息

### git commit specification
:::info
**Header:**

`<type>(<scope>): <subject>`

* type: 代表 commit 的類別：feat, fix, docs, style, refactor, test, chore，必要欄位。
* scope 代表 commit 影響的範圍，例如資料庫、控制層、模板層等等，視專案不同而不同，為可選欄位。
* subject 代表此 commit 的簡短描述，不要超過 50 個字元，結尾不要加句號，為必要欄位。

**Body:**

* 72-character wrapped. This should answer:
* Body 部份是對本次 Commit 的詳細描述，可以分成多行，每一行不要超過 72 個字元。
* 說明程式碼變動的項目與原因，還有與先前行為的對比。

**Footer:**

* 填寫任務編號（如果有的話）.
* BREAKING CHANGE（可忽略），記錄不兼容的變動， 以 BREAKING CHANGE: 開頭，後面是對變動的描述、以及變動原因和遷移方法。

**git commit Header**

* **feat:** 新增/修改功能 (feature)。
* **fix:** 修補 bug (bug fix)。
* **docs:** 文件 (documentation)。
* **style:** 格式 (不影響程式碼運行的變動 white-space, formatting, missing semi colons, etc)。
* **refactor:** 重構 (既不是新增功能，也不是修補 bug 的程式碼變動)。
* **perf:** 改善效能 (A code change that improves performance)。
* **test:** 增加測試 (when adding missing tests)。
* **chore:** 建構程序或輔助工具的變動 (maintain)。
* **revert:** 撤銷回覆先前的 commit 例如：revert: type(scope): subject (回覆版本：xxxx)。
:::

## 常用指令
```git!
git init
建立新的本地端儲存庫(Repository)

git clone [URL]
複製遠端的 Repository 檔案到本地端。

git status
檢查本地端檔案異動狀態。

git add [檔案或資料夾]
將指定的檔案(或資料夾)加入版本控制。用 git add . 可加入全部。

git commit -m "[修改內容]"
提交(commit)目前的異動並透過 -m 參數設定摘要說明文字。

git push
將本地端 Repository 的 commit 發佈到遠端。

git push origin [BRANCH_NAME]
發佈至遠端指定的分支(Branch)。

git pull
檔案抓取下來到自己的本地端。
```

:::warning
**clone v.s. download**
:::

## 什麼是 branch?
在開發軟體時，可能同時會有**多人在開發同一功能或修復錯誤**，也可能會有多個發佈版本的存在，並且需要針對每個版本進行維護，同一個數據庫可以同時進行多個修改。
:::info
**現在最常用來管理 Git 分支的方法:**
* master
* develop
* feature
* release
* hotfix
:::

## 分支常用指令
```git!
git branch
查看分支。

git branch [BRANCH_NAME]
建立分支。

git checkout [BRANCH_NAME]
取出指定的分支。

git checkout -b [BRANCH_NAME]
建立並跳到該分支。

git branch -D [BRANCH_NAME]
強制刪除指定分支(須先切換至其他分支再做刪除)。

git branch -m [OLD_BRANCH_NAME] [NEW_BRANCH_NAME]
修改分支名稱。

git merge [要合併的分支]
合併分支
```

## 分支用法
![](https://i.imgur.com/5jV889I.png =500x)

## 管理分支的方法
:::info
* **master:**
    負責**管理發布的狀態**。當準備好發佈指定版本時，最後的提交會給予一個發布版本標籤。
    
* **develop**
    Develop 分支是針對**日常開發的分支**。所有新功能開發最終都會合併到這裡。
    
* **feature**
    這個分支是**新功能的開發**或**修復錯誤**的時候從 develop 分支分開出來的。
    
* **release**
    Release分支是**為了發布而準備的**。通常這種分支的名稱最前面會加上 "**release-**"。
    
* **hotfit**
    Hot fix分支是在**發布的產品需要緊急修改**時，從 master 分支建立的分支。通常會在分支名稱的最前面會加上 "**hotfix-**"。
:::

<br/>

> [time=Fri, Nov 25, 2022 6:51 PM]

> [name=HsingyuChen]
