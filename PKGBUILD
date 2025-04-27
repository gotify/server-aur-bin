# Maintainer: buckket <felix@buckket.org>

_pkgname=gotify-server
pkgname=${_pkgname}-bin
pkgver=2.6.3
pkgrel=1
pkgdesc='A simple server for sending and receiving messages'
arch=('x86_64' 'i686' 'armv7h' 'aarch64')
url='https://github.com/gotify/server'
depends=('glibc')
license=('MIT')
provides=('gotify-server')
conflicts=('gotify-server')
backup=('etc/gotify/config.yml')
source=("https://raw.githubusercontent.com/gotify/server/v${pkgver}/config.example.yml"
        'config.patch'
        'tmpfiles.conf'
        'sysusers.conf'
        'gotify-server.service')

declare -xA _arches
_arches['x86_64']=amd64
_arches['i686']=386
_arches['armv7h']=arm-7
_arches['aarch64']=arm64

# Add sources for the supported architectures.
for key in "${!_arches[@]}"
do
  declare -n source_x="source_${key}"
  source_x=("gotify-linux-${_arches[$key]}-${pkgver}.zip::$url/releases/download/v${pkgver}/gotify-linux-${_arches[$key]}.zip")
done

sha256sums=('2fa745496bf234879d18b417f10911ac145be5ed13a5379e38e2b50a650f5fc0'
            'c251cc61b80968f606df9f9840519456fa5ba5f35ced44f73fda49059ed650b6'
            'e09a6bcb2bbc1d8d28ec21c86cef260a42a93650b13bd82f59cfc0e3838a3c66'
            '150a84f2f89d70c147cc3a2dbddb469f262ed2e8b6d3ffcb74eccb49dfdb2a24'
            '8c3832004ed6f46e01ab69c993773da19b50a45862354165ed065bf3d2147b92')
sha256sums_x86_64=('14e4ba3c8e50c988ad350be90d1fa8ea60b34ca827935123373e47faf38de3c3')
sha256sums_i686=('5b761717d324f49360735534d8f2609051b4a3afe0f6769dfbddf77c28ab02ba')
sha256sums_armv7h=('38b2cfb28f5985da258a7373310585122584460c2e411bd8356d0d8d1179ccc3')
sha256sums_aarch64=('c4fa97b18508b47fc13be180ac194e7b7699bdb56976831206af252cda28b1c1')

prepare() {
  patch --follow-symlinks --forward -o "$srcdir/config.yml" config.example.yml config.patch
}

# The _arches associative array is not visible inside package(). I don't know
# why. If someone does, please post a solution in the comments.
_exe_name=gotify-linux-${_arches[$CARCH]}
package() {
  install -Dm755 "${_exe_name}" "$pkgdir/usr/bin/$_pkgname"
  install -Dm644 "$srcdir/config.yml" "$pkgdir/etc/gotify/config.yml"
  install -Dm644 gotify-server.service "$pkgdir/usr/lib/systemd/system/$_pkgname.service"
  install -Dm644 tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/$_pkgname.conf"
  install -Dm644 sysusers.conf "$pkgdir/usr/lib/sysusers.d/$_pkgname.conf"
}
