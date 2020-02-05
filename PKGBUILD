# shellcheck shell=bash
# Maintainer: Ross Clark <https://github.com/Archiv8/nodejs-remark-preset-lint-markdown-style-guide/issues>
# Contributor: Ross Clark <https://github.com/Archiv8/nodejs-remark-preset-lint-markdown-style-guide/issues>

_relname="remark-preset-lint-markdown-style-guide"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=2.1.3
pkgrel=1
# epoch=
pkgdesc="remark lint preset that follows the markdown style guide recommendations"
arch=("any")
url="https://github.com/remarkjs/remark-lint/tree/master/packages/remark-preset-lint-markdown-style-guide"
license=("MIT")
# groups=()
depends=("nodejs")
# optdepends=()
makedepends=("jq" "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE"
"LICENSE.md"
"MIT.md"
"README.md"
)
# noextract=()
# validpgpkeys=()
sha256sums=('3bb0d55d83267de275579b2ab3362be636018bc5c529857ed91efff8ed7d45d6'
            '49fb2df507d496aab0958f436983723f3075c343f880345a90e5bd447ed35f0b'
            'a9633d5d79cd52cf932324dff9070e2febf47c9723830cb5b319797190170769'
            'cd19b8386331293497b786233164cf964a7a8cb210192042d38315db8b278472'
            'ed52f87c3904a045931ff47fef50c31840b2f80f3618ce7a9d188918a9f83357'
            'aeabcc7206f6559cfe8c912126895a48652e3cf2c18e73c422ed55e627f1e85f'
            'cfeedde795fa6b52e454665fc050d23bcdb678364d2b051b9e0bf6fd111708f5')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done <   <(find "$pkgdir" -not -group root -not -user root)

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # Remove references to $pkgdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

  # Install license
  install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Create Archiv8 documentation folder
  install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
