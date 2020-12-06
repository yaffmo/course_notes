[Git](http://zh.wikipedia.org/wiki/Git)是目前最流行的[版本管理系統](http://www.ruanyifeng.com/blog/2008/12/a_visual_guide_to_version_control.html)，學會Git幾乎成了開發者的必備技能。

Git有很多優勢，其中之一就是遠程操作非常簡便。本文詳細介紹5個Git命令，它們的概念和用法，理解了這些內容，你就會完全掌握Git遠程操作。

- git clone
- git remote
- git fetch
- git pull
- git push

本文針對初級用戶，從最簡單的講起，但是需要讀者對Git的基本用法有所了解。同時，本文覆蓋了上面5個命令的幾乎所有的常用用法，所以對於熟練用戶也有參考價值。

![git](http://image.beekka.com/blog/2014/bg2014061202.jpg)

## 一、git clone

遠程操作的第一步，通常是從遠程主機克隆一個版本庫，這時就要用到`git clone`命令。

> ```javascript
> $ git clone <版本庫的網址>
> ```

比如，克隆jQuery的版本庫。

> ```javascript
> $ git clone https://github.com/jquery/jquery.git
> ```

該命令會在本地主機生成一個目錄，與遠程主機的版本庫同名。如果要指定不同的目錄名，可以將目錄名作為`git clone`命令的第二個參數。

> ```javascript
> $ git clone <版本庫的網址> <本地目录名>
> ```

`git clone`支持多種協議，除了HTTP(s)以外，還支持SSH、Git、本地文件協議等，下面是一些例子。

> ```javascript
> $ git clone http[s]://example.com/path/to/repo.git/
> 
> 
> 
> $ git clone ssh://example.com/path/to/repo.git/
> 
> 
> 
> $ git clone git://example.com/path/to/repo.git/
> 
> 
> 
> $ git clone /opt/git/project.git 
> 
> 
> 
> $ git clone file:///opt/git/project.git
> 
> 
> 
> $ git clone ftp[s]://example.com/path/to/repo.git/
> 
> 
> 
> $ git clone rsync://example.com/path/to/repo.git/
> ```

SSH協議還有另一種寫法。

> ```javascript
> $ git clone [user@]example.com:path/to/repo.git/
> ```

通常來說，Git協議下載速度最快，SSH協議用於需要用戶認證的場合。各種協議優劣的詳細討論請參考[官方文檔](http://git-scm.com/book/en/Git-on-the-Server-The-Protocols)。

## 二、git remote

為了便於管理，Git要求每個遠程主機都必須指定一個主機名。`git remote`命令就用於管理主機名。

不帶選項的時候，`git remote`命令列出所有遠程主機。

> ```javascript
> $ git remote
> 
> 
> 
> origin
> ```

使用`-v`選項，可以參看遠程主機的網址。

> ```javascript
> $ git remote -v
> 
> 
> 
> origin  git@github.com:jquery/jquery.git (fetch)
> 
> 
> 
> origin  git@github.com:jquery/jquery.git (push)
> ```

上面命令表示，當前只有一台遠程主機，叫做origin，以及它的網址。

克隆版本庫的時候，所使用的遠程主機自動被Git命名為`origin`。如果想用其他的主機名，需要用`git clone`命令的`-o`選項指定。

> ```javascript
> $ git clone -o jQuery https://github.com/jquery/jquery.git
> 
> 
> 
> $ git remote
> 
> 
> 
> jQuery
> ```

上面命令表示，克隆的時候，指定遠程主機叫做jQuery。

`git remote show`命令加上主機名，可以查看該主機的詳細信息。

> ```javascript
> $ git remote show <主機名>
> ```

`git remote add`命令用於添加遠程主機。

> ```javascript
> $ git remote add <主機名> <網址>
> ```

`git remote rm`命令用於刪除遠程主機。

> ```javascript
> $ git remote rm <主機名>
> ```

`git remote rename`命令用於遠程主機的改名。

> ```javascript
> $ git remote rename <原主機名> <新主機名>
> ```

## 三、git fetch

一旦遠程主機的版本庫有了更新（Git術語叫做commit），需要將這些更新取回本地，這時就要用到`git fetch`命令。

> ```javascript
> $ git fetch <遠程主機名>
> ```

上面命令將某個遠程主機的更新，全部取回本地。

`git fetch`命令通常用來查看其他人的進程，因為它取回的代碼對你本地的開發代碼沒有影響。

默認情況下，`git fetch`取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。

> ```javascript
> $ git fetch <遠程主機名> <分支名>
> ```

比如，取回`origin`主機的`master`分支。

> ```javascript
> $ git fetch origin master
> ```

所取回的更新，在本地主機上要用"遠程主機名/分支名"的形式讀取。比如`origin`主機的`master`，就要用`origin/master`讀取。

`git branch`命令的`-r`選項，可以用來查看遠程分支，`-a`選項查看所有分支。

> ```javascript
> $ git branch -r
> 
> 
> 
> origin/master
> 
> 
> 
>  
> 
> 
> 
> $ git branch -a
> 
> 
> 
> * master
> 
> 
> 
>   remotes/origin/master
> ```

上面命令表示，本地主機的當前分支是`master`，遠程分支是`origin/master`。

取回遠程主機的更新以後，可以在它的基礎上，使用`git checkout`命令創建一個新的分支。

> ```javascript
> $ git checkout -b newBrach origin/master
> ```

上面命令表示，在`origin/master`的基礎上，創建一個新分支。

此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合併遠程分支。

> ```javascript
> $ git merge origin/master
> 
> 
> 
> # 或者
> 
> 
> 
> $ git rebase origin/master
> ```

上面命令表示在當前分支上，合併`origin/master`。

## 四、git pull

`git pull`命令的作用是，取回遠程主機某個分支的更新，再與本地的指定分支合併。它的完整格式稍稍有點複雜。

> ```javascript
> $ git pull <遠程主機名> <遠程分支名>:<本地分支名>
> ```

比如，取回`origin`主機的`next`分支，與本地的`master`分支合併，需要寫成下面這樣。

> ```javascript
> $ git pull origin next:master
> ```

如果遠程分支是與當前分支合併，則冒號後面的部分可以省略。

> ```javascript
> $ git pull origin next
> ```

上面命令表示，取回`origin/next`分支，再與當前分支合併。實質上，這等同於先做`git fetch`，再做`git merge`。

> ```javascript
> $ git fetch origin
> 
> 
> 
> $ git merge origin/next
> ```

在某些場合，Git會自動在本地分支與遠程分支之間，建立一種追踪關係（tracking）。比如，在`git clone`的時候，所有本地分支默認與遠程主機的同名分支，建立追踪關係，也就是說，本地的`master`分支自動"追踪"`origin/master`分支。

Git也允許手動建立追踪關係。

> ```javascript
> git branch --set-upstream master origin/next
> ```

上面命令指定`master`分支追踪`origin/next`分支。

如果當前分支與遠程分支存在追踪關係，`git pull`就可以省略遠程分支名。

> ```javascript
> $ git pull origin
> ```

上面命令表示，本地的當前分支自動與對應的`origin`主機"追踪分支"（remote-tracking branch）進行合併。

如果當前分支只有一個追踪分支，連遠程主機名都可以省略。

> ```javascript
> $ git pull
> ```

上面命令表示，當前分支自動與唯一一個追踪分支進行合併。

如果合併需要採用rebase模式，可以使用`--rebase`選項。

> ```javascript
> $ git pull --rebase <遠程主機名> <遠程分支名>:<本地分支名>
> ```

如果遠程主機刪除了某個分支，默認情況下，`git pull`不會在拉取遠程分支的時候，刪除對應的本地分支。這是為了防止，由於其他人操作了遠程主機，導致`git pull`不知不覺刪除了本地分支。

但是，你可以改變這個行為，加上參數`-p`就會在本地刪除遠程已經刪除的分支。

> ```javascript
> $ git pull -p
> 
> 
> 
> # 等同于下面的命令
> 
> 
> 
> $ git fetch --prune origin 
> 
> 
> 
> $ git fetch -p
> ```

## 五、git push

`git push`命令用於將本地分支的更新，推送到遠程主機。它的格式與`git pull`命令相仿。

> ```javascript
> $ git push <遠程主機名> <本地分支名>:<遠程分支名>
> ```

注意，分支推送順序的寫法是<來源地>:<目的地>，所以`git pull`是<遠程分支>:<本地分支>，而`git push`是<本地分支>:<遠程分支>。

如果省略遠程分支名，則表示將本地分支推送與之存在"追踪關係"的遠程分支（通常兩者同名），如果該遠程分支不存在，則會被新建。

> ```javascript
> $ git push origin master
> ```

上面命令表示，將本地的`master`分支推送到`origin`主機的`master`分支。如果後者不存在，則會被新建。

如果省略本地分支名，則表示刪除指定的遠程分支，因為這等同於推送一個空的本地分支到遠程分支。

> ```javascript
> $ git push origin :master
> 
> 
> 
> # 等同于
> 
> 
> 
> $ git push origin --delete master
> ```

上面命令表示刪除`origin`主機的`master`分支。

如果當前分支與遠程分支之間存在追踪關係，則本地分支和遠程分支都可以省略。

> ```javascript
> $ git push origin
> ```

上面命令表示，將當前分支推送到`origin`主機的對應分支。

如果當前分支只有一個追踪分支，那麼主機名都可以省略。

> ```javascript
> $ git push
> ```

如果當前分支與多個主機存在追踪關係，則可以使用`-u`選項指定一個默認主機，這樣後面就可以不加任何參數使用`git push`。

> ```javascript
> $ git push -u origin master
> ```

上面命令將本地的`master`分支推送到`origin`主機，同時指定`origin`為默認主機，後面就可以不加任何參數使用`git push`了。

不帶任何參數的`git push`，默認只推送當前分支，這叫做simple方式。此外，還有一種matching方式，會推送所有有對應的遠程分支的本地分支。Git 2.0版本之前，默認採用matching方法，現在改為默認採用simple方式。如果要修改這個設置，可以採用`git config`命令。

> ```javascript
> $ git config --global push.default matching
> 
> 
> 
> # 或者
> 
> 
> 
> $ git config --global push.default simple
> ```

還有一種情況，就是不管是否存在對應的遠程分支，將本地的所有分支都推送到遠程主機，這時需要使用`--all`選項。

> ```javascript
> $ git push --all origin
> ```

上面命令表示，將所有本地分支都推送到`origin`主機。

如果遠程主機的版本比本地版本更新，推送時Git會報錯，要求先在本地做`git pull`合併差異，然後再推送到遠程主機。這時，如果你一定要推送，可以使用`--force`選項。

> ```javascript
> $ git push --force origin 
> ```

上面命令使用`--force`選項，結果導致遠程主機上更新的版本被覆蓋。除非你很確定要這樣做，否則應該盡量避免使用`--force`選項。

最後，`git push`不會推送標籤（tag），除非使用`--tags`選項。

> ```javascript
> $ git push origin --tags
> ```