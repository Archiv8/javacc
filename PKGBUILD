#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [FixMe]: Namcap warnings and errors
# [FixMe]: Why can't I generated sha512sums?

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

# Based on AUR package by Rémi Saurel, patadune@gmail.com, and Matthew Longley randomticktock@gmail.com

pkgname=javacc
pkgver=7.0.10
pkgrel=1
pkgdesc="Parser/scanner generator for Java"
arch=('any')
url="http://javacc.org/"
license=('BSD')
depends=(
    'java-environment'
    'apache-ant'
)
makedepends=(
    'git'
)
source=(
    "git+https://github.com/javacc/javacc.git#tag=javacc-$pkgver"
)
sha512sums=(
    "SKIP"
)

build() {
    cd $srcdir/$pkgname
    ant
}

package() {
    cd $srcdir/$pkgname

    install -D LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    mkdir -m755 -p "$pkgdir/usr/share/java/$pkgname/bin" "$pkgdir/usr/bin"

    # install examples
    cp -R "examples/" "$pkgdir/usr/share/java/$pkgname/examples"

    # install documentation
    cp -R "docs/" "$pkgdir/usr/share/java/$pkgname/docs"

    # install jar
    install -m755 -D "target/$pkgname-$pkgver.jar" "$pkgdir/usr/share/java/$pkgname/bin/lib/$pkgname.jar"
    ln -s /usr/share/java/$pkgname/bin/lib/javacc.jar $pkgdir/usr/share/java/javacc.jar

    # generate scripts to allow direct execution
    for i in jjtree jjdoc javacc; do
        printf "#\!bin/sh\nJAR=\"/usr/share/java/$pkgname/bin/lib/javacc.jar\"\n\njava -classpath \"\$JAR\" $i \"\$@\"\n" >"$pkgdir/usr/share/java/$pkgname/bin/$i"
        ln -s "/usr/share/java/$pkgname/bin/$i" "$pkgdir/usr/bin/$i"
    done

    # Set permissions
    chmod -R 755 "$pkgdir/usr/share/java/$pkgname/bin" "$pkgdir/usr/bin"
}
