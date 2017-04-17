## Requirements in the wild

This is a curated list of technical `programming` requirements. You may be able to find 
these in stack overflow or Google it. However, this personally curated content helps me in 
focusing without sacrificing my time bound metrics. This should improve my productivity while 
implementing these in a larger scheme of things.

### K8s Snips

```yaml
StatefulSet:
  - Assigns persistent DNS names to pods
  - Allows to re-attach storage volume to another machine where the pod migrated to
  - A headless service that points to each member of pod:
    - It does not create cluster IP for load balancing
    - i.e. it does not have ClusterIP & NodePort specified
    - Its purpose is to trigger DNS name creation for PODs that will be launched
  - PODs are launched strictly one after the other
service.alpha.kubernetes.io/tolerate-unready-endpoints "true":
  - creates the pod endpoint ignoring its state
  - create the endpoint even if service is not healthy
  - e.g. MongoDB replica sets want to reach each other even before initialization
pod.alpha.kubernetes.io/init-containers: 
  - state the containers that need to be started before the main pod
```

### Ansible Snips

```yaml
To use Ansible:
  - PYTHONPATH must be set to include the lib and lib64 directories
  - PYTHONPATH=<>/lib/python2.7/site-packages/:<>/lib64/python2.7/site-packages bin/ansible
ANSIBLE_CONFIG:
  - /etc/ansible/ansible.cfg
  - INI format
pipelining=True:
  - Reduces the number of SSH operations to execute a module on the remote server
```

### Vagrant Verbose

```ruby
# Array of Structs
boxes = [
  {
    :name => "master01",
    :mem => "1024"
  },
  {
    :name => "worker01",
    :mem => "1024"
  }
]

# Add SSH Public Key to the Node
config.vm.provision "shell" do |s|
  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/kismaticuser.key.pub").first.strip
  s.inline = <<-SHELL
    mkdir -p /root/.ssh
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
  SHELL
end
```

### Vagrant Snips

```yaml
Vagrant && Insert SSH Key:
  - config.ssh.insert_key = true is default
  - Vagrant automatically inserts a keypair to use for SSH
  - Replaces Vagrant's default insecure key inside the machine if detected.
Turn off shared folders:
  - config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
CentOS & Networking:
  - config.vm.network :private_network, ip: 1.1.12.1
  - config.vm.provision "shell", inline: "nmcli connection reload; systemctl restart network.service"
```

### Shell Snips

```yaml
Fixes to Permission related Issues:
  - Permission Denied:
  - Command Not Found:
  - Operation Not Permitted:
    - Run the command with sudo
    - chmod + x path/to/bin
uname -s:
  - Linux
uname -m:
  - x86_64
Using uname:
  - $ curl -L "../releases/1.11.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
IPv4 Addr for a Network Device Named eth0:
  - ip -4 addr show scope global dev eth0 | grep inet | awk '{print \$2}' | cut -d / -f 1
Interpolation:
  - ETCD_NAME=${1:-"default"}
  - ETCD_LISTEN_IP=${2:-"0.0.0.0"}
  - ETCD_INITIAL_CLUSTER=${3:-}
Variable:
  - etcd_data_dir=/var/lib/etcd
  - mkdir -p ${etcd_data_dir}
Error Handling:
  - http://linuxcommand.org/lc3_wss0140.php
Which Program/Script:
  - PROGNAME=$(basename $0)
Line Number an Environment Variable:
  - $LINENO
  - helpful in getting the error prone line number
If Undefined Then Substitute:
  - ${1:-"Unknown Error"}
Interpolation, Nested Interpolation:
  - echo "${PROGNAME}: ${1:-"Unknown Error"}" 1>&2
OR Operator:
  - Avoid IF ELSE e.g. if [ "$?" = "0" ]; then ...
  - Functional Programming
  - Refer - http://linuxcommand.org/lc3_wss0140.php
  - cd $some_directory || error_exit "Cannot change directory! Aborting"
Execute a Command On Receiving Some Signals:
  - trap "echo "bye..bye..."; exit" SIGHUP SIGINT SIGTERM
SIGKILL As The Last Resort:
  - kill -9
  - Does not provide any chance for the program to execute cleanup actions
  - Can lead to locks
Prefer temp file at user s local tmp:
  - if [ -d "~/tmp" ]; then
  -  TEMP_DIR=~/tmp
  - else
  -  TEMP_DIR=/tmp
  - fi
Resistance to RACE attacks:
  - TEMP_FILE=$TEMP_DIR/printfile.$$.$RANDOM
Know What the Script Is Doing:
  - Turn on tracing at the beginning set -x
  - Turn off tracing at the end set +x
```

### GoLang Snips

```yaml
New vs Make:
  - fmt.Printf("%T  %v\n", new([10]int), new([10]int))
    - *[10]int  &[0 0 0 0 0 0 0 0 0 0]
  - fmt.Printf("%T  %v\n", make([]int, 10), make([]int, 10))   
    - []int  [0 0 0 0 0 0 0 0 0 0]
Type vs. Value Printing:
  - i := 10
  - s := strconv.Itoa(i)
  - fmt.Printf("%T, %v\n", s, s) will o/p string, 10
Composing Interfaces, Private Struct, Public Initializer, Naming of Initializer:
  - func (d *dumper) Dump(xyz) abc
  - type dumper struct composed of w io.Writer
  - func Dumper(w io.Writer) io.WriteCloser returns &dumper{w: w}
Templating:
  - import text/template    
  - strtmpl string, obj interface{}
  - tmpl, err := template.New("template").Parse(strtmpl)
  - err = tmpl.Execute(&buf, obj)
Forced Compilation Check:
  - // ensure kubeletVolumeHost implements VolumeHost interface
  - var _ volume.VolumeHost = &kubeletVolumeHost{}  
```

