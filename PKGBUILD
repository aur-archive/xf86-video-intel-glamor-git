# Maintainer: Eugeni Dodonov <eugeni@dodonov.net>

pkgname=xf86-video-intel-glamor-git
pkgver=2.99.917.62.g2e7ae76
pkgrel=1
pkgdesc='X.org Intel i810/i830/i915/945G/G965+ video drivers with Glamor acceleration'
arch=('i686' 'x86_64')
url='http://intellinuxgraphics.org/'
license=('custom')
depends=('intel-dri' 'libxvmc' 'libpciaccess' 'libdrm' 'xcb-util' 'libxfixes' 'udev')
makedepends=('git' 'xorg-server-devel' 'libx11' 'xf86driproto' 'glproto' 'mesa')
provides=('xf86-video-intel')
conflicts=('xf86-video-intel')
options=('!libtool')

_gitroot='git://anongit.freedesktop.org/xorg/driver/xf86-video-intel'
_gitname='xf86-video-intel'
pkgver() {
    cd "$_gitname"
    git describe | sed 's/^v//;s/-/./g'
}
 
build() {
  msg 'Connecting to git.freedesktop.org GIT server....'

  if [ -d $_gitname ] ; then
    pushd $_gitname
     git pull origin
    popd
    msg 'The local files are updated.'
  else
    git clone $_gitroot
  fi

  msg 'GIT checkout done or server timeout'

  [ -d build ] && rm -rf build
  git clone $_gitname build
  cd build

  msg 'Starting make...'

  ./autogen.sh \
    --prefix=/usr/ \
    --enable-glamor \
    --enable-dri
    #--enable-debug=full \
    # other useful options you can add (as you see fit):
    # --disable-xvmc          Disable XvMC support [[default=yes]]
    # --enable-kms-only       Assume KMS support [[default=no]]
    # --enable-sna            Enable SandyBridge's New Acceleration (SNA)
    #                         [options=default|gen2|gen3|ge4|gen5|gen6]

  make
}

package() {
  cd build

  make DESTDIR="$pkgdir" prefix=/usr/ install

  install -D -m644 COPYING "$pkgdir"/usr/share/licenses/$pkgname/COPYING
}
