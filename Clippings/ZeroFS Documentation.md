---
title: ZeroFS Documentation
source: https://www.zerofs.net/docs/
author:
published:
created: 2026-07-06
description: ZeroFS serves S3-compatible buckets as POSIX filesystems over NFS and 9P, and as block devices over NBD.
tags:
  - clippings
  - aws
  - s3
---
ZeroFS serves S3-compatible buckets as POSIX filesystems over NFS and 9P, and as block devices over NBD. NFS and 9P need nothing installed on the client because the OS already ships support; a bundled FUSE client mounts without root. The server runs in userspace, with no kernel modules. CI compiles the Linux kernel on ZeroFS mounts on every change.[Get Started](https://www.zerofs.net/docs/quickstart)

[

Configuration

](https://www.zerofs.net/docs/configuration)

## What ZeroFS does

ZeroFS stores your files in S3 and uses local disk only as a cache, so databases, code builds, and VM boot disks run directly against the bucket. It differs from other "filesystem on S3" projects in a few ways:

- **POSIX-conformant**, verified against pjdfstest and xfstests.
- **Near-raw-S3 throughput on large files**, and scales to workloads of millions of small files.
- **No external database service**; all state, data and metadata alike, lives in the bucket.
- **Encrypted before upload**: every block is compressed, then encrypted with XChaCha20-Poly1305, with no unencrypted mode.[Learn about encryption](https://www.zerofs.net/docs/encryption)

## Guides

### Encryption & Security

Learn how ZeroFS encrypts your data with XChaCha20-Poly1305.

[Read more](https://www.zerofs.net/docs/encryption)

### Configuration

Configure S3 backends, caching, and network settings.

[Read more](https://www.zerofs.net/docs/configuration)

### Troubleshooting

Debug common issues and understand error messages.

[Read more](https://www.zerofs.net/docs/troubleshooting)

### Advanced Use Cases

Build geo-distributed storage and tiered architectures.

[Read more](https://www.zerofs.net/docs/advanced-use-cases)

## Resources

### Quickstart

Get up and running with ZeroFS in minutes. Install, configure, and mount your first S3 filesystem.

### NFS File Access

Mount ZeroFS as a network filesystem on any OS. Access files with standard POSIX operations.

### 9P File Access

High-performance file access on Linux using the 9P protocol with advanced caching options.

### Client Libraries

Access ZeroFS from Python, TypeScript, or Go. Path-based filesystem operations over 9P, no mount required.

### NBD Block Devices

Create raw block devices backed by S3. Perfect for ZFS pools, databases, or any filesystem.

### Kubernetes CSI Driver

Dynamically provisioned persistent volumes backed by a shared ZeroFS gateway, mounted over 9P.