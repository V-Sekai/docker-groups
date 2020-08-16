FROM fedora:32

# Default args. Shouldn't get used.
ARG GODOT_REVISION="unknown"
ARG GROUPS_REVISION="unknown"
ARG SERVER_EXPORT="/workspace/server_export"

LABEL godot_revision="$GODOT_REVISION" \
  groups_revision="$GROUPS_REVISION" \
  description="Exported groups server" \
  url="https://github.com/V-Sekai/docker-groups"

COPY "$SERVER_EXPORT" /server

WORKDIR "/server"

ENTRYPOINT ["/server/v_sekai_linux_server"]
