# Troubleshooting Notes – Lab 30

## 1. Workspace Creation

### Command

```bash
mkdir -p ~/malware-yara-labs/lab30/{samples,evidence,output,tools}
```

### Result

The workspace was created successfully.

The following directories were present:

```text
./evidence
./output
./samples
./tools
```

No directory creation errors were observed.

## 2. Utility Availability

The following utilities were checked:

```bash
for tool in sha256sum sha1sum md5sum file stat; do
    echo -n "$tool: "
    command -v "$tool" || true
done
```

All utilities were available:

| Utility | Location |
|---|---|
| sha256sum | `/usr/bin/sha256sum` |
| sha1sum | `/usr/bin/sha1sum` |
| md5sum | `/usr/bin/md5sum` |
| file | `/usr/bin/file` |
| stat | `/usr/bin/stat` |

No additional installation was necessary.

## 3. Sample Creation

Two text files were created using shell redirection:

```bash
printf 'Lab 30 - File Hash Investigation\nSample content: baseline\n' \
> samples/sample-a.txt

printf 'Lab 30 - File Hash Investigation\nSample content: modified\n' \
> samples/sample-b.txt
```

Both files were successfully created and identified as ASCII text.

Each file initially measured 58 bytes.

## 4. Hash Calculation

The three hashing utilities executed successfully:

```bash
sha256sum samples/*
sha1sum samples/*
md5sum samples/*
```

All commands returned hash values for both samples.

No hashing errors were observed.

## 5. Integrity Verification

The original SHA-256 value for `sample-a.txt` was:

```text
781f4c817e37d056e4ce9955cf075667bd67856272b36bb372489c86dfe4a926
```

After appending additional test content, the new SHA-256 value was:

```text
024248d81991da4fb49c64bf80082af4bf8a60368b29204535089f1b01a0f68f
```

The values differed as expected.

This was not a command failure. It was the intended result of the integrity-verification exercise.

## 6. VirusTotal Lookup

### Observation

The earlier hash lookup did not produce a visible file report.

The Comments section displayed no matching comments, but that alone was not sufficient to determine whether a detection report existed.

After checking the page, no file report was available.

### Interpretation

The result was documented as:

```text
Lookup Result: No file report visible
Detection Ratio: Not available
File Reputation: Unknown
```

### Important

Do not interpret an unavailable report as a clean verdict.

Do not record a detection ratio unless an actual report displays one.

## 7. Evidence Preservation

Initial hash and metadata records were created before modifying `sample-a.txt`.

They were preserved separately from the modified SHA-256 record.

This avoids confusing the original sample state with its modified state.

The following files were collected:

```text
evidence/file-metadata.txt
evidence/md5-hashes.txt
evidence/sha1-hashes.txt
evidence/sha256-hashes.txt
evidence/sample-a-modified-hash.txt
```

## 8. Lessons Learned

- Verify tool availability before beginning an analysis.
- Preserve original hash values before modifying a file.
- Keep original and modified evidence separate.
- A hash lookup does not guarantee that a threat-intelligence report exists.
- Comments and detection results are different parts of a reputation service.
- Unknown reputation must not be interpreted as proof of benignness.
- Record only results that were actually observed.
