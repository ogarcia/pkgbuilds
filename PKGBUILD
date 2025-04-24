pkgname=mongodb-compass-bin
pkgver=1.46.1
pkgrel=1
pkgdesc="The official GUI for MongoDB - prebuilt binary"
arch=('x86_64')
url="https://www.mongodb.com/products/compass"
license=('custom')
depends=('glibc' 'gtk3' 'libxss' 'nss')
source=("https://downloads.mongodb.com/compass/mongodb-compass-1.46.1.x86_64.rpm")
noextract=("mongodb-compass-1.46.1.x86_64.rpm")
md5sums=('SKIP')

package() {
  cd "$srcdir"
  rpmextract.sh mongodb-compass-1.46.1.x86_64.rpm

  # Move main app files into /opt
  install -d "$pkgdir/opt/mongodb-compass"
  cp -r usr/lib/mongodb-compass/* "$pkgdir/opt/mongodb-compass/"

  # Symlink the binary to /usr/bin
  install -d "$pkgdir/usr/bin"
  ln -s /opt/mongodb-compass/MongoDB\ Compass "$pkgdir/usr/bin/mongodb-compass"

  # Install desktop entry
  install -Dm644 usr/share/applications/mongodb-compass.desktop \
    "$pkgdir/usr/share/applications/mongodb-compass.desktop"

  # Install icon (from pixmaps)
  install -Dm644 usr/share/pixmaps/mongodb-compass.png \
    "$pkgdir/usr/share/pixmaps/mongodb-compass.png"

  # Optionally fix .desktop file path (recommended)
  sed -i 's|Exec=.*|Exec=/usr/bin/mongodb-compass|' "$pkgdir/usr/share/applications/mongodb-compass.desktop"
  sed -i 's|Icon=.*|Icon=mongodb-compass|' "$pkgdir/usr/share/applications/mongodb-compass.desktop"
}

