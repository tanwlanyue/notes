---
title: 开发准备
date: 2024-03-29 23:20:53
categories: 
tags:
  - ohos
---
# 开发准备

## WSL安装

以管理员身份运行powershell

```powershell
# 启用适用于 Linux 的 Windows 子系统
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
# 启用虚拟机功能
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

重启电脑

下载安装 [适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

```powershell
# 将 WSL 2 设置为默认版本
wsl --set-default-version 2
```

下载安装 [Ubuntu 20.04](https://aka.ms/wslubuntu2004)

```shell
# 设置root密码
sudo passwd
```

添加一个网络位置 `\\wsl$\Ubuntu`，传文件可以使用网络位置直接拖拽，也可以通过挂载的`/mnt` 目录进行cp操作

WSL迁移出系统盘步骤：

方法1：

```powershell
# 查看wsl虚拟机的名称与状态
wsl -l -v
# 关闭运行中的WSL2（否则将无法移动安装目录）
wsl --shutdown
# 导出备份
wsl --export Ubuntu D:\Ubuntu\Ubuntu.tar
# wsl 卸载子系统
wsl --unregister Ubuntu
# 备份文件恢复
wsl --import Ubuntu D:\Ubuntu D:\Ubuntu\Ubuntu.tar
# 恢复默认用户
Ubuntu config --default-user tanwlanyue
```

方法2：[下载LxRunOffline-v3.5.0-msvc](https://github.com/DDoSolitary/LxRunOffline/releases/download/v3.5.0/LxRunOffline-v3.5.0-msvc.zip)

```powershell
# 关闭运行中的WSL2（否则将无法移动安装目录）
wsl.exe  --shutdown
# 移动指定的WSL2系统到目标目录
.\LxRunOffline.exe m -n Ubuntu -d D:\WSL2\Ubuntu
# 查看路径进行确认
lxrunoffline di -n Ubuntu
```

## 环境准备

```sh
# 更换系统软件源脚本 需root
bash <(curl -sSL https://linuxmirrors.cn/main.sh)

sudo apt-get update
sudo apt install python3-pip python3.9
sudo ln -s /usr/bin/python3.9 /usr/bin/python
sudo apt-get install -y bison ccache default-jdk flex zip ruby  libssl-dev libtinfo5 genext2fs  u-boot-tools mtools  mtd-utils scons  gcc-arm-none-eabi git git-lfs

git config --global user.name "zhanglei"
git config --global user.email "zhanglei737@huawei.com"
ssh-keygen -t rsa -C "zhanglei737@huawei.com"
cat ~/.ssh/id_rsa.pub
ssh -T git@gitee.com

sudo curl https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 -o /usr/local/bin/repo
sudo chmod a+x /usr/local/bin/repo
pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests

mkdir ~/openharmony
cd ~/openharmony
# 拉取代码
repo init -u git@gitee.com:openharmony/manifest.git -b master --no-repo-verify  
repo sync -c  
repo forall -c 'git lfs pull'

build/prebuilts_download.sh --no-check-certificatie -skip-ssl

./build.sh --no-prebuilt-sdk --product-name=rk3568 --ccache -T foundation/multimedia/camera_framework/frameworks/native/camera:camera_framework camera_napi camera_service -j32

# 同步代码
git remote add tanwlanyue https://gitee.com/tanwlanyue/multimedia_camera_framework.git
git fetch origin
git rebase origin/master
```

<!-- more -->
加快本地编译的一些参数，适当选择添加以下的编译参数可以加快编译的过程。

- --ccache : ccache会缓存c/c++编译的编译输出，下一次在编译输入不变的情况下，直接复用缓存的产物
- --fast-rebuild : 编译流程主要分为preloader->loader->gn->ninja这四个过程，在本地没有修改gn和产品配置相关文件的前提下，添加--fast-rebuild 会直接从 ninja 编译开始
- --build-target参数 : 该参数用于指定编译模块，可以通过相关仓下BUILD.gn中关注group、ohos_shared_library、ohos_executable等关键字找模块的名字

## vscode 插件 clangd 安装

项目执行prebuild会下载clang,不需要自己apt install clang

编译命令添加`--gn-flags='--export-compile-commands'`

```
./build.sh --no-prebuilt-sdk --product-name=rk3568 --gn-flags='--export-compile-commands'
```

因为build参数`--product-name =rk3568`, `compile_commands.json` 生成在out/rk3568目录下

编辑工程目录下的 .vscode/settings.json

```json
{
    "clangd.path": "${workspaceFolder}/prebuilts/clang/ohos/linux-x86_64/llvm/bin/clangd",
    "clangd.arguments": [
        "--compile-commands-dir=${workspaceFolder}/out/rk3568/",
        "--query-driver=${workspaceFolder}/prebuilts/clang/ohos/linux-x86_64/llvm/bin/clang++",
    ],
    "C_Cpp.intelliSenseEngine": "disabled",
    "cmake.configureOnOpen": false,
}
```

vscode 底部显示 indexing:xxx/xxxx 索引进度即设置成功

## 别名脚本

```sh
wget https://gitee.com/tanwlanyue/toolbox/raw/master/initEnv.sh
chmod 777 initEnv.sh
source initEnv.sh
```



```
# 设置全局代理配置
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy https://127.0.0.1:7890
# 取消全局代理配置
git config --global --unset http.proxy
git config --global --unset https.proxy
```