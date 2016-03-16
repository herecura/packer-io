# Maintainer: BlackIkeEagle <ike DOT devolder AT gmail DOT com>
# Contributor: Danilo Kuehn <dk[at]nogo-software[dot]de>

_name=packer
pkgname=packer-io
pkgver=0.10.0
pkgrel=1
pkgdesc="tool for creating identical machine images for multiple platforms from a single source configuration."
url="http://www.packer.io"
arch=('x86_64' 'i686')
license=('MPL2')
depends=('glibc')
makedepends=('git' 'go' 'mercurial')
source=(
	"$pkgname-bash-completion::git://github.com/mrolli/packer-bash-completion.git"
	"$pkgname-zsh-completion::git://github.com/gunzy83/packer-zsh-completion.git"
)
  
md5sums=(
	'SKIP'
	'SKIP'
)

prepare() {
	export GOPATH="$srcdir/go"
	export GOROOT=$(go env GOROOT)
	export PATH="$PATH:$GOPATH/bin"

	[[ ! -d "$srcdir/go" ]] && mkdir -p "$srcdir/go"
	
	go get -u -d github.com/mitchellh/packer
	cd "$srcdir/go/src/github.com/mitchellh/packer"
	git checkout -b v$pkgver v$pkgver

    # dont check version.go
    sed -e '/VersionPrerelease = ""/ {N;N;N;d;}' -i Makefile
}

build() {
	cd "$srcdir/go/src/github.com/mitchellh/packer"

	make dev
}

check() {
	cd "$srcdir/go/src/github.com/mitchellh/packer"

	make test
}

package() {
	cd "$srcdir/go/src/github.com/mitchellh/packer/bin"

	install -Dm755 packer "${pkgdir}/usr/bin/packer-io"

	cd "$srcdir/go/src/github.com/mitchellh/packer"
	# license
	install -Dm644 "${srcdir}/go/src/github.com/mitchellh/packer/LICENSE" \
		"${pkgdir}/usr/share/licenses/$pkgname/LICENSE"

	# completion not working atm, will investigate later

	## bash completion
	#install -Dm0644 "${srcdir}/packer-io-bash-completion/packer" \
		#"${pkgdir}/usr/share/bash-completion/completions/packer"

	#install -Dm644 "${srcdir}/packer-io-bash-completion/LICENSE" \
		#"${pkgdir}/usr/share/licenses/$pkgname/LICENSE-bash-completion"

	## zsh completion
	#install -Dm0644 "${srcdir}/packer-io-zsh-completion/_packer" \
		#"${pkgdir}/usr/share/zsh/site-functions/_packer"

	#install -Dm644 "${srcdir}/packer-io-zsh-completion/LICENSE" \
		#"${pkgdir}/usr/share/licenses/$pkgname/LICENSE-zsh-completion"
}
