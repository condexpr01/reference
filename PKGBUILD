# Maintainer: condexpr01(Vito Devlin) <condexpr01@outlook.com>
PACKAGER='condexpr01(Vito Devlin) <condexpr01@outlook.com>'
pkgname=reference
pkgver=2026.05.18.1
pkgrel=1
pkgdesc='Reference'
arch=('any')
url='https://github.com/condexpr01/ref'

license=('MIT')

#depends=()
#makedepends=('pandoc' 'gzip')

source=('readme.md' 'LICENSE.txt' 'ref')
sha256sums=('SKIP' 'SKIP' 'SKIP')

prefix=${PREFIX:-/usr}

prepare() {
	cp -r "$startdir"/color "$srcdir/"
	cp -r "$startdir"/ds "$srcdir/"
	cp -r "$startdir"/languages "$srcdir/"
	cp -r "$startdir"/major "$srcdir/"
	cp -r "$startdir"/tools "$srcdir/"
}

package() {
	if command -v pandoc 2>/dev/null >/dev/null;then
		pandoc -s -t man "$srcdir/readme.md" -o "$srcdir/readme.1"
		gzip -9c "$srcdir/readme.1" > "$srcdir/${pkgname}.gz"
		install -Dm644 "$srcdir/${pkgname}.gz" "$pkgdir${prefix}/share/man/man1/${pkgname}.1.gz"
	else
		echo "Warning: no pandoc, won't install man doc"
	fi

	install -Dm644 "$srcdir/LICENSE.txt" "$pkgdir${prefix}/share/license/${pkgname}/LICENSE.txt"
	
	install -Dm755 "$srcdir/ref" "$pkgdir${prefix}/bin/ref"

	install -dm755 "$pkgdir${prefix}/share/ref"

	cp -r "$srcdir"/color "$pkgdir${prefix}/share/ref"
	cp -r "$srcdir"/ds "$pkgdir${prefix}/share/ref"
	cp -r "$srcdir"/languages "$pkgdir${prefix}/share/ref"
	cp -r "$srcdir"/major "$pkgdir${prefix}/share/ref"
	cp -r "$srcdir"/tools "$pkgdir${prefix}/share/ref"

	find "$pkgdir${prefix}/share/ref" -type f -exec chmod 666 {} \;
}
