# Policy library

Rego policies evaluated against Terraform plan JSON with Conftest. Each file targets one NIST SP 800-53 control for one cloud. Control IDs are shared across clouds. The rules are not, because each cloud names and structures its resources differently.

## AWS policies (Lab 3.4)

| File | Control | Package | What it checks |
| --- | --- | --- | --- |
| sc28_encryption_aws.rego | SC-28 | compliance.sc28_aws | Every aws_s3_bucket has an aws_s3_bucket_server_side_encryption_configuration referencing it |
| ac3_no_public_aws.rego | AC-3 | compliance.ac3_aws | Every aws_s3_bucket has an aws_s3_bucket_public_access_block referencing it, with all four flags true |
| cm6_required_tags_aws.rego | CM-6 | compliance.cm6_aws | Taggable resources carry Project, Environment, ManagedBy, and ComplianceScope |

## GCP policies

Not present. The GCP labs were skipped because a Google Cloud billing account was not available, and the course instructor approved moving on. If GCP variants are added later, they belong in this same directory with their own packages, keeping the same control IDs.

## Notes on writing these rules

At plan time, an S3 bucket name is not yet known, so the rules match by reference rather than by value. SC-28 and AC-3 read `configuration.root_module.resources[].expressions.bucket.references` to find which resource points at which bucket. AC-3 then reads `planned_values` for the four flag values, which are literal booleans. CM-6 reads merged tags from `tags_all` when provider default_tags are in use, and falls back to `tags`, then to no tags.

## Running them

`scripts/policy-gate.sh --workspace <path>` runs every AWS policy against a workspace that already has a saved tfplan, writes results to `evidence/lab-3-4/conftest-results.json`, and exits non-zero if any policy fails.
