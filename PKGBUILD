_realname=gtk4
pkgbase=mingw-w64-${_realname}
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=3.96.0
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit (v4) (mingw-w64)"
arch=('any')
url="https://www.gtk.org"
license=("LGPL")
install=gtk4-${CARCH}.install
makedepends=("${MINGW_PACKAGE_PREFIX}-gcc"
             "${MINGW_PACKAGE_PREFIX}-pkg-config"
             "${MINGW_PACKAGE_PREFIX}-gobject-introspection"
             "${MINGW_PACKAGE_PREFIX}-meson")
depends=("${MINGW_PACKAGE_PREFIX}-gcc-libs"
         "${MINGW_PACKAGE_PREFIX}-adwaita-icon-theme"
         "${MINGW_PACKAGE_PREFIX}-atk"
         "${MINGW_PACKAGE_PREFIX}-cairo"
         "${MINGW_PACKAGE_PREFIX}-gdk-pixbuf2"
         "${MINGW_PACKAGE_PREFIX}-glib2"
         "${MINGW_PACKAGE_PREFIX}-graphene"
         "${MINGW_PACKAGE_PREFIX}-json-glib"
         "${MINGW_PACKAGE_PREFIX}-libepoxy"
         "${MINGW_PACKAGE_PREFIX}-pango"
         "${MINGW_PACKAGE_PREFIX}-gst-plugins-good"
         "${MINGW_PACKAGE_PREFIX}-shared-mime-info")
options=('!strip' '!debug')
source=("${_realname}::git+https://gitlab.gnome.org/GNOME/gtk.git#tag=3.96.0")
sha256sums=('SKIP')

prepare() {
  cd "${srcdir}/${_realname}"
  git cherry-pick ca2bffc06
}

build() {
  rm -rf "${srcdir}/build-${MINGW_CHOST}"
  mkdir "${srcdir}/build-${MINGW_CHOST}"
  cd "${srcdir}/build-${MINGW_CHOST}"

  meson \
    -Dbuild-examples=false \
    -Dbuild-tests=false \
    -Dman-pages=true \
    -Dvulkan=no \
    -Dintrospection=false \
    -Ddocumentation=false \
    ../${_realname}

  ninja
}

package() {
  cd "${srcdir}/build-${MINGW_CHOST}"

  DESTDIR="${pkgdir}${MINGW_PREFIX}" ninja install

  for pcfile in "${pkgdir}${MINGW_PREFIX}"/lib/pkgconfig/*.pc; do
    sed -s "s|$(cygpath -m ${MINGW_PREFIX})|${MINGW_PREFIX}|g" -i "${pcfile}"
  done

  install -Dm644 "${srcdir}/${_realname}/COPYING" "${pkgdir}${MINGW_PREFIX}/share/licenses/${_realname}/COPYING"
}
