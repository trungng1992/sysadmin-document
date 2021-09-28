# Container Storage Interfaces

Kubernetes has used Docker alone as the container runtime engine and all the code to work with, Docker was embedded within the k8s, a source code with other container runtime comming in, such as `rkt` and `cri-o`

```
        -----   -------------------
rkt    |  C  | |                   |
docker |  R  | |      k8s          |
cri    |  I  | |                   |
        -----   -------------------
```

The container runtime interface (CRI) is a standard that defines how an orchestration solution like k8s would communicate with CRI like Docker.

So in the future, if any new CRI is developed, they can simply follow the CRI-O standard.

And that new container runtime would work with k8s without really having to work with the k8s team of developer or touch k8s source code.

Similarly, as we saw in the networking clusters, to extend support for different networking solutions, the container networking interface was introduced next section.

Now any new networking vendors could simply develop their plug in based on the CNI standards and make their solution work with k8s.

``` shell
# Full the toplocy 
        -----   -------------------   -----    portworx
rkt    |  C  | |                   | |  C  |   amazon ebs
docker |  R  | |      k8s          | |  S  |   hard disk
cri    |  I  | |                   | |  I  |   dell emc
        -----   -------------------   -----    glusterfs
                  --------------
                 |     CNI      |
                  --------------
            waves, flannel, cilium, calico
```

As you can guess, the container storage interface (CSI), you can now write your own drives for your own storage to work with k8s such as: porworx, amazon ebs,  your disk, Dell EMC, Nutanix, HP, glusterfs, etc. Everyone's get their own CSI drivers.

***Note*** That CSI is not a k8s specific standard, it is meatnts to be a universal standard and if implemented, allows any container orchestration tool to work with any storage vendor with a supported plugin.

The CSI kind of looku like it defines a set of out pices or remote proceduce calls that will be called by the contaienr orchestrator and these must be implemented by the storage driver.

For example, CSI say that when a pod is created and requires a volume, the container oschestrator in this case, k8s should call the creae volume RPC and pass a set of details such as the volume name. The storage driver should implement the RBC and handle that request and provision a new volume on the storage area and return the results of the operation.

Simarly, Container Orchestrator should call the delete volume RBC when a volume is to be deleted and the storage driver should implement the code to decommision the volume from the array when that call is made.

