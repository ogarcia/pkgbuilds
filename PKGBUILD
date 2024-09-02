# Maintainer: Vital Reichmuth (furbyhaxx) <furbyhaxx@gmail.com>
# Contributor: Vyacheslav Konovalov <🦀vk@protonmail.com>

pkgname=polaris
pkgver=0.14.2
_webver=build-66
pkgrel=1
pkgdesc='Music streaming application, designed to let you enjoy your music collection from any computer or mobile device'
arch=('x86_64')
url='https://github.com/agersant/polaris'
license=('MIT')
depends=('openssl' 'sqlite')
makedepends=('cargo' 'npm' 'sqlite' 'zstd' 'openssl')
backup=('etc/polaris/config.toml')
# disable lto as ring is having isssues with it: https://github.com/briansmith/ring/issues/1444
options=('!lto')
source=(
    "polaris-$pkgver.tar.gz::https://github.com/agersant/polaris/archive/$pkgver.tar.gz"
    "polaris-web-$_webver.tar.gz::https://github.com/agersant/polaris-web/archive/$_webver.tar.gz"
    'config.toml'
    'polaris.tmpfiles'
    'polaris.sysusers'
    'polaris.service'
)
sha512sums=(
    'df455ddcf6bbb13414394f6de2668ad6baa5a780d387851ab6e5675b3ed35e9af64d2db80b7c6b92cf102064a1d531e3c8c00b16269988d5f88e2a99c9019a2d'
    '1965d8d1510bb3ceaec919631f2fce79a198ae19ecd2e26c93276ba175b1df480f99e1ef9815b3140a27071825805fe62738562f7ddba8161084f7b3ddf1e75a'
    '2e4fe41b394508cb6a767a5b5732745d48d08c32967f66696934346e78f42de529ae47b3102d269198781c04f76cdf8c15555f5090f6b08bce09b2a0c13779ff'
    'ca327748ca9c297a8facede92b6e8e8aa0c040228b1d84c5754b5f10a8e8a60a8a13b4e4db501b1bdd3c24ff13bec6ec0eec7dc3f2881ba6de72bf095e936644'
    '970c9e0e7fbd48a51a644da1ccb9563ba463c1b30bc60581f226c155a7c6b443bdeb1a4b272550a6d19bfc922f2ec6715364e55f6c589e4c87abc3c12b67a9fa'
    '4e3521cb664f5cc47551e38fab8d979f0de0be62c02f301b48bd0ddfc48479ea00f0309db1bd293192fc75d31968c7ea88dddaa5d91b8304558d7d6baef016a9'
)
install='polaris.install'

prepare() {
    cd $pkgname-$pkgver
	# rust 1.80+ introduced a bug which prevents time-rs from building
	# remove once this issue is resolved
	export RUSTUP_TOOLCHAIN=1.79.0
    #cargo fetch --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
    # Build the server
    cd $pkgname-$pkgver
    #echo "Applying Dependencies Patch"
    #git apply ../locked_build.patch
    cp res/unix/Makefile .
    make PREFIX=/usr LOCALSTATEDIR=/var RUNSTATEDIR=/run build

    # Build the client
    export CYPRESS_CACHE_FOLDER="$srcdir/npm-cache"
    cd "$srcdir/polaris-web-$_webver"
    npm install --cache "$srcdir/npm-cache"
    npm run production
}

check() {
    cd $pkgname-$pkgver
    cargo test --release --locked || true
}

package() {
    install -Dm644 config.toml -t "$pkgdir/etc/polaris"
    install -Dm644 polaris.service -t "$pkgdir/usr/lib/systemd/system"
    install -Dm644 polaris.tmpfiles "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"
    install -Dm644 polaris.sysusers "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"

    cd $pkgname-$pkgver
    install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname"
    install -Dm755 target/release/polaris -t "$pkgdir/usr/bin"
    install -dm755 "$pkgdir/usr/share/polaris"
    cp -rT "$srcdir/polaris-web-$_webver/dist" "$pkgdir/usr/share/polaris/web"
    cp -rT docs/swagger "$pkgdir/usr/share/polaris/swagger"
}
