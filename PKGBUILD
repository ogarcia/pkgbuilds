# Maintainer: "Amhairghin" Oscar Garcia Amor (https://ogarcia.me)
_themes=(standard black blue brown green grey orange pink purple red yellow manjaro ubuntu dracula nord)

_pkgbase='tela-circle-icon-theme'
pkgbase="${_pkgbase}-spl-git"
_pkgname=("${_themes[@]/#/${_pkgbase}-}")
pkgname=(${_pkgbase}-all-git ${_pkgname[@]/%/-git})
pkgdesc='A flat colorful design icon theme'
pkgver=2022.11.06.r26.g1aac9cf5
pkgrel=1
url="https://github.com/vinceliuice/${_pkgbase^}"
arch=('any')
license=('GPL3')
makedepends=('bash' 'git')
depends=('gtk-update-icon-cache' 'hicolor-icon-theme')
provides=("${_pkgbase}" "${_pkgbase}-git")
options=(!strip)
source=("${_pkgbase}::git+${url}.git")
b2sums=('SKIP')

pkgver() {
  cd "${_pkgbase}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

package_tela-circle-icon-theme-all-git() {
  pkgdesc="${pkgdesc} (all variants)"
  conflicts=(${pkgname[@]/${pkgbase%-git}-all-git} "${pkgname[@]/%-git/}")

  cd "${_pkgbase}"
  install -dm755 "${pkgdir}/usr/share/icons"
  ./install.sh -a -d "${pkgdir}/usr/share/icons"
}

_package() {
  pkgdesc="${pkgdesc} (${1} variant)"
  conflicts=("${_pkgbase}-all-git" "${_pkgbase}-all" "${_pkgbase}-${1}")

  cd "${_pkgbase}"
  install -dm755 "${pkgdir}/usr/share/icons"
  ./install.sh -d "${pkgdir}/usr/share/icons" "${1}"
}

for _theme in "${_themes[@]}"; do
  eval "package_${_pkgbase}-${_theme}-git() {
    _package ${_theme}
  }"
done
