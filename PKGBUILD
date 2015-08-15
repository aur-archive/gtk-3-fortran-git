# Maintainer: James Tappin <jtappin at gmail dot com>

pkgname=gtk-3-fortran-git
pkgver=20131229
pkgrel=1
pkgdesc="A binding of the GTK+ 3.x libraries for Fortran 95/2003"
arch=('i686' 'x86_64')
url="https://github.com/jerryd/gtk-fortran/wiki"
license="GPL3"
depends=('gtk3')
optdepends=("plplot: For integrated plotting")
makedepends=('gcc-fortran>=4.6.0' 'git' 'cmake>=2.8.5')

# These are here for when there is a release and/or a package in the 
# binary repositories, and to allow programs to depend on gtk-2-fortran
# or gtk-3-fortran.
provides=("gtk-3-fortran" "gtk-fortran")
conflicts=("gtk-3-fortran")

_gitroot="git://github.com/jerryd/gtk-fortran.git"
_gitname="gtk-3-fortran"
_gitbranch="gtk3"

pkgver() {
    # Note that we must ensure that the tree has been checked out before 
    # we can generate the version number (and pkgver() is called before build())
    cd "$srcdir"
    msg "Connecting to GIT server...."
    if [[ -d "$_gitname" ]]; then
	cd "$_gitname" && git pull --depth=1 origin
	msg "The local files are updated."
    else
	git clone --single-branch --branch=$_gitbranch --depth=1 \
	    "$_gitroot" "$_gitname"
	cd "$_gitname"
    fi
    date --date=@`git rev-list --timestamp --max-count=1 HEAD | awk '{print$1}'` +%Y%m%d
}

build() {
    cd "$srcdir"
    msg "Connecting to GIT server...."
    
    if [[ -d "$_gitname" ]]; then
	cd "$_gitname" && git pull --depth=1 origin
	msg "The local files are updated."
    else
	git clone --single-branch --branch=$_gitbranch --depth=1 \
	    "$_gitroot" "$_gitname"
    fi

    msg "GIT checkout done or server timeout"
    msg "Starting build..."

    rm -rf "$srcdir/$_gitname-build"
    mkdir	"$srcdir/$_gitname-build"
    cd "$srcdir/$_gitname-build/"
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DINSTALL_EXAMPLES=y \
	-DNO_BUILD_EXAMPLES=y ../$_gitname
    make
}

package() {
    cd "$srcdir/$_gitname-build/"
    make DESTDIR=${pkgdir} install
    msg "N.B. Warnings about reference to \$srcdir are normal for Fortran"
}
