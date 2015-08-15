# Maintainer: Gadget3000 <gadget3000@msn.com>
pkgname=flightgear-atlas-cvs
pkgver=20120514
pkgrel=1
pkgdesc="Displays high quality charts of the world for users of FlightGear"
arch=('x86_64' 'i686')
url="http://atlas.sourceforge.net/"
license=('GPL')
if [ "$(pacman -Qs flightgear-git)" != "" ]; then
depends=('flightgear-git' 'libjpeg' 'libpng' 'glew')
makedepends=('simgear-git')
else
depends=('flightgear' 'libjpeg' 'libpng' 'glew')
makedepends=('simgear')
fi
makedepends=('cvs')
provides=('flightgear-atlas')
conflicts=('flightgear-atlas')

_cvsroot=":pserver:anonymous@atlas.cvs.sourceforge.net:/cvsroot/atlas"
_cvsmod=Atlas

build() {
  cd "$srcdir"
  msg "Connecting to $_cvsmod.sourceforge.net CVS server...."

  if [[ -d "$_cvsmod/CVS" ]]; then
    cd "$_cvsmod"
    cvs -z3 update -d
  else
    cvs -z3 -d "$_cvsroot" co -D "$pkgver" -f "$_cvsmod"
    cd "$_cvsmod"
  fi

  msg "CVS checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_cvsmod-build"
  cp -r "$srcdir/$_cvsmod" "$srcdir/$_cvsmod-build"
  cd "$srcdir/$_cvsmod-build"

  #
  # BUILD HERE
  #
  sed "s/ vector</ std::vector</" -i src/{FlightTrack.hxx,NavaidsOverlay.hxx,Globals.hxx,Graphs.{c,h}xx}
  sed "s/ vector </ std::vector </" -i src/Graphs.hxx
  sed "s/get_gbs_center2/get_gbs_center/" -i src/Subbucket.cxx
  ./autogen.sh
  ./configure --prefix=/usr --enable-simgear-shared
  make
}

package() {
  cd "$srcdir/$_cvsmod-build"
  make DESTDIR="$pkgdir/" install
}