### Docker Verbose

```bash
# --install-option flag tells pip to install ansible package to /ansible
# The /ansible directory is Container volume that maps to Host's $(pwd)/out

docker run --rm -v $(pwd)/out:/ansible apprenda/vendor-ansible \
    pip install --install-option="--prefix=/ansible" ansible==2.1.2.0

```

### Docker Snips

```yaml
Docker & Whats Ephemeral Storage:
  - Docker containers use their root disk as ephemeral storage
  - A chunk of disk space from the host filesystem which runs the container
  - This disk space can’t be shared with other processes, nor easily mgrated to a new host.
Docker & Whats Docker Volume:
  - Allows to run a container with dedicated volume mounted.
  - This volume comprises another chunk of space from a host machine
  - This is persistent and independent from container lifecycle
  - It can be Network Storage, or Shared Filesystem Mount:
    - Depends on the storage plugin being used
Remove all Exited Containers:
  - sudo docker rm $(sudo docker ps -a -q -f "status=exited*")
Remove all Images:
  - sudo docker rmi $(sudo docker images -q)
Stop all Containers:
  - sudo docker stop $(sudo docker ps -a -q)
Remove all Containers & its Associated Volumes:
  - sudo docker rm -v $(sudo docker ps -a -q)
Using Docker to Store Vendored Packages:
  - github.com/apprenda/kismatic/tree/master/vendor-ansible#using-docker-to-create-vendored-package
Expose Port 80 of Container Without Publishing Port to the Host system’s Interfaces:
  - docker run --expose 80 ubuntu bash
Bind Port 8080 of Container to Port 80 on 127.0.0.1 of Host Machine:
  - docker run -p 127.0.0.1:80:8080 ubuntu bash
Connect Container to a Network:
  - docker run -itd --network=my-net busybox
Choose an IP and Network:
  - docker run -itd --network=my-net --ip=10.10.9.75 busybox
Add A Host into a Container s /etc/hosts:
  - docker run --add-host=dockerize:10.20.1.2 --rm -it debian
  - Login to Above Container:
    - root@f38c87f2a42d:/# ping dockerize
```

### Dockerfile

```yaml
Wrong vs. Right:
  - Does not work RUN [ "echo", "$HOME" ]
  - Works RUN [ "sh", "-c", "echo $HOME" ]
Wrong vs. Right:
  - Does not work CMD ["echo", "$HOME"]
  - Works CMD ["sh", "-c", "echo $HOME"]
Build Image using Current Directory as Context:
  - docker build .
Build Directory Best Practices:
  - Start with an empty directory that will host the Dockerfile
  - All the other files will form the context to this image building process
Tagging Image into Multiple Repositories:
  - docker build -t openebs/jiva:1.2.2 -t openebs/jiva:latest .
Running Dockerfile Instructions during Build Process:
  - Each run of instruction is independent
  - Result of each instruction is commited to tbe newly built image
Instruction Naming Practices:
  - They are UPPERCASE by convention
  - FROM, COPY, ENV, WORKDIR, ADD, COPY, EXPOSE, LABEL, USER, VOLUME, STOPSIGNAL, ONBUILD, RUN
Parser Directives:
  - Special type of comment in the form # directive=value
  - e.g.
  - # escape=\
  - # escape=`
Default Escape Character:
  - \
  - \$foo translates to $foo
  - \${foo} translates to ${foo}
RUN <command>:
  - runs a shell with /bin/sh -c on Linux
  - runs a shell with cmd /S /C -c on Windows
RUN ["executable", "param1", "param2"]:
  - runs in exec form
  - e.g. RUN ["/bin/bash", "-c", "echo hello"]
RUN vs CMD:
  - RUN runs a command & commits the result into the image
  - CMD does not execute anything at build time
  - CMD is used along with ENTRYPOINT
LABEL <key>=<value> <key>=<value> <key>=<value> ...:
  - Adds metadata to an image
  - An image's label(s) can be inspected via docker inspect command
EXPOSE:
  - This tells Docker to let container listen on the specified network port(s) at runtime
  - This does not expose the port(s) of container accessible to the host
ENV:
  - Can be set on each line
  - Can be set together
  - Setting multiple k=v together is preferred as it produces a single cache layer
  - These values will persist when when a container is run from resulting image
  - View these using docker inspect command
  - Change them using docker run --env <key>=<val>
  - To set for a single command RUN <key>=<value> <command>
  - Can cause unexpected side effects
ENTRYPOINT:
  - Has two forms
  - ENTRYPOINT ["executable", "param1", "param2"]
  - ENTRYPOINT command param1 param2
  - e.g. docker run -i -t --rm -p 80:80 nginx
  - CLI args to docker run <image> will be appended after all elements in an exec form ENTRYPOINT
  - Above will also override all elements specified using CMD
ENTRYPOINT vs. CMD:
  - fairly stable default commands are set in ENTRYPOINT
  - additional defaults that are likely to change in CMD
  - When you run the container the ENTRYPOINT executable is the ONLY PROCESS RUNNING
  - Verify via docker run -it --rm --name test top -H
  - Alternatively verify later via docker exec -it test ps aux
ENTRYPOINT vs. CMD:
  - ENTRYPOINT should be defined when using the container as executable
  - CMD can be used to set default args for ENTRYPOINT command
  - CMD can be used to run ad-hoc commands in a container
  - CMD will be overridden when running the container with alternative arguments
VOLUME:
  - Creates a mountpoint with the specified name
```

### References

- https://github.com/apprenda/kismatic/
