# acpidiag - ACPI Diagnostic and Decoder Utility for CXL and Memory Topology Tables

## Overview

`acpidiag` is a Python-based utility for extracting, decoding, and validating ACPI tables relevant to CXL (Compute Express Link) and memory topology. It automates the use of `acpidump`, `acpixtract`, and `iasl` to process tables such as CEDT, SLIT, HMAT, and PMTT, providing both human-readable and machine-readable outputs, including hierarchical tree and graph representations.

## Requirements

- Python 3.6+
- `acpidump`, `acpixtract`, and `iasl` binaries available in your PATH or specified via arguments.
- Root privileges (for ACPI table access).
- **Important:** This script expects the latest version of `acpidump`, `acpixtract`, `iasl` to be available. The version packaged with most Linux distributions is often too old to work with CXL tables. For instructions on building the latest version from source, see:
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
# Ensure acpidump, acpixtract, iasl are installed and accessible in your $PATH env.
```

## Usage

```bash
usage: acpidiag [-h] [--acpidump-path ACPIDUMP_PATH] [--iasl-path IASL_PATH] [--tables TABLES [TABLES ...]] [--working-dir WORKING_DIR] [--compare FILE1 FILE2] [-v]

A tool to decode and analyze ACPI tables, with a focus on CXL configurations.

options:
  -h, --help            show this help message and exit
  --acpidump-path ACPIDUMP_PATH
                        Path to the acpidump binary.
  --iasl-path IASL_PATH
                        Path to the iasl (ACPI Source Language) compiler/disassembler binary.
  --tables TABLES [TABLES ...]
                        List of ACPI tables to process (e.g., 'CEDT BGRT'). Defaults to all tables.
  --working-dir WORKING_DIR
                        Working directory for all output files. Defaults to `acpi_workdir`.
  --compare FILE1 FILE2
                        Compare ACPI dump files FILE1 vs FILE2. Similar to `diff`.
  -v, --verbose         Increase verbosity level (-v, -vv, -vvv)
```

**Examples**

1. Dump all tables and automatically process the tables with decoders (CEDT, SLIT, HMAT, and PMTT)

    ```bash
    sudo ./acpidiag
    ```

2. Dump and analyze the CEDT table

    ```bash
    sudo ./acpidiag --tables CEDT
    ```

## Output

- Disassembled ACPI tables in the working directory (`acpi_workdir`)
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

Example: The following shows how to analyze the CEDT table. 

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
