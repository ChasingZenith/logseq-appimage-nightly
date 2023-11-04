# Maintainer: <tangsquirrel@gmail.com>

_pkgname=Logseq
pkgname=logseq-desktop-appimage-nightly
pkgver=0.9.20r20231102
pkgrel=1
pkgdesc="Description of my project"
arch=('x86_64')
url="https://github.com/logseq/logseq/"
license=('AGPL3')
provides=("logseq-desktop")
conflicts=("logseq-desktop")
depends=(fuse2)
options=(!strip)
_appimage="${_pkgname}-linux-x64-${pkgver/r/-alpha+nightly.}.AppImage"
source_x86_64=("${_appimage}::https://github.com/logseq/logseq/releases/download/nightly/Logseq-linux-x64-${pkgver/r/-alpha+nightly.}.AppImage"
               "https://github.com/logseq/logseq/raw/nightly/LICENSE.md"
              )
noextract=("${_appimage}")
sha256sums_x86_64=("SKIP"
                   "SKIP")

pkgver() {
  # export GITHUB_TOKEN=your-personal-github-token before running this update
 curl --url 'https://api.github.com/repos/logseq/logseq/releases' --request GET -H "Authorization: Bearer ${GITHUB_TOKEN}" | grep nightly | grep AppImage | grep name | grep -o '[-0-9a-zA-Z%.+]*' | sed 's/.*-x64-//' | grep nightly | sed 's/\.AppImage.*//' | sed 's/-alpha+nightly./r/g'
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
    # install -Dm644 "${srcdir}/squashfs-root/resources/app/icons/logseq.png" -t "${pkgdir}/usr/share/pixmaps/"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/ios/icon/icon-20.png"   "${pkgdir}/usr/share/icons/hicolor/20x20/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/android/icon/drawable-mdpi-icon.png"    "${pkgdir}/usr/share/icons/hicolor/48x48/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/android/icon/drawable-hdpi-icon.png"    "${pkgdir}/usr/share/icons/hicolor/72x72/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/android/icon/drawable-ldpi-icon.png"   "${pkgdir}/usr/share/icons/hicolor/36x36/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/android/icon/drawable-xhdpi-icon.png"   "${pkgdir}/usr/share/icons/hicolor/96x96/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/android/icon/drawable-xxxhdpi-icon.png"    "${pkgdir}/usr/share/icons/hicolor/192x192/apps/Logseq.png"
    install -Dm644 "${srcdir}/squashfs-root/resources/app/icons/logseq.png"  "${pkgdir}/usr/share/icons/hicolor/512x512/apps/Logseq.png"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}.AppImage" "${pkgdir}/usr/bin/${_pkgname}"

    # Symlink license
    install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}/"
    ln -s "/opt/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname"
}
