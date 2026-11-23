# Investigation Notes – Lab 30: File Hash and Threat Intelligence Correlation

## 1. Investigation Objective

The objective was to examine how cryptographic hashes can identify files, support integrity verification, and provide indicators for correlation with external threat-intelligence databases.

The investigation used two controlled text files and a deliberate content modification to observe the effect on calculated hashes.

## 2. Environment and Tools

- Ubuntu 24.04 WSL
- `sha256sum`
- `sha1sum`
- `md5sum`
- `file`
- `stat`
- VirusTotal hash lookup

Workspace:

```text
/home/abhinav/malware-yara-labs/lab30
```

All required command-line utilities were available under `/usr/bin/`.

## 3. Sample Identification

Two controlled files were created:

| File | Initial Size | File Type |
|---|---:|---|
| `sample-a.txt` | 58 bytes | ASCII text |
| `sample-b.txt` | 58 bytes | ASCII text |

Both files were identified using the `file` utility.

The `stat` utility was used to collect file size and modification-time information.

The initial modification timestamps were:

```text
2026-11-23 05:18:05.470898661 +0000
```

## 4. Initial Hash Analysis

The following hashes were collected before modifying either sample.

### Sample A – `sample-a.txt`

| Algorithm | Hash |
|---|---|
| MD5 | `32426d55aa7bcc61c008cd122660004b` |
| SHA-1 | `65f455f9851ffaf595493bc19418c0f3a8f5e6a8` |
| SHA-256 | `781f4c817e37d056e4ce9955cf075667bd67856272b36bb372489c86dfe4a926` |

### Sample B – `sample-b.txt`

| Algorithm | Hash |
|---|---|
| MD5 | `ffb0d96a1bbf6b4427244203dd642ba1` |
| SHA-1 | `7bb68f6b96ab2c8248726b09dee4a3686ddf2315` |
| SHA-256 | `7ebe6d848ca1952e67fb5a73d2c2897d20d4ea6b0633c96da4330a1e34e3c575` |

### Observation

Although both files had the same initial size and file type, their hash values differed.

This is consistent with their different contents.

## 5. File Integrity Verification

The original SHA-256 hash of `sample-a.txt` was recorded:

```text
781f4c817e37d056e4ce9955cf075667bd67856272b36bb372489c86dfe4a926
```

The following text was appended:

```text
Additional test content
```

The SHA-256 hash was calculated again:

```text
024248d81991da4fb49c64bf80082af4bf8a60368b29204535089f1b01a0f68f
```

### Observation

The original and modified SHA-256 values were different.

This confirms that the file's content changed and demonstrates the use of cryptographic hashes for integrity verification.

The modified hash was saved separately in:

```text
evidence/sample-a-modified-hash.txt
```

Modified MD5 and SHA-1 values were not collected.

## 6. Threat Intelligence Correlation

### Source

VirusTotal

### Lookup Method

SHA-256 hash search through the VirusTotal web interface.

### Observation

No file report was visible for the earlier lookup.

The available result did not provide:

- A detection ratio
- A file reputation verdict
- Vendor detection names
- File analysis details

The exact hash used in that earlier lookup was not recorded alongside the terminal evidence. Therefore, it cannot be conclusively associated with either sample.

### Interpretation

An unavailable report indicates that no usable reputation result was obtained for the query.

It does not establish that the file is benign or malicious.

## 7. Evidence Collected

| Evidence File | Purpose |
|---|---|
| `evidence/file-metadata.txt` | Initial file type, size, and modification-time information |
| `evidence/md5-hashes.txt` | Initial MD5 values |
| `evidence/sha1-hashes.txt` | Initial SHA-1 values |
| `evidence/sha256-hashes.txt` | Initial SHA-256 values |
| `evidence/sample-a-modified-hash.txt` | SHA-256 after modifying Sample A |
| `output/threat-intelligence-report.txt` | Investigation summary and interpretation |

## 8. Evidence-Based Assessment

### Confirmed

- Two controlled ASCII text samples were created.
- Both initial samples were 58 bytes.
- Initial MD5, SHA-1, and SHA-256 hashes were calculated.
- Their initial SHA-256 hashes were different.
- Sample A was modified by appending additional text.
- Its SHA-256 hash changed after modification.
- Initial metadata and hash records were saved.

### Unknown or Not Established

- Whether either sample has a reputation record in VirusTotal.
- Whether either sample has ever been reported as malicious.
- Whether either sample exhibits malicious behavior.
- Any malware execution or system compromise.

No malicious behavior was established during this exercise.

## 9. Conclusion

The investigation successfully demonstrated cryptographic file identification and integrity verification.

The VirusTotal lookup did not produce a visible file report, so no threat-intelligence verdict could be assigned.

The results demonstrate that file hashes are useful investigation indicators, but reputation information must be interpreted alongside additional evidence.
