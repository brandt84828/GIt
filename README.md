# Git筆記

## 概述

### 版本控制的方式
* 集中式版本控制工具：集中式版控的版本庫是集中存放在中央伺服器的，團隊裡每個人工作時從中央伺服器下載Code，有網路才能工作(區網或網際網路)。 個人修改後然後提交到中央版本庫。
Ex：SVN和CVS


* 分散式版本控制工具：分散式版控系統沒有中央伺服器，每個人的電腦上都是一個完整的版本庫，這樣工作的時候不需要連上網路，因為版本庫就在自己的電腦上。 多人協作只需要將各自的修改推送給對方，就能看到對方的修改了。
Ex：Git


### Git
Git是一個開源的分散式版本控制系統，可以有效、高速處理從很小到非常大的專案版本管理。Git是分散式的所以不需要有中心伺服器，每台電腦擁相同的東西。我們使用Git並且有個中心伺服器，僅僅是為了方便交換大家的修改，但是這個伺服器的地位和個人電腦是一樣的。可以把它當做一個24小時不關機的機器，為了方便使用。

### Git工作流程
clone: 從遠程倉庫中複製Code到本地倉庫
checkout:從本地倉庫中檢出一個倉庫分支然後進行修訂
add: 在提交前先將Code提交到暫存區
commit: 提交到本地倉庫。 本地倉庫中保存修改的各個歷史版本
fetch: 從遠程庫抓取到本地倉庫，不進行任何的合併動作，一般操作比較少
pull: 從遠程庫拉到本地庫，自動進行合併(merge)，然後放到到工作區，相當於fetch+merge
push: 修改完成後，需要和團隊成員共用Code時，將Code推送到遠程倉庫

## Git安裝與常用命令
下載網址: https://git-scm.com/downloads

安裝完成後需要先配置user name & Email，每次提交都會使用此用戶的資訊。
1. Open Git Bash
2. 配置user name & Email
```bash
# 配置
git config --global user.name "user"
git config --global user.email "user@gmail.com"

# 查看配置
git config --global user.name
git config --global user.email
```

### 為常用指令配置別名
1. 打開使用者目錄，創建.bashrc文件
```bash
touch ~/.bashrc
```

2. 在檔案中輸入如下內容
```bash
# 輸出git commit的log 
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' 
# 輸出當前目錄所有文件及資訊 
alias ll='ls -al'
```

3. 開啟gitBash執行：
```bash
source ~/.bashrc
```

### Get local repository(本地倉庫)
要使用Git進行版控，需要先獲得本地倉庫

1. Create folder
2. Open git bash
3. git init
4. 成功後可以看到隱藏的.git目錄

### 基礎指令
Git工作目錄下對於檔案的修改（增加、刪除、更新）會存在幾個狀態，這些修改的狀態會隨著執行Git的指令而發生變化。

![git status](/pictures/git_status.jpg)

#### git status
查看修改的狀態(暫存區 & 工作區)
```bash
git status
```

#### git add
從工作區加入一個或多個檔案的修改至暫存區
```bash
git add test.txt # 單個檔案
git add . # 將所有修改加入暫存區
```

#### git commit
提交暫存區的內容到本地倉庫的**當前分支**
```bash
# git commit -m "description"
git commit -m "xxx update" 
```

#### git log
查看commit(提交)紀錄
```bash
# —all 顯示所有分支
# —pretty=oneline 將commit資訊顯示為一行
# —abbrev-commit 讓輸出的commit id更簡短
# —graph 以圖的形式顯示

git log 
```

#### git reset
切換version(版本)
```bash
# --hard 工作目錄以及暫存區的檔案都會丟掉。
# --soft 工作目錄跟暫存區的檔案都不會被丟掉，所以看起來就只有 HEAD 的移動而已。

git reset --hard commitID # commit id 可以使用git log 指令查看

# 查看已經刪除的紀錄，可以查看已經刪除的提交紀錄
git reflog
```

#### add file to .gitignore
一般會有些檔案無需納入Git 的管理，也不希望它們出現在未跟蹤文件清單。 通常都是些自動生成的檔，比如日誌檔，或者編譯過程中創建的臨時檔等。在這種情況下，我們可以在工作目錄中創建.gitignore，列出要忽略的檔案模式。
```bash
touch .gitignore
vi .gitignore
```

