#Contributor: Jerome Lebleu <lebleu.jerome@gmail.com>
pkgname=iep-hg
_pkgname=iep
pkgver=20130419
pkgrel=1
pkgdesc="Pronounced as 'eep'is a cross-platform Python IDE focused on interactivity and introspection, which makes it very suitable for scientific computing. Its practical design is aimed at simplicity and efficiency."
url="http://code.google.com/p/iep/"
license=("custom: New BSD")
arch=('any')
depends=('pyqt' 'python-pyzolib')
makedepends=('mercurial' 'python-distribute')
conflicts=('iep')
provides=('iep')

_hgroot='https://code.google.com/p/iep/'
_hgrepo='iep'

build() {
  cd $srcdir

  if [ -d "$_hgrepo" ]
  then
    cd $_hgrepo && hg revert --all && hg pull -u
  else
    hg clone $_hgroot && cd $_hgrepo
  fi
  
  python setup.py build
}

package(){
  cd ${srcdir}/${_hgrepo}
  python setup.py install --root="${pkgdir}/" --optimize=1 --skip-build

  # Remove left over directories from distribute utils.  
  find ${pkgdir} -type d -name "__pycache__" -exec rm -r {} \; -prune

  cat > ${_pkgname}.desktop <<EOF
[Desktop Entry]
Version=3.1
Type=Application
Encoding=UTF-8
Name=iep
Comment=IEP is a cross-platform Python IDE
Exec=/usr/bin/iep
Icon=/usr/lib/python3.3/site-packages/iep/resources/appicons/ieplogo.ico
Categories=Python;Development;IDE;
EOF

  mkdir -p ${pkgdir}/usr/share/applications
  install -m644 ${_pkgname}.desktop ${pkgdir}/usr/share/applications/${_pkgname}.desktop

  msg2 "Building iep shortcut"
  mkdir -p ${pkgdir}/usr/bin
  cat > ${pkgdir}/usr/bin/${_pkgname} <<EOF
#!/usr/bin/env python
import iep
iep.startIep()
EOF
  chmod 755 ${pkgdir}/usr/bin/${_pkgname}

  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  install -m444 ${srcdir}/${_hgrepo}/${_pkgname}/license.txt ${pkgdir}/usr/share/licenses/${pkgname}/license.txt
}
