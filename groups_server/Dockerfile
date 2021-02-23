FROM fedora:32

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

COPY "$SERVER_EXPORT" libatomic-10.2.1-9.fc32.x86_64.rpm /server/
RUN yum install -i /server/libatomic-10.2.1-9.fc32.x86_64.rpm && rm -f /server/libatomic-10.2.1-9.fc32.x86_64.rpm

# Avoid RUN commands to keep image size small.
# RUN chmod +x /server/v_sekai_linux_server && useradd -u "$USER" unpriv
# The USER does not need to be created

USER "$USER"

ENV HOME="$HOME"
ENV USER="$USER"

WORKDIR "/server"

ENTRYPOINT ["/server/v_sekai_linux_server"]