### branch(分支)
幾乎所有的版本控制系統都支援分支。使用分支代表你可以把你的工作從開發主線上分離開來進行重大的Bug修改、開發新的功能，以免影響開發主線。

#### 查看本地分支
```bash
git branch
```

#### 創建本地分支
```bash
git branch 分支名稱
````
#### 切換分支
```bash
git checkout 分支名稱

#還可以直接切換到一個不存在的分支(創建並切換)
git checkout -b 分支名稱
```

#### 合併分支
一個分支上的提交可以合併到另一個分支
```bash
git merge 分支名稱

# Fast-Forward模式: 如果master分支的狀態是沒有更改過的話，那麼這個合併是非常簡單的。只要移動 master分支到最新提交就可以導入合併的內容。這樣的合併被稱為 fast-forward（快轉）合併
```
#### 刪除分支
不能刪除當前分支，只能刪除其他分支
```bash
git branch -d 分支名稱 # 删除分支时，需要做各種檢查
git branch -D 分支名 # 強制刪除，不做任何檢查
```
#### 解決衝突
當兩個分支上對檔的修改可能會存在衝突，例如同時修改了同一個檔的同一行，這時就需要手動解決衝突，解決衝突步驟如下：

1. 處理文件中衝突的地方
2. 將解決完衝突的檔加入暫存區（add）
3. 提交到倉庫（commit）

#### 開發中分支使用原則與流程
幾乎所有的版本控制系統都以某種形式支援分支。 使用分支意味著你可以把你的工作從開發主線上分離開來進行重大的Bug修改、開發新的功能，以免影響開發主線。

在開發中，一般有如下分支使用原則與流程：

* master(生產)分支：線上分支、主分支，中小規模專案作為線上運行的應用對應的分支。

* develop(開發)分支：是從master創建的分支，一般作為開發部門的主要開發分支，如果沒有其他平行開發不同期上線要求，都可以在此版本進行開發，階段開發完成後，需要是合併到master分支準備上線。

* feature/xxxx分支：從develop創建的分支，一般是同期平行開發，但不同期上線時創建的分支，分支上的開發任務完成後合併到develop分支，之後該分支可以刪除。

* hotfix/xxxx分支：從master派生的分支，一般作為線上bug修復使用，修復完成後需要合併到master、test、develop分支。

* 還有一些其他分支，例如test分支(用於測試)、pre分支(預上線分支)等等。

## Git遠程倉庫

### 常用的託管服務
前面我們已經知道了Git中存在兩種類型的倉庫，即本地倉庫和遠端倉庫。 那麼我們如何搭建Git遠程倉庫呢？ 網路上有提供的一些託管服務來實現，其中比較常用的有GitHub、GitLab等。

GitHub（ 位址：https://github.com/ ）是一個面向開源及私有軟體專案的託管平臺，因為只支援Git作為唯一的版本庫格式進行託管，故名GitHub

GitLab （位址： https://about.gitlab.com/ ）是一個用於倉庫管理系統的開源專案，使用Git作為管理工具，並在此基礎上搭建起來的web服務，一般用於在企業、學校等內部網路搭建git私人服務。

### 操作遠程倉庫

#### 添加遠程倉庫
此操作是先初始化本地庫，然後與已創建的遠端庫進行對接。
```bash
# 遠端名稱：預設是origin，取決於遠端伺服器設置
# 倉庫位址：從遠端伺服器獲取的url
git remote add <遠端名稱> <倉庫位址>

git remote add origin git@github.com:brandt84828/GIt.git
```

#### 查看遠程倉庫
```bash
git remote
```

#### 推送到遠端倉庫
```bash
git push [-f] [--set-upstream] [遠端名稱] [本地分支名稱][:遠程分支名稱]

#如果遠端分支名和本地分支名相同，則可以只寫本地分支名

git push origin master
git push --set-upstream origin master:master
git push # set upstream綁定後可以省略掉名稱直接用

