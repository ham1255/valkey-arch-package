# Maintainer: Andrew Crerar <crerar@archlinux.org>
# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=valkey
pkgver=7.2.5
pkgrel=1
pkgdesc='An in-memory database that persists on disk'
arch=('x86_64')
url='https://valkey.io/'
license=('BSD')
depends=('jemalloc' 'grep' 'shadow' 'systemd-libs')
# pkg-config fails to detect systemd libraries if systemd is not installed
makedepends=('systemd' 'openssl')
backup=('etc/valkey/valkey.conf'
        'etc/valkey/sentinel.conf')
install=valkey.install
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/valkey-io/valkey/archive/${pkgver}.tar.gz"
        valkey.service
        valkey-sentinel.service
        valkey.sysusers
        valkey.tmpfiles
        valkey.conf-sane-defaults.patch
        valkey-5.0-use-system-jemalloc.patch)
sha512sums=('0b684a5ffe045ce51bb2f4f76429928784b8b46ee4817a95b658ffd69313a90b3d0fb12e0ddbb0b4cb57a7e0c79072f603eb4524a9bcce96ecc9ae8f1a5f02c3'
	          '286ce5be525166814f8ffce64fdb8ddc3abbaefe75bfc3044e8a4ee2111359fda3b35b3e548538bc05e4393bb4f5c716f549a23d069e5c1f4327b9a60594fbb9'
	          '8cb8aab70192b83ee90b184ae2115f401c95539296ffcd3e1888ae3134aaf32380969f1360a52d259628a78113f760f8677591cb050d561f6abe35749df1c30e'
	          '4f90647162b55d8c1c074ff224cb481ca49c2b25e8e5263c5dbe3f68943dbefbbd35488c7bb17a11a2ea05e29c31d8b72eb6f2f11be73774de4457abbb158df5'
	          '11cf6d6999329af7a9fa4bcbbcf22242b461cec0c16ad949cc6b0383703f19417092782569bf6224f94167a560de0b4ba53ec0d8522683736a14f01bc5986b28'
	          '8bf91f539ac3757da5ce4e1bf3cab52aecbd05cc7c2631767ec8aad629acae22979515a2333a01321da1604c1e4eee83acf802f86c4cc7a78a9ed97dfe489a58'
	          '0acb08a6e0eaba239db7461bcfeddfbe0c1aaa517dc33c3918c9e991a1d5067cfe135b7f75085caade8c3ababd51ec9cefcc4120f57818bea1f7029a548a7732')

prepare() {
  cd $pkgname-$pkgver
  patch -Np1 < ../valkey.conf-sane-defaults.patch
  patch -Np1 < ../valkey-5.0-use-system-jemalloc.patch
}

build() {
  make BUILD_TLS=yes \
       USE_SYSTEMD=yes \
       -C $pkgname-$pkgver
}

package() {
  cd $pkgname-$pkgver
  make PREFIX="$pkgdir"/usr install

  install -Dm644 COPYING "$pkgdir"/usr/share/licenses/valkey/LICENSE
  install -Dm644 -t "$pkgdir"/etc/valkey valkey.conf sentinel.conf
  install -Dm644 -t "$pkgdir"/usr/lib/systemd/system/ ../valkey.service ../valkey-sentinel.service
  install -Dm644 "$srcdir"/valkey.sysusers "$pkgdir"/usr/lib/sysusers.d/valkey.conf
  install -Dm644 "$srcdir"/valkey.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/valkey.conf
}
