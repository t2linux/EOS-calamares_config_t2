# Maintainer: Noa Himesaka <himesaka@noa.codes>
# On behalf of t2linux
# Original Maintainer: Portergos Linux <portergoslinux at gmail.com>
# EndeavourOS original Maintainer : EndeavourOS <info at endeavouros.com>
# Calamares installer configured for EndeavourOS for Macs with T2 security chip

pkgname=calamares-eos-t2
pkgver=23.11.2.4
pkgrel=1
release_name="Galileo Neo-T2"
pkgdesc="Calamares installer for EndeavourOS for Macs with T2 security chip"
arch=('any')
url="https://github.com/t2linux/calamares-eos-t2"
license=('GPL3')
makedepends=('git' 'cmake' 'extra-cmake-modules' 'kpmcore' 'boost' 'python-jsonschema' 'python-pyaml' 'python-unidecode' 'gawk')
conflicts=('calamares_current')
depends=( 'qt5-svg' 'qt5-webengine' 'yaml-cpp' 'networkmanager' 'upower' 'kcoreaddons5' 'kconfig5' 'ki18n5' 'kservice5' \
'kwidgetsaddons5' 'kpmcore' 'squashfs-tools' 'rsync' 'cryptsetup' 'qt5-xmlpatterns' 'doxygen' 'dmidecode' \
'gptfdisk' 'hwinfo' 'kparts5' 'polkit-qt5' 'python' 'solid5' 'qt5-tools' 'boost-libs' 'libpwquality' 'ckbcomp' 'qt5-quickcontrols2' )
provides=("calamares")
options=(!strip !emptydirs)
source=("https://github.com/t2linux/${pkgname}/archive/refs/tags/${pkgver}-t2.tar.gz")

sha256sums=('SKIP')

prepare() {
    # Update branding.desc with the proper values
    replace_command='
    {
        gsub(/\${version}/,version);
        gsub(/\${release_name}/,release);
        print
    }
    '
    awk -i inplace -v version="${pkgver}" -v release="${release_name}" "$replace_command" "${srcdir}/calamares-eos-t2-${pkgver}-t2/data/eos/branding/endeavouros/branding.desc"
}

build() {
    cmake -B build -S "${srcdir}/calamares-eos-t2-${pkgver}-t2" \
    -DWEBVIEW_FORCE_WEBKIT=OFF \
    -DWITH_PYTHONQT=OFF \
    -DWITH_KF5DBus=OFF \
    -DWITH_APPSTREAM=OFF \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DBoost_NO_BOOST_CMAKE=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
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
