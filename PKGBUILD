# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
pkgname="zfs-utils-common"

pkgver=0.7.11
pkgrel=1
pkgdesc="Kernel module support files for the Zettabyte File System."
makedepends=()
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${pkgver}/zfs-${pkgver}.tar.gz"
        "zfs-utils.bash-completion-r1"
        "zfs-utils.initcpio.install"
        "zfs-utils.initcpio.hook"
        "zfs-utils.initcpio.zfsencryptssh.install")
sha256sums=("4dff9ecce6e02061242d9435febe88c1250de83b96d392b712bccf31c459517a"
            "b60214f70ffffb62ffe489cbfabd2e069d14ed2a391fac0e36f914238394b540"
            "6e5e905a322d0426acdcbc05c5651ec78ee7b874b96d3c429c80f68b061170c5"
            "ae1cda85de0ad8b9ec8158a66d02485f3d09c37fb13b1567367220a720bcc9a5"
            "29080a84e5d7e36e63c4412b98646043724621245b36e5288f5fed6914da5b68")
license=("CDDL")
groups=("archzfs-linux")
provides=("zfs-utils")
install=zfs-utils.install
conflicts=("zfs-utils")
replaces=("zfs-utils-linux", "zfs-utils-linux-lts")
backup=('etc/zfs/zed.d/zed.rc' 'etc/default/zfs')

build() {
    cd "${srcdir}/zfs-${pkgver}"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --with-mounthelperdir=/usr/bin \
                --libdir=/usr/lib --datadir=/usr/share --includedir=/usr/include \
                --with-udevdir=/lib/udev --libexecdir=/usr/lib/zfs-${pkgver} \
                --with-config=user --enable-systemd
    make
}

package() {
    cd "${srcdir}/zfs-${pkgver}"
    make DESTDIR="${pkgdir}" install
    # Remove uneeded files
    rm -r "${pkgdir}"/etc/init.d
    rm -r "${pkgdir}"/usr/lib/dracut
    # move module tree /lib -> /usr/lib
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Autoload the zfs module at boot
    mkdir -p "${pkgdir}/etc/modules-load.d"
    printf "%s\n" "zfs" > "${pkgdir}/etc/modules-load.d/zfs.conf"
    # fix permissions
    chmod 750 ${pkgdir}/etc/sudoers.d
    # Install the support files
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.hook "${pkgdir}"/usr/lib/initcpio/hooks/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.install "${pkgdir}"/usr/lib/initcpio/install/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.zfsencryptssh.install "${pkgdir}"/usr/lib/initcpio/install/zfsencryptssh
    install -D -m644 "${srcdir}"/zfs-utils.bash-completion-r1 "${pkgdir}"/usr/share/bash-completion/completions/zfs
}
