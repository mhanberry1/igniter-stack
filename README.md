# Igniter Stack

A docker-compose stack for Igniter Engineering's private cloud services.

## Start the stack

To start the entire stack:

```
docker compose up
```

To start a particular service:

```
docker compose up <service>
```

## Plex

### Setup

On first boot, you will need to claim the server. You can do this in two ways:

1. Open a web browser directly on the machine, go to `localhost:32400/manager`, and follow the instructions.
	- This cannot be done easily if running on a cloud server, proceed to option 2.
2. Create an ssh-tunnel between your computer and the server on port 32400, then go to `localhost:32400/manager`, and follow the instructions.

### Adding Media

You will need to add media (Movies, Shows, etc.) to your server yourself.
If you have direct access to the server, then just download the files directly or transfer them via external drive.
If you are running this on a cloud server, use an sftp program like FileZilla or WinSCP to send files to the server.
Don't forget to give them the correct names and put them in the right directory! Read more [here](https://support.plex.tv/articles/naming-and-organizing-your-movie-media-files/).

## Mattermost

### Setup

Everything should run as-is. Once you start the service, mattermost will be running on port 8065.
