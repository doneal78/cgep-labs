# CGE-P Labs

Lab work and evidence for the GRC Engineering Club CGE-P (Practitioner) course. Infrastructure code lives under terraform/, scripts under scripts/, and captured evidence under evidence/.

Each completed lab was deployed to a real AWS account with Terraform, checked from the outside with the AWS CLI, captured as machine-readable evidence instead of screenshots, and then torn down. This is lab work, not production experience.

## Labs

| Lab | What it covers | Code | Evidence | Status |
|---|---|---|---|---|
| 2.3 Building Your First Compliant Resource | Terraform S3 primitive enforcing NIST 800-53 SC-28, AC-3, AU-3, AU-6, CM-6 | terraform/primitives/compliant-s3 | evidence/lab-2-3 | Complete |
| 2.4 Terraform Modules for Compliance (GCP) | GCS bucket module with customer managed keys | None | None | Skipped. GCP billing was not available, and the course allowed moving on |
| 2.5 IaC as Compliance Evidence | S3 Object Lock evidence vault, Bash capture script with SHA-256 manifest, Cosign signing | terraform/primitives/evidence-vault, scripts/capture-evidence.sh | evidence/lab-2-5 | Complete |

## Tools used

Terraform, AWS S3, AWS CLI, Bash, sha256sum, Cosign with Sigstore keyless signing, and Git.
