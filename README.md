# docker-capifony

Docker image to deploy Syfmony 2.x app with capifony.

Capifony is based on Capistrano v2.x and will stick to this version (i.e. Capifony is feature-frozen, and will only accept bug fixes).

At the time of writing, Capistrano v3 is the current major version, and Capifony is not compatible with it.
Don't worry, there is a plugin for that! Using Capistrano v3 + capistrano/symfony (heavily inspired by Capifony) may be the way to go for new projects! You can read more on capifony and its future.

This docker image is a simple solution to easily use capistrano 2.8 with capifony extension for symfony 2.x project.

## Requirements

- Linux with Docker installed

## How to use it

### Create docker image

```bash
git clone https://github.com/lolozere/docker-capifony.git
docker build -t lolozere/capifony:base docker-capifony/commons/
docker build -t lolozere/capifony:2.8 docker-capifony/2.8/
```

### Prepare to deploy

In your source code you have to deploy, create a root directory, for example "deploy" and copy the script ```cap``` from the bin directory :

```bash
mkdir /path/to/sf-app/deploy
cp docker-capifony/bin/cap /path/to/sf-app/deploy/
```

You can also edit the script to change the ```dir_source``` variable and copy it wherever.

Now, create the ```Capfile``` :

```bash
touch mkdir /path/to/sf-app/Capfile
vi /path/to/sf-app/Capfile
```

```ruby
load 'deploy' if respond_to?(:namespace) # cap2 differentiator

require 'capifony_symfony2'
load 'app/config/deploy'
```

And create your deploy.rb file in app/config. [You have an example here >](https://github.com/lolozere/docker-capifony/blob/master/2.8/deploy.rb.dist)

### Deploy

Use the command ```./deploy/cap <asyouwant>``` to init and deploy
