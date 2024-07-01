# Maintainer: copycat <simakr2512 | at | yandex [DOT] ru>
pkgname=('kesl-gui')
pkgver=12.0.0.6672
_pkgver_gui=12.0.0.6672
_pkgverbuild=$(echo ${pkgver} | cut -d "." -f 4)
_pkgver=$(echo ${pkgver} | cut -d "." -f 1-3)
pkgrel=2
arch=('x86_64')
pkgdesc='Kaspersky Endpoint Security 12.0.0 for Linux (GUI)'
url='https://support.kaspersky.com/KES4Linux/12.0.0/en-US/245017.htm'
license=('custom')
noextract=("kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb")
depends=('perl' 'kesl' 'freetype2')
options=("!strip")
install=${pkgname}.install
changelog=${pkgname}.changelog

source=( "https://products.s.kaspersky-labs.com/endpoints/keslinux10/${pkgver}/multilanguage-${_pkgver_gui}/3739343633387c44454c7c31/kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb"
         "${pkgname}.install")

sha256sums=('9b066af7d150c599c952e4002ed7b04905be0aba15992a3e2190e5b045332552'
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


