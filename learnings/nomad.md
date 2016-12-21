#### References

- https://gitter.im/hashicorp-nomad/Lobby
  - talks about rule, backend, check-type, notification, cron
- https://github.com/jippi/nomad-auto-scale
  - talks about auto scaling an individual service
- kind-of/remediation

> **"**..., we are using consul-template for dynamic scaling of a group in a job spec
  we keep the count in a consul key, and whenever consul-template detects changes to that key,
  it will rewrite the job spec file with the new count and issue the appropriate nomad run command
  to initiate the change we **also use consul-template on our job specs** for other things that need
  to change dynamically, which will also **trigger a rewrite of the job spec** file and a nomad run
  this approach has worked quite well for us in general, since nomad is good at detecting what 
  changed and taking minimal appropriate action in your case, I could imagine you writing a **small
  service that watches** certain telemetry data that you're interested in (like the size of a 
  RabbitMQ queue), then changing the consul key dictating the scale of the application **"**
