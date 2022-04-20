# Current t2linux Maintainer: Noa Himesaka <himesaka@noa.codes>
# EndeavourOS Maintainer: EndeavourOS <info@endeavouros.com>

pkgname=calamares_config_ce_t2
pkgver=22.04.1.4
pkgrel=2
_repo_name=EndeavourOS-calamares-t2
pkgdesc='EndeavourOS calamares configuration files for Community Editions for Apple devices with the T2 security chip'
arch=('any')
url='https://www.endeavouros.com'
license=('GPL3')
source=("https://github.com/t2linux/${_repo_name}/archive/refs/tags/${pkgver}-T2.tar.gz")

sha512sums=('SKIP')


_caldir="$pkgname/calamares"

prepare() {
    mkdir -p "$_caldir/modules"
    mv "$_repo_name-$pkgver-T2"            "$_repo_name"

    cp "${_repo_name}/calamares/modules/netinstall-ce-base.yaml"            "$_caldir/modules/"
    cp "${_repo_name}/calamares/modules/packagechooser_ce.conf"             "$_caldir/modules/"
    cp "${_repo_name}/calamares/modules/netinstall_community_base.conf"     "$_caldir/modules/"
    cp "${_repo_name}/calamares/settings_community.conf"                    "$_caldir/"
    cp -R "${_repo_name}/calamares/images-ce"                               "$_caldir/"
}

package() {
    install -dm 755 "$pkgdir/etc/calamares"
    cp -r --no-preserve=ownership "$_caldir" "$pkgdir/etc/"
}