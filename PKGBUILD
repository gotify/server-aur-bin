# Maintainer: buckket <felix@buckket.org>

_pkgname=gotify-server
pkgname=${_pkgname}-bin
pkgver=2.7.1
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
sha256sums_x86_64=('8e72c9bd1a68abdbbd763326c3206bf3fa1265f42580a17cf6ca2de0fb6d5abd')
sha256sums_i686=('288d5ba11f2c356b8873db177938a04bc606698133bbe47b21e3fce7ef351f97')
sha256sums_armv7h=('4ebb3d3a910c9ad6866d5e29aa14a37277ec995c9c7b041dc7f93c6161102fec')
sha256sums_aarch64=('32c52f3c255ec950c0b322f622c5c41de0ac42e6bc7c1c16212af4e1a6b1c6dd')

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
