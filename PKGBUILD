# Maintainer: Taegil Bae <esrevinu@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <Alexander@archlinux.org

_pkgname=xf86-input-evdev
pkgname=${_pkgname}-trackpoint
pkgver=2.8.3
pkgrel=1
_pkgname2=xf86-input-synaptics
_pkgver2=1.7.5
pkgdesc="X.org evdev input driver for trackpoint with clickpad"
arch=(i686 x86_64)
url="http://xorg.freedesktop.org/"
license=('custom')
depends=('glibc' 'systemd' 'mtdev' 'libxtst')
makedepends=('xorg-server-devel' 'resourceproto' 'scrnsaverproto' 'libxi' 'libx11' 'X-ABI-XINPUT_VERSION>=19' 'autoconf')
conflicts=('xorg-server<1.14.0' 'X-ABI-XINPUT_VERSION<19' 'X-ABI-XINPUT_VERSION>=21' 'xf86-input-evdev')
replaces=("${_pkgname}")
provides=("${_pkgname}")
options=('!makeflags')
backup=('etc/X11/xorg.conf.d/90-evdev-trackpoint.conf')
source=(${url}/releases/individual/driver/${_pkgname}-${pkgver}.tar.bz2
	${url}/releases/individual/driver/${_pkgname2}-${_pkgver2}.tar.bz2
	0001-implement-trackpoint-wheel-emulation.patch
	0005-can-be-responsive-to-touch-pressure-not-click.patch
	0006-SoftButtonAreas-is-replaced-by-new-three-options.patch
	0009-Set-default-finger_press-value-to-1000.-This-disable.patch
	0010-Fix-xinput-properties-mismatch.patch
	0011-Fix-unintended-right-button-click.patch
	0013-add-synatics-files-into-Makefile.am.patch
	90-evdev-trackpoint.conf)
sha256sums=('02ec7fd68635bd67be10275ba23f6c301a9109d72cac9c8646e28842003c06b0'
            '1f8423521ff747efa9d84ef33126ad4bbe27d5904731115aa70247bc388c91c7'
            '430e8528a230ace300480f06d55937355b228e0dfe6009b9ddd375e844febc06'
            '16e17ed1403f6d409a00141f25883211f91f29f550c1e6e3f8ff95469f938455'
            '998447914ea75d363be29e1f5cb163bf8bfae3f2d93154f923ba007c7cc5afa8'
            '0b927e491a716c525bcbd2e7b232ec6110b3ec925ccd68b9ee45270f4e4c8652'
            'c9e3b519e5c91947a18b08627d5351b254d99a4ae2f1763be95cab9c9f4e420a'
            '3028f8430195daaa68788c068adff749c271721be59bbee56936993d70656565'
            '07788a64f4033e2bc5dbefb3b29c21064d2759533a7f3f320bc6184b442d2e21'
            'baf38c7e399814de711fe0da1c55d345a04d19bd404336cb1f0581bfe46b118f')
install=xf86-input-evdev-trackpoint.install

prepare() {
  synaptics_srcdir=${srcdir}/${_pkgname2}-${_pkgver2}

  cd "${srcdir}/${_pkgname}-${pkgver}"

  cp ${synaptics_srcdir}/src/{eventcomm.c,eventcomm.h,properties.c,\
synaptics.c,synapticsstr.h,synproto.c,synproto.h} src/
  cp ${synaptics_srcdir}/include/synaptics-properties.h include/

  patch -p1 -i ../0001-implement-trackpoint-wheel-emulation.patch
  patch -p1 -i ../0005-can-be-responsive-to-touch-pressure-not-click.patch
  patch -p1 -i ../0006-SoftButtonAreas-is-replaced-by-new-three-options.patch
  patch -p1 -i ../0009-Set-default-finger_press-value-to-1000.-This-disable.patch
  patch -p1 -i ../0010-Fix-xinput-properties-mismatch.patch
  patch -p1 -i ../0011-Fix-unintended-right-button-click.patch
  patch -p1 -i ../0013-add-synatics-files-into-Makefile.am.patch
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
