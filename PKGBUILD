# Maintainer: Jianfeng Zhang <swordfeng123@gmail.com>

pkgname=poi
_pkgname=poi
pkgver=10.3.0.0.g9c7f0982
pkgrel=2
pkgdesc="Scalable KanColle browser and tool"
arch=('any')
url="https://github.com/poooi/poi/"
license=('MIT')
depends=('npm' 'sh' 'electron')
makedepends=('git' 'nodejs' 'coreutils' 'findutils' 'sed' 'imagemagick' 'tar' 'zlib' 'unzip' 'gulp' 'npm' 'python2')
provides=("${_pkgname}")
conflicts=("${_pkgname}")
source=("git+https://github.com/poooi/poi.git"
        "${_pkgname}.desktop"
        "${_pkgname}.sh")
sha256sums=('SKIP'
            '24f89c538a189a5db96be3e3228aba6e4e7d332c5a368b15dacb6e97ee6f7586'
            'f6c559d512379263b77a043e668eb846c53f5487fc932c1003f08c7dc1ad14e2')
options=('!strip') # nothing to strip


prepare() {
    cd ${srcdir}/${pkgname}
    git checkout -f $(git describe --tags $(git rev-list --tags --max-count=1))
}

pkgver() {
    cd "${srcdir}/${_pkgname}"
    git describe --tags --long | sed 's/^v//;s/-/./g'
}

build() {
    cd "${srcdir}/${_pkgname}"
    git clean -xdf
    npm install
    gulp build
}

package() {
    mkdir -p "${pkgdir}/usr/share"
    cp -r "${srcdir}/${_pkgname}/app_compiled" "${pkgdir}/usr/share/poi"
    # workaround for strange behavior of babel
    mkdir -p "${pkgdir}/usr/share/poi/node_modules/.cache/@babel/register"

    cd "${srcdir}"
    install -Dm644 "${_pkgname}.desktop" "${pkgdir}/usr/share/applications/${_pkgname}.desktop"

    for s in 16 24 32 48 64 96 128 512; do
        mkdir -p "${pkgdir}/usr/share/icons/hicolor/${s}x${s}/apps"
        convert "${_pkgname}/assets/icons/poi.png" -resize ${s}x${s} "${pkgdir}/usr/share/icons/hicolor/${s}x${s}/apps/${_pkgname}.png"
    done

    mkdir -p "${pkgdir}/usr/share/licenses/${_pkgname}"
    ln -s "../../${_pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"

    mkdir -p "${pkgdir}/usr/bin"
    cp "${_pkgname}.sh" "${pkgdir}/usr/bin/${_pkgname}"
    chmod 0755 "${pkgdir}/usr/bin/${_pkgname}"
}
