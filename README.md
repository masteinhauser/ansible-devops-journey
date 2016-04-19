### install vex
```
$ pip install vex
```

### create virtualenv to work in
```
$ vex -m doj
```

### install ansible (and other project deps)
```
$ pip install -r requirements.txt
```

### install ansible roles
```
ansible-galaxy install -r roles.yml
```

### install Docker Toolbox
### https://docs.docker.com/engine/installation/mac/
```
$ brew cask install dockertoolbox
```

### create a host machine, since we're on OSX and cannot run docker natively
```
$ docker-machine create --driver virtualbox default
```

> The command also creates a machine configuration in the
> ~/.docker/machine/machines/default directory. You only need to run the create
> command once. Then, you can use docker-machine to start, stop, query, and
> otherwise manage the VM from the command line.

### connect your shell to the newly created machine
```
$ eval "$(docker-machine env default)"
```

### run `hello-world` to verify the setup
```
$ docker run hello-world
```

> ```
Hello from Docker.
This message shows that your installation appears to be working correctly.
```

### build a docker container
```
$ packer build packer-docker.json
```
