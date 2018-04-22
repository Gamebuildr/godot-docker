FROM ubuntu:xenial

# Example command:
# godot -path /project/directory -export 'Mac OSX' /context/build.zip -v

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    wget \
    unzip \
    python \
    python-openssl \
    && rm -rf /var/lib/apt/lists/*

ENV GODOT_VERSION "2.1.4"

RUN wget https://downloads.tuxfamily.org/godotengine/${GODOT_VERSION}/Godot_v${GODOT_VERSION}-stable_linux_server.64.zip \
    && wget https://downloads.tuxfamily.org/godotengine/${GODOT_VERSION}/Godot_v${GODOT_VERSION}-stable_export_templates.tpz \
    && mkdir ~/.godot \
    && unzip Godot_v${GODOT_VERSION}-stable_linux_server.64.zip \
    && mv Godot_v${GODOT_VERSION}-stable_linux_server.64 /usr/local/bin/godot \
    && unzip -d ~/.godot Godot_v${GODOT_VERSION}-stable_export_templates.tpz \
    && rm -f Godot_v${GODOT_VERSION}-stable_export_templates.tpz Godot_v${GODOT_VERSION}-stable_linux_server.64.zip

