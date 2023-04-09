# This file is part of BlackArch Linux ( https://www.blackarch.org/ ).
# See COPYING for license details.

#pkgbase=wxgtk-dev
pkgbase=wxWidgets
pkgname=(wxgtk2-dev wxgtk3-dev wxgtk-common-dev)
pkgver=v3.2.2.1
pkgrel=1
pkgdesc='Cross-platform C++ GUI framework for native controls.'
arch=('x86_64')
groups=('blackarch') 
url='https://wxwidgets.org'
license=('custom:wxWindows')
depends=()
makedepends=('git' 'gst-plugins-base' 'glu' 'wbkit2gtk' 'libnotify' 'gtk2')
source=("git+https://github.com/wxWidgets/$pkgname.git")
sha512sums=('')

pkgver() {
  cd $pkgname

  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cp -a $pkgname $pkgname-gtk2
}

build() {
  cd $pkgname

  ./configure --prefix=/usr --libdir=/usr/lib --with-gtk=3 --with-opengl \
  --enable-unicode --enable-graphics_ctx --enable-mediactrl --enable-webview \
  --with-regex=builtin --with-libpng=sys --with-libxpm=sys --with-libjpeg=sys \
  --with-libtiff=sys --disable-precomp-headers
  make
  make -C locale allmo

  cd ../$pkgname-gtk2
  ./configure --prefix=/usr --libdir=/usr/lib --with-gtk=2 --with-opengl \
  --enable-unicode --enable-graphics_ctx --enable-mediactrl --enable-webview \
  --with-regex=builtin --with-libpng=sys --with-libxpm=sys --with-libjpeg=sys \
  --with-libtiff=sys --disable-precomp-headers
  make
}

package_wxgtk-common-dev() {
  pkgdesc='Common libraries and headers for wxgtk2 and wxgtk3'
  depends=('zlib' 'gcc-libs' 'expat')
  conflicts=('wxgtk-common')
  provides=('wxgtk-common')

  cd $pkgname

  make DESTDIR="${pkgdir}" install
  rm -r "$pkgdir"/usr/{bin/wx-config,lib/{wx,libwx_gtk}}

  install -D -m644 docs/license.txt "${pkgdir}/usr/share/licenses/${pkgdir}/LICENSE"
}

package_wxgtk2-dev() {
  pkgdesc='GTK2 implemention of wxWidgets API for GUI'
  depends=('gtk2' 'libgl'
           'gst-plugins-base-libs'
           'libsm' 'libxxf86vm'
           'wxgtk-common-dev' 
           'libnotify')
  conflicts=('wxgtk' 'wxgtk2')
  provides=('wxgtk' 'wxgtk2')
  replaces=('wxgtk')

  cd ${pkgname}-gtk2

  make DESTDIR="${pkgdir}" install
  rm -r "$pkgdir"/usr/{include,share,lib/libwx_base*,bin/wxrc*}
  mv "$pkgdir"/usr/bin/wx-config{,-gtk2}
 
  install -D -m644 docs/license.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

package_wxgtk3-dev() {
  pkgdesc='GTK3 implemention of wxWidgets API for GUI'
  depends=('gtk3'
           'gst-plugins-base-libs'
           'libsm'
           'libxxf86vm'
           'wxgtk-common-dev'
           'libnotify')
  optdepends=('webkit2gtk: for webview support')
  conficts=('wxgtk<3.0.3.1-2' 'wxgtk3')
  provides=('wxgtk3')

  cd $pkgname

  make DESTDIR="${pkgdir}" install
  rm -r "$pkgdir"/usr/{include,share,lib/libwx_base*,bin/wxrc*}
  ln -s wx-config "$pkgdir" /usr/bin/wx-config-gtk3

  install -D -m644 docs/license.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

