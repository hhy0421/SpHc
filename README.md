# SpHc binary distribution

Linux x86-64 executables for multi-FPGA partitioning and hardware cabling. This repository distributes binaries and usage instructions; C++ source files are not included.

## Download

The archive is stored in this repository:

[`dist/SpHc-linux-x86_64-20261118.tar.gz`](dist/SpHc-linux-x86_64-20261118.tar.gz)

Downloading this repository as a ZIP includes the archive as well, so a single repository download provides everything needed.

## Requirements

- Linux x86-64; tested on Ubuntu 24.04 / WSL Ubuntu.
- glibc providing `GLIBC_2.38` and libstdc++ providing `GLIBCXX_3.4.32` or newer compatible versions.
- These are Linux ELF executables, not native Windows `.exe` files.

## Run

```bash
tar -xzf dist/SpHc-linux-x86_64-20261118.tar.gz
cd SpHc-linux-x86_64-20261118
sha256sum -c SHA256SUMS

./sphc_main --input /path/to/design.net --output construction

./sphc_refine_main --input /path/to/design.net \
  --partition construction/partition.txt \
  --topology construction/banks.txt \
  --output refined --tdmax-weight GAMMA
```

`sphc_main` runs the original complete construction frontend, retaining its local refinement stages. `sphc_refine_main` runs multilevel joint refinement. Its `--topology` argument takes the **three-column `banks.txt`**, containing FPGA endpoint IDs and bank counts.

Replace `GAMMA` with the maximum-delay coefficient for your experiment; `0` selects total-TDM-only optimization. Use `--help` for command-line parameters. The input netlist is a text file whose lines contain a source node name, an integer communication weight, and drain node names, for example `source 1 drain_a drain_b`.

## Verification

The distributed executables have been stripped of debug/static symbols. A small end-to-end check confirmed their construction and refinement outputs match the unstripped executables. The archive includes usage instructions, runtime requirements, and SHA-256 checksums.
