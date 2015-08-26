# Maintainer: Christian Rebischke <echo Q2hyaXMuUmViaXNjaGtlQGdtYWlsLmNvbQo= | base64 -d>
# Contributor: David Manouchehri <david@davidmanouchehri.com>

_gitname=binnavi
pkgname="${_gitname}-git"
_gitbranch=master
_gitauthor=google
pkgver=v6.0.0.16.670eb55
pkgrel=1
pkgdesc="BinNavi is a binary analysis IDE that allows to inspect, navigate, edit and annotate control flow graphs and call graphs of disassembled code"
url="https://github.com/${_gitauthor}/${_gitname}"
license=('Apache')
source=("git://github.com/${_gitauthor}/${_gitname}#branch=${_gitbranch}" "${_gitname}.sh")
sha512sums=('SKIP'
            'bb274ca29a994ef8b98aa77e0be745e297bd2f7e65dd394594169ffec3910b4dd4982e353202c6b201472632a3f7b16bd7d905b7006e21d76b7299b78fc7f390')
arch=('any')
depends=('java-runtime>=8' 'postgresql')
makedepends=('git' 'maven' 'apache-ant' 'java-environment>=8')
conflicts=("${_gitname}")
provides=("${_gitname}")

pkgver() {
  cd "${srcdir}/${_gitname}"
  printf "%s.%s.%s" "$(git describe --tags --abbrev=0)" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  java -version 2>&1 | grep 'version "1.8' >/dev/null || (printf "\e[1;31mJDK8 is required.\e[0m\nsudo archlinux-java set java-8-openjdk\n" && exit 1)

  cd "${srcdir}/${_gitname}"
  mvn dependency:copy-dependencies
  ant -f src/main/java/com/google/security/zynamics/build.xml \
  build-binnavi-fat-jar
}

package() {
  mkdir -p "${pkgdir}/usr/share/java/${_gitname}"
  cd "${srcdir}/${_gitname}/target/"
  mv * "${pkgdir}/usr/share/java/${_gitname}/"
  install -D -m755 "${srcdir}/binnavi.sh" "${pkgdir}/usr/bin/${_gitname}"
}

# vim:set et sw=2 sts=2 tw=80: