## Setup

This was `https://linuxhint.com/install_docker_raspberry_pi-2/` used as a guide to installing Docker on the Pi.

After that, a guide to create a local NAS network `https://uk.pcmag.com/network-attached-storage/124258/how-to-turn-a-raspberry-pi-into-a-nas-for-whole-home-file-sharing` You will need to symlink the directory on your machine.

Then:
Installed php
Installed composer
Installed any missing lib/dependencies php requires (quick google as I can’t remember what I did)
All of these commands can be find with a quick google, as too much to really document.

After all this and symlinks created, install a fresh Laravel app using a bash script.

```bash
docker info > /dev/null 2>&1

# Ensure that Docker is running...
if [ $? -ne 0 ]; then
    echo "Docker is not running."

    exit 1
fi

docker run --rm \
    -v $(pwd):/opt \
    -w /opt \
    composer:latest \
    bash -c "composer create-project laravel/laravel example-app && cd example-app && php ./artisan sail:install --with=pgsql,redis"

cd example-app

CYAN='\033[0;36m'
LIGHT_CYAN='\033[1;36m'
WHITE='\033[1;37m'
NC='\033[0m'

echo ""

if sudo -n true 2>/dev/null; then
    sudo chown -R $USER: .
    echo -e "${WHITE}Get started with:${NC} cd example-app && ./vendor/bin/sail up"
else
    echo -e "${WHITE}Please provide your password so we can make some final adjustments to your application's permissions.${NC}"
    echo ""
    sudo chown -R $USER: .
    echo ""
    echo -e "${WHITE}Thank you! We hope you build something incredible. Dive in with:${NC} cd example-app && ./vendor/bin/sail up"
fi

```

> NOTE: This in only a good case for setting up a new app, which was used for testing to see if everything above had been installed correctly. ( which it was :) )


