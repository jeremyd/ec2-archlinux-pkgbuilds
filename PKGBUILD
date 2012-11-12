pkgname=cloud-init-bzr
pkgver=722
pkgrel=1
pkgdesc="cloud-init bzr checkout from canonical"
arch=(any)
license=("GPLv3")
# cloud-init python lib requirements (according to Requires file):
# cheetah (aur), PrettyTable (aur), oauth (aur), boto (aur), configobj (community), pyyaml (community), argparse (aur)
# the ONLY non-match is we are using python2-yaml vs pyyaml.
depends=(systemd python2 python2-yaml python2-cheetah python2-prettytable python2-oauth2 python2-boto python2-argparse python2-configobj)
makedepends=('bzr' 'python2')
_bzrtrunk="lp:cloud-init"
_bzrmod="cloud-init"
# Archlinux specific cloud.cfg
source=(archlinux.cloud.cfg)
noextract=(archlinux.cloud.cfg)
sha1sums=(4e32767ac0e18f3b6f34cfb184af17c8a84d563c)

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
  echo Final install steps:
  echo Copy into place the archlinux config /etc/cloud/archlinux.cloud.cfg /etc/cloud/cloud.cfg 
  echo Enable the cloud-init services: cloud-init.service cloud-config.service and cloud-final.service.
  cp archlinux.cloud.cfg ${pkgdir}/etc/cloud/archlinux.cloud.cfg
}
