This scenario uses the default `.yaml` file to create a topology with a shared subnet for both hosts, allowing its direct communication via VXLAN.

It uses OSPF as underlay routing protocol to redistribute the system IPs that will be later used for the overlay.

As overlay control plane, there can be an iBGP-EVPN full-mesh between all 4 Leafs, or 2 of them will take the RR role. As EDA prefers the second approach, add the tag `eda.nokia.com/rr=true` to a couple of them to make them RR nodes and the tag `eda.nokia.com/rr=false` to the other 2 Leafs to make them RR clients after you add the fabric configuration.

YAML files must be created in the following order:

1. ROUTING POLICIES/Policies
2. FABRICS/Fabrics (pick just **one** of the available files)
3. CONFIGURATION/Configlets
4. VIRTUAL NETWORKS/Virtual Networks

You can apply all the YAML files within a single directory with the following command:

```bash
kubectl apply -f <directory>/'*.yml'
```
