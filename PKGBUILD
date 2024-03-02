pkgname=aya-prover-nightly
pkgver=r$(date +%Y%m%d)
pkgrel=1
pkgdesc="Aya Prover - a proof assistant"
arch=('x86_64')
url="https://github.com/aya-prover/aya-dev"
license=('MIT')  # Adjust according to the software's license
depends=()  # Specify runtime dependencies here, if any
source=("$pkgname-$pkgver.zip::https://github.com/aya-prover/aya-dev/releases/download/nightly-build/aya-prover_jlink_linux-x64.zip"
        "$pkgname-$pkgver.sha256.txt::https://github.com/aya-prover/aya-dev/releases/download/nightly-build/aya-prover_jlink_linux-x64.zip.sha256.txt")
sha256sums=('SKIP' 'SKIP')  # We'll manually verify the SHA256 checksum in prepare()

prepare() {
  cd "$srcdir"
  expected_sha256=$(cat "$pkgname-$pkgver.sha256.txt" | awk '{print $1}')
  actual_sha256=$(sha256sum "$pkgname-$pkgver.zip" | awk '{print $1}')
  
  if [ "$expected_sha256" != "$actual_sha256" ]; then
    echo "SHA256 checksum does not match"
    exit 1  # Fail the build if checksums don't match
  fi
}

package() {
  cd "$srcdir"
  install -dm755 "$pkgdir/usr/share/$pkgname"

  unzip "$pkgname-$pkgver.zip" -d "$pkgdir/usr/share/$pkgname"
  local binpath="$pkgdir/usr/share/$pkgname/bin"
  find "$binpath" -type f -exec sed -i 's:APP_HOME="`pwd -P`":APP_HOME=/usr/share/'"$pkgname"'/bin:g' {} +

  
  install -dm755 "$pkgdir/usr/bin"
  install -m755 "$pkgdir/usr/share/$pkgname/bin/"* "$pkgdir/usr/bin/"
}
