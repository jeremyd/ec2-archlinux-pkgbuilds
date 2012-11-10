pkgname=cloud-init-bzr
pkgver=700
pkgrel=1
pkgdesc="cloud-init bzr checkout from canonical"
arch=(any)
license=("GPLv3")
depends=(systemd python2 python2-yaml python2-cheetah python2-prettytable python2-oauth2 python2-boto python2-argparse python2-configobj)
makedepends=('bzr' 'python2')
#_bzrtrunk="lp:~jeremydei/cloud-init/archlinux"
_bzrtrunk="lp:~harlowja/cloud-init/boto-metadata-fixings"
_bzrmod="cloud-init"
source=(http://dl.dropbox.com/u/402975/archlinux.cloud.cfg)
noextract=(archlinux.cloud.cfg)
sha1sums=(e5e1f6c04dbb45e828ef3d3f99831e339be76940)
# python lib requirements (according to Requires file):
# cheetah (aur) PrettyTable (aur), oauth (aur), boto (aur), configobj (community), pyyaml (community), argparse (aur)
# ONLY non-match is python2-yaml vs pyyaml?

build() {
  cd $srcdir

  msg "Connecting to Bazaar server..."
  if [ -d $_bzrmod ]; then
    cd ${_bzrmod} && bzr pull ${_bzrtrunk} -r ${pkgver}
    msg "The local files are updated."
  else
    bzr branch ${_bzrtrunk} ${_bzrmod} -q -r ${pkgver}
    cd $_bzrmod
  fi
  msg "Bazaar checkout done or server timeout"

  python2 ./setup.py install --root=${pkgdir} --init-system systemd
}

package() {
  # Don't want a default ubuntu config in the package!
  mv $pkgdir/etc/cloud/cloud.cfg ${pkgdir}/etc/cloud/cloud.cfg.ubuntu_default

  # Cloud.cfg crafted for archlinux
  # User has to copy this into place in /etc/cloud/cloud.cfg
  curl http://dl.dropbox.com/u/402975/archlinux.cloud.cfg -o archlinux.cloud.cfg
  cp archlinux.cloud.cfg ${pkgdir}/etc/cloud/archlinux.cloud.cfg
}