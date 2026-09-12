PersistentVolume (PV) = the actual piece of storage that exists in the cluster (like a plot of land or an apartment). It's provisioned by an admin (or dynamically via a CSI driver) and represents real storage — an AWS EBS volume, a GCE Persistent Disk, an NFS share, etc.
PersistentVolumeClaim (PVC) = a request made by a user/Pod for storage (like signing a lease). The Pod doesn't ask for a specific PV directly — it just says "I need 10Gi of storage with these access modes," and Kubernetes matches it to an available PV that satisfies that request.

The Three (Really Four) Access Modes
1. ReadWriteOnce (RWO)
The volume can be mounted as read-write by a single node at a time.
Multiple Pods can use it, but only if they're scheduled on the same node (this changed in newer Kubernetes versions — see note below).
Most common mode — used by almost all cloud block storage (AWS EBS, GCE PD, Azure Disk).
yaml
accessModes:
  - ReadWriteOnce

Typical use case: Databases (MySQL, PostgreSQL, MongoDB) where only one Pod should be writing to the disk at a time.

2. ReadOnlyMany (ROX)
The volume can be mounted as read-only by many nodes simultaneously.
No writes allowed at all once mounted this way.
yaml
accessModes:
  - ReadOnlyMany

Typical use case: Serving static content (e.g., shared config files, reference data, ML model files) across multiple Pods that only need to read, not write.

3. ReadWriteMany (RWX)
The volume can be mounted as read-write by many nodes at the same time.
Requires storage backends that support this — most cloud block storage does not support RWX (you'd need something like NFS, CephFS, GlusterFS, or Azure Files).
yaml
accessModes:
  - ReadWriteMany

Typical use case: Shared file storage across multiple Pods — e.g., a shared uploads folder for a web app running multiple replicas.

4. ReadWriteOncePod (RWOP) — newer addition (Kubernetes 1.22+)
Stricter than RWO: the volume can be mounted as read-write by only a single Pod in the whole cluster (not just a single node).
Solves a subtle gap in RWO, where technically multiple Pods on the same node could still mount it.
yaml
accessModes:
  - ReadWriteOncePod

Typical use case: When you need a hard guarantee that only one Pod — not just one node — has write access (important for things like leader-election-based storage or strict data consistency requirements).