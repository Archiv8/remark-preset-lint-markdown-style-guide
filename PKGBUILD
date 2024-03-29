#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# ToDo: Add files: User documentation
# ToDo: Add files: Tooling
# FixMe: Namcap warnings and errors

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>


# pkgbase=
pkgname="remark-preset-lint-markdown-style-guide"
pkgver=5.1.2
pkgrel=2
# epoch=
pkgdesc="A remark lint preset to enforce consistency."
arch=("any")
url="https://github.com/remarkjs/remark-lint/tree/master/packages/remark-preset-lint-consistent"
license=("MIT")
# groups=()
depends=(
  "nodejs"
  "remark-lint"
)
# optdepends=()
makedepends=(
  "jq"
  "npm"
)
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
# changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/${pkgname}/-/${pkgname}-$pkgver.tgz"
# "CC-by-SA-v4.md"
# "CHANGELOG.md"
# "ISSUES.md"
# "LICENSE"
# "LICENSE.md"
# "MIT.md"
# "README.md"
)
noextract=("${pkgname}-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('3080219f3d300ceabf32a2ee7126803eab8a1851362394b1a918e0593f9918aed39aa4f1aeba39dded75fcf735f713d7e1ebe6cc8e4265dbda467228c4fdf2b1')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/${pkgname}-$pkgver.tgz

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
  local pkgjson="$pkgdir/usr/lib/node_modules/${pkgname}/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

rm -rf "$pkgdir/usr/lib/node_modules/root"

  # Install license
  # install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Create Archiv8 documentation folder
  # install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  # install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  # install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  # install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
 #  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  # install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
 #  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
