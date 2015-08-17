# Maintainer: Hugo Osvaldo Barrera <hugo@osvaldobarrera.com.ar>

pkgname=opensmtpd-portable
_pkgname=opensmtpd
pkgver=201303311750p1
_openbsdver=5.3.xp1
pkgrel=1
pkgdesc='A FREE implementation of the server-side SMTP protocol'
arch=('i686' 'x86_64')
url="http://www.opensmtpd.org/portable.html"
license=('BSD')
depends=('libevent')
conflicts=('sendmail' 'postfix' 'opensmtpd')
provides=('opensmtpd')
options=(!strip)
backup=("etc/mail/smtpd.conf")
source=(http://www.opensmtpd.org/archives/${_pkgname}-${pkgver}.tar.gz
        smtpd.service
        smtpd.conf.patch)
md5sums=('84b5149c231a033789da9bbd75e940e6'
         '636da7986f5ec40b80e72b71a6a7ac59'
         'ed249a24aa13081130e77f19c5b2d968')
install="${pkgname}.install"
depends=('libevent' 'sqlite')

build() {
  export LDFLAGS="-lrt -Wl,-O1,--sort-common,--as-needed,-z,relro"

  cd "${srcdir}/${_pkgname}"-${_openbsdver}

  ./bootstrap
  ./configure --prefix=/usr --sysconfdir=/etc/mail --with-pam

  make
}

package() {
  install -Dm 644 smtpd.service ${pkgdir}/usr/lib/systemd/system/smtpd.service

  cd "$srcdir/$_pkgname"-${_openbsdver}
  make DESTDIR="${pkgdir}/" install

  patch "${pkgdir}/etc/mail/smtpd.conf" < "${srcdir}/smtpd.conf.patch"
  ln -s smtpctl ${pkgdir}/usr/sbin/sendmail
}

