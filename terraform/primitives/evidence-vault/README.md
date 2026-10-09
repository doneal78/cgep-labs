# Evidence Vault (Lab 2.5)

An AWS S3 bucket built with Terraform to hold compliance evidence that cannot be quietly deleted. This is lab work from the GRC Engineering Club CGE-P course.

## What it enforces

| Setting | Purpose |
|---|---|
| Object Lock enabled at bucket creation | Required for locking; it cannot be added to an existing bucket |
| Versioning on | Object Lock requires it, and each upload gets its own version |
| Default retention, GOVERNANCE mode, 1 day | Every upload is locked automatically |
| AES-256 encryption | SC-28, protection at rest |
| All four public access block settings | AC-3, no public access |
| Bucket policy denying s3:DeleteBucket except to the account root | Extra protection for the bucket itself |

## How it was used

scripts/capture-evidence.sh collected the state, latest git commit, and Terraform version from the Lab 2.3 workspace, wrote a SHA-256 manifest, bundled everything, uploaded it here, and printed a receipt with the S3 version ID. The receipt is in evidence/lab-2-5/receipt.json.

Verified during the lab: the retention was applied automatically at upload, a delete attempt without the bypass flag returned AccessDenied because the object was protected by Object Lock, and the bundle was signed with Cosign keyless signing.

## Lab settings vs production

GOVERNANCE mode lets a privileged user bypass the lock with --bypass-governance-retention, which is how the lab was cleaned up. Real evidence would use COMPLIANCE mode, where nobody can delete a locked object until retention expires, including the account root.
