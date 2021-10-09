# Maintainer: Karl-Felix Glatzer <karl.glatzer@gmx.de>
# Contributor: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Chocobo1 <chocobo1 AT archlinux DOT net>

pkgname=mingw-w64-aom
pkgver=3.1.3
pkgrel=1
pkgdesc="Alliance for Open Media video codec (mingw-w64)"
url="https://aomedia.org/"
arch=('any')
license=(BSD custom:PATENTS)
depends=(mingw-w64-crt)
options=(!strip !buildflags staticlibs)
makedepends=(mingw-w64-gcc mingw-w64-cmake ninja yasm)
source=(https://storage.googleapis.com/aom-releases/libaom-$pkgver.tar.gz{,.asc}
        "cmake.patch")
b2sums=('b950c2cda0788d41e16ff771015ab934489c1aa53e366e394a17504845f152d93afc9ccf22a0f95fe828f1fdab27e617b8f81c61b250e64f8903e64043e746d1'
        'SKIP'
        '627c000cc5b152e78714898156ebebb2524749bd1d701bbbdca0b431301426c2f821403299a6fd4420be80133d4e7178dea8b2f4aae2ab34e9e81e584ebda345')
validpgpkeys=(B002F08B74A148DAA01F7123A48E86DB0B830498) # AOMedia release signing key <av1-discuss@aomedia.org>
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}"

  patch -Np1 -i "${srcdir}"/cmake.patch
}

build() {
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}"/build-static-${_arch} && cd "${srcdir}"/build-static-${_arch}
    ${_arch}-cmake -G Ninja \
      -DBUILD_SHARED_LIBS=0 \
      -DENABLE_TESTS=0 \
      ..
    cmake --build .
  done

  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}"/build-${_arch} && cd "${srcdir}"/build-${_arch}
    ${_arch}-cmake -G Ninja \
      -DBUILD_SHARED_LIBS=1 \
      -DENABLE_TESTS=0 \
      ..
    cmake --build .
  done
}

package() {
  cd "${srcdir}"
  for _arch in ${_architectures}; do
    DESTDIR="$pkgdir" cmake --install build-static-${_arch}
    DESTDIR="$pkgdir" cmake --install build-${_arch}

    ${_arch}-strip -s "${pkgdir}"/usr/${_arch}/bin/*.exe
    ${_arch}-strip --strip-unneeded "${pkgdir}"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "${pkgdir}"/usr/${_arch}/lib/*.a
  done
}

# vim:set et sw=2:
