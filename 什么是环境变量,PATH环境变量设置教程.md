在命令行上键入命令时，实质是在告诉Shell运行指定可执行文件的名称

在终端键入命令时，实质是在告诉Shell运行指定可执行文件的名称。在Linux，这些可执行文件可以是[ls](https://www.myfreax.com/how-to-list-files-in-linux-using-the-ls-command/)，[find](https://www.myfreax.com/how-to-find-files-in-linux-using-the-command-line/)，[file](https://www.myfreax.com/linux-file-command/)`等命令。

通常在你的系统会默认设置几个不同的目录。这些目录中存储着具有可执行权限的二进制文件，这些二进制文件都可以在任何目录中运行。

常见目录是`/bin`，`/sbin`，`/usr/sbin`，`/usr/local/bin`和`/usr/local/sbin`。

当您在终端运行命令时，shell程序会在`$PATH`环境变量包含的目录中搜索命令的可执行文件。

本教程介绍如何在Linux 设置PATH环境变量。要查看PATH环境变量可以使用echo命令或者printenv命令打印当前的$PATH环境变量。

## 什么是 PATH 环境变量

$PATH[环境变量](https://www.myfreax.com/how-to-set-and-list-environment-variables-in-linux/)是由冒号分隔的目录列表，告诉shell哪些目录是用于搜索命令的可执行二进制文件。

您可以使用`printenv`或[echo](https://www.myfreax.com/echo-command-in-linux-with-examples/)命令查看`$PATH`包含的目录。两个命令都有相同的输出。

如果您在两个不同的目录中有两个共享相同名称的可执行文件，则Shell将运行从`$PATH`搜索到的第一个目录执行该文件。

```bash
printenv PATH
echo $PATH
```
例如
```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

## 将目录添加 PATH 环境变量

在某些情况下，您可能需要添加其他目录`$PATH`环境变量。例如，某些程序可能安装在不同的目录。

但是为了在不指定可执行文件绝对路径运行它。因此，您需要将目录添加到`$PATH`环境变量。

假设家目录中有一个`bin`的目录，要将目录添加到`$PATH`环境变量，请运行命令`export PATH="$HOME/bin:$PATH"`。

现在，您只需键入可执行二进制文件的名称即可运行应用。但是，此更改只是临时的，仅在当前的Shell会话中有效。

```bash
export PATH="$HOME/bin:$PATH"
```

## 持久化PATH

`export`命令用于[设置环境变量](https://www.myfreax.com/how-to-set-and-list-environment-variables-in-linux/)，使用`export`命令设置环境变量仅在当前的shell会话可用。要使更改永久生效，您需要在PATH配置文件设置`$PATH`环境变量。

在大多数Linux发行版中，当系统启动时，将从`/etc/environment`和`/etc/profile`以及`~/.bashrc`文件中读取环境变量。

全局的配置文件是`/etc/environment`和`/etc/profile`。如果要为系统所有用户设置`$PATH`环境变量，请在这两个文件中修改。

当你为指定的用户设置`$PATH`环境变量时，请在`~/.bashrc`文件修改`$PATH`变量，如果使用的是Zsh，则文件是`~/.zshrc`。

在本教程中，我们将在`/etc/environment`文件设置PATH环境变量，也就是为所有用户设置。

命令`echo PATH="$HOME/bin:$PATH" | sudo tee -a /etc/environment`将会把目录`$HOME/bin`添加到PATH环境变量，使用[echo命令](https://www.myfreax.com/echo-command-in-linux-with-examples/)和[tee命令](https://www.myfreax.com/linux-tee-command/)。

```shell
echo PATH="$HOME/bin:$PATH" | sudo tee -a /etc/environment
```

现在我们已经持久化PATH环境变量，如果你还没有使用export命令设置PATH，那么当前shell会话PATH环境变量并不会被修改。

你可以运行[`source`命令](https://www.myfreax.com/bash-source-command/)将加载环境变量到当前的shell会话。当然也可以使用export命令设置PATH环境变量。

要确定是否正确设置PATH环境变量，可以运行命令`echo $PATH`查看当前`$PATH`环境变量的值。

```bash
source /etc/environment
echo $PATH
```

## 结论

在在Linux 设置PATH环境变量非常简单。这使您可以在任何目录运行你的命令或者应用，且不需要键入可执行文件的完整路径。

相同的说明适用于任何Linux发行版，包括Ubuntu，CentOS，RHEL，Debian和Linux Mint。