#### Certificates

- To create security certificate we need to set up a CA
- Why CA ?
  - for signing, verifying & bundling TLS certificates
- Any CA tool readily available for SSL/TLS testing ?
  - https://github.com/cloudflare/cfssl
- Certificates & clustered Servers
  - Create all the certificates for each host participating in the cluster
  - i.e. a certificate, a key & a certificate chain for each host
- Client communicating to clustered Servers model
  - Need to create a client certificate & a client key
- A sample systemd unit file for etcd w.r.t security only
  
  ```bash
  [Unit]
  Description=etcd
  After=network.target

  [Install]
  WantedBy=multi-user.target

  [Service]  
  #security
  Environment=ETCD_TRUSTED_CA_FILE=/root/server1.ca.crt
  Environment=ETCD_CERT_FILE=/root/server1.crt
  Environment=ETCD_KEY_FILE=/root/server1.key.insecure
  Environment=ETCD_CLIENT_CERT_AUTH=1

  Environment=ETCD_PEER_TRUSTED_CA_FILE=/root/server1.ca.crt
  Environment=ETCD_PEER_CERT_FILE=/root/server1.crt
  Environment=ETCD_PEER_KEY_FILE=/root/server1.key.insecure
  Environment=ETCD_PEER_CLIENT_CERT_AUTH=1
  ```
- A sample environment setting for etcdctl i.e. client w.r.t security only
  - /etc/profile.d/etcd.sh
  
  ```bash
  export ETCDCTL_CERT_FILE=/root/client.crt
  export ETCDCTL_KEY_FILE=/root/client.key.insecure
  export ETCDCTL_CA_FILE=/root/server1.ca.crt
  ```
- Talk to a specific etcd node via curl
  
  ```bash
  curl --cacert /root/server1.ca.crt \
    --cert /root/client.crt \
    --key /root/client.key.insecure \
    -L https://$ETCD_IP1:2379/v2/keys/foo
  ```

#### References

- http://blog.dixo.net/2015/05/setting-up-a-secure-etcd-cluster/
