# b2-helper-scripts

Helper scripts for working on [Bridges-2](https://www.psc.edu/resources/bridges-2/), the high-performance computing system at the Pittsburgh Supercomputing Center (PSC).

## What is Bridges-2?

Bridges-2 is an HPC system operated by the Pittsburgh Supercomputing Center and funded by the National Science Foundation. It is designed to support a wide range of computational science workloads, from traditional HPC simulations to large-scale AI/ML training and data analytics.

Key characteristics:

- **Scheduler**: SLURM
- **Partitions**: Multiple partitions serve different hardware tiers — general CPU nodes, GPU nodes (V100, A100, H100, L40S), AMD GPU nodes (MI210), and specialty nodes for memory-intensive work
- **Storage**: Networked filesystems including `/ocean` (project storage) and `/jet` (home directories), plus node-local scratch (`/local`)
- **Module system**: Software is managed via `module load`; use `module avail` to browse available packages

Access requires an allocation through [ACCESS](https://access-ci.org/) (formerly XSEDE) or a PSC local allocation.

---

## Scripts

### `b2-gpus`

Queries SLURM and prints a table of all GPU nodes across every partition on Bridges-2, with optional filtering by partition or node state. Can also display detailed hardware specifications and an example `sbatch` script for any supported GPU model.

#### Requirements

- Must be run on a Bridges-2 login or compute node (requires `sinfo`)
- Optional: `module load rich` for formatted output (automatically attempted at startup)

#### Usage

```
b2-gpus [OPTIONS]
```

| Option | Description |
|---|---|
| `-p, --partition PARTITION` | Filter nodes by partition name (e.g. `GPU`, `GPU-dev`, `ROBO`, `HACC`) |
| `-s, --state STATE` | Filter nodes by state (e.g. `idle`, `mixed`, `alloc`) |
| `-i, --info GPU` | Print hardware specs and an example sbatch script for a GPU model |
| `-h, --help` | Show help and exit |

#### Examples

List all GPU nodes across all partitions:

```bash
b2-gpus
```

Show only idle nodes in the `GPU` partition:

```bash
b2-gpus --partition GPU --state idle
```

Show hardware specs and a sample job script for the H100:

```bash
b2-gpus --info h100
```

#### Supported GPU models (`--info`)

| Key | GPU | Partition | GRES type |
|---|---|---|---|
| `h100` | NVIDIA H100 SXM5 | `ROBO` | `h100` |
| `a100` | NVIDIA A100 PCIe | `GPU-dev` | `a100` |
| `v100` | NVIDIA V100 SXM2 | `GPU` | `v100-32` |
| `l40s` | NVIDIA L40S | `GPU` | `l40s-48` |
| `mi210` | AMD Instinct MI210 | `HACC` | `mi210` |

#### Node states

| State | Meaning |
|---|---|
| `idle` | Node is free and accepting jobs |
| `mixed` | Node is partially allocated |
| `alloc` / `allocated` | Node is fully allocated |
| `drained` / `draining` | Node is being taken offline |
| `down` | Node is unavailable |
| `maint` | Node is under maintenance |

---

### Example

```
> b2-gpus -p GPU-shared -s mixed

⚡ SLURM GPU nodes across all partitions on Bridges-2

                             GPU Nodes
╭───────┬────────────┬────────────────────────┬───────┬───────────╮
│ Node  │ Partition  │ GPU GRES               │ State │ Available │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ gl001 │ GPU-shared │ gpu:l40s-48:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ gl003 │ GPU-shared │ gpu:l40s-48:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v013  │ GPU-shared │ gpu:v100-32:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v019  │ GPU-shared │ gpu:v100-32:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v021  │ GPU-shared │ gpu:v100-32:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v029  │ GPU-shared │ gpu:v100-16:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v030  │ GPU-shared │ gpu:v100-16:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v031  │ GPU-shared │ gpu:v100-16:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v032  │ GPU-shared │ gpu:v100-16:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ v034  │ GPU-shared │ gpu:v100-32:16(S:0-95) │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w001  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w002  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w003  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w004  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w005  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w006  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w007  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w008  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w009  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
├───────┼────────────┼────────────────────────┼───────┼───────────┤
│ w010  │ GPU-shared │ gpu:h100-80:8          │ mixed │ up        │
╰───────┴────────────┴────────────────────────┴───────┴───────────╯
```

---
Copyright © 2020-2026 Pittsburgh Supercomputing Center. All Rights Reserved.

The [Biomedical Applications Group](https://www.psc.edu/biomedical-applications/) at the [Pittsburgh Supercomputing Center](http://www.psc.edu) in the [Mellon College of Science](https://www.cmu.edu/mcs/) at [Carnegie Mellon University](http://www.cmu.edu).
