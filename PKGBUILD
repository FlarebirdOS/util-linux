pkgname=(util-linux util-linux-libs)
pkgbase=util-linux
pkgver=2.41.1
pkgrel=3
pkgdesc="Miscellaneous system utilities for Linux"
arch=('x86_64')
url="https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git/"
license=('BSD-2-Clause'
    'BSD-3-Clause'
    'BSD-4-Clause-UC'
    'GPL-2.0-only'
    'GPL-2.0-or-later'
    'GPL-3.0-or-later'
    'ISC'
    'LGPL-2.1-or-later'
    'LicenseRef-PublicDomain')
groups=('base')
makedepends=(
    'bzip2'
    'coreutils'
    'file'
    'gcc-libs'
    'glibc'
    'libcap'
    'libxcrypt'
    'ncurses'
    'readline'
    'shadow'
    'python'
    'systemd'
    'xz'
    'zlib'
    'zstd'
)
options=('strip')
source=(https://www.kernel.org/pub/linux/utils/util-linux/v${pkgver%.*}/${pkgbase}-${pkgver}.tar.xz)
sha256sums=(be9ad9a276f4305ab7dd2f5225c8be1ff54352f565ff4dede9628c1aaa7dec57)

build() {
    cd ${pkgbase}-${pkgver}

    local configure_args=(
        --runstatedir=/run
        --bindir=/usr/bin
        --sbindir=/usr/sbin
        --disable-chfn-chsh
        --disable-login
        --disable-nologin
        --disable-su
        --disable-setpriv
        --disable-runuser
        --disable-pylibmount
        --disable-liblastlog2
        --disable-static
        --without-python
        --without-selinux
        ADJTIME_PATH=/var/lib/hwclock/adjtime
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        ${configure_options}
    )

    ./configure "${configure_args[@]}"

    make
}

package_util-linux() {
    depends=(
        'bzip2'
        'coreutils'
        'file'
        'gcc-libs'
        'glibc'
        'libcap'
        'libxcrypt'
        'ncurses'
        'readline'
        'shadow'
        'systemd-libs'
        "util-linux-libs=${pkgver}"
        'xz'
        'zlib'
        'zstd'
    )
    cd ${pkgbase}-${pkgver}

    make DESTDIR=${pkgdir} install

    install -vdm755 ${pkgdir}/var/lib/hwclock

    install -vdm755 ${pkgdir}/etc
    cat > ${pkgdir}/etc/adjtime << "EOF"
0.0 0 0.0
0
LOCAL
EOF

    _pick util-linux-libs ${pkgdir}/usr/lib64
    _pick util-linux-libs ${pkgdir}/usr/include
    _pick util-linux-libs ${pkgdir}/usr/share/man/man3
}

package_util-linux-libs() {
    pkgdesc="util-linux runtime libraries"
    depends=('glibc')

    mv ${pkgname}/* ${pkgdir}
}
