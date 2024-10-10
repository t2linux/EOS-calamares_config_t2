# Maintainer: Noa Himesaka <himesaka@noa.codes>
# On behalf of t2linux
# Original Maintainer: Portergos Linux <portergoslinux at gmail.com>
# EndeavourOS original Maintainer : EndeavourOS <info at endeavouros.com>
# Calamares installer configured for EndeavourOS for Macs with T2 security chip

pkgname=calamares-eos-t2
pkgver=24.06.2.1
pkgrel=2
release_name="Endeavour-T2"
pkgdesc="Calamares installer for EndeavourOS for Macs with T2 security chip"
arch=('any')
url="https://github.com/t2linux/calamares-eos-t2"
license=('GPL3')
makedepends=('git' 'cmake' 'extra-cmake-modules' 'python-jsonschema' 'python-pyaml' 'python-unidecode' 'gawk')
conflicts=('calamares-git')
depends=( 'qt6-svg' 'qt6-webengine' 'yaml-cpp' 'networkmanager' 'upower' 'kcoreaddons' 'kconfig' 'ki18n' 'kservice' \
'kwidgetsaddons' 'kpmcore' 'squashfs-tools' 'rsync' 'pybind11' 'cryptsetup' 'doxygen' 'dmidecode' \
'gptfdisk' 'hwinfo' 'kparts' 'polkit-qt6' 'python' 'solid' 'qt6-tools' 'libpwquality' 'ckbcomp' 'qt6-declarative' )
provides=("calamares")
options=(!strip !emptydirs)
source=("https://github.com/t2linux/${pkgname}/archive/refs/tags/${pkgver}-t2.tar.gz")

sha256sums=('b51f3f18064db15f0cd4a5b24d292d34c2425ab8e58dbf67e11ee7e3919d063f')

prepare() {
    # Update branding.desc with the proper values
    replace_command='
    {
        gsub(/\${version}/,version);
        gsub(/\${release_name}/,release);
        print
    }
    '
    awk -i inplace -v version="${pkgver}-t2" -v release="${release_name}" "$replace_command" "${srcdir}/calamares-eos-t2-${pkgver}-t2/data/eos/branding/endeavouros/branding.desc"
}

build() {
    cmake -B build -S "${srcdir}/calamares-eos-t2-${pkgver}-t2" \
    -DWITH_QT6=ON \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINSTALL_CONFIG=OFF \
    -DSKIP_MODULES="dracut \
    dummycpp dummyprocess dummypython dummypythonqt \
    finishedq initcpio keyboardq license localeq notesqml oemid \
    openrcdmcryptcfg plymouthcfg plasmalnf services-openrc \
    summaryq tracking usersq webview welcomeq"
    export DESTDIR="$srcdir/build"
    make -C build
}

package() {
    make -C build DESTDIR="${pkgdir}" install
    install -dm 755 "${pkgdir}/etc"
    cp -rp "${srcdir}/calamares-eos-t2-${pkgver}-t2/data/eos" "${pkgdir}/etc/calamares"
}

