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

