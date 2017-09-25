!> The Basics for Git and Github

- ## Git 初始设置

  - ### 配置用户信息

    > 初次使用Git前应该要设置你的用户名和邮件地址。

    ```bash
    $ git config --global user.name "your name"
    $ git config --global user.email "email@example.com"
    ```

  - ### 配置文本编辑器

    > 当 Git 需要你输入信息时会调用文本编辑器，Git 会使用操作系统默认的文本编辑器，通常是 Vim。 如果你想使用自定义的文本编辑器，可以这样做：

    ```bash
    $ git config --global core.editor atom
    ```

    > 这样，就设置了 Atom 为你的默认文本编辑器了。

  - ### 检查配置信息

    > 如果想要检查你的 Git 配置，可以使用 `git config --list` 命令来列出所有 Git 当前的配置项。

    ```bash
    $ git config --list
    core.symlinks=false
    core.autocrlf=true
    core.fscache=true
    color.diff=auto
    color.status=auto
    color.branch=auto
    color.interactive=true
    user.name=your name
    user.email=email@example.com
    core.editor=atom
    ...
    ```

    > 你如果想查看某一特定项的配置信息，你可以通过输入 `git config <key>` 来查看。

    ```bash
    $ git config user.email
    email@example.com
    ```

- ## Git 基础

  - ### 获取 Git 仓库

    - #### 在现有目录中初始化仓库

      > 如果你打算使用 Git 对现有的项目进行管理，你只需要进入该项目目录并输入：

      ```bash
      $ git init
      ```

      > 该命令将创建一个名为 .git 的子目录，这个子目录包含你初始化的 Git 仓库中所有的必须文件。 该命令仅仅是做了一个初始化操作，项目中的文件还并没有被跟踪。 你可以通过 `git add` 命令来实现对指定文件的跟踪，然后执行 `git commit` 命令提交：

      ```bash
      $ git add index.md
      $ git commit -m 'added index.md file to this repository'
      ```

    - #### 克隆现有的仓库

      > 如果你想获得一份已经存在的 Git 仓库的拷贝，就需要用到 `git clone` 命令。 克隆仓库的命令格式是 `git clone [url]`。比如，要克隆 jQuery 库，可以用下面的命令：

      ```bash
      $ git clone git@github.com:jquery/jquery.git
      ```

      > 这个命令执行之后，会在当前目录下创建一个名为 "jquery" 的目录，并在这个目录下初始化一个 `.git` 文件夹，从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝。 如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以使用如下命令：

      ```bash
      $ git clone git@github.com:jquery/jquery.git my-jquery
      ```

      > 这将执行与上一个命令相同的操作，不过在本地创建的仓库名字变为 `my-jquery`。

  - ### 记录每次更新到仓库

    - #### 检查当前文件状态

      > 要查看哪些文件处于什么状态，可以用 `git status` 命令。

      ```bash
      $ git status
      ```

    - #### 跟踪新文件

      > 使用命令 `git add <filename>` 开始跟踪一个文件。

      ```bash
      $ git add index.md
      ```

      > 或者使用 `git add .` 命令跟踪所有新增文件。

      ```bash
      $ git add .
      ```

    - #### 暂存已修改文件

      > 如果一个文件已经被跟踪，如果再对这个文件进行修改，可以使用 `git add` 命令暂存已修改的文件。

      ```bash
      git add README.md
      ```

    - #### 状态简览

      > `git status` 命令的输出十分详细，但内容比较多。如果你使用 `git status -s` 命令或者 `git status --short` 命令，你将得到一种更为紧凑的格式输出。

      ```bash
      $ git status -s
      M  README.md
       M .gitignore
      MM Rakefile
      A  lib/git.rb
      ?? LICENSE.md
      ```

      > 新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。 你可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 M 表示该文件被修改了并放入了暂存区。

    - #### 忽略文件

      > 一般项目中有些文件我们不想纳入 Git 的管理，也不希望它们出现在未跟踪的文件列表中。在这种情况下，我们可以在项目根目录创建一个名为 `.gitignore` 的文件，列出要忽略的文件。

      ```bash
      $ cat .gitignore
      node_modules/
      /*.log
      .DS_Store
      ...
      ```

      > 文件 `.gitignore` 的格式规范如下：

      > - 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
      > - 可以使用标准的 glob 模式匹配。
      > - 匹配模式可以以（/）开头防止递归。
      > - 匹配模式可以以（/）结尾指定目录。
      > - 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

    - #### 查看已暂存和未暂存的修改

      > 如果 `git status` 命令的输出对于你来说过于模糊，你想知道具体修改了什么地方，可以用 `git diff` 命令。

      > 查看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`。

      > 若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令。（Git 1.6.1 及更高版本还允许使用 `git diff --staged`，效果是相同的，但更好记些。）

    - #### 提交更新

      > 当所有的新增文件和已修改的文件都放到暂存区后，我们可以利用 `git commit` 命令提交到本地仓库。提交之前一般要运行 `git status` 查看下当前所有文件状态是否都已经被放到暂存区。

      ```bash
      $ git commit
      ```

      > 这种方式会启动文本编辑器输入本次提交的说明。 (默认会启用 shell 的环境变量 $EDITOR 所指定的软件，一般都是 vim 或 emacs。当然也可以使用 git config --global core.editor 命令设定你喜欢的编辑软件。）

      > 另外，你也可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行，如下所示：

      ```bash
      $ git commit -m "added some adjust"
      ```

    - #### 跳过使用暂存区域

      > 尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤：

      ```bash
      $ git commit -a -m 'added all files to this repository'
      ```

    - #### 移除文件

      > 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

      > 如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 "Changes not staged for commit" 部分（也就是 未暂存清单）看到：

      ```bash
      $ rm PROJECTS.md
      $ git status
      On branch master
      Your branch is up-to-date with 'origin/master'.
      Changes not staged for commit:
        (use "git add/rm <file>..." to update what will be committed)
        (use "git checkout -- <file>..." to discard changes in working directory)

              deleted:    PROJECTS.md

      no changes added to commit (use "git add" and/or "git commit -a")
      ```

      > 然后再运行 `git rm` 记录此次移除文件的操作：

      ```bash
      $ git rm PROJECTS.md
      rm 'PROJECTS.md'
      $ git status
      On branch master
      Changes to be committed:
        (use "git reset HEAD <file>..." to unstage)

          deleted:    PROJECTS.md
      ```

      > 下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

      > 另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 `.a` 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 选项：

      ```bash
      $ git rm --cached README.md
      ```

      > `git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。 比方说：

      ```bash
      $ git rm log/\*.log
      ```

      > 注意到星号 `*` 之前的反斜杠 `\`， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 `log/` 目录下扩展名为 `.log` 的所有文件。 类似的比如：

      ```bash
      $ git rm \*~
      ```

      > 该命令为删除以 `~` 结尾的所有文件。

    - #### 移动文件

      > Git 中移动文件可以用这个命令 `git mv <file_from> <file_to>`

      ```bash
      $ git mv index.js ./src/index.js
      ```

  - ### 查看提交历史

    > 如果想查看一个项目的提交历史，可以使用 `git log` 命令进行查看。

    > 如果想显示每次提交的内容差异，一个常用的选项是 `-p`，你也可以加上 `-2` 来显示最近两次提交：

    ```bash
    $ git log -p
    $ git log -p -2
    ```

    > 如果你想看到每次提交的简略的统计信息，你可以使用 `--stat` 选项：

    ```bash
    $ git log --stat
    ```

    > `--stat` 选项在每次提交的下面列出额外的所有被修改的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了，每次提交的最后还有一个总结。

    > 另外一个常用的选项是 `--pretty`。这个选项可以制定使用不同于默认格式的方式展示提交历史。这个选项有一些内建的子选项可供使用。比如 `oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。另外还有 `short` , `full` 和 `fuller` 可以用。

    ```bash
    $ git log --pretty=oneline
    $ git log --pretty=short
    $ git log --pretty=full
    $ git log --pretty=fuller
    ...
    ```

    > 其中，最有意思的是 `format`，可以定制要显示的记录格式。这样的输出对后期提取分析格外有用----因为你知道输出的格式不会随着 Git 的更新而发生改变：

    ```bash
    $ git log --pretty=format:"%h - %an, %ar : %s"
    7f32bb6 - armdong, 3 days ago : 移动文件
    a6105db - armdong, 3 days ago : 移除文件
    129a1fd - armdong, 3 days ago : 跳过使用暂存区域
    d3a5ac8 - armdong, 3 days ago : 提交更新
    2331e2f - armdong, 3 days ago : 查看已暂存和未暂存的修改
    cb0395d - armdong, 3 days ago : 忽略文件
    f5a40db - armdong, 3 days ago : 状态简览调整
    264c059 - armdong, 3 days ago : 状态简览
    ...
    ```

    `git log --pretty=format` 常用的选项如下：

    item | description :--- | :--- %H | 提交对象（commit）的完整哈希字串 %h | 提交对象的简短哈希字串 %T | 树对象（tree）的完整哈希字串 %t | 树对象的简短哈希字串 %P | 父对象（parent）的完整哈希字串 %p | 父对象的简短哈希字串 %an | 作者（author）的名字 %ae | 作者的电子邮件地址 %ad | 作者修订日期（可以用 --date= 选项定制格式） %ar | 作者修订日期，按多久以前的方式显示 %cn | 提交者（committer）的名字 %ce | 提交者的电子邮件地址 %cd | 提交日期 %cr | 提交日期，按多久以前的方式显示 %s | 提交说明

    `git log` 常用的选项如下：

    item | description :--------------- | :------ -p | 按补丁格式显示每个更新之间的差异。 --stat | 显示每次更新的文件修改统计信息。 --shortstat | 只显示 --stat 中最后的行数修改添加移除统计。 --name-only | 仅在提交信息后显示已修改的文件清单。 --name-status | 显示新增、修改、删除的文件清单。 --abbrev-commit | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。 --relative-date | 使用较短的相对时间显示（比如，"2 weeks ago"）。 --graph | 显示 ASCII 图形表示的分支合并历史。 --pretty | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。

  - ### 撤消操作

    > 有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交：

    ```bash
    $ git commit --amend
    ```

    > 这个命令会将暂存区中的文件提交。如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。

    > 文本编辑器启动后，可以看到之前的提交信息。编辑后保存会覆盖原来的提交信息。

    > 例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

    ```bash
    $ git commit -m 'initial commit'
    $ git add <forgotten_file>
    $ git commit --amend
    ```

    > 最终你只会有一个提交，第二次提交将代替第一次提交的结果。

    - #### 取消暂存的文件

      > 例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 `git add .` 暂存了他们两个。如何只取消暂存两个中的一个呢？ `git status` 命令提示了你：

    ```bash
    $ git add .
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
        modified:   CONTRIBUTING.md
    ```

    > 在 "Changes to be commited" 文字正下方，提示使用 `git reset HEAD <file>...` 来取消暂存。所以，我们可以这样来取消暂存 `CONTRIBUTING.md` 文件：

    ```bash
    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M    CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
    ```

    > 这个命令有点奇怪，但是起作用了。`CONTRIBUTING.md` 文件已经是修改未暂存的状态了。

    - #### 撤销对文件的修改

      > 如果你并不想保留对 `CONTRIBUTING.md` 文件的修改怎么办？你该如何方便地撤消修改 - 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚它放入工作目录时的样子）？幸运的是，`git status` 也告诉了你应该如何做。在最后一个例子中，未暂存区域是这样：

    ```bash
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
    ```

    > 它非常清楚地告诉了你如何撤消之前所做的修改。让我们来按照提示修改：

    ```bash
    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
    ```

    > 可以看到那些修改已经被撤消了。

  - ### 远程仓库的使用

    - #### 添加远程库

      > 现在的情景是，你已经在本地创建了一个 Git 仓库后，又想在 Github 上创建一个 Git 仓库，并且让这两个仓库进行远程同步。这样，Github 上的仓库既可以作为备份，又可以让其他人通过该仓库进行协作。

      > 首先，登录 Github，然后在右上角找到 `New repository` 按钮，点击创建一个新的仓库；

      > 其次，在 `Repository name` 中填入与本地 Git 仓库名称一样的远程仓库名，其他保持默认，点击 `Create repository` 按钮，就成功创建了一个新的 Git 仓库。

      > 目前，在 Github 上这个新创建的 Git 仓库还是空的，我们需要将它与我们本地的 Git 仓库进行关联，命令如下：

      ```bash
      $ git remote add origin git@github.com:armdong/survivejs-webpack.git
      ```

      > 添加后，远程仓库的名字就是 `origin`，这是 Git 默认的叫法，也可以改成别的，但是 `origin` 这个名字一看就知道是远程库。

      > 下一步，就可以把本地库的所有内容推送到远程库上了。

      > 推送本地仓库 `master` 分支的内容到远程库：

      ```bash
      $ git push -u origin master
      ```

      > 如果本地还有其他分支，例如 `chapter-01` 分支，远程仓库没有这个分支，这时候可以用下面这个命令推送并使其跟踪远程同名分支：

      ```bash
      $ git push --set-upstream origin chapter-01
      ```

      > 假如我们在远程仓库新建了一个分支，本地仓库没有该分支，此时我们可以利用 `git checkout --track origin/<branch_name>` 命令在本地新建一个同名分支并自动跟踪远程的分支，例如 `chapter-02`：

      ```bash
      $ git checkout --track origin/chapter-02
      ```

  - ### Git 分支管理

    - #### 新建分支

    > 新建一个分支可以通过 `git branch <branch_name>` 命令新建，然后通过 `git checkout <branch_name>` 切换到刚刚新建的分支：

    ```bash
    $ git branch feature_a
    $ git checkout feature_a
    ```

    > 如果想要新建一个分支并同时切换到那个分支上，可以运行一个带有 `-b` 参数的 `git checkout` 命令：

    ```bash
    $ git checkout -b feature_b
    ```

    > 这个命令等同下面两条命令：

    ```bash
    $ git branch feature_b
    $ git checkout feature_b
    ```

    - #### 合并分支

    > 当新分支上的开发完成并测试 OK 以后，就可以合并到主分支上，首先切换回主分支 `git checkout master`，然后执行 `git merge <branch_name>`，以 `feature_b` 为例：

    ```bash
    $ git checkout master
    $ git merge feature_b
    ```

    这样，新分支做的修改就合并到主分支上了。

    - #### 删除分支

    > 当代码合并到主分支以后，新分支无需再进行开发时，可以删除掉新分支，首先确认你处于主分支上，然后执行 `git branch -d <branch_name>` 带参数 `-b` 命令即可：

    ```bash
    $ git branch -d feature_b
    ```

    - #### 分支管理

    > `git branch` 命令不仅仅可以创建与删除分支。如果不加任何参数运行它，会得到当前所有分支列表：

    ```bash
    $ git branch
      iss53
    * master
      testing
    ```

    > 注意 `master` 分支前的 `*` 字符：它代表现在检出的那一个分支（也就是说，当前 `HEAD` 指针所指向的分支）。这意味着如果在这时候提交，`master` 分支将会随着新的工作向前移动。如果需要查看每一个分支的最后一次提交，可以运行 `git branch -v` 命令：

    ```bash
    $ git branch -v
      iss53   93b412c fix javascript issue
    * master  7a98805 Merge branch 'iss53'
      testing 782fd34 add scott to the author list in the readmes
    ```

    > `--merged` 与 '--no-merged' 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。如果要查看哪些分支已经合并到当前分支，可以运行 `git branch --merged`：

    ```bash
    $ git branch --merged
      iss53
    * master
    ```

    > 因为之前已经合并了 `iss53` 分支，所以现在看到它在列表中。 在这个列表中分支名字前没有 `*` 号的分支通常可以使用 `git branch -d` 删除掉；你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。

    > 查看所有包含未合并工作的分支，可以运行 `git branch --no-merged`：

    ```bash
    $ git branch --no-merged
      testing
    ```

    > 这里显示了其他分支。 因为它包含了还未合并的工作，尝试使用 `git branch -d` 命令删除它时会失败：

    ```bash
    $ git branch -d testing
    error: The branch 'testing' is not fully merged.
    If you are sure you want to delete it, run 'git branch -D testing'.
    ```

    > 如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 `-D` 选项强制删除它
