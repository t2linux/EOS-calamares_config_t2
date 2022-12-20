# Original Maintainer: Portergos Linux <portergoslinux@gmail.com>
# Maintainer : EndeavourOS <info@endeavouros.com>
# Calamares installer configured for EndeavourOS
#
# Configurations are in extra packages:
# calamares_config_default - default settings
# calamares_config_ce - Community Edition settings

pkgname=calamares_current-t2
_eos_changes=EndeavourOS-calamares-t2
_reponame=calamares
_calamares_ver=3.2.61
pkgver=22.09.2.3
pkgrel=1
pkgdesc="Calamares installer for EndeavourOS for Macs with T2 security chip"
arch=('x86_64')
url="https://t2linux.org"
license=('GPL3')
optdepends=('calamares_config_default' 'calamares_config_ce')
makedepends=('git' 'cmake' 'extra-cmake-modules' 'kpmcore' 'boost' 'python-jsonschema' 'python-pyaml' 'python-unidecode')
conflicts=('calamares-git')
depends=( 'qt5-svg' 'qt5-webengine' 'yaml-cpp' 'networkmanager' 'upower' 'kcoreaddons' 'kconfig' 'ki18n' 'kservice' \
'kwidgetsaddons' 'kpmcore' 'squashfs-tools' 'rsync' 'cryptsetup' 'qt5-xmlpatterns' 'doxygen' 'dmidecode' \
'gptfdisk' 'hwinfo' 'kparts' 'polkit-qt5' 'python' 'solid' 'qt5-tools' 'boost-libs' 'libpwquality' 'ckbcomp' 'mkinitcpio-openswap')
provides=("calamares")
options=(!strip !emptydirs)
source=(
  "https://github.com/t2linux/$_eos_changes/archive/refs/tags/${pkgver}-t2.tar.gz"
  "https://github.com/calamares/calamares/releases/download/v$_calamares_ver/${_reponame}-${_calamares_ver}.tar.gz"
  "calamares.desktop.patch"
)

sha256sums=('76c49bca26ca27eccf7b51e4e33c4544daf6c4ebed8e907431d8043f6006ba66'
            '7591b9b60738bdba7b9de2b8da5462ab21006db06a006f0dd21ac5b832711dd2'
            '66f7b90ab3bc32103d2c3485ee32a4978b57aea466473cfbd06fcb93058d0df8')

prepare() {
    # remove upstream modules packagechooser and netinstall for replacing with testing ones
    rm -r "${_reponame}-${_calamares_ver}/src/modules/"{fstab,mount,packages}

    # move extra modules from external repo inside calamares sources
    rsync -va "${_eos_changes}-${pkgver}/calamares-extra-modules/"*       "${_reponame}-${_calamares_ver}/"
    rm -rf "${_eos_changes}-${pkgver}"

    mkdir -p "${_reponame}-${_calamares_ver}/build/$pkgname"

    # remove default branding // keep package small
    rm -r "${_reponame}-${_calamares_ver}/src/branding/default"

    # change some files on the go - distro-specific
    sed -i "s?configuration files\" OFF?configuration files\" ON?g" "${_reponame}-${_calamares_ver}/CMakeLists.txt"
    patch "${_reponame}-${_calamares_ver}/calamares.desktop" < calamares.desktop.patch

    # remove hardcoded quiet from grubcfg
    sed -i "s?    kernel_params = \[\"quiet\"\]?    kernel_params = \[\]?" "${_reponame}-${_calamares_ver}/src/modules/grubcfg/main.py"
}

build() {
    cd "${_reponame}-${_calamares_ver}/build"
    cmake .. \
    -DCMAKE_BUILD_TYPE=Debug  \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DBoost_NO_BOOST_CMAKE=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINSTALL_CONFIG=OFF \
    -DWEBVIEW_FORCE_WEBKIT=OFF \
    -DWITH_PYTHONQT=OFF \
    -DWITH_KF5DBus=OFF \
    -DWITH_APPSTREAM=OFF \
    cmake .. \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DBoost_NO_BOOST_CMAKE=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINSTALL_CONFIG=OFF \
    -DSKIP_MODULES="dracut dracutlukscfg \
    dummycpp dummyprocess dummypython dummypythonqt \
    finishedq keyboardq license localeq notesqml oemid \
    openrcdmcryptcfg packagechooserq \
    plymouthcfg plasmalnf services-openrc \
    summaryq tracking usersq webview welcomeq"
    export DESTDIR="$srcdir/${_reponame}-${_calamares_ver}/build/$pkgname"
    make install
}

package() {
    local destdir="/usr"

    # Commom install -D doesn't work
    cp -r "${srcdir}/${_reponame}-${_calamares_ver}/build/$pkgname/"* "${pkgdir}${destdir}"

}
