# $Id$
# Maintainer: Ng Oon-Ee <ngoonee.talk@gmail.com>
# Based of package in [extra] by: Andreas Radke <andyrtr@archlinux.org>

pkgname=dovecot-systemd
_pkgname=dovecot
pkgver=2.1.5
pkgrel=1
pkgdesc="An IMAP and POP3 server written with security primarily in mind. With systemd support"
arch=('i686' 'x86_64')
url="http://dovecot.org/"
license=("LGPL")
depends=('krb5' 'openssl' 'sqlite>=3.7.5' 'libmysqlclient>=5.5.10'
        'postgresql-libs>=9.0.3' 'bzip2' 'expat' 'curl' 'systemd')
makedepends=('pam>=1.1.1' 'libcap>=2.19' 'libldap>=2.4.22' 'clucene')
optdepends=('libldap: ldap plugin'
           'clucene: alternative FTS indexer')
provides=('imap-server' 'pop3-server' 'dovecot')
conflicts=('dovecot')
options=('!libtool')
backup=(etc/dovecot/dovecot.conf
	etc/dovecot/conf.d/{10-auth,10-director,10-logging,10-mail,10-master,10-ssl}.conf
	etc/dovecot/conf.d/{15-lda,20-imap,20-lmtp,20-pop3}.conf
	etc/dovecot/conf.d/{90-acl,90-plugin,90-quota}.conf
	etc/dovecot/conf.d/auth-{checkpassword,deny,ldap,master,passwdfile,sql,static,system,vpopmail}.conf.ext
	etc/ssl/dovecot-openssl.cnf)
install=$pkgname.install
source=(http://dovecot.org/releases/2.1/${_pkgname}-${pkgver}.tar.gz dovecot.sh)
md5sums=('c857e3442f2f14b3e46f1154b13b0b4b'
         '587159e84e2da6f83d70b3c706ba87cc')

build() {
  cd ${srcdir}/$_pkgname-$pkgver

  # configure with openssl, mysql, and postgresql support
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
  	--libexecdir=/usr/lib  --with-moduledir=/usr/lib/dovecot/modules \
	--disable-static \
	--with-nss \
	--with-pam \
  --with-systemdsystemunitdir=/lib/systemd/system \
	--with-mysql \
	--with-pgsql \
	--with-sqlite \
	--with-ssl=openssl --with-ssldir=/etc/dovecot/ssl \
	--with-gssapi \
	--with-ldap=plugin \
	--with-zlib --with-bzlib \
	--with-libcap \
	--with-solr \
	--with-lucene \
	--with-docs
  make
}

package() {
  cd ${srcdir}/$_pkgname-$pkgver
  make DESTDIR=${pkgdir} install

  # install the launch script
  install -D -m755 ${srcdir}/$_pkgname.sh ${pkgdir}/etc/rc.d/$_pkgname

  # install example conf files and ssl.conf
  install -d -m755 ${pkgdir}/etc/dovecot/conf.d
  install -m 644 ${pkgdir}/usr/share/doc/dovecot/example-config/dovecot.conf ${pkgdir}/etc/dovecot/dovecot.conf.sample
  install -d -m755 ${pkgdir}/etc/ssl
  install -m 644  ${srcdir}/$_pkgname-$pkgver/doc/dovecot-openssl.cnf ${pkgdir}/etc/ssl/dovecot-openssl.cnf.sample

  # install mkcert helper script
  install -m 755  ${srcdir}/$_pkgname-$pkgver/doc/mkcert.sh ${pkgdir}/usr/lib/dovecot/mkcert.sh

  rm ${pkgdir}/etc/dovecot/README
}
