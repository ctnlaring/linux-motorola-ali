# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm/configs/(CHANGEME!)

pkgname="linux-motorola-ali"
pkgver=3.18.140
pkgrel=0
pkgdesc="Motorola Moto g6 kernel fork"
arch="armv7"
_carch="arm"
_flavor="motorola-ali"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps"
makedepends="perl sed installkernel bash gmp-dev bc linux-headers elfutils-dev devicepkg-dev"

# Compiler: latest GCC from Alpine
HOSTCC="${CC:-gcc}"
HOSTCC="${HOSTCC#${CROSS_COMPILE}}"

# Source
_repository="kernel-msm"
_commit="a1186768fc35f81c7f4d3d672896b2d946bb4030"
_config="config-${_flavor}.${arch}"
source="
        $pkgname-$_commit.tar.gz::https://github.com/Collinthegeek/${_repository}/archive/${_commit}.tar.gz
        $_config
	focaltech_mmi.patch
"
builddir="$srcdir/${_repository}-${_commit}"


prepare() {
	default_prepare
	downstreamkernel_prepare "$srcdir" "$builddir" "$_config" "$_carch" "$HOSTCC"
}

build() {
	unset LDFLAGS
	make ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	# kernel.release
	install -D "$builddir/include/config/kernel.release" \
		"$pkgdir/usr/share/kernel/$_flavor/kernel.release"

	# zImage (find the right one)
	cd "$builddir/arch/$_carch/boot"
	_target="$pkgdir/boot/vmlinuz-$_flavor"
	for _zimg in zImage-dtb Image.gz-dtb *zImage Image; do
		[ -e "$_zimg" ] || continue
		msg "zImage found: $_zimg"
		install -Dm644 "$_zimg" "$_target"
		break
	done
	if ! [ -e "$_target" ]; then
		error "Could not find zImage in $PWD!"
		return 1
	fi
}

sha512sums="af65e178439909bf316ef0d633b69d4791c602e2d02d781173260efcefdc37966d0cc33b7595e405a25b94cb650be7d9c23789eebe2a366c102fac2c667dae9e  linux-motorola-ali-a1186768fc35f81c7f4d3d672896b2d946bb4030.tar.gz
17a9116fae4a3ba54696fb472507cfaa13ce7dac01df72a87057e258db8f1229db2d51b5a952739f83de9a5d712a5866d3ae24f646e4995302f151023f3dbf60  config-motorola-ali.armv7"
