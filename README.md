sudo eselect repository enable LoongArch-Gentoo-Configs

If you prefer LoongArch-Gentoo-Configs, you can use the following command:

sudo layman -a LoongArch-Gentoo-Configs

After that, sync repositories:

sudo emerge --sync

After that, you are able to install packages from this overlay.

It is a good habit to mask all of the packages in other overlay and only unmask as you need, which could prevent your system from being breaked by third party overlays. Although this overlay do not contains such dangerous packages, I still recommend you to do that.

To mask all packages from this overlay, you can use the following command:

echo "*/*::LoongArch-Gentoo-Configs" | sudo tee -a /etc/portage/package.mask

After that, unmask as you needed. For example:

sudo emerge -av nameof --autounmask

我这里记录的是使用系统自带命令启用官方overlay列表内仓库流程

（使用layman也是一种常用的管理方法）

启用第三方仓库

emerge --ask app-eselect/eselect-repository 安装第三方仓库管理插件
emerge --ask dev-vcs/git 安装git（大部分第三方仓库都是git管理）
eselect repository list 查看[第三方仓库列表](Gentoo Overlays)
eselect repository enable guru gentoo-zh 启用第三方仓库（开这两个就基本够用了）
emerge --sync 更新所有仓库，可以加 -r <仓库名> 更新指定仓库

guru是官方的测试包仓库，系统默认配置仅允许稳定包（make.conf里的KEYWORD指定），安装此仓库的包需配置标记：

- 编辑 /etc/portage/package.accept_keywords （没有就创建）

gui-apps/wofi ~amd64 （允许安装此包的测试版）

(UN)MASK软件包

注意这里的配置不覆盖KEYWORDS，即优先级低于上面的package.accept_keywords

Masking installed but unsafe ebuild repositories
	When using large ebuild repositories or those with unknown/low quality code, it is best practice to hard mask the whole ebuild repository and only accept specific ebuilds on a case-by-case basis.
	For example, for an overlay named "repository-foobar":

FILE [/etc/portage/package.mask] Mask all packages in an ebuild repository
*/*::repository-foobar
Then add the specific package(s) from the repository-foobar overlay so that they will be available visible to Portage for installation:

FILE [/etc/portage/package.unmask] Unmask a specific package in an ebuild repository
foo-category/bar::repository-name

After the above unmask the package named "foo-category/bar" should be available and none of the other packages from the repository-foobar overlay will be available, which is by design.

上面是官方WIKI的摘抄，很好理解，编辑/etc/portage/ 下 package.mask与package.unmask文件/目录内的文件，按指定格式添加 包名::仓库名，即可实现手动标记包
