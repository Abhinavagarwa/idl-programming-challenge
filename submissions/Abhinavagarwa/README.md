<h1>RISC-V PMP Checker - Abhinavagarwa<h1>

Author: Abhinavagarwa

Overview:

This script is designed to verify access permissions for physical addresses in RISC-V processors based on Physical Memory Protection (PMP) rules. It follows the guidelines outlined in Chapter 3.7 of the RISC-V Privileged Architecture specification. The program accepts a PMP configuration file, a physical address, a privilege mode, and an access type as input arguments and determines whether the access is permitted or results in a fault.


Execution Instructions:

This script does not require compilation and can be run directly using Python.

To execute the program, use the following command:

```bash
python pmp_checker.py <pmp_config_file> <physical_address> <privilege_mode> <operation>
```

Where:

- `<pmp_config_file>`: Path to the PMP configuration file. The file consists of 128 lines:
  - The first 64 lines represent the hexadecimal values of pmpNcfg registers (N = 0 to 63).
  - The last 64 lines represent the hexadecimal values of pmpaddrN registers (N = 0 to 63).
- `<physical_address>`: The target address for verification, provided in hexadecimal format (e.g., 0xdeadbeef).
- `<privilege_mode>`: The privilege level of the access attempt. Acceptable values: `M` (Machine), `S` (Supervisor), or `U` (User).
- `<operation>`: The type of memory access: `R` (Read), `W` (Write), or `X` (Execute).

Example Usage:
```bash
python pmp_checker.py pmp_configuration.txt 0x80000000 M R
```

---

Dependencies:

This script is implemented using standard Python libraries and does not require any additional dependencies.

---

Implementation Details:

1. Reading Configuration: The script loads the PMP settings from the configuration file, storing `pmpNcfg` and `pmpaddrN` values in separate lists.
2. Parsing Inputs: It processes command-line arguments, converting the physical address to an integer and validating privilege modes and access types.
3. PMP Region Matching: The script iterates through PMP entries to determine if the given address falls within a defined PMP region. It evaluates:
   - `TOR (Top of Range)`: Defines a memory region between a previous address and the current PMP address.
   - `NA4 (Naturally Aligned 4-byte)`: Protects a fixed 4-byte region.
   - `NAPOT (Naturally Aligned Power of Two)`: Covers larger aligned memory blocks.
4. Access Permission Check: If the address falls within a matching PMP region, the script checks the access permissions based on `R/W/X` bits in `pmpNcfg`.
5. Output Decision: If the access is allowed, the script prints `Access allowed`; otherwise, it prints `Access fault`.

Testing Scenarios:

The script has been tested under multiple conditions, including:

- Access requests inside and outside PMP regions.
- Various combinations of `R`, `W`, and `X` permissions.
- Addresses located at PMP region boundaries.
- Handling of `NAPOT` regions with different configurations.
- Error handling for invalid inputs (e.g., incorrect file paths, malformed addresses, and unsupported modes).

This ensures robust functionality across different access conditions.

Conclusion:

The RISC-V PMP Checker provides a straightforward method to validate memory access permissions on RISC-V processors. It efficiently interprets PMP configurations and verifies access control based on defined security rules. This tool is particularly useful for developers working on RISC-V-based embedded systems, operating systems, and security-sensitive applications.

