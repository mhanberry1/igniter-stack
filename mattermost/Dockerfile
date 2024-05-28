from ubuntu:22.04

arg db_user
arg db_password

env DEBIAN_FRONTEND=noninteractive
env TZ=Etc/UTC
env POSTGRES_CMD="/usr/lib/postgresql/16/bin/postgres --config-file=/etc/postgresql/16/main/postgresql.conf"

# Install dependencies
run \
  apt update && \
  apt install -y curl lsb-release gpg systemctl

# Install postgres 16 repository
run \
  echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > \
  /etc/apt/sources.list.d/pgdg.list && \
  curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg && \
  apt update && \
  apt install -y postgresql-16


# Install mattermost
run \
  curl -o- https://deb.packages.mattermost.com/repo-setup.sh | \
  sed 's/sudo//g' | \
  bash -s mattermost && \
  apt update && \
  apt install mattermost -y

# Configure environment
add \
  config.json /opt/mattermost/config/config.json

run \
  locale-gen en_US.utf8 && \
  chown mattermost:mattermost /opt/mattermost/config/config.json && \
  chmod 600 /opt/mattermost/config/config.json && \
  sed -i \
    -e "s/<POSTGRES_USER>/$db_user/g" \
    -e "s/<POSTGRES_PASSWORD>/$db_password/g" \
    /opt/mattermost/config/config.json

# Setup database
user postgres

run \
  $POSTGRES_CMD & \ 
  sleep 1 && \
  psql -c "create user $db_user with password '$db_password';" && \
  psql -c "create database mattermost;" && \
  psql -c "grant all privileges on database mattermost to $db_user;"

# Clean up
user root

run \
  apt remove -y curl lsb-release gpg systemctl && \
  apt clean

cmd \
  su -c "$POSTGRES_CMD" postgres & \
  sleep 5 && \
  mattermost
