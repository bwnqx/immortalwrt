- **高通部分源码取自以下项目：**

https://github.com/JiaY-shi/openwrt.git

https://github.com/qosmio/openwrt-ipq.git

https://github.com/LiBwrt-op/openwrt-6.x.git

https://github.com/King-Of-Knights/openwrt-6.x.git

https://github.com/VIKINGYFY/immortalwrt.git


- **不要用 `root` 用户进行编译⚠**
- 国内用户编译前最好准备好梯子
- 默认登陆IP 192.168.1.1

1. 首先装好 Linux 系统，推荐 Debian 或 Ubuntu LTS

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip libpython3-dev qemu-utils \
   rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   git clone --depth 1 --single-branch https://github.com/laipeng668/immortalwrt.git
   cd immortalwrt
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 后面是线程数，为便于排除错误推荐用单线程）

   ```bash
   make download -j$(nproc)
   make -j1 V=s
   ```

5. 二次编译：

   ```bash
   cd immortalwrt
   git fetch && git reset --hard origin/openwrt-24.10
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make menuconfig
   make V=s -j$(nproc)
   ```

6. 如果需要重新配置：

   ```bash
   rm -rf .config
   make menuconfig
   make V=s -j$(nproc)
   ```

7. 编译完成后输出路径：bin/targets