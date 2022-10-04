# Current t2linux Maintainer: Noa Himeaska <himesaka@noa.codes>
# Original EOS Maintainer: EndeavourOS <info@endeavouros.com>

pkgname=calamares_config_t2
pkgver=22.09.2.3
pkgrel=4
_repo_name=EndeavourOS-calamares-t2
pkgdesc='EndeavourOS calamares configuration files and branding'
arch=('any')
url='https://www.endeavouros.com'
license=('GPL3')
source=("https://github.com/t2linux/${_repo_name}/archive/refs/tags/${pkgver}-${pkgrel}-t2.tar.gz")

sha512sums=('SKIP')


_date=$(date +%Y.%m.%d)

prepare() {
    mv "$_repo_name-$pkgver-$pkgrel-t2"            "$_repo_name"

    # set date for calamares_branding
    sed -i "$srcdir/${_repo_name}/calamares/branding/endeavouros/branding.desc" \
    -e "s|^\(    version:[ ]*\).*$|\1$_date|" \
    -e "s|^\(    shortVersion:[ ]*\).*$|\1$_date|"

    chmod +x "$srcdir/${_repo_name}/calamares/scripts/"{cleaner_script.sh,chrooted_cleaner_script.sh,update-mirrorlist,pacstrap_calamares}

    rm "$srcdir/${_repo_name}/calamares/modules/netinstall-ce-base.yaml"
    rm "$srcdir/${_repo_name}/calamares/settings_community.conf"
    rm "$srcdir/${_repo_name}/calamares/modules/"{packagechooser_ce.conf,netinstall_community_base.conf}
    rm -R "$srcdir/${_repo_name}/calamares/images-ce"
}

package() {
    install -dm 755 "$pkgdir/etc/calamares"
    cp -r --no-preserve=ownership "$srcdir/${_repo_name}/calamares" "$pkgdir/etc/"
}

