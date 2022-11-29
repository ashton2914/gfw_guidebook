# WSL访问挂在在Windows端的SMB共享文件夹

# **安装DrvF**

要使用DrvF安装Windows驱动器，可以使用常规Linux安装命令。例如，要安装可移动驱动器D：as / mnt / d目录，请运行以下命令：

```bash
$ sudo mkdir /mnt/d
$ sudo mount -t drvfs D: /mnt/d

```

现在，您将能够在/ mnt / d下访问D：驱动器的文件。如果要卸载驱动器，例如可以安全地将其删除，请运行以下命令：

```bash
$ sudo umount /mnt/d

```

# **安装网络位置**

当您希望安装网络位置时，您当然可以在Windows中创建映射的网络驱动器并按照上面的说明进行安装。但是，也可以使用UNC路径直接安装它们：

```bash
$ sudo mkdir /mnt/share
$ sudo mount -t drvfs '\\server\share' /mnt/share

```

注意UNC路径周围的单引号;这些是防止需要逃脱反斜杠的必要条件。如果没有用单引号括起UNC路径，则需要通过加倍反转来逃避反斜杠（例如 `\\\\server\\share` ）。

WSL无法指定用于连接到网络共享的凭据。如果需要使用不同的凭据连接到服务器，请在Windows中通过使用Windows凭据管理器或net use命令导航到文件资源管理器中的共享来指定它们。可以通过interop从WSL内部（使用net.exe使用）调用net use命令。键入net.exe help以获取有关如何使用此命令的更多信息。