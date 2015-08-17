# Maintainer: Florian Pritz <flo@xssn.at>


pkgname=thunar-notrash
pkgver=1.3.0
pkgrel=1
pkgdesc="modern file manager for Xfce with patch to disable trash"
arch=('i686' 'x86_64')
license=('GPL2' 'LGPL2.1')
url="http://thunar.xfce.org"
groups=('xfce4')
conflicts=(thunar)
provides=("thunar=$pkgver")
depends=('desktop-file-utils' 'libexif' 'hicolor-icon-theme' 'libnotify' 'udev'
         'gtk2' 'exo>=0.6.0' 'libxfce4util>=4.8.1' 'libxfce4ui' 'libpng')
makedepends=('intltool' 'gtk-doc' 'xfce4-panel>=4.8.0')
optdepends=('gvfs: for trash support, mounting with udisk and remote filesystems'
            'xfce4-panel: for trash applet'
            'tumbler: for thumbnail previews'
            'thunar-volman: manages removable devices'
            'thunar-archive-plugin: create and deflate archives'
            'thunar-media-tags-plugin: view/edit id3/ogg tags')
options=('!libtool')
install=thunar-notrash.install
backup=('etc/polkit-1/localauthority/50-local.d/org.freedesktop.udisks.pkla')
source=(http://archive.xfce.org/src/xfce/thunar/1.3/Thunar-${pkgver}.tar.bz2
    Ability_to_disable_trash_on_Thunar_$pkgver.patch
    org.freedesktop.udisks.pkla)

md5sums=('ab6f728384c0d925b40afae2f41268f3'
         'a2c1ccb1148667395bb3e1f27ba9d467'
         'a7ddb5eec02d9a8e91a2997862e73cd8')

build() {
  cd ${srcdir}/Thunar-${pkgver}
  patch -p2 < $srcdir/Ability_to_disable_trash_on_Thunar_$pkgver.patch

	./configure --prefix=/usr \
	--sysconfdir=/etc \
	--libexecdir=/usr/lib \
	--localstatedir=/var \
	--disable-static \
	--enable-gio-unix \
	--enable-dbus \
	--enable-startup-notification \
	--enable-gudev \
	--enable-notifications \
	--enable-exif \
	--enable-pcre \
	--enable-gtk-doc \
	--disable-debug
  make
}

package() {
  cd ${srcdir}/Thunar-${pkgver}
  make DESTDIR=${pkgdir} install
  sed -i 's:x-directory/gnome-default-handler;::' \
    ${pkgdir}/usr/share/applications/Thunar-folder-handler.desktop

  # install udisks permission file
  install -dm700 ${pkgdir}/etc/polkit-1/localauthority
  install -dm755 ${pkgdir}/etc/polkit-1/localauthority/50-local.d
  install -m644 ${srcdir}/org.freedesktop.udisks.pkla ${pkgdir}/etc/polkit-1/localauthority/50-local.d/
}
