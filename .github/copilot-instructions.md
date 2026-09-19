## Copilot instructions for Trident documentation

### Repository overview
Product: Trident

Trident is NetApp's storage provisioner and data-management framework for Kubernetes and containerized workloads. It dynamically provisions persistent storage, manages storage classes and backends, and supports operational features such as snapshots, replication, and application protection.

### Repository structure
- `trident-get-started/` – Introductory docs covering Trident overview, architecture, quick-start onboarding, and prerequisites for new users.
- `trident-install/` – Installation and deployment guidance for Kubernetes, OpenShift, Helm, the Trident operator, customizations, and `tridentctl`-based setups.
- `trident-use/` – Day-to-day storage usage and management tasks for creating, using, and operating volumes in Kubernetes workloads.
- `trident-managing-k8s/` – Upgrade, uninstall, and lifecycle management procedures for Trident in Kubernetes clusters.
- `trident-docker/` – Docker-specific setup, deployment, and legacy configuration guidance for non-Kubernetes environments.
- `trident-concepts/` – Core storage concepts and abstractions, including provisioning, snapshots, virtual storage pools, and volume access groups.
- `trident-protect/` – Documentation for Trident Protect, including installation, authorization, monitoring, backups, restores, replication, migration, and KubeVirt application protection.
- `trident-reco/` – Recommended practices and operational guidance for designing reliable, resilient Trident deployments.
- `trident-reference/` – Reference material, command details, and backend or configuration specifics for administrators and integrators.
- `_include/` – Shared AsciiDoc snippets, notes, and reusable content fragments used throughout the documentation set.
- `media/` – Graphics, screenshots, and other visual assets referenced in the docs.
- `.github/` – Repository automation, issue templates, and AI/copilot guidance files used to support contributors and workflows.
- Top-level docs such as `faq.adoc`, `troubleshooting.adoc`, `known-issues.adoc`, `get-help.adoc`, `trident-rn.adoc`, `blog-posts.adoc`, `earlier-versions.adoc`, and `legal-notices.adoc` – Support, release, troubleshooting, lifecycle, and navigation content for the documentation site.

### Product-specific context

**Architecture and components:**
- Trident runs as a storage orchestrator in Kubernetes, with a controller and driver interfaces that communicate with backend storage systems such as NetApp ONTAP and Element.
- StorageClass objects define how workloads request storage, while Trident maps those requests to backend capabilities and policies.
- The `tridentctl` CLI and the Trident operator are the primary deployment and operational management tools for installation, upgrades, and configuration.
- Trident Protect builds on the core provisioner to provide application-centric backup, restore, replication, and migration workflows.

**Key concepts:**
- `StorageClass` – The Kubernetes object that tells Trident which storage backend and policy to use for a workload.
- `Backend` – The underlying storage system or set of storage resources configured for Trident.
- `Volume` – The persistent storage object created by Trident for a workload.
- `Snapshot` – A point-in-time copy of a volume for backup or restore workflows.
- `Virtual storage pool` – A policy-based grouping of backend capabilities such as performance, protection, or capacity.
- `Volume access group` – A mechanism for controlling which workloads or nodes can access a volume.

**Naming conventions and terminology:**
- Use “Trident” for the core storage provisioner; use “Trident Protect” only when referring to the application data protection product.
- Use `tridentctl` for the CLI command and “Trident operator” for the operator-based deployment model.
- “Backend” is the configured storage system or storage pool definition that Trident provisions from; do not confuse it with a Kubernetes workload or backing pod.
- “OpenShift” is a Kubernetes distribution; treat it as a variant of Kubernetes that may require OpenShift-specific installation or certification guidance.
- “ONTAP,” “Element,” and “backend configuration” are common storage-system terms in this repo; use them consistently when describing storage capabilities.

### Typical user workflows

**Initial setup:** Learn about Trident → Review requirements → Install with the operator or `tridentctl` → Configure a backend and `StorageClass` → Provision and attach workloads

**Operational maintenance:** Monitor backend health → Upgrade or patch Trident → Manage snapshots and persistent volumes → Troubleshoot storage configuration issues

**Data protection:** Define application protection requirements → Install and configure Trident Protect → Back up, restore, or replicate applications → Validate recovery and migration workflows