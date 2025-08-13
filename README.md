# acpidiag

**ACPI Diagnostic and Decoder Utility for CXL and Memory Topology Tables**

## Overview

`acpidiag` is a Python-based utility for extracting, decoding, and validating ACPI tables relevant to CXL (Compute Express Link) and memory topology. It automates the use of `acpidump`, `acpixtract`, and `iasl` to process tables such as CEDT, SLIT, HMAT, and PMTT, providing both human-readable and machine-readable outputs, including hierarchical tree and graph representations.

## Requirements

- Python 3.6+
- `acpidump`, `acpixtract`, and `iasl` binaries available in your PATH or specified via arguments.
- Root privileges (for ACPI table access).
- **Important:** This script expects the latest version of `acpidump` to be available. The version packaged with most Linux distributions is often too old to work with CXL tables. For instructions on building the latest version from source, see:
  - [How to build acpidump from source and use it to debug complex CXL and PCI issues](https://stevescargall.com/blog/2025/08/how-to-build-acpidump-from-source-and-use-it-to-debug-complex-cxl-and-pci-issues/)

## References

- **CXL Specification:** [Compute Express Link (CXL) Specification](https://www.computeexpresslink.org/spec-landing)
- **ACPI Specification:** [ACPI Specification](https://uefi.org/specifications)
- **UEFI Specification:** [UEFI Specification](https://uefi.org/specifications)

## Features

- Automated ACPI Table Extraction
- Disassembly and Decoding
- Robust Parsing for CXL and memory topology
- Validation Engine for spec compliance and logical consistency
- Output Formats: text, ASCII art, hierarchical tree, graph, and JSON
- Error Handling and Extensibility

## Installation

Clone the repository and ensure dependencies are available:

```bash
git clone https://github.com/sscargal/acpidiag.git
cd acpidiag
# Ensure acpidump, acpixtract, iasl are installed and accessible
```

## Usage

```bash
sudo ./acpidiag --tables CEDT,SLIT,HMAT,PMTT
```

### Options

- `--tables <TABLES>`: Comma-separated list of ACPI tables to process (e.g., CEDT, SLIT).
- `--acpidump <PATH>`: Path to `acpidump` binary (optional).
- `--iasl <PATH>`: Path to `iasl` binary (optional).
- `--workdir <DIR>`: Working directory for output files (default: `acpi_workdir`).
- `--verbose`: Increase output verbosity.

## Output

- Disassembled ACPI tables in the working directory.
- Topology visualizations:
  - `cedt_tree_<key>.txt`: Hierarchical tree (text).
  - `cedt_graph_<key>.txt`: Simple graph (text).
  - `cedt_tree_<key>.json`: Hierarchical tree (JSON).
- Validation report:
  - `validation_report_<key>.txt`: Detailed pass/fail results for spec and logical checks.

## Example Workflow

1. Run the tool with root privileges to extract and decode tables.
2. Review the output files in `acpi_workdir/` for topology and validation results.
3. Use the JSON output for integration with other tools or visualization frameworks.

## Validation Checks

- Host Bridge UID uniqueness.
- Interleave member/target consistency.
- Memory window address continuity.
- XOR interleave math structure pairing.
- SLIT matrix symmetry.
- PMTT device presence.
- Extensible for additional rules.

## Troubleshooting

- **Permission Denied**: Run with `sudo` to access ACPI tables.
- **Missing Binaries**: Ensure `acpidump`, `acpixtract`, and `iasl` are installed.
- **No Output**: Check verbose logs for extraction/disassembly errors.

## Contributing

Contributions are welcome! Please submit issues or pull requests for bug fixes, new features, or spec updates.

## License

See `LICENSE` for details.
