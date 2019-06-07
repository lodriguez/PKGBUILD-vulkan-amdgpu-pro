pkgname=vulkan-amdgpu-pro
pkgver=18.30_633530
pkgrel=1
arch=('x86_64')
url='http://www.amd.com'
license=('custom:AMD')
makedepends=('wget')

DLAGENTS='https::/usr/bin/wget --referer https://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Install.aspx -N %u'

source=(https://www2.ati.com/drivers/linux/ubuntu/amdgpu-pro-${pkgver//_/-}.tar.xz)
sha256sums=('2fbc37aef0c259855178eb88b0f4914625619d204e8f39cff1aba49e6d6b9d2a')


# extracts a debian package
# $1: deb file to extract
extract_deb() {
	local tmpdir="$(basename "${1%.deb}")"
	rm -Rf "$tmpdir"
	mkdir "$tmpdir"
	cd "$tmpdir"
	ar x "$1"
	tar -C "${pkgdir}" -xf data.tar.xz
}
# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: library dir
# $2: destination (optional)
move_libdir() {
	local libdir="usr/lib"
	if [ -n "$2" ]; then
		libdir="$2"
	fi
	if [ -d "$1" ]; then
		if [ -d "${pkgdir}/${libdir}" ]; then
			cp -ar -t "${pkgdir}/${libdir}/" "$1"/*
			rm -rf "$1"
		else
			mkdir -p "${pkgdir}/${libdir}"
			mv -t "${pkgdir}/${libdir}/" "$1"/*
			rmdir "$1"
		fi
	fi
}

package_amdgpu-pro-vulkan () {
	pkgdesc="The AMDGPU Pro Vulkan driver"
	arch=('x86_64')
	provides=('vulkan-driver')

	extract_deb "${srcdir}"/amdgpu-pro-${pkgver//_/-}/./vulkan-amdgpu-pro_${pkgver//_/-}_amd64.deb

	move_libdir "${pkgdir}/lib"

	# extra_commands:
	mkdir -p "${pkgdir}"/usr/share/vulkan/icd.d/
	mv "${pkgdir}"/etc/vulkan/icd.d/amd_icd64.json "${pkgdir}"/usr/share/vulkan/icd.d/
	sed -i "s@abi_versions\(.*\)0.9.0\(.*\)@api_version\11.0.61\2@" "${pkgdir}"/usr/share/vulkan/icd.d/amd_icd64.json
	rm -rf "${pkgdir}"/etc/vulkan/
}

package_lib32-amdgpu-pro-vulkan () {
	pkgdesc="The AMDGPU Pro Vulkan driver (32bit libraries)"
	arch=('x86_64')
	provides=('lib32-vulkan-driver')

	extract_deb "${srcdir}"/amdgpu-pro-${pkgver//_/-}/./vulkan-amdgpu-pro_${pkgver//_/-}_i386.deb

	move_libdir "${pkgdir}/lib" "usr/lib32"

	# extra_commands:
	mkdir -p "${pkgdir}"/usr/share/vulkan/icd.d/
	mv "${pkgdir}"/etc/vulkan/icd.d/amd_icd32.json "${pkgdir}"/usr/share/vulkan/icd.d/
	sed -i "s@abi_versions\(.*\)0.9.0\(.*\)@api_version\11.0.61\2@" "${pkgdir}"/usr/share/vulkan/icd.d/amd_icd32.json
	rm -rf "${pkgdir}"/etc/vulkan/

	# lib32 cleanup
	rm -rf "${pkgdir}"/usr/share/doc 
}
