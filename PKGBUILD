# Maintainer: Zoddo <archlinux+aur@zoddo.fr>
# Contributor: Thaodan <AUR+me@thaodan.de>
# Contributor: huyizheng
# Contributor: johnnyapol <arch@johnnyapol.me>
# Contributor: Hayden <hayden@hbjy.dev>
# Based off the discord community repo PKGBUILD by Filipe Laíns (FFY00) <lains@archlinux.org>
_pkgname=discord
_electron=electron
pkgname=${_pkgname}_arch_electron
pkgver=0.0.28
pkgrel=1
pkgdesc="Discord (popular voice + video app) using the system provided electron for increased security and performance"
arch=('x86_64')
provides=("${_pkgname}")
conflicts=("${_pkgname}")
url='https://discord.com'
license=('custom')
options=(!strip)
depends=("${_electron}" 'libxss')
makedepends=('asar')
optdepends=('libpulse: Pulseaudio support'
            'xdg-utils: Open files')
source=("https://dl.discordapp.net/apps/linux/$pkgver/$_pkgname-$pkgver.tar.gz"
        'discord-launcher.sh'
        'LICENSE.html::https://discord.com/terms'
        'OSS-LICENSES.html::https://discord.com/licenses')
sha512sums=('763fe47a0fb21a13e852bcc818d4e0e2ea4faf23fcfdc02fddfe06e8c829499e028e27b45d807d3b3edcc36788990f3f21c0460b9b8efc538b62f3b41aeb744d'
            '6ca6dfbfb65bf4fec34aac4676f66bb602b5c4c3318fcc96236056d632c0c9af3c4eb775b491c2e722ed5de6a4c253677d6ee1a7be69e13045702fa3df8cf52f'
            SKIP
            SKIP)

prepare() {
  sed -i "s|@PKGNAME@|${_pkgname}|;s|@ELECTRON@|${_electron}|" discord-launcher.sh
  sed -i "s|Exec=.*|Exec=/usr/bin/$_pkgname|" Discord/discord.desktop

  # HACKS FOR SYSTEM ELECTRON
  asar e Discord/resources/app.asar Discord/resources/app
  rm Discord/resources/app.asar
  sed -i "s|process.resourcesPath|'/usr/lib/$_pkgname'|" Discord/resources/app/app_bootstrap/buildInfo.js
  sed -i "s|exeDir,|'/usr/share/pixmaps',|" Discord/resources/app/app_bootstrap/autoStart/linux.js
  asar p Discord/resources/app Discord/resources/app.asar --unpack-dir '**'
  rm -rf Discord/resources/app
}

package() {
  # Install the app
  install -d "$pkgdir"/usr/lib/$_pkgname

  # Copy Relevanat data
  cp -r Discord/resources/*  "$pkgdir"/usr/lib/$_pkgname/

  install -d "$pkgdir"/usr/{bin,share/{pixmaps,applications}}
  install -Dm 755 "${srcdir}/discord-launcher.sh" "${pkgdir}/usr/bin/${_pkgname}"

  cp Discord/discord.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
  cp Discord/discord.desktop "$pkgdir"/usr/share/applications/$_pkgname.desktop

  # Licenses
  install -Dm 644 LICENSE.html "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.html
  install -Dm 644 OSS-LICENSES.html "$pkgdir"/usr/share/licenses/$pkgname/OSS-LICENSES.html
}
