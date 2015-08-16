# Maintainer: Daniel Kirchner <daniel at ekpyron dot org>
pkgname=mingw-w64-oglimg
pkgver=20120414
pkgrel=1
pkgdesc="OpenGL image loading library for C++ (mingw-w64)"
license=('GPL')
arch=('i686' 'x86_64')
url="http://github.com/ekpyron/oglimg/"
depends=('mingw-w64-glmath' 'mingw-w64-oglp' 'mingw-w64-libpng')
makedepends=('mingw-w64-gcc' 'mingw-w64-pkg-config')
_gitroot="git://github.com/ekpyron/oglimg"
_gitname="oglimg"
options=(!strip !buildflags)

_targetarch32=i686-w64-mingw32
_targetarch64=x86_64-w64-mingw32

build() {
  # mingw32 has problems with --hash-style=gnu (default value)
  unset LDFLAGS
  
  cd "$srcdir"
  
  msg 'Connecting to GIT server...'
  
  if [ -d ${_gitname} ]; then
  	cd ${_gitname} && git pull origin
  	cd ..
  else
  	git clone ${_gitroot}
  fi
  
  msg 'GIT checkout done or server timeout.'

  cd "${srcdir}/$_gitname"
  ./autogen.sh
  
  rm -rf build32
  mkdir build32
  cd build32
  
  ../configure --prefix=/usr/${_targetarch32} --host=${_targetarch32}
  make
  
  cd ..
  
  rm -rf build64
  mkdir build64
  cd build64
  
  ../configure --prefix=/usr/${_targetarch64} --host=${_targetarch64}
  make
  
}

package() {
  cd "${srcdir}/$_gitname/build32"
  make install DESTDIR="${pkgdir}"

  cd "${srcdir}/$_gitname/build64"
  make install DESTDIR="${pkgdir}"
}
