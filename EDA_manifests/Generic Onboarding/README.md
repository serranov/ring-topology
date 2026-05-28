This folder is just for you to check what are the main files that `clab-connector` tool creates by itself.

**Pay attention to the folder called TOPOLOGY/Interfaces, specially to the interface speed and the LAG interfaces towards the hosts**

You can apply all the YAML files within a single directory with the following command:

```bash
kubectl apply -f <directory>/'*.yml'
```

After applying all the YAML's that belong to `TOPOLOGY/Interfaces` directory, delete the interfaces that end with `ethernet-1-4`. You can do that in the same transaction by selecting all of them on EDA's UI and deleting all at the same time.
