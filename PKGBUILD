# Maintainer: Noa Himesaka <himesaka@noa.codes>
# On behalf of t2linux
# Original Maintainer: Portergos Linux <portergoslinux at gmail.com>
# EndeavourOS original Maintainer : EndeavourOS <info at endeavouros.com>
# Calamares installer configured for EndeavourOS for Macs with T2 security chip

pkgname=calamares-eos-t2
pkgver=24.04.1.3
pkgrel=1
release_name="Gemini-T2"
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

sha256sums=('716cf93932d5141f65d56261679554a4c7a40614ea92a0e298e5dc9d6d687695')

prepare() {
    # Update branding.desc with the proper values
    replace_command='
    {
        gsub(/\${version}/,version);
        gsub(/\${release_name}/,release);
        print
    }
    '
    awk -i inplace -v version="${pkgver}-t2" -v release="${release_name}" "$replace_command" "${srcdir}/calamares-${pkgver}-t2/data/eos/branding/endeavouros/branding.desc"
}

build() {
    cmake -B build -S "${srcdir}/calamares-${pkgver}-t2" \
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
    cp -rp "${srcdir}/calamares-${pkgver}-t2/data/eos" "${pkgdir}/etc/calamares"
}

