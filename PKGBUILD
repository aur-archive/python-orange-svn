# Contributor: Blaž Tomažič <blaz.tomazic@gmail.com>
pkgname=python-orange-svn
pkgver=12741
pkgrel=1
pkgdesc="Open source data visualization and analysis for novice and experts. Data mining through visual programming or Python scripting."
arch=('i686' 'x86_64')
url="http://www.ailab.si/orange"
license=('GPL')
depends=('python2' 'python2-numpy' 'python-numarray' 'python-numeric' 'python2-matplotlib')
makedepends=('subversion')
optdepends=('pyqwt: GUI support' 'python-networkx')
conflicts=('python-orange')
provides=('python-orange')
source=()
md5sums=()
install='python-orange-svn.install'

_svntrunk="http://orange.biolab.si/svn/orange/trunk/orange"
_svnmod="orange"
_svntrunk2="http://orange.biolab.si/svn/orange/trunk/source"
_svnmod2="source"

build() {
  cd "${srcdir}"

  msg "Connecting to Orange SVN server...."
  
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
    svn co $_svntrunk2 --config-dir ./ -r $pkgver $_svnmod/$_svnmod2
  fi
  
  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf ${_svnmod}-build
  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build
  
  python2 setup.py build || return 1
}

package() {
  cd "${srcdir}/$_svnmod-build"
  python2 setup.py install --root="${pkgdir}" || return 1
  
  find ${pkgdir} -name '*.py' | while read FILE; do
    sed -i -e "s|#![ ]*/usr/bin/python$|#!/usr/bin/python2|" \
           -e "s|#![ ]*/usr/bin/env python$|#!/usr/bin/env python2|" \
           -e "s|#![ ]*/bin/env python$|#!/usr/bin/env python2|" \
      "$FILE"
done
}

# vim:set ts=2 sw=2 et:
