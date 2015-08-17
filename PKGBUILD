#
# Maintainer: Padfoot <padfoot@exemail.com.au>
#

pkgname=plymouth-release
_pkgname=plymouth
pkgver=0.8.8
pkgrel=7
pkgdesc="A graphical boot splash screen with kernel mode-setting support"
url="http://www.freedesktop.org/wiki/Software/Plymouth/"

arch=('i686' 'x86_64')
license=('GPL')

depends=('libdrm' 'pango' 'systemd')
makedepends=('docbook-xsl')
optdepends=('ttf-dejavu')

options=('!libtool' '!emptydirs')

provides=('plymouth')
conflicts=('plymouth' 'plymouth-git')
backup=('etc/plymouth/plymouthd.conf')

source=('http://www.freedesktop.org/software/plymouth/releases/plymouth-0.8.8.tar.bz2' 
        'arch-logo.png'
        'plymouth.encrypt_hook'
        'plymouth.encrypt_install'
        'gdm-plymouth.service'
        'kdm-plymouth.service'
	    'lxdm-plymouth.service'
	    'lightdm-plymouth.service'
	    'slim-plymouth.service'
        'plymouth.initcpio_hook'
        'plymouth.initcpio_install'
        'plymouth-quit.service.in.patch'
        'plymouth-set-default-theme.in.patch'
        'plymouth-update-initrd.patch'
        'plymouth-configure.ac.patch')
        
md5sums=('38f5e613e5ab17806b950cee2d0d0d4e'
         '65fa2763d5c9bb9f80973ea5e6e3db3e'
         '79613b70a0a0be4c33978ef37c30f00a'
         '65eb269910f1b81e3f0970af456394ca'
         '043410ebf89488602d4e5457f147e48b'
         'dcacff05835053242f52b9e375248ec3'
         '22130f7e6b20d2a43dbed924de75180b'
         '870ea3e63c6989e2badf79d1fbafa914'
         'fb2eb948e2198cfb54cc2b710a05ee61'
         '4f3be343eea9956bbc94d615d5f41def'
         '0a62557836483822c04a5da7a47362f4'
         '165a39dbedcc6e123c8ca05d5b4b2e25'
         'f79edbbb30c71b0dbcd102c7dd31660b'
         '6ef414ed2b34fd04e397727e7d1d7347'
         'a6e5941ce861aaa4ee7255f968970ae4')

prepare() {
	cd "$srcdir"/${_pkgname}-${pkgver}
	
	patch -p1 -i $srcdir/plymouth-configure.ac.patch
	patch -p1 -i $srcdir/plymouth-set-default-theme.in.patch
	patch -p1 -i $srcdir/plymouth-update-initrd.patch
    patch -p1 -i $srcdir/plymouth-quit.service.in.patch
}

build() {
	cd "$srcdir"/${_pkgname}-${pkgver}
	
	LDFLAGS="$LDFLAGS -ludev" ./configure --prefix=/usr \
		--exec-prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--libdir=/usr/lib \
		--libexecdir=/usr/lib \
		--sbindir=/usr/bin \
		--enable-systemd-integration \
		--enable-drm \
		--enable-tracing \
		--enable-pango \
		--with-release-file=/etc/os-release \
		--with-gdm-autostart-file=yes \
		--with-logo=/usr/share/plymouth/arch-logo.png \
		--with-background-color=0x000000 \
		--with-background-start-color-stop=0x000000 \
		--with-background-end-color-stop=0x4D4D4D \
		--without-rhgb-compat-link \
		--without-system-root-install

	make
}

package() {
  cd "$srcdir"/${_pkgname}-${pkgver}

  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir/arch-logo.png"             "$pkgdir/usr/share/plymouth/arch-logo.png"
  install -Dm644 "$srcdir/plymouth.encrypt_hook"     "$pkgdir/usr/lib/initcpio/hooks/plymouth-encrypt"
  install -Dm644 "$srcdir/plymouth.encrypt_install"  "$pkgdir/usr/lib/initcpio/install/plymouth-encrypt"
  install -Dm644 "$srcdir/plymouth.initcpio_hook"    "$pkgdir/usr/lib/initcpio/hooks/plymouth"
  install -Dm644 "$srcdir/plymouth.initcpio_install" "$pkgdir/usr/lib/initcpio/install/plymouth"

for i in {gdm,kdm,lxdm,slim,lightdm}-plymouth.service; do
  install -Dm644 "$srcdir/$i" "$pkgdir/usr/lib/systemd/system/$i"
done
}
