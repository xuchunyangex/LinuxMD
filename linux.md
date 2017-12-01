# Linux CMD CheetSheet

CMD

[命令]()查询网站

http://man.linuxde.net/

-----

FAQ

1. 如何卸载软件？ Ans:使用Synaptic Package Manager

--------------------------------------------------------------------------
### 装机必备软件

```bash
#输入法
sudo apt-get install fcitx fcitx-googlepinyin 
#Git
sudo apt-get install git - git
#文本编辑器
`` - sublime text
#Markdown编辑器
`` - typora
```

--------------------------------------------------------------------------

### 常用命令
`nano <path/file.postfix>` - 用nano文本编辑器打开某路径上的文本文件 

#### C语言命令
```bash
# C语言编译和运行
#编译
gcc -Wall -o blink blink.c -lwiringPi
#运行
sudo ./blink  
```

---

### 软件包维护，删除和查询 apt / apt-get / apt-cache / dpkg

### 常用apt命令

```bash
#安装软件 
apt-get install softname1 softname2 softname3……
#卸载软件 
apt-get remove softname1 softname2 softname3……
#卸载并清除配置 
apt-get remove –purge softname1
#更新软件信息数据库 
apt-get update
#进行系统升级 
apt-get upgrade
#搜索软件包 
apt-cache search softname1 softname2 softname3……

#如果使用 apt-get 遇到速度慢或者源不存在等错误，可能需要更换源，请参考此处。
http://shumeipai.nxez.com/2013/08/31/raspbian-chinese-software-source.html

#安装deb软件包 
dpkg -i xxx.deb
#删除软件包 
dpkg -r xxx.deb
#连同配置文件一起删除 
dpkg -r –purge xxx.deb
#查看软件包信息 
dpkg -info xxx.deb
#查看文件拷贝详情 
dpkg -L xxx.deb
#查看系统中已安装软件包信息 dpkg -l
#重新配置软件包 dpkg-reconfigure xxx

#清除所有已删除包的残馀配置文件
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
#如果报如下错误，证明你的系统中没有残留配置文件了，无须担心。
dpkg: –purge needs at least one package name argument
#dpkg安裝的可以用apt卸載，反之亦可。

apt-cache search # ------(package 搜索包)
apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)
sudo apt-get install # ------(package 安装包)
sudo apt-get install # -----(package - - reinstall 重新安装包)
sudo apt-get -f install # -----(强制安装?#"-f = --fix-missing"当是修复安装吧...)
sudo apt-get remove #-----(package 删除包)
sudo apt-get remove - - purge # ------(package 删除包，包括删除配置文件等)
sudo apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)
sudo apt-get update #------更新源
sudo apt-get upgrade #------更新已安装的包
sudo apt-get dist-upgrade # ---------升级系统
sudo apt-get dselect-upgrade #------使用 dselect 升级
apt-cache depends #-------(package 了解使用依赖)
apt-cache rdepends # ------(package 了解某个具体的依赖?#当是查看该包被哪些包依赖吧...)
sudo apt-get build-dep # ------(package 安装相关的编译环境)
apt-get source #------(package 下载该包的源代码)
sudo apt-get clean && sudo apt-get autoclean # --------清理下载文件的存档 && 只清理过时的包
sudo apt-get check #-------检查是否有损坏的依赖
```

#### 软件包维护

