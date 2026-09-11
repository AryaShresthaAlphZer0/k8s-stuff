# Kubernetes Core Interfaces: CRI, CNI, CSI

This breaks down the three major "plug-in" interfaces that sit beneath Kubernetes core, each letting Kubernetes delegate a specific job to swappable, vendor-built implementations rather than baking that logic into Kubernetes itself.

## Kubernetes Core

Kubernetes itself is the orchestrator — it decides *what* should happen (schedule this pod, attach this volume, connect this network) but it doesn't do the low-level work itself. Instead, it defines standard interfaces and lets other software do the actual work. That's where CRI, CNI, and CSI come in.

---

## CRI — Container Runtime Interface

**What it does:** Tells Kubernetes how to actually create and run containers on a node.

- **Examples:** CRI-O, containerd, Kata Containers
- **Responsibilities:**
  - Container lifecycle management (start, stop, restart containers)
  - Image management (pulling and storing container images)
  - Logging (exposes container logs back to Kubernetes)
  - Security (sets up cgroups and network namespaces for isolation)

Think of CRI as the layer that turns a Pod spec into an actual running process on a machine.

---

## CNI — Container Network Interface

**What it does:** Handles how pods talk to each other, to services, and to the outside world.

- **Examples:** Calico, AWS VPC CNI, Flannel, Cilium
- **Responsibilities:**
  - Pod networking (giving every pod a working network stack)
  - IPAM (IP Address Management — assigning IPs to pods)
  - Routing & connectivity (making sure traffic actually gets where it needs to go)
  - Security enforcement (network policies, segmentation)

Without a CNI plugin, pods would have no way to communicate with each other across nodes.

---

## CSI — Container Storage Interface

**What it does:** Connects Kubernetes to storage systems so pods can use persistent volumes.

- **Examples:** AWS EBS CSI driver, GCE-PD CSI driver
- **Responsibilities:**
  - Volume lifecycle management (creating/deleting volumes)
  - Snapshot & cloning (backing up or duplicating volumes)
  - Expansion & resizing (growing a volume without downtime)
  - Dynamic volume provisioning (auto-creating storage on demand, e.g., when a PVC is created)

CSI is what handles *persistent* storage (PVCs). By contrast, `emptyDir` is a Kubernetes-native volume type that doesn't need CSI at all, since it's just local, ephemeral node storage tied to the Pod's lifetime.

---

## Big Picture

Kubernetes stays lean and consistent by not hardcoding networking, storage, or runtime logic. Instead, it exposes CRI/CNI/CSI as contracts, and the ecosystem builds pluggable drivers against them — which is why you can swap Docker for containerd, or Flannel for Cilium, without changing how Kubernetes itself behaves.
