# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Johannes Löthberg <johannes@kyriasis.com>
# Contributor: Ivan Shapovalov <intelfx@intelfx.name>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
fi
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=pynacl
_Pkg="PyNaCl"
pkgname="${_py}-${_pkg}"
pkgver=1.5.0
pkgrel=3
_pkgdesc=(
  'Python binding to the Networking'
  'and Cryptography (NaCl) library'
)
_pypi="https://pypi.python.org/pypi"
url="${_py}/${_Pkg}"
arch=(
  'aarch64'
  'arm'
  'armv7l'
  'i686'
  'mips'
  'pentium4'
  'x86_64'
)
license=(
  'Apache-2.0'
)
depends=(
  "${_libc}"
  'libsodium'
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-cffi"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-pycparser"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-hypothesis"
)
_pypa="https://pypi.org/packages/source"
_tarname="${_Pkg}-${pkgver}"
source=(
  "${_pypa}/${_Pkg::1}/${_Pkg}/${_tarname}tar.gz"
)
sha512sums=(
  'cea3e4556432588630382abae6debf9203c7f55da286509da547a7921e4dbad98c915743625c68e5f7187fcaf6d4cdaf7ed2ed3ba60bd4c10ae6e3f88608dc65'
)

build() {
  export \
    SODIUM_INSTALL="system"
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  local \
    _python_version
  _python_version=$( \
    "${_py}" \
      -c \
        'import sys; print("".join(map(str, sys.version_info[:2])))')
  cd \
    "${_tarname}"
  PYTHONPATH="${PWD}/build/lib.linux-${CARCH}-cpython-${_python_version}" \
  pytest
}

package() {
  export \
    SODIUM_INSTALL=system
  cd \
   "${_tarname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    "dist/"*".whl"
}
