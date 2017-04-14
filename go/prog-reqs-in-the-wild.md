## Requirements in the wild

This is a curated list of technical `programming` requirements. You may be able to find 
these in stack overflow or Google it. However, this personally curated content helps me in 
focusing without sacrificing my time bound metrics. This should improve my productivity while 
implementing these in a larger scheme of things.

### Shell Snips

```yaml
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

### Dockerfile

```yaml
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