- `apt-get update` - 在你更改了/etc/apt/sources.list 或 /etc/apt/preferences 后，需要运行这个命令以令改动生效。同时也要定期运行该命令，以确保你的源列表是最新的。该命令等价于新立得软件包管理器中的“刷新”，或者是 Windows和OS X 下的 Adept 软件包管理器的 “check for updates”。
- `apt-get upgrade` - 更新所有已安装的软件包。类似一条命令完成了新立得软件包管理器中的“标记所有软件包以便升级”并且“应用”。
- `apt-get dist-upgrade` - 更新整个系统到最新的发行版。等价于在新立得软件包管理器中“标记所有更新”，并在首选项里选择“智能升级” -- 这是告诉APT更新到最新包，甚至会删除其他包（注：不建议使用这种方式更新到新的发行版）。
- `apt-get -f install` -- 等同于新立得软件包管理器中的“编辑->修正（依赖关系）损毁的软件包”再点击“应用。如果提示“unmet dependencies”的时候，可执行这行命令。
- `apt-get autoclean` - 如果你的硬盘空间不大的话，可以定期运行这个程序，将已经删除了的软件包的.deb安装文件从硬盘中删除掉。如果你仍然需要硬盘空间的话，可以试试`apt-get clean`，这会把你已安装的软件包的安装包也删除掉，当然多数情况下这些包没什么用了，因此这是个为硬盘腾地方的好办法。
- `apt-get clean` 类似上面的命令，但它删除包缓存中的所有包。这是个很好的做法，因为多数情况下这些包没有用了。但如果你是拨号上网的话，就得重新考虑了。
- 包缓存的路径为/var/cache/apt/archives，因此，`du -sh /var/cache/apt/archives`将告诉你包缓存所占用的硬盘空间。
- `dpkg-reconfigure foo` - 重新配置“foo”包。这条命令很有用。当一次配置很多包的时候， 要回答很多问题，但有的问题事先并不知道。例如，`dpkg-reconfigure fontconfig-config`，在Ubuntu系统中显示字体配置向导。每次我安装完一个 Ubuntu 系统，我都会运行这行命令，因为我希望位图字体在我的所有应用程序上都有效。
- `echo "foo hold" | dpkg --set-selections` - 设置包“foo”为hold，不更新这个包，保持当前的版本，当前的状态，当前的一切。类似新立得软件包管理器中的“软件包->锁定版本”。
  - 注： `apt-get dist-upgrade` 会覆盖上面的设置，但会事先提示。 另外，你必须使用 sudo。输入命令`echo "foo hold" | sudo dpkg --set-selections`而不是`sudo echo "foo hold" | dpkg --set-selections`
- `echo "foo install` -- 删除“hold”“locked package”状态设置。命令行为`echo "foo install" | sudo dpkg --set-selections`

#### 软件包删除

- `apt-get remove 软件包名称` - 删除已安装的软件包（保留配置文件）
- `apt-get --purge remove 软件包名称` - 删除已安装包（不保留配置文件）
- 特别技巧：如果你想在删除‘foo’包同时安装‘bar’： `apt-get --purge remove foo bar+`。
- `apt-get autoremove` - 删除为了满足其他软件包的依赖而安装的，但现在不再需要的软件包。

#### 软件包搜索

- `apt-cache search foo` - 搜索和"foo"匹配的包。
- `apt-cache show foo` - 显示"foo"包的相关信息，例如描述、版本、大小、依赖以及冲突。
- `dpkg --print-avail 软件包名称` - 与上面类似。
- `dpkg -l \*foo*` - 查找包含有"foo"字样的包。与`apt-cache show foo`类似，但是还会显示每个包是安装了还是没安装。
- `dpkg -l package-name-pattern` - 列出名为package-name-pattern的软件包。除非你知道软件包的正确全称，否则可以使用“*package-name-pattern*”.
- `dpkg -L foo` - 显示名为“foo”的包都安装了哪些文件以及它们的路径，很有用的命令。
- `dlocate foo` - 在已安装的包中搜索“foo”的文件。对于回答“这个文件来源于哪个包”这个问题，是非常实用的。dlocate是一个软件包，必须安装它才能使用本命令。
- `dpkg -S foo` - 和上面的命令一样，但相比更慢一些。他只能在Debian或Ubuntu系统下运行。另外，不需要安装dlocate包。
- `apt-file search foo` - 类似dlocate和dpkg -S，但搜索所有有效软件包，不单单只是你系统上的已安装的软件包。-- 它所回答的问题是“哪些软件包提供这些文件”。你必须安装有apt-file软件包，并且确保apt-file数据库是最新的。
- `dpkg -c foo.deb` - “foo.deb”包含有哪些文件？注：foo.deb是含路径的文件名。-- 这个是针对你自己下载的.deb包。
- `apt-cache dumpavail` - 显示所有可用软件包，以及它们各自的详细信息（会产生很多输出）。
- `apt-cache show 软件包名称` - 显示软件包记录，类似`dpkg --print-avail 软件包名称`。
- `apt-cache pkgnames` - 快速列出已安装的软件包名称。
- `apt-file search filename` - 查找包含特定文件的软件包（不一定是已安装的），这些文件的文件名中含有指定的字符串。apt-file是一个独立的软件包。您必须先使用 apt-get install 来安装它，然后运行 apt-file update。如果 apt-file search filename 输出的内容太多，您可以尝试使用 `apt-file search filename | grep -w filename`（只显示指定字符串作为完整的单词出现在其中的那些文件名）或者类似方法，例如：apt-file search filename | grep /bin/（只显示位于诸如/bin或/usr/bin这些文件夹中的文件，如果您要查找的是某个特定的执行文件的话，这样做是有帮助的）。