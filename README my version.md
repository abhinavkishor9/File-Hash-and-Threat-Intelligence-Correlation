# File-Hash-and-Threat-Intelligence-Correlation
## Overview
### 1. What Is File Hashing?

File hashing is the process of applying a mathematical algorithm to file contents to generate a fixed-length digital value called a hash or digest.

A hash acts as a digital fingerprint for the contents of a file.

For example, two files with different contents normally produce different hash values, even when they have the same filename, file extension, or size.

Hashing is useful in cybersecurity because it helps analysts identify files, compare file versions, and verify whether evidence has changed.

### 2. Common Hashing Algorithms

#### MD5

- Produces a 128-bit digest.
- Usually represented as 32 hexadecimal characters.
- Commonly encountered in older malware reports and file databases.
- Not considered collision-resistant for security-sensitive integrity verification.

#### SHA-1

- Produces a 160-bit digest.
- Usually represented as 40 hexadecimal characters.
- Still appears in legacy threat-intelligence records.
- Has known collision weaknesses and should not be relied on for modern collision-resistant integrity verification.

#### SHA-256

- Produces a 256-bit digest.
- Usually represented as 64 hexadecimal characters.
- Widely used for modern file identification and integrity verification.
- Preferred over MD5 and SHA-1 for this lab.

### 3. The Avalanche Effect

Cryptographic hash functions are designed so that a small change in the input produces a substantially different digest.

For example:

```text
Original file:
Sample content: baseline

Modified file:
Sample content: baseline
Additional test content
```

Even though only one line was appended, the resulting SHA-256 hash changed.

This property is called the avalanche effect.

It helps analysts identify differences between file contents through hash comparison.

### 4. File Integrity Verification

File integrity verification uses hashes to determine whether file contents have changed.

The process is:

1. Calculate a hash before a file is transferred, stored, or modified.
2. Preserve the original hash.
3. Calculate the hash again later.
4. Compare both values.

If the values match, the contents are consistent with the original file at the time of hashing.

If the values differ, the contents have changed or a different file was selected.

A matching hash supports integrity verification, but the reliability of the process also depends on correct evidence handling and preservation of the original reference value.

### 5. What Is Threat Intelligence Correlation?

Threat-intelligence correlation is the process of comparing observed indicators with external intelligence sources to identify previously reported threats or suspicious infrastructure.

Common indicators include:

- File hashes
- IP addresses
- Domains
- URLs
- File names
- Malware family identifiers

This lab focuses on file hashes.

A security analyst can calculate a file's SHA-256 hash and search for it in a threat-intelligence database.

If a report exists, the analyst can review its reputation information and available detections.

### 6. VirusTotal Hash Lookup

VirusTotal aggregates file and URL reputation information from multiple security vendors and analysis sources.

An analyst can search using a file hash without uploading the file itself.

A report may contain:

- Detection ratio
- Security vendor verdicts
- File type
- File size
- First and last submission dates
- Associated file names
- Related analysis information

Not every hash has a report.

A missing report means that the queried hash did not return available reputation information. It does not establish that the file is safe.

### 7. Interpreting Detection Results

#### Multiple detections

Several security engines identify the file as malicious or suspicious.

This is a strong investigation lead, but the analyst should still validate the result and consider file context.

#### Single detection

One engine flags the file.

The analyst should examine the vendor verdict, file characteristics, and possible false-positive explanations.

#### Zero detections

No reporting engine flags the file at the time of lookup.

This does not guarantee that the file is benign.

#### No report found

The service has no available report for the queried hash.

The reputation remains unknown.

### 8. Why Hash Reputation Is Not Enough

A malicious hash does not independently prove that a system is compromised.

An analyst must also investigate:

- Whether the file was present on the endpoint.
- Where the file originated.
- Whether it was executed.
- Which process launched it.
- Whether it created persistence.
- Whether it communicated with suspicious infrastructure.
- Whether related endpoint or network evidence exists.

Similarly, an unknown hash does not establish that a file is safe.

### 9. SOC Analyst Workflow

A practical hash-correlation workflow is:

1. Acquire or identify the file.
2. Record its name, size, type, and location.
3. Calculate its SHA-256 hash.
4. Preserve the hash as investigation evidence.
5. Search the hash in a threat-intelligence source.
6. Review the available reputation and detection information.
7. Correlate results with endpoint and network telemetry.
8. Document confirmed findings, investigation leads, and unknowns.

### 10. Key Takeaway

File hashes are useful indicators for file identification, integrity verification, and threat-intelligence correlation.

However, a hash is not proof of execution, malicious behavior, or system compromise.

A reliable investigation combines hash reputation with contextual evidence and careful interpretation.


This lab demonstrates how security analysts use cryptographic file hashes to identify files, verify file integrity, and correlate file indicators with external threat-intelligence sources.

