# Maintainer: TF <mail | at | sedi [DOT] one>
pkgname=('kesl' 'kesl-gui')
pkgver=12.0.0.6672
_pkgver_gui=12.0.0.6672
_pkgverbuild=$(echo ${pkgver} | cut -d "." -f 4)
_pkgver=$(echo ${pkgver} | cut -d "." -f 1-3)
pkgrel=4
arch=('x86_64')
pkgdesc='Kaspersky Endpoint Security 12.0.0 for Linux'
url='https://support.kaspersky.com/KES4Linux/12.0.0/en-US/245017.htm'
license=('custom')
noextract=("kesl_${_pkgver}-${_pkgverbuild}_amd64.deb" "kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb")
depends=('perl')
options=("!strip")
conflicts=( 'eea'
            'esets'
            'eea-dkms'
            'eea7-dkms')
install=${pkgname}.install
changelog=${pkgname}.changelog

source=( "https://products.s.kaspersky-labs.com/endpoints/keslinux10/${pkgver}/multilanguage-${pkgver}/3739343634317c44454c7c31/kesl_${_pkgver}-${_pkgverbuild}_amd64.deb"
         "https://products.s.kaspersky-labs.com/endpoints/keslinux10/${pkgver}/multilanguage-${_pkgver_gui}/3739343633387c44454c7c31/kesl-gui_${_pkgver}-${_pkgverbuild}_amd64.deb"
         "${pkgname}.install"
         "kesl.ini"
         "kesl.start.conf")
sha256sums=('cf124a7dcb0c70dd85503fa250737bc981ff56630a891710dbc579e239d153b2'
            '9b066af7d150c599c952e4002ed7b04905be0aba15992a3e2190e5b045332552'
            '695023473d087c9d641240b16e4a9d2c74c811b4d2c7fb018425170927b7439c'
            '86203f1dcd663763bc9c8d51a98e510523189c7e78a7fb293183095b89bfa6cf'
            '29efcd166bb0fc5baa5a85dc0f41c6c2e253f6b8fd3ee723862496364281cb4c')
validpgpkeys=('6AFE173577C4CBD621DF217FD093435AA3ED2C4A')

package_kesl() {
    # prepare dirs
    mkdir -p ${pkgdir}/usr/src
    mkdir -p ${pkgdir}/etc/init.d

    KESLIDIR=${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}

    # uncompress base packages
    bsdtar -xf kesl_${_pkgver}-${_pkgverbuild}_amd64.deb
    bsdtar -xf data.tar.xz -C ${pkgdir}/

    # install deb post/pre scripts
    [ ! -d "${pkgdir}/var/opt/kaspersky/kesl/pkgscripts" ] && mkdir -p ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts
    bsdtar -xf control.tar.gz -C ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts
    cp kesl.ini ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/

    # /usr/local/share/man is owned by pkg filesystem
    mv ${KESLIDIR}/usr/local/share ${KESLIDIR}/usr/
    chmod 755 -R ${KESLIDIR}/usr/
    rm -rf ${KESLIDIR}/usr/local/ ${pkgdir}/usr/local

    # man pages
    [ ! -d ${KESLIDIR}/usr/local/share/man/man1 ] && mkdir -p ${KESLIDIR}/usr/local/share/man/man1
    for m in $(find ${KESLIDIR}/usr/share/man/man1 -maxdepth 1 -mindepth 1 -type f);do
        ln -s /usr/share/man/man1/${m/*\/} ${KESLIDIR}/usr/local/share/man/man1/${m/*\/}
    done
    [ ! -d ${KESLIDIR}/usr/local/share/man/man5 ] && mkdir -p ${KESLIDIR}/usr/local/share/man/man5
    for m in $(find ${KESLIDIR}/usr/share/man/man5 -maxdepth 1 -mindepth 1 -type f);do
        ln -s /usr/share/man/man5/${m/*\/} ${KESLIDIR}/usr/local/share/man/man5/${m/*\/}
    done

    # install licenses
    for lic in $(find ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/doc/ -maxdepth 1 -mindepth 1 -type f |grep license);do
        install -Dm644 ${lic} "$pkgdir/usr/share/licenses/$pkgname/${lic/*\/}"
    done

    # install startup config
    cp kesl.start.conf ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/
    sed -i "s/@PKGVER@/$pkgver/g" ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/kesl.start.conf
}

package_kesl-gui(){
    pkgdesc='Kaspersky Endpoint Security 12.0.0 for Linux (GUI)'
    depends=('kesl' 'freetype2' 'qt5-svg')
    install=${pkgname}.install

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

    mkdir -p ${pkgdir}/usr/share/kio/servicemenus/
    mv ${pkgdir}/usr/share/kservices5/ServiceMenus/kaspersky-kesl.desktop ${pkgdir}/usr/share/kio/servicemenus/kaspersky-kesl.desktop
    rmdir ${pkgdir}/usr/share/kservices5/ServiceMenus
    rmdir ${pkgdir}/usr/share/kservices5
}


