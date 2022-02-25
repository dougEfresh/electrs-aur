# Maintainer: Douglas Chimento dchimento@gmail.com
pkgname=electrs
pkgver=0.9.5
pkgrel=1
pkgdesc="An efficient re-implementation of Electrum Server in Rust"
arch=(x86_64 aarch64)
url="https://github.com/romanz/electrs"
license=('MIT')
depends=('rocksdb')
makedepends=('git' 'clang' 'cmake' 'rust')

# patched from https://gitlab.alpinelinux.org/alpine/aports/-/merge_requests/30466/diffs?commit_id=2fdc0e96a8ea824b36a90b2d668af740e57f6684#a9c0e6a87987d020830dfff2cd070102a5eb0844
source=("https://github.com/romanz/electrs/archive/v$pkgver/electrs-$pkgver.tar.gz"
	10-rocksdb.patch
	11-rocksdb-locked.patch
	12-db.rs.patch
	config.toml
	"${pkgname}@.service"
	"${pkgname}.sysuser"
	)
provides=('electrs')
sha256sums=("d95c1603bc4672c5fecacf47f44eaf187fbdfd70f677f8820a64f6ea3e082343"
	    "2da98231c92136839c26d200d2158aa91efb651d27ba8558872efcf7bb15e138"
	    "d1c2fb6f6a06d1fe6576aab3ddd04efda109dd5e4f502dc73f95dab03c5c1a9c"
	    "458020df059d271e69e88998d805654ad827193ea19634040c27f099339527a3"
            SKIP SKIP SKIP)
validpgpkeys=('15C8C3574AE4F1E25F3F35C587CAE5FA46917CBB')
install="${pkgname}.install"
backup=("etc/${pkgname}/config.toml")

prepare() {
    cd "$pkgname-$pkgver"
    patch --forward --strip=1 --input="${srcdir}/10-rocksdb.patch"
    patch --forward --strip=1 --input="${srcdir}/11-rocksdb-locked.patch"
    patch --forward --strip=1 --input="${srcdir}/12-db.rs.patch"
}

build() {
  cd "$pkgname-${pkgver}"
  ROCKSDB_INCLUDE_DIR=/usr/include ROCKSDB_LIB_DIR=/usr/lib cargo build  --release --locked
}

package() {
  install -Dm644 "${pkgname}@.service" -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm644 "${pkgname}.sysuser" "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
  install -Dm644 config.toml -t "${pkgdir}/etc/electrs"
  cd "${pkgname}-${pkgver}"
  install -Dm755 target/release/${pkgname} -t "${pkgdir}/usr/bin"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
