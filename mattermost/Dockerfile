from ubuntu

arg db_user
arg db_password

env DEBIAN_FRONTEND=noninteractive
env TZ=Etc/UTC
env POSTGRES_CMD="/usr/lib/postgresql/14/bin/postgres --config-file=/etc/postgresql/14/main/postgresql.conf"

# Install dependencies
run \
  apt update && \
  apt install -y curl lsb-release gpg systemctl postgresql postgresql-contrib

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
