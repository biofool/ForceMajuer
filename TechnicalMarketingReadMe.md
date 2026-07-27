# Technical Marketing Summary — ForceMajuer

## One-Line Positioning

A security hardening methodology that combines file system analysis and code coverage analysis to identify and remove unused binaries and dead code from running systems, minimizing attack surface.

## Target Users / Personas

- **Security engineers** responsible for system hardening and attack surface reduction
- **DevOps/SRE teams** building minimal, lean production images
- **Compliance teams** needing evidence that only necessary software is deployed
- **Embedded systems developers** minimizing firmware footprint and exposure

## Key Features (Grounded in Code)

- **File system analysis methodology** — configures file system logs to track reads/writes, then cross-references accessed files against all installed binaries to identify unused ones (`README.md`)
- **Python script for package/binary inventory** — creates an SQLite database of all installed packages and their associated binaries, then cross-references with file system access data (`README.md`)
- **Code coverage analysis integration** — uses Gcov (C/C++), JaCoCo (Java), and Coverage.py (Python) to identify unused code blocks within active binaries (`README.md`)
- **Iterative removal process** — structured approach: remove something you think is unused, verify everything still works, then progressively remove more (`README.md`)
- **Non-intrusive monitoring** — uses file system access data rather than real-time tools (auditd, SELinux, eBPF) to avoid disturbing the system under test (`README.md`)

## Technical Differentiators

- **Non-intrusive approach** — deliberately avoids auditd, SELinux, and eBPF due to their potential to disturb system behavior; uses file system access logs instead
- **Two-layer analysis** — combines file system analysis (unused binaries) with code coverage analysis (unused code within active binaries) for deeper exposure reduction
- **Package-level view** — starts from installed packages and their binaries, providing a system-wide inventory perspective
- **Iterative, safe removal** — structured methodology that verifies functionality after each removal step

## Use Cases

- Hardening production server images by removing unused packages and binaries
- Identifying dead code in deployed applications for security-driven refactoring
- Auditing container images to ensure minimal attack surface
- Compliance evidence that only necessary software is deployed

## Benefits / Value Proposition

- Reduces attack surface by removing unused binaries and dead code — every removed component eliminates potential entry points for malicious actors
- Non-intrusive methodology preserves system stability during analysis
- Evidence-based approach — removal decisions are backed by file system access data and code coverage metrics
- Combines two analysis techniques for comprehensive exposure reduction

## Tech Stack

- **Analysis tools**: File system access logging, Python + SQLite (package/binary inventory)
- **Code coverage**: Gcov (C/C++), JaCoCo (Java), Coverage.py (Python)
- **Methodology**: Iterative removal with functional verification
- **License**: CC0 1.0 Universal (Creative Commons Public Domain)

## Known Limitations

- **Conceptual/methodology project** — the README describes the approach but no actual scripts or tooling are included in the repository
- **No runnable code** — the Python script for SQLite-based inventory is described but not provided
- **Risk of breaking systems** — removing "unused" packages can break functionality if the package is used infrequently or under specific conditions (acknowledged in the README)
- **No automation** — the iterative removal process is manual
- **File system log configuration unspecified** — the README does not detail how to configure file system access logging on specific operating systems
