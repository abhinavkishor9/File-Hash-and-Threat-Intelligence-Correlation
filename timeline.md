# Investigation Timeline – Lab 30

## 23 November 2026

| Stage | Activity | Result |
|---|---|---|
| 1 | Created Lab 30 workspace | Successful |
| 2 | Checked required utilities | All five utilities available |
| 3 | Created `sample-a.txt` and `sample-b.txt` | Both files created successfully |
| 4 | Identified file types | Both identified as ASCII text |
| 5 | Collected initial metadata | Both files measured 58 bytes |
| 6 | Calculated SHA-256 hashes | Different hashes obtained |
| 7 | Calculated SHA-1 hashes | Different hashes obtained |
| 8 | Calculated MD5 hashes | Different hashes obtained |
| 9 | Saved initial hash evidence | Three hash evidence files created |
| 10 | Recorded original Sample A SHA-256 | Original value preserved |
| 11 | Modified Sample A | Additional test content appended |
| 12 | Recalculated Sample A SHA-256 | New hash differed from original |
| 13 | Saved modified hash | Evidence file created |
| 14 | Attempted VirusTotal hash lookup | No file report visible |
| 15 | Prepared investigation report | Report created |

## Key Hash Change

**File:** `samples/sample-a.txt`

Original SHA-256:

```text
781f4c817e37d056e4ce9955cf075667bd67856272b36bb372489c86dfe4a926
```

Modified SHA-256:

```text
024248d81991da4fb49c64bf80082af4bf8a60368b29204535089f1b01a0f68f
```

## Final Status

- Workspace and evidence directories created.
- Two controlled samples analyzed.
- MD5, SHA-1, and SHA-256 calculations completed.
- File integrity verification completed.
- VirusTotal lookup attempted.
- No threat-intelligence verdict established.

The lab demonstrated file hashing, integrity verification, and the limitations of hash-based reputation lookup.
