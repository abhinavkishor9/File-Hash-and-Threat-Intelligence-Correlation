# Lab 30 – File Hash and Threat Intelligence Correlation

## Overview

This lab demonstrates how security analysts use cryptographic file hashes to identify files, verify file integrity, and correlate file indicators with external threat-intelligence sources.

The exercise uses two controlled text files created in Ubuntu 24.04 WSL. MD5, SHA-1, and SHA-256 hashes were calculated, basic file metadata was collected, and a controlled modification was performed to demonstrate how file content changes affect its SHA-256 hash.

A VirusTotal hash lookup was also attempted to understand how analysts handle files without an available reputation report.

## Objectives

- Understand file hashing and its role in malware analysis.
- Calculate MD5, SHA-1, and SHA-256 hashes.
- Collect basic file metadata.
- Compare hashes of different files.
- Verify file integrity after a controlled modification.
- Understand hash-based threat-intelligence lookups.
- Interpret unavailable reputation results without making unsupported conclusions.

## Environment

| Component | Details |
|---|---|
| Operating System | Ubuntu 24.04 WSL |
| Workspace | `~/malware-yara-labs/lab30` |
| Hash utilities | `sha256sum`, `sha1sum`, `md5sum` |
| File identification | `file` |
| Metadata collection | `stat` |
| Threat intelligence | VirusTotal |

## Lab Structure

```text
lab30/
├── evidence/
│   ├── file-metadata.txt
│   ├── md5-hashes.txt
│   ├── sample-a-modified-hash.txt
│   ├── sha1-hashes.txt
│   └── sha256-hashes.txt
├── output/
│   └── threat-intelligence-report.txt
├── samples/
│   ├── sample-a.txt
│   └── sample-b.txt
└── tools/
```

## Investigation Summary

Two synthetic ASCII text files were created:

- `sample-a.txt`
- `sample-b.txt`

Both files initially contained 58 bytes.

The following hash algorithms were used:

- MD5
- SHA-1
- SHA-256

The initial SHA-256 hashes were different, confirming that the files had different content.

A controlled modification was then performed on `sample-a.txt` by appending an additional line:

```text
Additional test content
```

The SHA-256 hash changed from:

```text
781f4c817e37d056e4ce9955cf075667bd67856272b36bb372489c86dfe4a926
```

To:

```text
024248d81991da4fb49c64bf80082af4bf8a60368b29204535089f1b01a0f68f
```

This demonstrated how a content modification produces a different cryptographic digest.

## Threat Intelligence Correlation

A SHA-256 hash lookup was attempted using VirusTotal.

No file report was visible for the earlier lookup. Therefore:

- Detection ratio was unavailable.
- File reputation could not be established.
- No malicious or benign verdict could be assigned.

The exact hash used in that earlier lookup was not recorded alongside the terminal evidence. Consequently, the result is not attributed to a particular sample.

## Key Findings

- All required hash and metadata utilities were available.
- Both controlled samples were identified as ASCII text.
- Initial MD5, SHA-1, and SHA-256 values were collected.
- The initial SHA-256 hashes differed between the two samples.
- Modifying `sample-a.txt` changed its SHA-256 digest.
- File metadata and hash evidence were saved.
- VirusTotal did not provide a visible file report for the earlier lookup.

## Limitations

- The samples were synthetic text files, not malware.
- No malware execution or malicious activity was investigated.
- No VirusTotal detection verdict was obtained.
- Modified MD5 and SHA-1 values were not collected.
- The earlier VirusTotal lookup hash was not preserved with the available evidence.
- Hash reputation alone cannot establish execution or system compromise.

## Conclusion

This lab demonstrated practical file identification, cryptographic hashing, metadata collection, and integrity verification.

It also illustrated an important threat-intelligence principle: an unavailable hash report means that reputation is unknown, not that the file is necessarily safe or malicious.

The investigation remained limited to controlled file analysis and hash-based reputation lookup.
