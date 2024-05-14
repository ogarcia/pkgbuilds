# Maintainer: Radu C. Martin <radu dot c dot martin at gmail dot com>

pkgname=textpieces
pkgver=4.0.3_1
pkgrel=1
pkgdesc="Transform text without using random websites"
arch=('x86_64' 'aarch64')
url="https://gitlab.com/liferooter/textpieces"
license=('GPL3')
depends=('libadwaita' 'libportal-gtk4' 'gtksourceview5' 'json-glib' 'libgee' 'python-pyaml')
makedepends=('blueprint-compiler' 'meson' 'vala')
checkdepends=('appstream-glib')
source=($url/-/archive/v${pkgver/_/-}/$pkgname-v${pkgver/_/-}.tar.gz)
b2sums=('9af945cb50bf344db698b95dcfd2af62f745627f831d50bd1572e256a785b26d55e5a6145e88f8f9df79dd852c18237eddb5dd9f9d7df7668dcb134e42d5070a')

build() {
  arch-meson $pkgname-v${pkgver/_/-} build
  meson compile -C build
}

check() {
  meson test -C build --print-errorlogs || :
}

package() {
  meson install -C build --destdir "$pkgdir"
}
