# Maintainer: Fredy García <frealgagu at gmail dot com>
# Maintainer: fuero <fuerob@gmail.com>

pkgname=lazygit
pkgver=0.23.6
pkgrel=1
pkgdesc="A simple terminal UI for git commands"
arch=("x86_64")
url="https://github.com/jesseduffield/${pkgname}"
license=("MIT")
depends=("git" "glibc")
makedepends=("go")
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/jesseduffield/${pkgname}/archive/v${pkgver}.tar.gz")
sha256sums=("b2383a70e54a67382b0f052149891d3d0f3e1eba952a0fa5009214c6591dfc6b")
_commit="7c1889cd70b81d9f8438c197c3d2fc89e5695160"

prepare() {
  mkdir -p "${srcdir}/src/github.com/jesseduffield"
  ln -rTsf "${srcdir}/${pkgname}-${pkgver}" "${srcdir}/src/github.com/jesseduffield/${pkgname}"
}

build () {
  cd "${srcdir}/src/github.com/jesseduffield/${pkgname}"
  export GOPATH="${srcdir}"
  export GOPATH="${srcdir}/gopath"
  export PATH="${PATH}:${GOPATH}/bin"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw -x -v"
  go build \
    -ldflags "\
      -extldflags ${LDFLAGS} \
      -X main.commit=${_commit} \
      -X main.date=$(date -u +%Y-%m-%dT%H:%M:%SZ) \
      -X main.buildSource=binaryRelease \
      -X main.version=${pkgver} \
    " \
    -o "${pkgname}.bin"

  # To avoid issues deleting directories next time
  go clean --modcache
}

package () {
  install -Dm755 "${srcdir}/${pkgname}-${pkgver}/${pkgname}.bin" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 "${srcdir}/${pkgname}-${pkgver}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  for _file in "${srcdir}/${pkgname}-${pkgver}/"*.md "${srcdir}/${pkgname}-${pkgver}/docs/"*.md
  do
    install -Dm644 "${_file}" "${pkgdir}/usr/share/doc/${pkgname}/$(basename ${_file})"
  done
}
