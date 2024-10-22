# Maintainer: copycat <simakr2512 | at | yandex [DOT] ru>
pkgname=('kesl-gui')
pkgver=12.1.0.1508
_pkgverbuild=$(echo ${pkgver} | cut -d "." -f 4)
_pkgver=$(echo ${pkgver} | cut -d "." -f 1-3)
pkgrel=1
arch=('x86_64')
pkgdesc='Kaspersky Endpoint Security 12.1.0 for Linux (GUI)'
url='https://support.kaspersky.com/help/KES4Linux/12.1.0/en-US/264264.htm'
license=('custom')
noextract=("kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb")
depends=('perl' 'kesl' 'freetype2')
options=("!strip")
install=${pkgname}.install
changelog=${pkgname}.changelog

#https://www.kaspersky.com/small-to-medium-business-security/downloads/endpoint?utm_content=downloads
#They always change that "3837323739337c44454c7c31" thing so there is no point of generating download link based on pkg version
source=( "https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.1.0.1508/multilanguage-12.1.0.1508/3931313439377c44454c7c31/kesl-gui_12.1.0-1508_amd64.deb"
         "${pkgname}.install")

sha256sums=('51e0cd6dee3f9f7371811b38b1d3b054ebd1908fda2f5c0bb75f51c8a9f98777'
            '6eb8fdafdd0811ed25d7b36541f8214e1c3b4f9989d8e1df448311535827121a')

validpgpkeys=('6AFE173577C4CBD621DF217FD093435AA3ED2C4A')

package_kesl-gui(){
    KESLIDIR=${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}

    bsdtar -xf kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb
    bsdtar -xf data.tar.xz -C ${pkgdir}/

    [ ! -d "${pkgdir}/var/opt/kaspersky/kesl-gui/pkgscripts" ] && mkdir -p ${pkgdir}/var/opt/kaspersky/kesl-gui/pkgscripts
    bsdtar -xf control.tar.gz -C ${pkgdir}/var/opt/kaspersky/kesl-gui/pkgscripts

    chmod 711 ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/shared/loc/

    chmod 511 ${pkgdir}/var/opt/kaspersky/kesl/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver} \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/bin/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/lib64/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/libexec/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/shared/ \
        ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/shared/init/storage/

    chmod 500 ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/shared/init/

    #plasma 6 stores service menu in different location
    mkdir -p ${pkgdir}/usr/share/kio/servicemenus/
    mv ${pkgdir}/usr/share/kservices5/ServiceMenus/kaspersky-kesl.desktop ${pkgdir}/usr/share/kio/servicemenus/kaspersky-kesl.desktop
    rmdir ${pkgdir}/usr/share/kservices5/ServiceMenus
    rmdir ${pkgdir}/usr/share/kservices5
}


