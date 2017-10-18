# Maintainer: peerchemist <peerchemist@protonmail.ch>
# Maintainer: Graham Higgins <gjh@bel-epa.com>

pkgname=('slimcoin-qt' 'slimcoind')
pkgbase=slimcoin
_gitname=slimcoin
pkgver=0.6.4
pkgrel=6
pkgdesc="Slimcoin client."
makedepends=('gcc' 'make' 'qrencode' 'boost' 'miniupnpc' 'openssl' 'qt5-base')
depends=('boost-libs' 'openssl' 'miniupnpc' 'qt5-base')
replaces=("slimcoin-daemon" "slimcoin-qt" "slimcoind")
arch=('x86_64' 'i686')
url='slimcoin.club'
license=('MIT')
source=(https://minkiz.co/noodlings/slm/slimcoin.tar.gz
        slimcoin-qt.desktop
        slimcoin-qt@.service
        slimcoin-qt-tor@.service
        slimcoind@.service
	slimcoind-tor@.service)
sha256sums=('27955d4b96c50923058481b6a5069ca7f14729cd3550005dbf3e4711aa4e9338'
            '44209ffeb522c7a69b1160cea59611a38706ea8438d1a9a40bb7f1ce9ea87b0b'
            '7e90361dc25fd59bc4d1b85f255c2bd4c450b59d820d748bb3d0acc11d0d600c'
            '5b98ffab21bc76f03fd3547b6441dbbddd73f1fc65940a4d4c2401c304b7221c'
            '80dcdf2bf3540a3ddd3c2cd1299aa97db06bf1efdadee4ad847e3371658dd62f'
	    '3a40552151feb7d5043c9c996560e4db321cc3fa9a716ed3dfb6fa25d843b09a')

prepare() {
	## cd "$srcdir/${_gitname}-${pkgver}ppc/src"
	cd "$srcdir/${_gitname}"
	git checkout master
}

build() {
	## cd "$srcdir/${_gitname}-${pkgver}ppc"
	cd "$srcdir/${_gitname}"
	
	## make qt gui
	qmake USE_QRCODE=1 USE_UPNP=1 USE_SSL=1 \
	    QMAKE_CFLAGS="${CFLAGS}"\
    	QMAKE_CXXFLAGS="${CXXFLAGS} -pie"
	make

	## make slimcoind
  	make -f makefile.unix USE_UPNP=1 -e PIE=1 -C src
}

check() {
  ## cd "$srcdir/${_gitname}-${pkgver}ppc"
  cd "$srcdir/${_gitname}"
  
  make -f makefile.unix test_${_gitname} -C src
}

package_slimcoin-qt() {
	
	pkgdesc="Implementation of Slimcoin Qt wallet."
	makedepends=('qt5-base' 'boost' 'gcc' 'make' 'qrencode' 'openssl' 'miniupnpc')
	depends=('qt5-base' 'miniupnpc' 'boost-libs' 'qrencode' 'miniupnpc')
	optdepeds=('systemd' 'tor')
	install=slimcoin.install

	install -Dm644 ${pkgname}.desktop "${pkgdir}/usr/share/applications/${pkgname}.desktop"
	install -Dm644 $pkgname@.service "${pkgdir}/usr/lib/systemd/system/$pkgname@.service"
	install -Dm644 $pkgname-tor@.service "${pkgdir}/usr/lib/systemd/system/$pkgname-tor@.service"

	## cd "$srcdir/${_gitname}-${pkgver}ppc"
	cd "$srcdir/${_gitname}"
	install -Dm755 slimcoin-qt "${pkgdir}/usr/bin/$pkgname"
	#install -Dm644 COPYING "${pkgdir}/usr/share/licenses/slimcoin/COPYING"
	install -Dm644 "src/qt/res/icons/slimcoin.png" "${pkgdir}/usr/share/pixmaps/slimcoin.png"
	
}

package_slimcoind() {
	
	makedepends=('boost' 'gcc' 'make' 'openssl' 'miniupnpc')
	depends=('boost-libs' 'miniupnpc')
	optdepeneds=('systemd' 'tor')
	pkgdesc="Implementation of Slimcoin daemon."
	install=slimcoin.install

	install -Dm644 $pkgname@.service "$pkgdir/usr/lib/systemd/system/$pkgname@.service"
	install -Dm644 $pkgname-tor@.service "$pkgdir/usr/lib/systemd/system/$pkgname-tor@.service"
	## install -Dm644 "$srcdir/${_gitname}-${pkgver}ppc/COPYING" "$pkgdir/usr/share/licenses/slimcoin/COPYING"
	install -Dm644 "$srcdir/${_gitname}/COPYING" "$pkgdir/usr/share/licenses/slimcoin/COPYING"

	## cd "$srcdir/${_gitname}-${pkgver}ppc"
	cd "$srcdir/${_gitname}"
	install -Dm755 "src/slimcoind" "$pkgdir/usr/bin/$pkgname"
}
