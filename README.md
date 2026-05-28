# Topology

![topology.png](https://github.com/user-attachments/assets/e330abbc-2507-4ab0-a8d2-b0aa230a7f0c)

Deploy the CLAB topology available at `topology.yml` file with the following command:

```bash
containerlab deploy -t topology.yml --reconfigure
```

To onboard CLAB nodes into EDA, the easiest way to do that is by using [clab-connector](https://docs.eda.dev/26.4/user-guide/containerlab-integration/). **Contact your Nokia representative to obtain a valid trial license for EDA**. Execute the following command to onboard the CLAB topology:

```bash
clab-connector integrate -t <your_path>/ring-topology/clab-topology/topology-data.json -e https://<your_EDA_URL:Port> -n clab-topology --isl-encapsulation untagged --edge-encapsulation untagged
```

**The recommendation is to start with the scenario called *Stretched L2 using EVPN-VXLAN*, following by *Symmetric IRB using EVPN-VXLAN*.**

If you want to remove the namespace, where nodes have been onboarded into EDA, and all its configuration, just execute the following command at any time:

```bash
clab-connector remove -t <your_path>/ring-topology/clab-topology/topology-data.json -e https://<your_EDA_URL:Port> -n clab-topology
```

If you want to remove the CLAB topology to modify the `.yml` file and start from scratch, just execute the following command at any time:

```bash
containerlab destroy -t topology.yml --cleanup
```
