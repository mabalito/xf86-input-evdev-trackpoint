# Maintainer: Taegil Bae <esrevinu@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <Alexander@archlinux.org

_pkgname=xf86-input-evdev
pkgname=${_pkgname}-trackpoint
pkgver=2.9.2
pkgrel=1
_pkgname2=xf86-input-synaptics
_pkgver2=1.8.2
pkgdesc="X.org evdev input driver for trackpoint with clickpad"
arch=(i686 x86_64)
url="http://xorg.freedesktop.org/"
license=('custom')
depends=('glibc' 'systemd' 'mtdev' 'libxtst' 'libevdev')
makedepends=('xorg-server-devel' 'resourceproto' 'scrnsaverproto' 'libxi' 'libx11' 'X-ABI-XINPUT_VERSION>=19' 'autoconf')
conflicts=('xorg-server<1.14.0' 'X-ABI-XINPUT_VERSION<19' 'X-ABI-XINPUT_VERSION>=22' 'xf86-input-evdev')
replaces=("${_pkgname}")
provides=("${_pkgname}")
options=('!makeflags')
backup=('etc/X11/xorg.conf.d/90-evdev-trackpoint.conf')
source=(${url}/releases/individual/driver/${_pkgname}-${pkgver}.tar.bz2
	${url}/releases/individual/driver/${_pkgname2}-${_pkgver2}.tar.bz2
	0001-implement-trackpoint-wheel-emulation.patch
	0004-disable-clickpad_guess_clickfingers.patch
	0006-add-synatics-files-into-Makefile.am.patch
	0007-detect-palm-pressure.patch
	90-evdev-trackpoint.conf)
sha256sums=('792329b531afc6928ccda94e4b51a5520d4ddf8ef9a00890a5d0d31898acefec'
            '7b0e164ebd02a680e0c695955e783059f37edb0c2656398e0a972adc8e698c80'
            '1df30c030522d48dd1a134f9cf3acb44876950836b80eb56436414d6fd062d34'
            '74c508e1173254af8e4a851bf05762a1cd7dc194079b875ea0226913297db362'
            '9fc80868f6fae1c41a4d31bd77daa5d4e8832628c7e4e13f9645647bdd01b0f8'
            '750215baa375e83d77bc35906408bf340e26dc792ac7f460f37159d154413a6f'
            '9dd49bcfe68224a7a826070f44b54cbdd349f76f9e16291d5051b3a87e8bf81e')
install=xf86-input-evdev-trackpoint.install

prepare() {
  synaptics_srcdir=${srcdir}/${_pkgname2}-${_pkgver2}

  cd "${srcdir}/${_pkgname}-${pkgver}"

  cp ${synaptics_srcdir}/src/{eventcomm.c,eventcomm.h,properties.c,\
synaptics.c,synapticsstr.h,synproto.c,synproto.h} src/
  cp ${synaptics_srcdir}/include/synaptics-properties.h include/

  patch -p1 -i ../0001-implement-trackpoint-wheel-emulation.patch
  patch -p1 -i ../0004-disable-clickpad_guess_clickfingers.patch
  patch -p1 -i ../0006-add-synatics-files-into-Makefile.am.patch
  patch -p1 -i ../0007-detect-palm-pressure.patch
}

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  autoreconf -i
  ./configure --prefix=/usr
  make
}

package() {
  cd "${srcdir}/${_pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"
  install -m755 -d "${pkgdir}/etc/X11/xorg.conf.d"
  install -m644 ${srcdir}/90-evdev-trackpoint.conf "${pkgdir}/etc/X11/xorg.conf.d/"
}
