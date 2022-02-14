# Maintainer: Noa Himesaka <himesaka@noa.codes>
pkgname=calamares_config_t2
pkgver=1.0
pkgrel=31
pkgdesc='EndeavourOS calamares configuration files and branding for Apple T2-based devices'
arch=('any')
url='https://www.endeavouros.com'
license=('GPL3')
source=(
    git+https://github.com/t2linux/EndeavourOS-calamares-t2.git#branch=Atlantis_neo
)

sha512sums=('SKIP')

_date=$(date +%Y.%m.%d)

prepare() {
    # set date for calamares_branding
    sed -i $srcdir/EndeavourOS-calamares-t2/calamares/branding/endeavouros/branding.desc \
    -e "s|^\(    version:[ ]*\).*$|\1$_date|" \
    -e "s|^\(    shortVersion:[ ]*\).*$|\1$_date|"

    chmod +x $srcdir/EndeavourOS-calamares-t2/calamares/scripts/{cleaner_script.sh,chrooted_cleaner_script.sh,update-mirrorlist,pacstrap_calamares}

    cp $srcdir/EndeavourOS-calamares-t2/calamares/files/netinstall.yaml  $srcdir/EndeavourOS-calamares-t2/calamares/modules/
    rm $srcdir/EndeavourOS-calamares-t2/calamares/settings_community.conf
    rm $srcdir/EndeavourOS-calamares-t2/calamares/modules/{contextualprocess.conf,packagechooser.conf,netinstall_community-base.conf,shellprocess_plist.conf}
    rm -R $srcdir/EndeavourOS-calamares-t2/calamares/{ce,images}
}

package() {
    install -dm 755 $pkgdir/etc/calamares
    cp -r --no-preserve=ownership $srcdir/EndeavourOS-calamares-t2/calamares $pkgdir/etc/

    rm -rf ../EndeavourOS-calamares-t2    # cleanup
}
