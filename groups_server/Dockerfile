FROM fedora:34

# Default args. Shouldn't get used.
ARG GODOT_REVISION="unknown"
ARG GROUPS_REVISION="unknown"
ARG SERVER_EXPORT="/workspace/server_export"
ARG USER="root"
ARG HOME="/root"

LABEL godot_revision="$GODOT_REVISION" \
  groups_revision="$GROUPS_REVISION" \
  description="Exported groups server" \
  url="https://github.com/V-Sekai/docker-groups"

COPY "$SERVER_EXPORT" /server/
RUN dnf install libatomic libXcursor libXinerama -yq
RUN dnf clean all

# Avoid RUN commands to keep image size small.
# RUN chmod +x /server/v_sekai_linux_server && useradd -u "$USER" unpriv
# The USER does not need to be created

USER "$USER"

ENV HOME="$HOME"
ENV USER="$USER"

WORKDIR "/server"

ENTRYPOINT ["/server/v_sekai_linuxbsd"]
