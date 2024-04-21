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
provides=("${_pkgbase}")
options=(!strip !debug)
source=("${_pkgbase}::git+${url}.git")
b2sums=('SKIP')

pkgver() {
  cd "${_pkgbase}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

_package() {
  cd "${_pkgbase}"
  install -dm755 "${pkgdir}/usr/share/icons"
  ./install.sh -d "${pkgdir}/usr/share/icons" ${1}
}

package_tela-circle-icon-theme-all-git() {
  pkgdesc="${pkgdesc} (all variants)"
  conflicts=("${_pkgbase}" "${_pkgbase}"-all)

  _package -a
}

for _theme in "${_themes[@]}"; do
  eval "package_${_pkgbase}-${_theme}-git() {
    pkgdesc='${pkgdesc} (${_theme} variant)'
    conflicts=(${_pkgbase}-${_theme})

    _package '${_theme}'
  }"
done