# -f: 表示强制推送，一般在公司内沒有這個的使用權限，否则容易破壞的遠程倉庫的所有內容
# --set-upstream: 推送到遠程的同时，建立起和遠程分支的關聯。用於第一次push時
# 如果當前分支已经和遠程分支有關聯，則可以省略分支名稱和遠程分支名稱
# git push 将master分支推送到已關聯的遠程分支
```

#### 本地分支與遠程分支的關聯

```bash
# 查看本地分支與遠程分支的對應關係
git branch -vv
```

#### 從遠端倉庫克隆
如果已經有一個遠端倉庫，我們可以直接clone到本地。

```bash
git clone <遠端倉庫位置> [本地目錄]]
# 本地目錄可以為空，會自動生成一個目錄
```
#### 從遠程倉庫中抓取和拉取
遠程分支和本地的分支一樣可以進行merge操作，只是需要先把遠程倉庫裡的更新都下載到本地，再進行操作。

```bash
#fetch: 抓取指令是將遠程倉庫裡的更新都抓取到本地，不會進行合併。如果不指定遠程名稱和分支名稱，則抓取所有分支。
git fetch [remote name] [branch name]

#pull: 拉取指令是將遠程倉庫的修改拉到本地並自動進行合併，等同於fetch+merge。如果不指定遠程名稱和分支名稱，則抓取所有並更新當前分支。
git pull [remote name] [branch name]
```

#### 解决合併衝突
在同一時間，A、B使用者修改了同一份文件且修改同一行位置的code，這時候會發生合併衝突。

A在本地修改後先推送到遠程倉庫，這時B在本地修改提交到本地倉庫後，也需要推送到遠程倉庫，此時B晚於A，所以需要先拉取遠程倉庫的提交，經過合併後才能推送到遠程分支。

![conflict](/pictures/conflict.png)

在B拉取程式時，因為A、B同一段時間修改了同一份文件的相同位置，所以會發生合併衝。

遠程分支也是分支，所以合併衝突的解决方式也和解决本地分支衝突相同。

## 其他

### HEAD指標
HEAD指向當前的分支，HEAD代表當前分支的最新提交名稱。

HEAD可以指向當前分支的最新commit，也可以指向歷史紀錄中的某一次 commit。

當切換分支時，HEAD會跟著切換到對應分支。
### Stash
還未提交的修改內容或新增檔案，留在工作目錄尚未提交的情況下checkout到其他的分支時，修改內容會從原來的分支移動到切換後的分支。

但如果在切換後的分支中有相同檔案，而且有任何修改的話，checkout會失敗。這時一定要先提交修改內容，或者將修改內容放到stash中暫時儲存後再checkout。

Stash是暫時儲存檔案修改內容的區域。Stash可以暫時儲存工作目錄還沒提交的修改內容，您可以在事後再取出暫時儲存的修改，應用到原先的分支或者其他的分支中。

### fast-forward(快進模式)
假設情況：有兩個分支，master分支和feature分支，然後把feautre分支合併至master分支。

* fast-forward: 在條件允許的情況下，通過將master分支的HEAD位置移动到feature分支的最新提交，這樣就完成合併了。這種情況不會新產生commit。
* 非快進: master分支會新產生一次commit紀錄。

```bash
git checkout master 
git merge --no-ff feature

#如果滿足條件，merge預設採用fast-forward進行合併，除非加上--no-ff選項，而條件不滿足時，merge也無法用快進模式完成合併。

#條件:合併分支和目標分支沒有其他branch
```
### 強制拉取
從遠程倉庫強制拉取以更新本地倉庫。
```bash
git fetch --all  
git reset --hard origin/master 
git pull
```
### 遠程分支合併
```bash
git clone

#在本地建立dev分支並和遠程dev關聯
git checkout -b dev origin/dev

git checkout master

git merge dev

#推送到遠程的master上(需有操作遠程master分支的權限）
git push origin master
```
### rebase
這裡的 “Base” 代表「基礎版本」的意思，表示你想要重新修改特定分支的「基礎版本」，把另外一個分支的變更，當成我這個分支的基礎。
```bash
# 切換至 branch1 分支：
git checkout branch1

# 然後執行 Rebase 動作，把 master 當成我們的基礎版本： 
git rebase master
```