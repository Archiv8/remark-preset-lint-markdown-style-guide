# Maintainer: Archiv8 <archiv8@artisteducator.com>
# Contributor: Archiv8 <archiv8@artisteducator.com>

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
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('1ff8d2a0bbd363c69b51c07eeffd3ad88da81debe51dc1ddadf44c3f644c875f50035c2601180d127ded65f7415c5ab54e9ce17af1a06fa57049d3a3754f0d39'
            'a8df88b9fafbe0715969bff4d7b4f8cdc25af2d177a34e99635693d0899d47bb893b5f9b4728eb91ee546aee32d77c5185a17154bb4f5a91912bc6810423a52b'
            '2eb6033ace369245ee4d34ae0cd7932ceff68e31dd13d95ffa50967407e7ff5f20cd5370f49096dd6d0267cce5ba9613f55e9e13addb66fa2dabe11f990ab049'
            '383ae3c08c2c2e2ff205a0d8ddf7784f631133d395837963747700fea304adf968a2c298600fbba460a5a30a8cf2ef32738dcbf8f03ca2246e57ab54c9b62eca'
            '0a033730e23dc9c031929022e9be6e6a0bae20229ce6f0ae9389494f563e3e31a2bc996c755af89ad9c45dffd8f32fd774947164afc44e8edb86a3f80a7e96b4'
            'b0518febd95410b3b7fcd27d06f4d7829eeffdc84ad37bdeece951b7ded871941edc8f3d4b54ea6f03d66922e36629a2439a875f2442eb3d64851e02961084f3'
            '832304189cefd4bba75bca372d46bcdd4995c0f1f7734f2ee3a0d878c4b9b7599e80a69a80fa71a606eb32efb329178e745c11044bf9fcfb9f9dd86072b12eca'
            '191eb8575168b0ca85679da4e4255e5d941b297af06e99f3a1f8ea39f930159013a2575ba13b1c401817ea9b92e509b51087777da60096c20cfa20eccc68ba67')

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
