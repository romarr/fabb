# Development

## On docker native
    docker run --name cefqf --rm -it -p 4000:4000 -v $PWD:/src jekyll

## On docker for windows
    docker run --name cefqf --rm -it -p 4000:4000 -v="$(pwd):/srv" jekyll

## Build only
    docker run --name cefqf --rm -it -v="$(pwd):/srv" jekyll b

# Automated deployment 
## Git post-receive hook
    #!/bin/bash
    GIT_REPO=$HOME/cefqf.git
    TMP_GIT_CLONE=$HOME/tmp/cefqf
    PUBLIC_WWW=/srv/cefqf/_html

    sudo mkdir -p $PUBLIC_WWW
    mkdir -p $TMP_GIT_CLONE
    git clone $GIT_REPO $TMP_GIT_CLONE
    docker build -t jekyllb $TMP_GIT_CLONE
    docker run --rm -v "$PUBLIC_WWW:/src/_site" jekyllb jekyll b
    sudo rm -Rf $TMP_GIT_CLONE
    exit

## httpd container
### Behind traefik
    docker run -dit --name cefqf --network=traefik_front --name cefqf -v $PWD/_site:/usr/share/nginx/html -l traefik.port=80 -l traefik.frontend.entryPoints=http,https -l traefik.frontend.rule=Host:dev.romsys.ovh -v $PWD/_html:/usr/local/apache2/htdocs/ httpd:alpine

Snipcart is disabled ! Add this to footer
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="{{ " /js/bootstrap.min.js " | prepend: site.baseurl }}"></script>
        <script type="text/javascript" src="https://cdn.snipcart.com/scripts/snipcart.js" id="snipcart" data-api-key="MjMxMzYzMjYtYjc3Ni00NmQ2LTljZTktODM4NzJmNzIwOGFm"></script>

    <link type="text/css" href="https://cdn.snipcart.com/themes/base/snipcart.min.css" rel="stylesheet" />