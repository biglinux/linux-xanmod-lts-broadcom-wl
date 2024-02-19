# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Contributor: Eli Schwartz <eschwartz@archlinux.org>

_linuxprefix=linux-xanmod-lts

_module=broadcom-wl
pkgname="${_linuxprefix}-${_module}"
pkgver=6.30.223.271
pkgrel=66172
pkgdesc='Broadcom 802.11 Linux STA wireless driver'
arch=('x86_64')
url='https://www.broadcom.com/support/download-search/?pf=Wireless+LAN+Infrastructure'
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=("${_module}-dkms=$pkgver" "${_linuxprefix}-headers")

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  fakeroot dkms build --dkmstree "$srcdir" -m "${_module}/$pkgver" -k "${_kernver}"
}

package(){
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  install -Dm644 -t "${pkgdir}/usr/lib/modules/${_kernver}/extramodules" \
    ${_module}/${pkgver}/${_kernver}/${CARCH}/module/*

  # compress kernel modules
  find "$pkgdir" -name "*.ko" -exec xz {} +

  install -Dm644 "/usr/share/licenses/${_module}-dkms"/* -t \
    "${pkgdir}/usr/share/licenses/$pkgname/"
  install -Dm644 "/usr/lib/modprobe.d/${_module}-dkms.conf" \
      "${pkgdir}/usr/lib/modprobe.d/$pkgname.conf"
}
