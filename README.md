# Knative Goofin

1. `./kind-with-registry.sh`
1. `./install-knative.sh`
1. wait for everything to come up
1. `tilt up`
1. wait for things to get deployed
1. `kubectl logs event-display-00001-deployment-xxxxxxxxxxxxxxxx
   user-container -f`
