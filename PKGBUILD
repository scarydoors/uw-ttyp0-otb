# Maintainer: Alan Strauhs <scarydoorsyeah@gmail.com>

_dotted_zero=false
_centered_tilde=true
_encodings='uni i15'

pkgname='uw-ttyp0-otb'
pkgver=1.3
pkgrel=1
pkgdesc='Monospaced bitmap font with unicode support and powerline symbols.'
arch=('any')
url='http://people.mpi-inf.mpg.de/~uwe/misc/uw-ttyp0'
license=('custom:TTYP0')
makedepends=('fonttosfnt' 'perl')
conflicts=('otb-uw_ttyp0' 'uw-ttyp0-font')
source=("${url}/uw-ttyp0-${pkgver}.tar.gz")
md5sums=("7c25f8488ac7738c07f9e3e312843314")

prepare() {
	cd "uw-ttyp0-${pkgver}"
	if [[ -f ${SRCDEST}/VARIANTS.dat ]] ; then
		cp "${SRCDEST}/VARIANTS.dat" .
	else
		if ${_dotted_zero} ; then
			echo 'COPYTO Digit0Dotted Digit0'
		else
			echo 'COPYTO Digit0Slashed Digit0'
		fi >> VARIANTS.dat

		if ${_centered_tilde} ; then
			echo 'COPYTO MTilde AccTildeAscii'
		fi >> VARIANTS.dat
	fi

	if [[ -f ${SRCDEST}/TARGETS.dat ]] ; then
		cp "${SRCDEST}/TARGETS.dat" .
	else
		if [[ -n ${_encodings} ]] ; then
			echo "ENCODINGS = ${_encodings}"
		fi >> TARGETS.dat
	fi
}

build() {
    	cd "uw-ttyp0-${pkgver}"
	./configure
	make bdf
	
	mkdir -p otb
	for f in genbdf/*.bdf ; do
	    f=${f##*/}
	    fonttosfnt -o "otb/uw-ttyp0-${pkgver}-${f/bdf/otb}" "genbdf/${f}"
	done
}

package() {
	cd "uw-ttyp0-${pkgver}"

	install -d "$pkgdir/usr/share/fonts/misc"

	install -m644 otb/*.otb "${pkgdir}/usr/share/fonts/misc/"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
