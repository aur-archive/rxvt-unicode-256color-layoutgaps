# Contributor: Nils Schweinsberg <mail@n-sch.de>

pkgname="rxvt-unicode-256color-layoutgaps"
pkgver=9.07
pkgrel=1
pkgdesc="a unicode enabled rxvt-clone terminal emulator (urxvt), with 256 colour support and a layout gap fix"
arch=("i686" "x86_64")
depends=("gcc-libs" "libxft")
makedepends=("ncurses" "perl" "pkgconfig")
optdepends=("gtk2-perl: for urxvt-tabbed usage")
provides=("rxvt-unicode")
conflicts=("rxvt-unicode" "rxvt-unicode-256color")
url="http://software.schmorp.de/pkg/rxvt-unicode.html"
license=("GPL2")
install=rxvt-unicode-256color.install
source=("rxvt-unicode.desktop" \
	"rxvt-unicode.png" \
	"font-width-fix.patch" \
	"gcc4.4.patch" \
	"layoutgaps.patch" \
	http://dist.schmorp.de/rxvt-unicode/rxvt-unicode-${pkgver}.tar.bz2)
md5sums=('ef2dfa44a86cae36a60f45559d8ad783'
	'84328cada91751df07324d95f8be4d1b'
	'df0c3a8b6bb0578d1b91e4081c47881c'
	'1b9b112df2204e1e58c66bf2d5776422'
	'43f1654010e088ef847d75a96c705a58'
	'49bb52c99e002bf85eb41d8385d903b5')

build() {
	cd ${srcdir}/rxvt-unicode-${pkgver}

	# Add 256 color support
	patch -p1 -i doc/urxvt-8.2-256color.patch || return 1

	# Fix font width bug fix
	patch -p0 -i ${srcdir}/font-width-fix.patch || return 1

	# Fix layout gaps (for example with tiled window managers)
	patch -p0 -i ${srcdir}/layoutgaps.patch || return 1

	# Port to compile with GCC4.4
#	patch -p0 -i ${srcdir}/gcc4.4.patch || return 1

	./configure --prefix=/usr \
		--with-terminfo=/usr/share/terminfo \
		--with-term=rxvt-256color \
		--enable-smart-resize \
		--disable-iso14755

	msg "Starting build process."
	make || return 1
	install -d ${pkgdir}/usr/share/terminfo
	TERMINFO=${pkgdir}/usr/share/terminfo
	make DESTDIR=${pkgdir} install

	# install the tabbing wrapper
	sed -i 's/\"rxvt\"/"urxvt"/' doc/rxvt-tabbed
	install -D -m755 doc/rxvt-tabbed ${startdir}/pkg/usr/bin/urxvt-tabbed

	# install freedesktop menu and icon ( icon from cvs checkout )
	install -D -m644 ${srcdir}/rxvt-unicode.desktop \
		${pkgdir}/usr/share/applications/rxvt-unicode.desktop
	install -Dm644 ${srcdir}/rxvt-unicode.png \
		${pkgdir}/usr/share/pixmaps/rxvt-unicode.png
}
