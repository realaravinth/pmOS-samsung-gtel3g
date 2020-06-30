# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm/configs/(CHANGEME!)

pkgname=linux-samsung-gtel3g
pkgver=3.10.17
pkgrel=0
pkgdesc="Samsung Galaxy Tab E 9.6 kernel fork"
arch="armv7"
_carch="arm"
_flavor="samsung-gtel3g"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="bash bc bison devicepkg-dev flex openssl-dev perl dtbtool-sprd linux-headers dtbtool"

# Source
_repository="kernel_samsung_gtel3g"
_commit="bcbcb2c5e1b9f65d7a70168b2b357b17b633c3cd"
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/realaravinth/$_repository/archive/$_commit.tar.gz
	$_config
	gcc7-give-up-on-ilog2-const-optimizations.patch
	gcc8-fix-put-user.patch
	fix_recordmcount.patch
	00_fix_return_address.patch
"
#	kernel-use-the-gnu89-standard-explicitly.patch
#"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
	. downstreamkernel_prepare
}

build() {
	unset LDFLAGS
	make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
	
	dtbTool -s 2048 -p scripts/dtc/ -o "$_outdir/arch/$_carch/boot/"dt.img "$_outdir/arch/$_carch/boot/dts"
}

package() {
	downstreamkernel_package "$builddir" "$pkgdir" "$_carch" "$_flavor" "$_outdir"

	# Master DTB (deviceinfo_bootimg_qcdt)
	install -Dm644 "$_outdir/arch/$_carch/boot"/dt.img "$pkgdir"/boot/dt.img
}

sha512sums="317eabf42737997a72cf0210b925ca2e96759949afed057ddc4b4e9c90b6cacdb4e7ff23c44fc89ef243c9f0da95154fcac59643b7e87c7407db4c7a10851326  linux-samsung-gtel3g-bcbcb2c5e1b9f65d7a70168b2b357b17b633c3cd.tar.gz
9319b003bc41a3f9ad3f7793ca17da265df0d1c5ecfe665e38605eac57b31909d774fc9f38de79c3f9bb74124db0dd8baade0129bc8f9cc0b94c7073cda9e149  config-samsung-gtel3g.armv7
77eba606a71eafb36c32e9c5fe5e77f5e4746caac292440d9fb720763d766074a964db1c12bc76fe583c5d1a5c864219c59941f5e53adad182dbc70bf2bc14a7  gcc7-give-up-on-ilog2-const-optimizations.patch
197d40a214ada87fcb2dfc0ae4911704b9a93354b75179cd6b4aadbb627a37ec262cf516921c84a8b1806809b70a7b440cdc8310a4a55fca5d2c0baa988e3967  gcc8-fix-put-user.patch
6aa11a75f422ac5c20cddfce23bff81940e61e65bc86fe1070c60714a6ccf631b2da70bff20e2b88e723706f0f233eb03540a8d9389adffd495592e8ab6bd82a  fix_recordmcount.patch
ea1d3b5a234fa565e3c1a792de48f4fc4e6023d281d303c8e319c7ef28edc5739ab0e4dea0139a41f0a5c7d03e27921ccaa214fd0ac5c72245a094ce60128864  00_fix_return_address.patch"
