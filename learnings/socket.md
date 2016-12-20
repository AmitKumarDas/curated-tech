### Socket Activation

- [socket-activation](http://0pointer.de/blog/projects/socket-activation.html)
  - Starting of services parallely
  - Services will no more have configuration dependencies w.r.t other services
  - No more losing of messages even with service restarts
  - Service can receive pre-initialized sockets from systemd
  - Services can listen on more than one socket

- What was socket activation ? 

> Basically, socket activation simply means that systemd sets up listening sockets
(IP or otherwise) on behalf of your services (without these running yet), and
then starts (activates) the services as soon as the first connection comes in.
