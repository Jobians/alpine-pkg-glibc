# Maintainer: Sasha Gerrand <alpine-pkgs@sgerrand.com>

pkgname="glibc"
pkgver="2.39"
pkgrel="0"
pkgdesc="GNU C Library compatibility layer"
arch="aarch64"
url="https://github.com"
license="LGPL"
# !tracedeps is the key: it stops the main package from failing on the symlinks
options="!check !scanelf !tracedeps"

source="
https://github.com/sgerrand/docker-glibc-builder/releases/download/unreleased/glibc-bin-2.39-0-aarch64.tar.gz
ld.so.conf
"

subpackages="$pkgname-bin $pkgname-dev $pkgname-i18n:i18n:noarch"
triggers="$pkgname-bin.trigger=/lib:/usr/lib:/usr/glibc-compat/lib"

package() {
    mkdir -p "$pkgdir/lib" "$pkgdir/usr/glibc-compat/lib/locale" "$pkgdir"/usr/glibc-compat/lib64 "$pkgdir"/etc
    cp -a "$srcdir"/usr "$pkgdir"
    cp "$srcdir"/ld.so.conf "$pkgdir"/usr/glibc-compat/etc/ld.so.conf
    
    # Cleanup files moved to subpackages
    rm -rf "$pkgdir"/usr/glibc-compat/etc/rpc \
           "$pkgdir"/usr/glibc-compat/bin \
           "$pkgdir"/usr/glibc-compat/sbin \
           "$pkgdir"/usr/glibc-compat/lib/gconv \
           "$pkgdir"/usr/glibc-compat/lib/getconf \
           "$pkgdir"/usr/glibc-compat/lib/audit \
           "$pkgdir"/usr/glibc-compat/share \
           "$pkgdir"/usr/glibc-compat/var

    # Main package symlinks
    ln -s /usr/glibc-compat/lib/ld-linux-aarch64.so.1 "$pkgdir"/lib/ld-linux-aarch64.so.1
    ln -s /usr/glibc-compat/lib/ld-linux-aarch64.so.1 "$pkgdir"/usr/glibc-compat/lib64/ld-linux-aarch64.so.1
    ln -s /usr/glibc-compat/etc/ld.so.cache "$pkgdir"/etc/ld.so.cache
}

bin() {
    pkgdesc="GNU C Library binaries"
    provides="
        so:ld-linux-aarch64.so.1=1
        so:libc.so.6=6
        so:libm.so.6=6
        so:libresolv.so.2=2
        so:librt.so.1=1
        so:libdl.so.2=2
        so:libpthread.so.0=0
        so:libutil.so.1=1
    "
    depends="$pkgname bash libgcc"
    
    mkdir -p "$subpkgdir"/usr/glibc-compat
    cp -a "$srcdir"/usr/glibc-compat/bin "$subpkgdir"/usr/glibc-compat
    cp -a "$srcdir"/usr/glibc-compat/sbin "$subpkgdir"/usr/glibc-compat
}

i18n() {
    pkgdesc="GNU C Library i18n data"
    depends="$pkgname-bin"
    mkdir -p "$subpkgdir"/usr/glibc-compat
    cp -a "$srcdir"/usr/glibc-compat/share "$subpkgdir"/usr/glibc-compat
}

sha512sums="
176a4f6dba9af88b96e7337897492093988d69f0b1c8dd88927a83318d0b7f65a226ca62b508e817569cff76927fd8639e20973cef73f477f2245351e5874d69  glibc-bin-2.39-0-aarch64.tar.gz
2912f254f8eceed1f384a1035ad0f42f5506c609ec08c361e2c0093506724a6114732db1c67171c8561f25893c0dd5c0c1d62e8a726712216d9b45973585c9f7  ld.so.conf
"
