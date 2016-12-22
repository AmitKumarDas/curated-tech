#### ETCD

- env variables:
  - ETCD_INITIAL_CLUSTER
  - ETCD_INITIAL_CLUSTER_STATE
  - ETCD_INITIAL_CLUSTER_TOKEN
  - ETCD_INITIAL_ADVERTISE_PEER_URLS
  - ETCD_DATA_DIR
  - ETCD_LISTEN_PEER_URLS
  - ETCD_LISTEN_CLIENT_URLS
  - ETCD_ADVERTISE_CLIENT_URLS
  - ETCD_NAME
- releases:
  - [etcd-v2.0.9-linux-amd64.tar.gz](https://github.com/coreos/etcd/releases/download/v2.0.9/etcd-v2.0.9-linux-amd64.tar.gz)
- binaries:
  - etcd & etcdctl
- provisioning script i.e. logic
  - [provision](https://github.com/lowescott/learning-tools/blob/master/etcd-2.0/provision.sh)
- systemd file

  ```bash
  [Unit]
  Description=etcd
  After=network.target

  [Install]
  WantedBy=multi-user.target

  [Service]
  #basic config
  Environment=ETCD_DATA_DIR=/var/lib/etcd
  Environment=ETCD_NAME=etcd1
  Environment=ETCD_LISTEN_PEER_URLS=https://$ETCD_IP1:2380
  Environment=ETCD_LISTEN_CLIENT_URLS=https://$ETCD_IP1:2379
  Environment=ETCD_ADVERTISE_CLIENT_URLS=https://$ETCD_IP1:2379

  #initial cluster configuration
  Environment=ETCD_INITIAL_CLUSTER=etcd1=https://$ETCD_IP1:2380,etcd2=https://$ETCD_IP2:2380,etcd3=https://$ETCD_IP3:2380
  Environment=ETCD_INITIAL_CLUSTER_TOKEN=your-unique-token
  Environment=ETCD_INITIAL_CLUSTER_STATE=new
  Environment=ETCD_INITIAL_ADVERTISE_PEER_URLS=https://$ETCD_IP1:2380

  #security
  
  #tuning see https://github.com/coreos/etcd/blob/master/Documentation/tuning.md
  Environment=ETCD_HEARTBEAT_INTERVAL=100
  Environment=ETCD_ELECTION_TIMEOUT=2500

  ExecStart=/usr/bin/etcd
  Restart=always
  ```
- systemd commands
  
  ```bash
  systemctl daemon-reload
  systemctl enable etcd.service
  systemctl restart etcd.service
  ```


#### References

- https://github.com/lowescott/learning-tools/blob/master/etcd-2.0/
- http://blog.dixo.net/2015/05/setting-up-a-secure-etcd-cluster/
