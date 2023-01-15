FROM alpine:3.17 AS verify
RUN apk add --no-cache curl tar zstd

# https://gitlab.archlinux.org/archlinux/archlinux-docker/-/releases/v20230108.0.116909
RUN ROOTFS="$(curl -sOJL -w "%{filename_effective}" "https://gitlab.archlinux.org/archlinux/archlinux-docker/-/package_files/3870/download")" && \
    echo "8ec202bfdd763584babd7b09495a8d96ab2b08ed46578f9e85e15871805d0206  base-20230108.0.116909.tar.zst" > /tmp/rootfs.tar.sha256 && \
    cat /tmp/rootfs.tar.sha256 && \
    sha256sum -c /tmp/rootfs.tar.sha256 && \
    mkdir /rootfs && \
    tar -C /rootfs --extract --file "${ROOTFS}"

FROM scratch AS root
COPY --from=verify /rootfs/ /

RUN ldconfig && \
    sed -i '/BUILD_ID/a VERSION_ID=TEMPLATE_VERSION_ID' /etc/os-release

ENV LANG=C.UTF-8
RUN pacman -Syyu --noconfirm
RUN pacman -S python python-pip --noconfirm
RUN pip install torch
CMD ["/usr/bin/bash"]


