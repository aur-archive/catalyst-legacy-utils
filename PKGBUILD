# Maintainer: Giuseppe Marco "zeld" Randazzo <gmrandazzo@gmail.com>
# Great Contributor: Vi0L0 (maintainer of other catalyst drivers)

pkgname=catalyst-legacy-utils
pkgver=13.1
pkgrel=2
pkgdesc="AMD/ATI Catalyst drivers utilities and libraries with OpenCL implementation. SUPPORTED RADEONS: HD 2 3 4 xxx"
arch=('i686' 'x86_64')
url="http://www.ati.amd.com"
license=('custom')
depends=('xorg-server>=1.7.0' 'xorg-server<1.13.0' 'netkit-bsd-finger' 'libxrandr' 'libsm' 'fontconfig' 'libxcursor' 'libxi' 'gcc-libs' 'libxinerama')
optdepends=('qt: to run ATi Catalyst Control Center (amdcccle)'
	    'libxxf86vm: to run ATi Catalyst Control Center (amdcccle)'
	    'opencl-headers: headers necessary for OpenCL development')
conflicts=('catalyst-utils' 'catalyst-test' 'nvidia-utils' 'libgl' 'libcl')
provides=('libgl' "libatical=${pkgver}" 'libcl' 'dri' 'libtxc_dxtn')
install=${pkgname}.install


source=(
    #http://www2.ati.com/drivers/legacy/amd-driver-installer-catalyst-13.1-legacy-linux-x86.x86_64.zip
	http://www2.ati.com/drivers/legacy/amd-driver-installer-catalyst-${pkgver}-legacy-linux-x86.x86_64.zip
    catalyst.sh
    amdcccle.desktop
    atieventsd.sh)

md5sums=('c07fd1332abe4c742a9a0d0e0d0a90de'
         'bdafe749e046bfddee2d1c5e90eabd83'
         '4efa8414a8fe9eeb50da38b5522ef81d'
         'f729bf913613f49b0b9759c246058a87')
build() {
  ## Unpack archive
  /bin/sh ./amd-driver-installer-catalyst-${pkgver}-legacy-linux-x86.x86_64.run --extract archive_files
}