The exercise uses two controlled text files created in Ubuntu 24.04 WSL. MD5, SHA-1, and SHA-256 hashes were calculated, basic file metadata was collected, and a controlled modification was performed to demonstrate how file content changes affect its SHA-256 hash.

A VirusTotal hash lookup was also attempted to understand how analysts handle files without an available reputation report.

## Lab Objectives

### Primary Objective

Develop practical skills in identifying files through cryptographic hashes, verifying file integrity, and interpreting external file-reputation information during a basic malware-triage investigation.

### Specific Objectives

#### 1. Hash Calculation

- Use Linux hashing utilities to calculate MD5, SHA-1, and SHA-256 values.
- Understand the different digest lengths produced by these algorithms.
- Recognize SHA-256 as the preferred algorithm for modern file identification and integrity verification.

#### 2. File Identification and Metadata Collection

- Identify file types using the `file` utility.
- Collect file sizes and modification timestamps using `stat`.
- Understand why file metadata should be recorded alongside hash values.
- Distinguish file names and extensions from actual file content.

#### 3. Integrity Verification

- Calculate and preserve an original file hash.
- Perform a controlled modification to a test file.
- Recalculate its hash after modification.
- Compare the original and modified values.
- Explain why a content modification changes the resulting digest.

#### 4. Threat-Intelligence Lookup

- Search a SHA-256 hash through a public threat-intelligence service.
- Understand the difference between an available report and an unavailable report.
- Identify the information normally included in a file-reputation report.
- Record detection information only when it is actually available.

#### 5. Evidence Handling

- Preserve original hash records separately from modified-file records.
- Maintain metadata and hash evidence in organized directories.
- Document the investigation in a repeatable format.
- Avoid replacing original evidence with later test results.

#### 6. Analytical Interpretation

- Distinguish unknown reputation from a benign verdict.
- Recognize that vendor detections require validation.
- Understand that a hash match does not prove file execution.
- Identify the additional endpoint and network evidence required for a broader security assessment.

### Expected Learning Outcomes

By completing this lab, the learner should be able to:

- Calculate and interpret common cryptographic file hashes.
- Perform a basic file-integrity verification exercise.
- Collect supporting file metadata.
- Conduct a hash-based threat-intelligence lookup.
- Interpret unavailable or inconclusive reputation results.
- Produce an evidence-based file-hash investigation report.

### Success Criteria

The exercise is considered complete when:

- Both controlled samples have been created and identified.
- Initial MD5, SHA-1, and SHA-256 hashes have been recorded.
- A controlled file modification has produced a different SHA-256 hash.
- The hash lookup outcome has been documented accurately.
- Evidence files and the investigation report have been preserved.

## Lab Scenario

### Scenario

A SOC analyst receives a request to examine an unidentified file discovered during an initial security investigation.

Before deciding whether the file requires further malware analysis, the analyst needs to establish its identity and collect basic evidence.

The analyst must also determine whether the file's hash has previously been reported by external threat-intelligence sources.

Rather than immediately executing or uploading an unknown file, the analyst begins with static identification and cryptographic hashing.

### Investigation Context

For this exercise, two controlled text files are created to simulate files collected during an investigation.

The analyst is required to:

- Identify each file's type and size.
- Calculate MD5, SHA-1, and SHA-256 hashes.
- Preserve the initial hash values.
- Modify one controlled sample.
- Recalculate its hash to verify file integrity.
- Search a hash through VirusTotal.
- Document the reputation lookup outcome.

The samples are intentionally harmless and contain no executable malware.

### Investigation Challenge

During the exercise, the analyst encounters two important observations.

First, the two samples have the same initial file size and file type, but their hash values differ. This requires the analyst to understand that file identification depends on content, not simply filenames or sizes.

Second, after additional text is appended to one sample, its SHA-256 value changes. The analyst must explain why this is useful for verifying file integrity.

The analyst also attempts a VirusTotal hash lookup but does not obtain a visible file report.

This introduces an important investigation limitation: an unavailable reputation report cannot be treated as a clean verdict.

### Analyst Responsibilities

The analyst must:

1. Preserve the original file-hash evidence.
2. Record file metadata and identify each sample.
3. Compare initial and modified hash values.
4. Interpret the threat-intelligence lookup without assuming a verdict.
5. Separate confirmed findings from unknown information.
6. Prepare a concise investigation report.

### Expected Investigation Outcome

The analyst should demonstrate successful file hashing and integrity verification.

The investigation should also establish an understanding of the limitations of reputation-based analysis.

No malware verdict is expected from the synthetic samples, and no compromise should be declared without supporting evidence.

### Scenario Conclusion

The exercise represents an early-stage SOC and malware-triage workflow in which file identity and reputation are investigated before making further decisions.

The key requirement is to follow the evidence, preserve the original state, and avoid interpreting an unknown hash as proof that a file is either safe or malicious.

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

