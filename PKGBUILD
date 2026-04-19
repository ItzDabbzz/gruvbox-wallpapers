# Maintainer: (upstream repo)

pkgname=gruvbox-wallpapers-git
pkgver=0
pkgrel=1
pkgdesc="Gruvbox wallpapers with gruvwall CLI helper"
arch=(any)
url="https://github.com/ItzDabbzz/gruvbox-wallpapers"
license=(custom)
depends=(bash findutils)
makedepends=(git)
optdepends=(
  "fzf: interactive picker"
  "fd: faster file discovery"
  "plasma-workspace: plasma-apply-wallpaperimage KDE backend"
  "qt6-tools: qdbus6 fallback for KDE backend"
  "qt5-tools: qdbus fallback for older Plasma"
  "swww: Hyprland swww wallpaper backend"
  "hyprland: hyprctl (needed for hyprpaper backend)"
  "hyprpaper: Hyprland hyprpaper wallpaper backend"
  "jq: improved Hyprland monitor detection"
)
provides=(gruvbox-wallpapers gruvwall)
conflicts=(gruvbox-wallpapers)

source=("git+$url.git")
sha256sums=(SKIP)

pkgver() {
  cd "$srcdir/gruvbox-wallpapers"
  git describe --long --tags --always 2>/dev/null \
    | sed 's/^v//; s/-/./g' \
    || printf '0.r%s.g%s\n' "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

package() {
  cd "$srcdir/gruvbox-wallpapers"

  install -Dm755 bin/gruvwall "$pkgdir/usr/bin/gruvwall"

  install -d "$pkgdir/usr/share/gruvbox-wallpapers"
  cp -a wallpapers "$pkgdir/usr/share/gruvbox-wallpapers/"

  install -Dm644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"

  install -Dm644 NOTICE "$pkgdir/usr/share/licenses/$pkgname/NOTICE"
}
