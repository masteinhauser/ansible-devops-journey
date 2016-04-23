# DEVELOPMENT

### install vex
```
pip install vex
```

### create virtualenv to work in
```
vex -m doj
```

### install ansible (and other project deps)
```
pip install -r requirements.txt
```

### install ansible roles
```
ansible-galaxy install -r roles.yml
```

# PRODUCTION: AWS

### build your ami
```
packer build packer-amazon-ebs.json
```

# PRODUCTION: DIGITALOCEAN
### build a droplet snapshot
```
packer build --only digitalocean packer.json
```

### install doctl tool and login
```
brew install doctl
doctl auth login
```

### run the newly created image
```
# find the image id
image=$(doctl compute image list --format ID,Name | grep packer | tail -n 1 | cut -f1)
ssh_key_fingerprint=$(doctl compute ssh-key list | grep primary | cut -f3)

# launch a droplet from the packer created snapshot
doctl compute droplet create test-$image --size 512mb --image $image --region nyc2 --ssk-keys $ssh_key_fingerprint
```

### check it's actually running and view the app
```
ip=$(doctl compute droplet list | grep test-$image | cut -f4)
curl http://$ip
echo "Open up your web browser to http://$ip to interact with the app!"
```

# DEVELOPMENT: DOCKER (DOCKERDOCKER)
### install Docker Toolbox
### https://docs.docker.com/engine/installation/mac/
```
brew cask install dockertoolbox
```

### create a host machine, since we're on OSX and cannot run docker natively
```
docker-machine create --driver virtualbox default
```

> The command also creates a machine configuration in the
> ~/.docker/machine/machines/default directory. You only need to run the create
> command once. Then, you can use docker-machine to start, stop, query, and
> otherwise manage the VM from the command line.

### connect your shell to the newly created machine
```
eval "$(docker-machine env default)"
```

### run `hello-world` to verify the setup
```
docker run hello-world
```

> ```
Hello from Docker.
This message shows that your installation appears to be working correctly.
```

# PRODUCTION(-ish): DOCKER (DOCKERDOCKERDOCKER)
### build a docker container
```
packer build --only docker packer.json
```

### run the newly created image
```
docker run -p 80:80 -it masteinhauser/ansible-devops-journey /bin/bash -c "cd /opt/simple-chat/ && /opt/simple-chat/simple-chat & /usr/sbin/nginx
```

### check it's actually running and view the app
```
ip=$(docker-machine ip default)
curl http://$ip
echo "Open up your web browser to http://$ip to interact with the app!"
```
