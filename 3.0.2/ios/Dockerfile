FROM ubuntu:xenial

RUN apt-get update \ 
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF \
    && apt-get install -y --no-install-recommends apt-transport-https ca-certificates \
    && echo "deb https://download.mono-project.com/repo/ubuntu stable-xenial main" | tee /etc/apt/sources.list.d/mono-official-stable.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    git \
    vim \
    wget \
    unzip \
    python \
    python-openssl \
    scons \
    pkg-config \
    build-essential \ 
    libx11-dev \ 
    libxcursor-dev \
    libxinerama-dev \
    libgl1-mesa-dev \ 
    libglu-dev \ 
    libasound2-dev \
    libpulse-dev \ 
    libfreetype6-dev \ 
    libssl-dev \ 
    libudev-dev \
    libxrandr-dev \
    libxi-dev \
    mono-complete \
    monosgen-2.0 \
    && rm -rf /var/lib/apt/lists/*

ENV GODOT_VERSION "3.0.2"

# Compile Godot engine for headless server use
RUN git clone -b ${GODOT_VERSION}-stable --depth 1 --progress https://github.com/godotengine/godot.git \
    cd godot/ \
    && scons -j 4 p=server tools=yes module_mono_enabled=yes mono_glue=no \
    && mv bin/godot_server.server.tools.64.mono /bin/godot \
    && bin/godot --generate-mono-glue modules/mono/glue \
    && scons -j 4 p=server target=release_debug tools=yes module_mono_enabled=yes
    # && scons -j 4 p=x11 target=debug tools=no module_mono_enabled=yes \
    # && scons -j 4 p=x11 target=release tools=no module_mono_enabled=yes

