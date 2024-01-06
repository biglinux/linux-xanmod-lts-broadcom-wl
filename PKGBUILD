# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Arch credits:
# Based on the file created for Arch Linux by: Frank Vanderham

_linuxprefix=linux-xanmod-lts
_extramodules=$(find /usr/lib/modules -type d -iname 6.1.71*xanmod* | rev | cut -d "/" -f1 | rev)
pkgname=$_linuxprefix-broadcom-wl
_pkgname=broadcom-wl
pkgver=6.30.223.271
pkgrel=61711
pkgdesc='Broadcom 802.11 Linux STA wireless driver BCM43142.'
url='https://bbs.archlinux.org/viewtopic.php?id=145884'
arch=('x86_64')
license=('custom')
depends=("$_linuxprefix")
makedepends=("broadcom-wl-dkms>=$pkgver"
             'dkms'
             "$_linuxprefix" "$_linuxprefix-headers")
groups=("$_linuxprefix-extramodules")
provides=("$_pkgname=$pkgver")
install=$_pkgname.install
backup=('etc/modprobe.d/$_linuxprefix-broadcom-wl.conf')
source=(broadcom-wl-dkms.conf)
sha256sums=('b97bc588420d1542f73279e71975ccb5d81d75e534e7b5717e01d6e6adf6a283')

build() {
  _kernver=$(find /usr/lib/modules -type d -iname 6.1.71*xanmod* | rev | cut -d "/" -f1 | rev)

  # build host modules
  msg2 'Build module'

  CFLAGS="${CFLAGS} -Wno-error"
  CXXFLAGS="${CXXFLAGS}  -Wno-error"

  msg2 "$CFLAGS, ${CXXFLAGS}"

  fakeroot dkms build --dkmstree "$srcdir" -m "broadcom-wl/$pkgver" -k "$_kernver"
}

package(){
  _kernver=$(find /usr/lib/modules -type d -iname 6.1.71*xanmod* | rev | cut -d "/" -f1 | rev)

  install -dm755 "$pkgdir/usr/lib/modules/$_extramodules"
  cd "broadcom-wl/$pkgver/$_kernver/$CARCH/module"
  install -m644 * "$pkgdir/usr/lib/modules/$_extramodules"
  find "$pkgdir" -name '*.ko' -exec gzip -9 {} +
  sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='$_extramodules'/" "$startdir/$_pkgname.install"
  install -D -m 644 "${srcdir}/broadcom-wl-dkms.conf" "${pkgdir}/etc/modprobe.d/${_linuxprefix}-broadcom-wl.conf"
}
