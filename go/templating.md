
#### REFERENCES

- go's text/template package is used in docker

```bash
# Get the PID
$ docker inspect -f '{{ .State.Pid }}' $INSTANCE_ID

# Diplay IP Address of the container
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_ID

# Display MAC Address of the container
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.MacAddress}}{{end}}' $INSTANCE_ID

# List all port bindings
$ docker inspect \
  --format= \
  '{{range $p, $conf := .NetworkSettings.Ports}} {{$p}} -> {{(index $conf 0).HostPort}} {{end}}' \
  $INSTANCE_ID

# Find specific port mapping
$ docker inspect --format='{{(index (index .NetworkSettings.Ports "8787/tcp") 0).HostPort}}' \
  $INSTANCE_ID

# Get a sub-section in json format
$ docker inspect --format='{{json .Config}}' $INSTANCE_ID
```

- index is a function in text/template pkg
- json is a template function provided by docker