package() {
  ## Install userspace tools and libraries
    # Create directories
      install -m755 -d "${pkgdir}/etc/ati"
      install -m755 -d "${pkgdir}/etc/rc.d"
      install -m755 -d "${pkgdir}/etc/profile.d"
      install -m755 -d "${pkgdir}/etc/acpi/events"
      install -m755 -d "${pkgdir}/etc/security/console.apps"
      install -m755 -d "${pkgdir}/etc/OpenCL/vendors"  # since 11.11

      install -m755 -d "${pkgdir}/usr/lib/xorg/modules/dri"
      install -m755 -d "${pkgdir}/usr/lib/xorg/modules/drivers"
      install -m755 -d "${pkgdir}/usr/lib/xorg/modules/extensions"
      install -m755 -d "${pkgdir}/usr/lib/xorg/modules/extensions/fglrx" # since 11.4
      install -m755 -d "${pkgdir}/usr/lib/xorg/modules/linux"
      install -m755 -d "${pkgdir}/usr/lib/dri"
      install -m755 -d "${pkgdir}/usr/lib/fglrx" # since 11.4

      install -m755 -d "${pkgdir}/usr/bin"
      install -m755 -d "${pkgdir}/usr/sbin"

      install -m755 -d "${pkgdir}/usr/include/X11/extensions"
      install -m755 -d "${pkgdir}/usr/include/GL"

      install -m755 -d "${pkgdir}/usr/share/applications"
      install -m755 -d "${pkgdir}/usr/share/ati/amdcccle"
      install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
      install -m755 -d "${pkgdir}/usr/share/man/man8"
      install -m755 -d "${pkgdir}/usr/share/pixmaps"

    # X.org driver
      if [ "${CARCH}" = "i686" ]; then
	cd "${srcdir}/archive_files/xpic/usr/X11R6/lib/modules" || return 1
      elif [ "${CARCH}" = "x86_64" ]; then
	cd "${srcdir}/archive_files/xpic_64a/usr/X11R6/lib64/modules" || return 1
      fi

     # *.a added in 11.2, and removed in 11.3...
      #install -m644 *.a "${pkgdir}/usr/lib/xorg/modules/" || return 1
      install -m755 *.so "${pkgdir}/usr/lib/xorg/modules/" || return 1
      install -m755 drivers/*.so "${pkgdir}/usr/lib/xorg/modules/drivers/" || return 1
      install -m755 linux/*.so "${pkgdir}/usr/lib/xorg/modules/linux/" || return 1
      #install -m755 extensions/libglx.so "${pkgdir}/usr/lib/xorg/modules/extensions/" || return 1 #before 11.4
      install -m755 extensions/fglrx/fglrx-libglx.so "${pkgdir}/usr/lib/xorg/modules/extensions/fglrx/fglrx-libglx.so" || return 1 # since 11.5
      ln -snf /usr/lib/xorg/modules/extensions/fglrx/fglrx-libglx.so "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so" # since 11.4
      #install -m755 extensions/libdri.so "${pkgdir}/usr/lib/xorg/modules/extensions/libdri.ati" || return 1

    # Controlcenter / libraries
      if [ "${CARCH}" = "i686" ]; then
	cd "${srcdir}/archive_files/arch/x86/usr" || return 1
	_lib=lib
      elif [ "${CARCH}" = "x86_64" ]; then
	cd "${srcdir}/archive_files/arch/x86_64/usr" || return 1
	_lib=lib64
      fi

      install -m755 X11R6/bin/* "${pkgdir}/usr/bin/" || return 1
      install -m755 sbin/* "${pkgdir}/usr/sbin/" || return 1
      #install -m755 X11R6/${_lib}/*.so* "${pkgdir}/usr/lib/" || return #before 11.4
      install -m755 X11R6/${_lib}/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/fglrx" || return 1 # since 11.5
      ln -snf /usr/lib/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/fglrx/libGL.so.1.2" # since 11.4
      ln -snf /usr/lib/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/fglrx-libGL.so.1.2" # since 11.4
      ln -snf /usr/lib/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/libGL.so.1.2" # since 11.4
      ln -snf /usr/lib/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/libGL.so.1" # since 11.4
      ln -snf /usr/lib/fglrx/fglrx-libGL.so.1.2 "${pkgdir}/usr/lib/libGL.so" # since 11.4
      install -m755 X11R6/${_lib}/libAMDXvBA.so.1.0 "${pkgdir}/usr/lib/" || return 1 # since 11.4
      ln -snf libAMDXvBA.so.1.0 "${pkgdir}/usr/lib/libAMDXvBA.so.1" # since 11.4
      ln -snf libAMDXvBA.so.1.0 "${pkgdir}/usr/lib/libAMDXvBA.so" # since 11.4
      install -m755 X11R6/${_lib}/libatiadlxx.so "${pkgdir}/usr/lib/" || return 1 # since 11.4
      install -m755 X11R6/${_lib}/libfglrx_dm.so.1.0 "${pkgdir}/usr/lib/" || return 1 # since 11.4
      install -m755 X11R6/${_lib}/libXvBAW.so.1.0 "${pkgdir}/usr/lib/" || return 1 # since 11.4
      ln -snf libXvBAW.so.1.0 "${pkgdir}/usr/lib/libXvBAW.so.1" # since 11.4
      ln -snf libXvBAW.so.1.0 "${pkgdir}/usr/lib/libXvBAW.so" # since 11.4
      install -m644 X11R6/${_lib}/*.a "${pkgdir}/usr/lib/" || return 1 # really needed?
      install -m644 X11R6/${_lib}/*.cap "${pkgdir}/usr/lib/" || return 1
      install -m755 X11R6/${_lib}/modules/dri/*.so "${pkgdir}/usr/lib/xorg/modules/dri/" || return 1
      install -m755 ${_lib}/*.so* "${pkgdir}/usr/lib/" || return 1

    ## QT libs (only 2 files) - un-comment 2 lines below if you don't want to install qt package
#      install -m755 -d "${pkgdir}/usr/share/ati/${_lib}"
#      install -m755 share/ati/${_lib}/*.so* "${pkgdir}/usr/share/ati/${_lib}/" || return 1

      ln -snf /usr/lib/xorg/modules/dri/fglrx_dri.so ${pkgdir}/usr/lib/dri/fglrx_dri.so
      ln -snf libfglrx_dm.so.1.0 "${pkgdir}/usr/lib/libfglrx_dm.so.1"
      ln -snf libfglrx_dm.so.1.0 "${pkgdir}/usr/lib/libfglrx_dm.so"
      ln -snf libatiuki.so.1.0 "${pkgdir}/usr/lib/libatiuki.so.1"
      ln -snf libatiuki.so.1.0 "${pkgdir}/usr/lib/libatiuki.so"
      ln -snf libOpenCL.so.1 "${pkgdir}/usr/lib/libOpenCL.so" # since 11.11


      cd "${srcdir}"/archive_files/common
      install -m644 etc/ati/* "${pkgdir}/etc/ati/" || return 1
      chmod 755 "${pkgdir}/etc/ati/authatieventsd.sh" || return 1

      #security provided with 10.9, is it working fine?
      install -m644 etc/security/console.apps/amdcccle-su "${pkgdir}/etc/security/console.apps/" || return 1

     # *.h removed in 11.3...
      #install -m644 usr/X11R6/include/X11/extensions/*.h "${pkgdir}/usr/include/X11/extensions/" || return 1
      install -m755 usr/X11R6/bin/amdupdaterandrconfig "${pkgdir}/usr/bin/" || return 1
      install -m644 usr/include/GL/*.h "${pkgdir}/usr/include/GL/" || return 1
      install -m755 usr/sbin/*.sh "${pkgdir}/usr/sbin/" || return 1
      install -m644 usr/share/ati/amdcccle/* "${pkgdir}/usr/share/ati/amdcccle/" || return 1
      install -m644 usr/share/icons/*.xpm "${pkgdir}/usr/share/pixmaps/" || return 1
      install -m644 usr/share/man/man8/*.8 "${pkgdir}/usr/share/man/man8/" || return 1
      install -m644 "${srcdir}/amdcccle.desktop" "${pkgdir}/usr/share/applications/" || return 1

    # ACPI example files
      install -m755 usr/share/doc/fglrx/examples/etc/acpi/*.sh "${pkgdir}/etc/acpi/" || return 1
      sed -i -e 's/usr\/X11R6/usr/g' "${pkgdir}/etc/acpi/ati-powermode.sh" || return 1
      install -m644 usr/share/doc/fglrx/examples/etc/acpi/events/* "${pkgdir}/etc/acpi/events/" || return 1

    # Add ATI Events Daemon launcher
      install -m755 "${srcdir}/atieventsd.sh" "${pkgdir}/etc/rc.d/atieventsd" || return 1

    # thanks to cerebral, we dont need that damned symlink
      install -m755 "${srcdir}/catalyst.sh" "${pkgdir}/etc/profile.d/" || return 1

    # License
      install -m644 "${srcdir}/archive_files/LICENSE.TXT" "${pkgdir}/usr/share/licenses/${pkgname}/" || return 1


    # since 11.11 : opencl files
      if [ "${CARCH}" = "i686" ]; then
	cd "${srcdir}/archive_files/arch/x86" || return 1
	_arc=32
      elif [ "${CARCH}" = "x86_64" ]; then
	cd "${srcdir}/archive_files/arch/x86_64" || return 1
	_arc=64
      fi

      # since 11.11: amd's vendor file for it's opencl library
      install -m644 etc/OpenCL/vendors/amdocl${_arc}.icd "${pkgdir}/etc/OpenCL/vendors/" || return 1

      # since 11.11: clinfo binary
      install -m755 usr/bin/clinfo "${pkgdir}/usr/bin/" || return 1
}
