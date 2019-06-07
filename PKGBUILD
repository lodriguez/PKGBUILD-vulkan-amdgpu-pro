pkgname=vulkan-amdgpu-pro
pkgver=19.10_785425
pkgver_=${pkgver//_/-}-ubuntu-18.04
pkgrel=1
arch=('x86_64')
url='http://www.amd.com'
license=('custom:AMD')
makedepends=('wget')

DLAGENTS='https::/usr/bin/wget --referer https://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Install.aspx -N %u'

source=(https://drivers.amd.com/drivers/linux/amdgpu-pro-${pkgver_}.tar.xz)
sha256sums=('a0bd71417d0c0ddd404be8c86653135c4e0190a54bb8dc62eef231d5275f37bd')

build() {
  cd $srcdir
  ar -x ${startdir}/src/amdgpu-pro-${pkgver_}/vulkan-amdgpu-pro_${pkgver//_/-}_amd64.deb
  tar -xJf ${startdir}/src/data.tar.xz
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
