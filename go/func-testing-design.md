#### References

- https://github.com/coreos/etcd/tree/master/tools/functional-tester
  - A well designed functional testing tool to test cluster (*here etcd*)
    - Use of OOPs concepts. Refer: ./etcd-tester/*.go
  - Pushes test results & metrics to a 3rd party system
  - The design has elements of agent, runner, tester, http rpc
  - The design is meant for failure testing & hence does the following:
    - Detours from use of systemd etc init based program
    - Takes full control of start, stop, or kill -ing of process(es)
    - NOTE: You might think of controlling systemd etc init process also
    - NOTE: Above applies if cluster is based on systemd type init process
  - Mention of *goreman (foreman)* to run the cluster in a single machine
    - NOTE: Should be used for development purposes
    - NOTE: Systemd etc may be used for prod purposes
  - Mention of Docker to test the cluster
    - NOTE: Not suitable to test all cluster failure conditions
  - Simulation of failure cases
