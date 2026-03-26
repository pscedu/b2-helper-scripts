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

## License

Copyright (C) 2026 Ivan Cao-Berg. Released under the GNU General Public License v2. See script headers for full license text.
