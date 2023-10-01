# Maintainer: My name <myemail at domain dot me>

_pkgname=Logseq
pkgname=logseq-desktop-appimage-nightly
pkgver=0.9.18r20230928
pkgrel=1
pkgdesc="Description of my project"
arch=('x86_64')
url="https://github.com/logseq/logseq/"
license=('AGPL3')
provides=("logseq-desktop")
conflicts=("logseq-desktop")
depends=()
options=(!strip)
_appimage="${_pkgname}-${pkgver/_/-alpha+nightly.}.AppImage"
source_x86_64=("${_appimage}::https://github.com/logseq/logseq/releases/download/nightly/Logseq-linux-x64-${pkgver/r/-alpha+nightly.}.AppImage"
               "https://github.com/logseq/logseq/raw/nightly/LICENSE.md"
              )
noextract=("${_appimage}")
sha256sums_x86_64=("SKIP"
                   "SKIP")

pkgver() {
  # export GITHUB_TOKEN=your-personal-github-token before running this update
 curl --url 'https://api.github.com/repos/logseq/logseq/releases' --request GET -H "Authorization: Bearer ${GITHUB_TOKEN}" | rg nightly | rg AppImage | rg name | grep -o '[-0-9a-zA-Z%.+]*' | sed 's/.*-x64-//' | rg nightly | sed 's/\.AppImage.*//' | sed 's/-alpha+nightly./r/g'
}


build() {
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract

    # Adjust .desktop so it will work outside of AppImage container
    # sed -i -E "s|Exec=|Exec=env DESKTOPINTEGRATION=false |" "squashfs-root/Logseq.desktop"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {

    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" "${pkgdir}/opt/${pkgname}/${pkgname}.AppImage"
    install -Dm644 "${srcdir}/LICENSE.md" "${pkgdir}/opt/${pkgname}/LICENSE.md"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/Logseq.desktop" "${pkgdir}/usr/share/applications/${_pkgname}.desktop"

    # Icon images
    install -Dm644 "${srcdir}/squashfs-root/resources/app/icons/logseq.png" -t "${pkgdir}/usr/share/pixmaps/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}.AppImage" "${pkgdir}/usr/bin/${_pkgname}"

    # Symlink license
    install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}/"
    ln -s "/opt/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname"
}
