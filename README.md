# pi-doctor
`pi-doctor` is a read-only CLI for checking common Raspberry Pi problems:
identity mismatches, thermal throttling, `config.txt` drift, GPIO conflicts,
camera detection failures, and Python environment pitfalls.

It is designed for both interactive troubleshooting and automation. Human
output explains what changed and what to run next; JSON output gives scripts
stable schemas. The current stable package release is `1.0.0`.

## Usage

```bash
pi-doctor check
pi-doctor --json check
pi-doctor --timeout 5 check
pi-doctor explain throttling
pi-doctor explain config
pi-doctor explain python
pi-doctor doctor camera
pi-doctor doctor gpio
pi-doctor support-bundle --dry-run
pi-doctor support-bundle --output ./bundles
```

Support bundles are sanitized by default and include `manifest.txt` with
SHA-256 hashes for payload files. Sensitive bundles require an explicit
acknowledgement:

```bash
pi-doctor support-bundle --include-sensitive --acknowledge-sensitive-data
```

`pi-doctor check` exits with:

| Code | Meaning |
| ---: | --- |
| 0 | healthy |
| 1 | warning |
| 2 | degraded |
| 3 | critical |
| 4 | unexpected runtime failure |

Automation should prefer:

```bash
pi-doctor --json check
```

JSON includes `probe_health` so scripts can tell a healthy subsystem from an
incomplete inspection.

Operational logs are written to stderr. For CI or field debugging, use
privacy-preserving JSON-lines logs:

```bash
PI_DOCTOR_LOG=debug PI_DOCTOR_LOG_FORMAT=json pi-doctor --json check > report.json 2> pi-doctor.log.jsonl
```

The JSON contract is documented in [`docs/cli-contract.md`](docs/cli-contract.md),
[`docs/json-schema.md`](docs/json-schema.md), and the schema files in
[`schema/`](schema/).

## Project Operations

- Security policy: [`SECURITY.md`](SECURITY.md)
- Contributing guide: [`CONTRIBUTING.md`](CONTRIBUTING.md)
- Maintainer policy: [`MAINTAINERS.md`](MAINTAINERS.md)
- Release process: [`docs/release-process.md`](docs/release-process.md)
- Known limitations: [`docs/known-limitations.md`](docs/known-limitations.md)

## License

`pi-doctor` is licensed under the [MIT](LICENSE).
