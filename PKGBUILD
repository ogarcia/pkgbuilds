# Maintainer: "Amhairghin" Oscar Garcia Amor (https://ogarcia.me)

_themes=(standard black blue brown green grey orange pink purple red yellow manjaro ubuntu dracula nord)

pkgbase=tela-circle-icon-theme
pkgname=(${pkgbase}-all ${_themes[@]/#/${pkgbase}-})
pkgdesc='A flat colorful design icon theme'
pkgver=2024.04.19
pkgrel=1
url="https://github.com/vinceliuice/${pkgbase^}"
arch=('any')
license=('GPL3')
makedepends=('bash')
depends=('gtk-update-icon-cache' 'hicolor-icon-theme')
provides=("${pkgbase}")
options=(!strip)
source=("${pkgbase}-${pkgver}.tar.gz::${url}/archive/${pkgver//./-}.tar.gz")
b2sums=('5bfafee284d0efaee5088adbdb5f6f4e93345ae031679f9b71e0b996472700225b26407246ddf8c74f9e92e74b311691233baf4fd33c5e005dc88f017c52b46c')

package_tela-circle-icon-theme-all() {
  pkgdesc="${pkgdesc} (all variants)"
  conflicts=(${pkgname[@]/${pkgbase}-all} "${pkgname[@]/%/-git}")

  cd "${pkgbase^}-${pkgver//./-}"
  install -dm755 "${pkgdir}/usr/share/icons"
  ./install.sh -a -d "${pkgdir}/usr/share/icons"
}

_package() {
  pkgdesc="${pkgdesc} (${1} variant)"
  conflicts=("${pkgbase}-all" "${pkgbase}-all-git" "${pkgbase}-${1}-git")

  cd "${pkgbase^}-${pkgver//./-}"
  install -dm755 "${pkgdir}/usr/share/icons"
  ./install.sh -d "${pkgdir}/usr/share/icons" "${1}"
}

for _theme in "${_themes[@]}"; do
  eval "package_${pkgbase}-${_theme}() {
    _package ${_theme}
  }"
done
