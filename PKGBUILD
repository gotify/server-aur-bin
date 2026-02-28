# Maintainer: buckket <felix@buckket.org>

_pkgname=gotify-server
pkgname=${_pkgname}-bin
pkgver=2.9.1
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

sha256sums=('7d7992f1b5a2ef5b331817402fe4f54f8aedc0de168d65735bf16c3cade90db6'
            '7d095eb7c7317ad90f1f2da321bea2b1f5540212447dbdeeab384ed003bf3c35'
            'e09a6bcb2bbc1d8d28ec21c86cef260a42a93650b13bd82f59cfc0e3838a3c66'
            '150a84f2f89d70c147cc3a2dbddb469f262ed2e8b6d3ffcb74eccb49dfdb2a24'
            '8c3832004ed6f46e01ab69c993773da19b50a45862354165ed065bf3d2147b92')
sha256sums_x86_64=('34707b68f6e2b1be69458a7974e9d0e1acbfaa7a95d7349eed9219c9f215c4b3')
sha256sums_i686=('1f0ee4c1aaaf477e37ac9d582f67a445e4d8cc79cd6cfbb984bea85906437ebc')
sha256sums_armv7h=('1f8128ca89e45da5cc321160786a6094cba648b0a36f95686d0880e0734d3f3c')
sha256sums_aarch64=('f8c16cec664d59033e1588c766b3ef45d5c7c39bb995ca6deba298cd2b1db268')

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
