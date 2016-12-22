#### Snippets

- restart if already running, else start
  - `initctl status etcd && initctl restart etcd || initctl start etcd`
- install if needed

  ```bash
  if [[ ! -e /usr/bin/curl ]]; then
    apt-get update
    apt-get -yqq install curl
  fi
  ```
