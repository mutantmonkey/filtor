# Maintainer: mutantmonkey <aur@mutantmonkey.in>
pkgname=tor-control-port-proxy
pkgver=0.3
pkgrel=1
pkgdesc="Whitelisting Tor control port proxy written in Python"
arch=('any')
url="https://github.com/mutantmonkey/tor-control-port-proxy"
license=('WTFPL')
depends=('python' 'stem')
options=(!emptydirs)
source=('tor-control-port-proxy' 'tor-control-port-proxy.service' 'COPYING')
sha256sums=('e2c0a779c1f68463b3360cfbee1e9b613853d66bad0bfad44e919f73b4d31de1'
            'eda5f0f43c9d2c8956743ba6485bb50fdb307af9941634902385214ecda14ce4'
            '96f17857f3eb28a7d93dad930bc099a3cb65a9a2afb37069bfd1ba5ec5964389')

package() {
  install -Dm755 tor-control-port-proxy \
    "${pkgdir}/usr/bin/tor-control-port-proxy"
  install -Dm644 tor-control-port-proxy.service \
    "${pkgdir}/usr/lib/systemd/system/tor-control-port-proxy.service"
  install -Dm644 COPYING \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}

# vim:set ts=2 sw=2 et:
