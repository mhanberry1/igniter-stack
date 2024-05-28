from ubuntu

# Install dependencies
run \
	apt update && \
	apt install -y wget

# Install plex
run \
	wget https://downloads.plex.tv/plex-media-server-new/1.40.2.8395-c67dce28e/debian/plexmediaserver_1.40.2.8395-c67dce28e_amd64.deb && \
	dpkg -i *.deb

# Configure environment
run \
	useradd plex

# Clean up
run \
	rm *.deb && \
	apt remove -y wget && \
	apt clean

# Sometimes plex fails to start, so start it twice
cmd \
	/usr/lib/plexmediaserver/lib/plexmediaserver.init start && \
	/usr/lib/plexmediaserver/lib/plexmediaserver.init start && \
	tail -f /dev/null
