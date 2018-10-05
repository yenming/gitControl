# Git使用狀況與解法

#### 關於檔名的大小寫

Git 裡如果只有檔名大小寫改變而沒有改變內容的話，git status 指令不會感受到有任何的變化：
```
  $ ls -al
  total 0
  drwxr-xr-x@ 13 eddie  wheel  416 Aug 22  2017 .
  drwxr-xr-x@  7 eddie  wheel  224 Jun  4 17:49 ..
  drwxr-xr-x@ 14 eddie  wheel  448 Jun  5 16:56 .git
  -rw-r--r--@  1 eddie  wheel    0 Aug 20  2017 cat1.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 21  2017 cat2.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 21  2017 cat3.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 21  2017 cat4.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 21  2017 dog1.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 21  2017 dog2.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 22  2017 fish.html
  -rw-r--r--@  1 eddie  wheel    0 Aug 20  2017 index.html

  $ mv cat1.html Cat1.html

  $ git status
  On branch master
  nothing to commit, working tree clean
```
首先，這件事跟作業系統的檔案系統（File System）有關，以我自己的電腦（macOS High Sierra 10.13.4）來說，檔案系統是無視檔名大小寫的差別的（case insensitive），所以當我執行下面這個指令：
```
$ rm CAT1.HTML
```
雖然刪的是大寫檔名，但會把小寫檔名的 cat1.html 給刪掉。

在 Git 還是有一些解決方法：
使用 git 的 mv 指令：
```
$ git mv cat1.html Cat1.html

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  renamed:    cat1.html -> Cat1.html
```
修改 git 設定檔
```
$  git config --local core.ignorecase false
```
這樣一來就可以把該專案的「忽略大小寫」設定改成 true。如果想改成全域設定，只要把 --local 改成 --global 就行了。設定檔一改完立馬就有效果了：
```
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

  Cat1.html

nothing added to commit but untracked files present (use "git add" to track)
```

要注意的是，Cat1.html 會變成 Untracked files 狀態
順便翻一下 Git 關於 core.ignoreCase 的說明手冊：
```
core.ignoreCase
   If true, this option enables various workarounds to enable Git to work better on filesystems that
   are not case sensitive, like FAT. For example, if a directory listing finds "makefile" when Git
   expects "Makefile", Git will assume it is really the same file, and continue to remember it as
   "Makefile".

   The default is false, except git-clone(1) or git-init(1) will probe and set core.ignoreCase true
   if appropriate when the repository is created.
```
也就是把 core.ignoreCase 設定成 true 的時候，Git 有自己一套 workaround 來搞定這件事（即使檔案系統本身是 case insensitive）。


[參考資料](https://gitbook.tw/posts/2018-06-05-case-sensitive)

-------------
