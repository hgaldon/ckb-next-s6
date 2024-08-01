# Maintainer: revsuine <paradoor at protonmail dot com>
pkgname=ckb-next-s6
pkgver=1.0.0
pkgrel=1
pkgdesc="s6 service for ckb-next"
arch=("i686" "x86_64" "pentium4")
url="https://codeberg.org/revsuine/${pkgname}"
license=("GPL-3.0-or-later")
depends=("s6" "ckb-next")
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/${pkgver}.tar.gz")
sha256sums=("55c8efca2bddac983920aabc08651902462b9a0cfca84fb14958483be11ba395")

package() {
    service_dir="/etc/s6/sv"
    echo "Warning: This package assumes that ${service_dir} is where you store s6 services. If your service directory is elsewhere, you can easily edit the PKGBUILD to reflect your service directory."

    install_dir="${pkgdir}${service_dir}/ckb-next-srv"
    sudo mkdir -p "${install_dir}"
    sudo mkdir -p "${install_dir}-log"

    # Create the run script for the service
    install -Dm 755 /dev/stdin "${install_dir}/run" <<'EOF'
#!/bin/sh
exec /usr/bin/ckb-next-daemon
EOF

    # Create the type file for the service
    sudo install -Dm 644 /dev/stdin "${install_dir}/type" <<'EOF'
longrun
EOF

    # Create the producer-for file
    sudo install -Dm 644 /dev/stdin "${install_dir}/producer-for" <<'EOF'
ckb-next-log
EOF

    # Create the run script for the logger
    sudo install -Dm 755 /dev/stdin "${install_dir}-log/run" <<'EOF'
#!/bin/sh
exec s6-log /var/log/ckb-next
EOF

    # Create the type file for the logger
    sudo install -Dm 644 /dev/stdin "${install_dir}-log/type" <<'EOF'
longrun
EOF

    # Create the consumer-for file for the logger
    sudo install -Dm 644 /dev/stdin "${install_dir}-log/consumer-for" <<'EOF'
ckb-next-srv
EOF

    # Reload the s6-rc database
    sudo s6-db-reload

    #Add and start service
    sudo touch /etc/s6/adminsv/default/contents.d/ckb-next
    sudo s6-rc -u change ckb-next-srv
}
