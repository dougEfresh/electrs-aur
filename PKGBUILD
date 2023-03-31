# Maintainer: Douglas Chimento dchimento@gmail.com
pkgname=electrs
pkgver=0.9.13
pkgrel=1
pkgdesc="An efficient re-implementation of Electrum Server in Rust"
arch=(x86_64 aarch64)
url="https://github.com/romanz/electrs"
license=('MIT')
depends=('rocksdb')
makedepends=('git' 'clang' 'cmake' 'rust')
source=("https://github.com/romanz/electrs/archive/v$pkgver/electrs-$pkgver.tar.gz"
	config.toml
	"${pkgname}@.service"
	"${pkgname}.sysuser"
	)
provides=('electrs')
sha256sums=("7e1273998d78263173d2f17987a8623a29718751f1ad13f49c67e88221381211"
            SKIP SKIP SKIP)
validpgpkeys=('15C8C3574AE4F1E25F3F35C587CAE5FA46917CBB')
install="${pkgname}.install"
backup=("etc/${pkgname}/config.toml")

build() {
  cd "$pkgname-${pkgver}"
  cargo build  --release --locked
}

package() {
  install -Dm644 "${pkgname}@.service" -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm644 "${pkgname}.sysuser" "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
  install -Dm644 config.toml -t "${pkgdir}/etc/electrs"
  cd "${pkgname}-${pkgver}"
  install -Dm755 target/release/${pkgname} -t "${pkgdir}/usr/bin"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
