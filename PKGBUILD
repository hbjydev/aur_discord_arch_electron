# Maintained by johnnyapol (arch@johnnyapol.me)
# Contributors: huyizheng
# Based off the discord community repo PKGBUILD by Filipe Laíns (FFY00) <lains@archlinux.org>
pkgname=discord_arch_electron
_pkgname=discord
pkgver=0.0.17
pkgrel=2
pkgdesc="Discord (popular voice + video app) using the system provided electron for increased security and performance"
arch=('x86_64')
provides=('discord')
conflicts=('discord')
url='https://discordapp.com'
license=('custom')
depends=('electron')
makedepends=('asar')
optdepends=('libpulse: Pulseaudio support'
            'xdg-utils: Open files')
source=("https://dl.discordapp.net/apps/linux/$pkgver/$_pkgname-$pkgver.tar.gz"
        'LICENSE.html::https://discordapp.com/terms'
        'OSS-LICENSES.html::https://discordapp.com/licenses')
sha512sums=('728c760f04770b197635d1fc1a2833c707ab462708cc0bf6fb165691c1375e14e3192f3806d3719325790459b39bd3425f5942d69a53019695f02c965214a5c9'
             SKIP
             SKIP)

prepare() {
  cd Discord

  sed -i "s|Exec=.*|Exec=/usr/bin/$_pkgname|" $_pkgname.desktop
  echo 'Path=/usr/bin' >> $_pkgname.desktop
}

package() {
  # Install the app
  install -d "$pkgdir"/usr/lib/$_pkgname

  # HACKS FOR SYSTEM ELECTRON
  asar e Discord/resources/app.asar Discord/resources/app
  rm Discord/resources/app.asar
  sed -i "s|process.resourcesPath|'/usr/lib/$_pkgname'|" Discord/resources/app/app_bootstrap/buildInfo.js
  sed -i "s|exeDir,|'/usr/share/pixmaps',|" Discord/resources/app/app_bootstrap/autoStart/linux.js
  asar p Discord/resources/app Discord/resources/app.asar --unpack-dir '**'
  rm -rf Discord/resources/app

  # Copy Relevanat data
  cp -r Discord/resources/*  "$pkgdir"/usr/lib/$_pkgname/

  # Create starter script for discord
  echo "#!/bin/sh" > "$srcdir"/$_pkgname
  echo "exec electron /usr/lib/$_pkgname/app.asar \$@" >> "$srcdir"/$_pkgname

  install -d "$pkgdir"/usr/{bin,share/{pixmaps,applications}}
  install -Dm 755 $_pkgname "$pkgdir"/usr/bin/$_pkgname

  cp Discord/discord.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
  cp Discord/discord.desktop "$pkgdir"/usr/share/applications/$_pkgname.desktop

  # Licenses
  install -Dm 644 LICENSE.html "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.html
  install -Dm 644 OSS-LICENSES.html "$pkgdir"/usr/share/licenses/$pkgname/OSS-LICENSES.html
}
