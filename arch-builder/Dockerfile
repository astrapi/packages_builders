FROM archlinux:latest

# Simpsons-Mirror: core/extra/multilib via $repo-Variable, SSL für interne CA deaktiviert
RUN echo 'Server = https://mirror.simpsons.lan/files/archlinux/$repo/os/$arch' \
        > /etc/pacman.d/mirrorlist && \
    sed -i '/^\[options\]/a XferCommand = /usr/bin/curl -k -L -C - -f --no-progress-meter -o %o %u' /etc/pacman.conf && \
    printf '\n[simpsons]\nSigLevel = Optional TrustAll\nServer = https://mirror.simpsons.lan/files/archlinux/simpsons/os/$arch\n' \
        >> /etc/pacman.conf

RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm base-devel git sudo namcap && \
    pacman -Scc --noconfirm

RUN sed -i "s/#MAKEFLAGS=\"-j2\"/MAKEFLAGS=\"-j\$(nproc)\"/" /etc/makepkg.conf && \
    sed -i '/^OPTIONS=/s/\bdebug\b/!debug/' /etc/makepkg.conf

RUN useradd -m -G wheel makepkg && \
    echo "makepkg ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

COPY build.sh /usr/local/bin/build.sh
RUN chmod +x /usr/local/bin/build.sh

USER makepkg
WORKDIR /home/makepkg

# yay direkt von den GitHub-Releases installieren statt ueber das
# yay-bin-PKGBUILD von aur.archlinux.org -- das PKGBUILD laedt ohnehin nur
# genau dieses Release-Tarball herunter, macht also keinen Unterschied
# ausser dass wir nicht mehr von der Erreichbarkeit von AUR abhaengen
# (dort kam es zu TLS-Verbindungsabbruechen/Ausfaellen).
RUN cd /tmp && \
    ver=$(curl -fsSL https://api.github.com/repos/Jguer/yay/releases/latest | grep -oP '"tag_name":\s*"v\K[^"]+') && \
    curl -fsSL -o yay.tar.gz "https://github.com/Jguer/yay/releases/download/v${ver}/yay_${ver}_x86_64.tar.gz" && \
    mkdir yay-bin && tar -xzf yay.tar.gz -C yay-bin --strip-components=1 && \
    sudo install -Dm755 yay-bin/yay /usr/bin/yay && \
    rm -rf yay.tar.gz yay-bin

ENTRYPOINT ["/usr/local/bin/build.sh"]
