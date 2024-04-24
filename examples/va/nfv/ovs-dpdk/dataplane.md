# Configuring and deploying the dataplane

## Assumptions

- The [control plane](control-plane.md) has been created and successfully deployed

## Initialize

Switch to the "openstack" namespace
```
oc project openstack
```
Change to the nfv/ovs-dpdk directory
```
cd architecture/examples/va/nfv/ovs-dpdk
```
Edit the [values.yaml](nodeset/values.yaml) file to suit
your environment.
```
vi nodeset/values.yaml
```
Generate the dataplane CRs.
```
kustomize build nodeset > nodeset.yaml
kustomize build deployment > deployment.yaml
```

## Create CRs
Create the nodeset CR
```
oc apply -f nodeset.yaml
```
Wait for dataplane nodeset setup to finish
```
oc wait osdpns openstack-edpm --for condition=SetupReady --timeout=600s
```

Create the deployment CR
```
oc apply -f deployment.yaml
```
Wait for dataplane deployment to finish
```
oc wait osdpd edpm-deployment --for condition=Ready --timeout=1200s
```
