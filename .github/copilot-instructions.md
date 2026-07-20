## Copilot instructions for Trident documentation

### Repository overview
Product: NetApp Trident

Trident is a fully supported open source Container Storage Interface (CSI) compliant dynamic storage orchestrator maintained by NetApp that integrates natively with Kubernetes to provision and manage persistent storage from NetApp storage platforms. It also provides direct integration with Docker through the NetApp Docker Volume Plugin (nDVP).

### Repository structure
- `trident-get-started/` – Introduction, architecture overview, quickstart, and requirements for Trident on Kubernetes
- `trident-install/` – Installation methods (Trident operator, Helm, tridentctl) for Kubernetes and OpenShift, including standard, offline, and remote modes
- `trident-managing-k8s/` – Day-2 operations: managing Trident with tridentctl, upgrading, and uninstalling
- `trident-use/` – Backend configuration for all supported storage platforms, storage class management, and volume operations (snapshots, clones, expansion, import, migration)
- `trident-concepts/` – Core concepts: provisioning workflow, virtual storage pools, volume access groups, and snapshots
- `trident-protect/` – Trident Protect installation, configuration, and use cases: application protection (snapshots, backups), restore, migration, SnapMirror replication, and KubeVirt VM support
- `trident-reco/` – Recommendations and best practices for storage configuration, security, backup, and Trident integration design
- `trident-reference/` – Reference documentation: Kubernetes objects, REST API, tridentctl CLI, ports, and pod security
- `trident-docker/` – Trident deployment and configuration for Docker environments using nDVP
- `_include/` – Shared content snippets reused across multiple pages (AsciiDoc includes)
- `media/` – Images and diagrams used in documentation pages

### Product-specific context

**Architecture and components:**
- Trident runs as a single *Controller Pod* (manages volume provisioning and snapshots, runs as a Kubernetes Deployment) and one or more *Node Pods* (handles mounting/unmounting storage on each worker node, runs as a Kubernetes DaemonSet)
- The Trident operator (`TridentOrchestrator` CR) is the recommended installation method; it manages Trident lifecycle, provides self-healing, and handles Kubernetes upgrades automatically
- `tridentctl` is the command-line utility for managing Trident installations and resources
- *Trident Protect* is a separate component that provides advanced application data management (backup, restore, snapshot scheduling, SnapMirror replication, migration) for stateful Kubernetes applications

**Key concepts:**
- *Backend* – Defines the relationship between Trident and a NetApp storage system; specifies how Trident communicates with the storage system and provisions volumes from it
- *Storage class* – A Kubernetes `StorageClass` that maps to one or more Trident backends via storage pool attributes and selectors
- *Virtual pool* – An abstraction layer within a backend that lets administrators define groups of storage pools with distinct attributes (performance, location, protection) without exposing backend details to `StorageClasses`
- *Storage pool* – A subset of storage capacity within a backend; Trident matches storage pools to storage classes based on requested attributes
- *AppVault* – A Trident Protect object that defines a location (object store bucket) where application backup and snapshot data is stored
- *TridentOrchestrator* – The custom resource (CR) used to install and configure Trident when using the operator installation method

**Supported storage platforms and backend drivers:**
- *ONTAP NAS drivers*: `ontap-nas` (FlexVolume per PV), `ontap-nas-economy` (qtrees per FlexVolume), `ontap-nas-flexgroup` (FlexGroup per PV) — use NFS or SMB protocols
- *ONTAP SAN drivers*: `ontap-san` (LUN per PV in its own FlexVolume), `ontap-san-economy` (multiple LUNs per FlexVolume) — use iSCSI, NVMe/TCP, or FC protocols
- *Cloud platforms*: Amazon FSx for NetApp ONTAP, Azure NetApp Files (ANF), Google Cloud NetApp Volumes (GCNV), Cloud Volumes ONTAP
- *Element software*: Used for NetApp HCI and SolidFire environments
- NAS drivers support ReadWriteMany (RWX) access mode; SAN drivers in Filesystem volume mode do not support RWX

**Naming conventions and terminology:**
- *Trident* always refers to the CSI storage orchestrator (not "NetApp Trident" in running text unless introducing the product)
- *tridentctl* is the CLI utility (lowercase, no spaces)
- *nDVP* is the NetApp Docker Volume Plugin used for Docker integration
- *TridentOrchestrator* is the exact CR name (CamelCase, no spaces)
- Backend driver names are lowercase with hyphens: `ontap-nas`, `ontap-san`, `ontap-nas-economy`, `ontap-san-economy`, `ontap-nas-flexgroup`
- Access modes use abbreviations: RWO (ReadWriteOnce), ROX (ReadOnlyMany), RWX (ReadWriteMany), RWOP (ReadWriteOncePod)
- *AppVault* and *AppArchive* are Trident Protect-specific terms (CamelCase)

### Typical user workflows

**Deploy Trident on Kubernetes:** Review requirements → Choose installation method (operator/Helm/tridentctl) → Install Trident → Verify deployment with `tridentctl version`

**Configure storage backend:** Create backend configuration file (JSON or YAML) → Apply with `tridentctl create backend` or `kubectl apply` → Verify backend is online

**Provision persistent storage:** Configure backend → Create `StorageClass` referencing backend attributes → Create `PersistentVolumeClaim` (PVC) → Trident provisions volume from matching backend

**Protect applications with Trident Protect:** Install Trident Protect → Configure AppVault (object store) → Define application scope → Create protection policy or on-demand snapshot/backup → Restore from snapshot or backup as needed

**Upgrade Trident:** Review upgrade requirements → Choose upgrade method matching installation method (operator/Helm/tridentctl) → Perform upgrade → Verify with `tridentctl version`
