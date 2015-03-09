#Personalizing Git with Aliases

#使用 Alias 来定制 Git

Part of getting comfortable with the command line is making it your own. Small customizations, shortcuts, and time saving techniques become second nature once you spend enough time fiddling around in your terminal. Since Git (http://git-scm.com/) is my Version Control System (http://en.wikipedia.org/wiki/Revision_control) of choice (due partially to its incredible popularity via GitHub (https://github.com/)), I like to spend lots of time optimizing my experience there.

想要舒适地使用命令行，那就去拥有它。小小的定制，快捷方式已经节省时间的技巧会变的很自然，当你在终端上用了足够的时间。因为选择了[Git](http://git-scm.com/)这个[版本控制系统](http://en.wikipedia.org/wiki/Revision_control)(有一部分原因是 [Github](https://github.com/) 让它变得难以置信的受欢迎)，我愿意花大量的时间来优化经验。

Once you’ve become comfortable enough with Git to push, pull, add, and commit, and you feel like you’d like to pursue more, you can customize it to make it your own. A great way to start doing this is with aliases. Aliases can help you by providing shorthand commands so you can move faster and have to remember less of Git’s sometimes very murky UI. Luckily, Git makes itself easy to customize by setting global options in a file named .gitconfig in our home directory.

或许你很熟练的使用Git的 push，pull，add以及commit命令，你觉得自己的追求不止这些，你可以把它定制为你自己的。要这么做，使用别名是一个很好的方式。别名为你提供了简短的命令，你可以更快的上手并且只需记住 Git 通常都很含糊的 UI。很幸运，Git可以变得更简单，通过在主目录下名为 .gitconfig 的文件进行全局的设置。

Quick note: for me, the home directory is /Users/jlembeck, you can get there on OSX or most any other Unix platform by typing cd ~ and hitting enter or return. On Windows, if you’re using Powershell, you can get there with the same command and if you’re not using Powershell, cd %userprofile% should do the trick.

速记：我的主文件目录是 /Users/jlembeck，在 OS X 或者 Unix平台上输入 cd ~ 然后敲一下 enter 或者 return。在 Windows ，如果你在用 Powershell，可以使用同样的命令进入目录。如果没有，cd %userprofile% 命令应该也可以。

Now, let’s take a look. First, open your .gitconfig file (from your home directory):

现在，我们看一下。首先，打开 .gitconfig 文件（从你的主目录）:

~/code/grunticon master* $ cd ~ ~ $ open .gitconfig 

You might see a file that looks similar to this:

你会看到类似下面的文件：

``[user]
  name = Jeff Lembeck
  email = your.email.address@host.com
[alias]
  st = status
  ci = commit
  di = diff
  co = checkout
  amend = commit --amend
  b = branch``

Let’s look at the different lines and what they mean.

现在来看下每一行分别是什么意思。

``
[user] 
	name = Jeff 
	Lembeck email = your.email.address@host.com 
``

First up, the global user configuration. This is what Git references to say who you are when you make commits.

首先，全局用户配置。在这里，每一次提交 Git 都会提供关于你的信息。

``[alias] st = status ci = commit di = diff co = checkout amend = commit --aend b = branch ``

Following the user information is what we’re here for, aliases.

紧接着在用户信息下面的就是我们要讨论的——别名。

Any command given in that screen is prefaced with git. For example, git st is an alias for git status and git ci is git commit. This allows you to save a little time while you’re typing out commands. Soon, the muscle memory kicks in and git ci -m “Update version to 1.0.2” becomes your keystroke-saving go-to.

比如，``git st`` 是 ``git status`` 的别名，``git ci`` 是 ``git commit`` 的别名。 在你使用这些命令式会节省一些时间了。慢慢地，肌肉记忆开始工作，并且 ``git ci -m "Update version to 1.0.2"``变成了你的按键储蓄。

Ok, so aliases can be used to shorten commands you normally type and that’s nice, but a lot of people don’t really care about saving 10 keystrokes here and there. For them, I submit the use case of aliases for those ridiculous functions that you can never remember how to do. As an example, let’s make one for learning about a file that was deleted. I use this all of the time.

好了，别名可以用来简短你经常使用的命令，这非常棒。但是很多人不会在乎10个分散的按键。对他们来说，我提供了用例的别名，那些你绝不会去记忆如何去使用的一些很荒唐的函数。

Now, to check the information on a deleted file, you can use git log --diff-filter=D -- path/to/file. Using this information I can create an alias.

现在，查看已经删除的文件的信息，你会使用 ``git log --diff-filter=D -- path/to/file``。有了这个信息，我可以创建一个别名。

``d = log --diff-filter=D -- $1 ``

Let’s break that down piece by piece.



This should look pretty familiar. It is almost the exact command from above, with a few changes. The first change you’ll notice is that it is missing git. Since we are in the context of git, it is assumed in the alias. Next, you’ll see a $1, this allows you to pass an argument into the alias command and it will be referenced there.

这看起来相当的熟悉。这几乎就是上面原本的命令，一点点改变。你可能注意到这个变化，git 被丢掉了。由于我们在git环境下，在别名里指定了它。然后，会看到 ``$1``，允许你传递一个参数到 alias 命令，并且它将会在那被引用。

Now, with an example. git d lib/fileIDeleted.js. d is not a normal command in git, so git checks your config file for an alias. It finds one. It calls git log --diff-filter=D -- $1. And passes the argument lib/fileIDeleted.js into it. That will be the equivalent of calling git log --diff-filter=D -- lib/fileIDeleted.js.

现在，举个例子。``git d lib/fileIDeleted.js``。 ``d`` 不是一个git命令，所以 git 会检查config 文件中的别名。找到了一个。他就是 ``git log --diff-filter=D -- $1``。并且给它传递了 ``lib/fileIDeleted.js`` 参数。等价于调用了 ``git log --diff-filter=D -- lib/fileIDeleted.js``。

Now you never have to remember how to do that again. Time to celebrate the time you saved that would normally be spent on Google trying to figure out how to even search for this. I suggest ice cream.

现在你没必要再去记忆该怎么做。是时候去庆祝一下你节省的时间，那些通常花费在Google中找出如何去搜索这个。我建议买个冰淇淋。

For further digging into this stuff: I got most of my ideas from Gary Bernhardt’swonderful dotfiles repository (https://github.com/garybernhardt/dotfiles). I strongly recommend checking out dotfiles repos to see what wild stuff you can do out there with your command line. Gary’s is an excellent resource and Mathias’s might be the most famous (https://github.com/mathiasbynens/dotfiles). To learn more about Git aliases from the source, check them out in the Git documentation (http://git-scm.com/book/en/Git-Basics-Tips-and-Tricks#Git-Aliases).

深入的钻研这个东西：我的大多数想法源于Gary Bernhardt的[精彩的dotfiles库](https://github.com/garybernhardt/dotfiles)。我强烈的建议检出 dotfiles 类库去看一下有哪些令人激动的东西是可以使用命令行完成的。Gary的提交是非常好的资源，[Mathias的提交可能是最知名的](https://github.com/mathiasbynens/dotfiles)。在这些资源里学习更多关于 Git 别名的知识，使用 [Git documentation](http://git-scm.com/book/en/Git-Basics-Tips-and-Tricks#Git-Aliases) 检出。

原文：[Personalizing Git with Aliases](http://alistapart.com/blog/post/personalizing-git-with-aliases)

