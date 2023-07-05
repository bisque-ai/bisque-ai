# Data Upload from CLI

BisQue manages data using [iRODS](https://irods.org) open source data management software. Users can access their data in BisQue using an iRODS command line tool [GoCommands](https://github.com/cyverse/gocommands) for more efficient large data transfer. In this section, we show how to use GoCommands.

## Download GoCommands

Here are commands to download and uncompress the GoCommands package. Make sure you are  using the right command for your target system's OS and architecture.

GoCommands is a single executable file, `gocmd`, which does not require installation.&#x20;

### Linux AMD64 (Intel, AMD CPUs)

```bash
GOCMD_VER=$(curl -L -s https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt); \
curl -L -s https://github.com/cyverse/gocommands/releases/download/${GOCMD_VER}/gocmd-${GOCMD_VER}-linux-amd64.tar.gz | tar zxvf -
```

### Linux ARM64 (Raspberry Pi, Nvidia Jetson, ...)

```bash
GOCMD_VER=$(curl -L -s https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt); \
curl -L -s https://github.com/cyverse/gocommands/releases/download/${GOCMD_VER}/gocmd-${GOCMD_VER}-linux-arm64.tar.gz | tar zxvf -
```

### MacOS AMD64 (Intel)

```bash
GOCMD_VER=$(curl -L -s https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt); \
curl -L -s https://github.com/cyverse/gocommands/releases/download/${GOCMD_VER}/gocmd-${GOCMD_VER}-darwin-amd64.tar.gz | tar zxvf -
```

### MacOS ARM64 (M1/M2)

```bash
GOCMD_VER=$(curl -L -s https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt); \
curl -L -s https://github.com/cyverse/gocommands/releases/download/${GOCMD_VER}/gocmd-${GOCMD_VER}-darwin-arm64.tar.gz | tar zxvf -
```

### Windows AMD64 (via Command Prompt)

```bash
curl -L -s -o gocmdv.txt https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt && set /p GOCMD_VER=<gocmdv.txt
curl -L -s -o gocmd.zip https://github.com/cyverse/gocommands/releases/download/%GOCMD_VER%/gocmd-%GOCMD_VER%-windows-amd64.zip && tar zxvf gocmd.zip && del gocmd.zip gocmdv.txt
```



## Configure GoCommands

To configure, type below:

```
./gocmd init
```

When prompted, enter the values below:

| Fields   | Value                   |
| -------- | ----------------------- |
| Hostname | brain.ece.ucsb.edu      |
| Port     | 1247                    |
| Username | \<your BisQue username> |
| Zone     | ucsb                    |
| Password | \<your BisQue password> |

Your accounts of iRODS and BisQue are synced, meaning that the username and password are the same. If you are having problems signing into iRODS, **simply changing your password in BisQue will update your iRODS the password in sync**.



## Use GoCommands&#x20;

### Upload files/directories from local computer to iRODS

Upload a directory using the `put` command. With `--progress` flag, GoCommands will display progress bars.&#x20;

```
./gocmd put --progress /local_dir /ucsb/home/username/dest_dir
```

### Download files/directories from iRODS to local computer

```
./gocmd get --progress /ucsb/home/username/target_dir /local_dir
```

### Full list of commands

| `env`          | Display current configuration                                                                                                                 |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `init`         | Initialize configuration                                                                                                                      |
| `cd`           | Change current working directory                                                                                                              |
| `pwd`          | Display current working directory                                                                                                             |
| `ls`           | List files or directories                                                                                                                     |
| `cp`           | Copy iRODS files or directories to a target directory                                                                                         |
| `mv`           | Move iRODS files or directories to a target directory                                                                                         |
| `rm`           | Remove iRODS files or directories                                                                                                             |
| `rmdir`        | Remove iRODS directories                                                                                                                      |
| `mkdir`        | Make iRODS directories                                                                                                                        |
| `get`          | Download iRODS files or directories                                                                                                           |
| `put`          | Upload local files or directories to a target iRODS directory                                                                                 |
| `bput`         | Upload local files or directories to a target iRODS directory by bundling and transferring in parallel (optimized for many small file upload) |
| `sync`         | Sync local directory with iRODS directory                                                                                                     |
| `copy-sftp-id` | Upload SSH public key to iRODS for SFTP access                                                                                                |



## Frequently asked questions

Q: In which scenarios should I use GoCommands to upload data vs. the BisQue web service?

A:&#x20;

You can use GoCommands on systems that does not provide web browser access. For example, if you are uploading data from HPC, or downloading to HPC, GoCommands will provide access to your data.

If files are large or many, use GoCommands for fast data transfer. GoCommands uses parallel data transfer to make full use of the available bandwidth.



Q: Will changing my iRODS password affect the registration of my iRODS data on BisQue?

A:&#x20;

No, if you change your password using the `passwd` command in GoCommand, there will not be any effect on your ability to access your iRODS data on the BisQue web service.

If you change BisQue user's password, then the corresponding iRODS account's password is also changed. However, the converse is not true.



Q: When should I use `bput` instead of `put` command?

A:

`bput` is efficient to move many small files. It also works fine at any case. But generally, it will work well if the data has more than 50 files, and each file is less than 100MB.&#x20;

