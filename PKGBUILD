# $Id$
# Maintainer: Jason Graham <jason@the-graham.com>
# Contributor: Thomas Baechler <thomas@archlinux.org>
# Contributor: James Rayner <iphitus@gmail.com>

_pkgbasename=nvidia-utils-304xx
pkgname=lib32-$_pkgbasename
pkgver=304.64
pkgrel=1
pkgdesc="NVIDIA drivers utilities and libraries. (32-bit)"
arch=('x86_64')
url="http://www.nvidia.com/"
depends=('lib32-libxvmc' 'lib32-zlib' 'lib32-gcc-libs' 'nvidia-304xx-utils')
conflicts=('lib32-libgl')
provides=('lib32-libgl')
license=('custom')
options=('!strip')

_arch='x86'
_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
md5sums=('6964415cf648a5f4f38117b168369de2')

build() {
    cd "${srcdir}"
    sh ${_pkg}.run --extract-only
}

package() {
    cd "${srcdir}/${_pkg}"

    # OpenGL library
    install -D -m755 libGL.so.${pkgver} "${pkgdir}/usr/lib32/libGL.so.${pkgver}"
    # OpenGL core library
    install -D -m755 libnvidia-glcore.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-glcore.so.${pkgver}"
    # XvMC
    install -D -m644 libXvMCNVIDIA.a "${pkgdir}/usr/lib32/libXvMCNVIDIA.a"
    install -D -m755 libXvMCNVIDIA.so.${pkgver} "${pkgdir}/usr/lib32/libXvMCNVIDIA.so.${pkgver}"
    # VDPAU
    install -D -m755 libvdpau_nvidia.so.${pkgver} "${pkgdir}/usr/lib32/vdpau/libvdpau_nvidia.so.${pkgver}"
    # CUDA
    install -D -m755 libcuda.so.${pkgver} "${pkgdir}/usr/lib32/libcuda.so.${pkgver}"
    install -D -m755 libnvcuvid.so.${pkgver} "${pkgdir}/usr/lib32/libnvcuvid.so.${pkgver}"
    # nvidia-tls library
    install -D -m755 tls/libnvidia-tls.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-tls.so.${pkgver}"
    # OpenCL
    install -D -m755 libnvidia-compiler.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-compiler.so.${pkgver}"
    install -D -m755 libOpenCL.so.1.0.0 "${pkgdir}/usr/lib32/libOpenCL.so.1.0.0"

    install -D -m755 libnvidia-cfg.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-cfg.so.${pkgver}"
    install -D -m755 libnvidia-ml.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-ml.so.${pkgver}"

    # create soname links
    for _lib in $(find "${pkgdir}" -name '*.so*'); do
        _soname="$(dirname ${_lib})/$(LC_ALL=C readelf -d "$_lib" | sed -nr 's/.*Library soname: \[(.*)\].*/\1/p')"
        if [ ! -e "${_soname}" ]; then
            ln -s "$(basename ${_lib})" "${_soname}"
            ln -s "$(basename ${_soname})" "${_soname/.[0-9]*/}"
        fi
    done

    rm -rf "${pkgdir}"/usr/{include,share,bin}
    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s $_pkgbasename "${pkgdir}/usr/share/licenses/${pkgname}"
}
