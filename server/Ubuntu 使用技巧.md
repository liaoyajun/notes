# Ubuntu 使用技巧

## 终端指令

### 常用指令

- 更新包缓存（可以知道包的哪些版本可以被安装或升级）
    ```bash
    sudo apt update
    ```

- 根据本地包缓存的数据，安装可升级包的最新版本
    ```bash
    sudo apt upgrade
    ```

- 删除所有的软件安装包
    ```bash
    sudo apt clean
    ```

- 删除不再可用的软件安装包
    ```bash
    sudo apt autoclean
    ```

- 删除不再需要的依赖软件包
    ```bash
    sudo apt autoremove
    ```

- 删除特定软件
    ```bash
    sudo apt remove 软件名
    ```

- 删除软件残余
    ```bash
    sudo apt purge 软件名
    ```

### 安装 `nodejs` 和 `npm`
    ```bash
    sudo apt install nodejs npm
    ```

### 打开终端历史命令行自动补全功能

- 打开配置文件 `/etc/inputrc`

- 取消这两行注释，重启终端
    ```bash
    # alternate mappings for "page up" and "page down" to search the history
    "\e[5~": history-search-backward
    "\e[6~": history-search-forward
    ```

### 其他

- `apt` 和 `apt-get` 区别：`apt` 是 `apt-get` 命令的一个更新而更简单的版本，一般情况下使用 `apt` 交互更好更方便
