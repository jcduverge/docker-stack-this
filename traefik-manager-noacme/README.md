## General Considerations
This project will run Traefik, WordPress, Nginx, Caddy and whoami containers in one commands (git clone included). Powerfull stuff folks :)

#### What’s special about this directory
It doesn’t use ACME. I decided to create this as I still have issues about in `traefik-manager`. 

This project is stable.

## Deploy!
1. Go to http://labs.play-with-docker.com/ 
2. Create **one instance** and wait for the node to provision
3. On **node1**, copy paste:

```
# Create Swarm
docker swarm init --advertise-addr eth0;

# List nodes
docker node ls;

# Install common apps
apk update && apk upgrade && apk add nano curl bash git wget unzip ca-certificates;

# Clone repo
cd /root;
git clone https://github.com/pascalandy/docker-stack-this.git;
cd docker-stack-this;

# If you prefer to use a branch...
git checkout 1.11;

# Go to the actual project
cd traefik-manager-noacme;

# Make script executable
chmod +x /runup;
chmod +x /rundown;

# Launch all services
./runup;
```

## Confirm Traefik is working
1. The scripts run `docker service ls` is automatically.

2. After a while (about 20 sec) we see the logs from Traefik. Do `CTRL-C` to quits Traefik’s log and return to the terminal’s prompt.

3. Click on `9090` to see **Traefik dashboard**. We see all the services that run behind traefik.

4. Click on `80`. We have access to Wordpress (stateful).

5. At the end of the URL generate by play-with-ghost on port 80, add `who1` or `/who2` or `/who3`.

```
# home, the IP is probably different in you #pwg session
http://pwd10-0-7-3-80.host1.labs.play-with-docker.com/

http://pwd10-0-7-3-80.host1.labs.play-with-docker.com/who1
http://pwd10-0-7-3-80.host1.labs.play-with-docker.com/who2
http://pwd10-0-7-3-80.host1.labs.play-with-docker.com/who3
```

#### Web apps details:
- **/** = [wordpress](https://hub.docker.com/_/wordpress/)
- **who1** = [caddy](https://hub.docker.com/r/abiosoft/caddy/)
- **who2** = [nginx](https://hub.docker.com/_/nginx/)
- **who3** = [whoami](https://hub.docker.com/r/emilevauge/whoami/) 

#### All commands
In the active path, just execute those bash-scripts:

- `./runup`
- `./rundown`

See for yourself that Wordpress keeps the data even after you did  shutdown the containers.

## What is Traefik?
[Traefik](https://docs.traefik.io/configuration/backends/docker/) is a powerful layer 7 reverse proxy. Once running, the proxy will give you access to many web apps. I think this is a solid use cases to understand how this reverse-proxy works.

#### Traefik version 
In `toolproxy.yml` look for something like `traefik:1.4.0-rc3`.

## What’s missing to make this stack perfect?
- Secure traefik dashboard
- Use SSL endpoints (ACME)
- There is a glitch with portainer. (work-in-progress)

## Something looks weird? Please let me know
I consider this README crystal clear. If there is anything that I could improve, please let me know via an issue.

## Shameless promotion :-p
Looking to **kick-start your website** (static page + a CMS) ? Take a look at [play-with-ghost](http://play-with-ghost.com/) (another project I shared). It allows you to see and edit websites made with Ghost. In short, you can try Ghost on the spot without having to sign up!

## P.S.
If you have solid skills 🤓 with Docker Swarm, Linux bash and the gang and you’re looking to help a startup to launch a solid project, I would love to get to know you. Buzz me 👋 on Twitter [@askpascalandy](https://twitter.com/askpascalandy). You can see the things that are done and the things we have to do [here](http://firepress.org/blog/technical-challenges-we-are-facing-now/).

I’m looking for bright and caring people to join this [journey](http://firepress.org/blog/tag/from-the-heart/) with me.

```
 ____                     _      _              _
|  _ \ __ _ ___  ___ __ _| |    / \   _ __   __| |_   _
| |_) / _` / __|/ __/ _` | |   / _ \ | '_ \ / _` | | | |
|  __/ (_| \__ \ (_| (_| | |  / ___ \| | | | (_| | |_| |
|_|   \__,_|___/\___\__,_|_| /_/   \_\_| |_|\__,_|\__, |
                                                  |___/
```

Cheers!
Pascal