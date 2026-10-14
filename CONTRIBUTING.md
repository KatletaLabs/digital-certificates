# Adding a credential

## Naming and placement

| File | Convention |
| :--- | :--- |
| Certificate | `certificates/<issuer>/YYYY-MM-DD-official-title.pdf` |
| Badge image | `badges/<issuer>/credential-slug.png` |
| Study notes | `notes/<issuer>/credential-slug.md` |

Use lowercase ASCII names with hyphens. Keep official PDF contents unchanged. Use the actual award date, not the upload date. If the date is unknown, use `undated-title.pdf` and mark it as unconfirmed in the table.

## Workflow

1. Read the certificate and open the personal issuer/Credly link. Match recipient, exact title, issuer and badge ID.
2. Record type (certification, course completion or badge), issue date, expiry date and current status. Do not infer an expiry or label every vendor course a certification.
3. Save the original PDF under its issuer folder. Save a genuine badge image, if available, under `badges/<issuer>/`. Link the image to the personal credential page.
4. Add one README row in the appropriate section. Keep certification records above course completions; order courses newest first. Link related records without overstating the number of independent qualifications.
5. Add a concise description and supported topic tags. Use `Not confirmed` when data cannot be established. Update `notes/credential-evidence.md` with sources and verification limits.
6. Update `SHA256SUMS`. On Linux or Git Bash, from the repository root, run:

```bash
find certificates badges assets/badges -type f \( -name '*.pdf' -o -name '*.png' -o -name '*.svg' \) -print0 | sort -z | xargs -0 sha256sum > SHA256SUMS
sha256sum -c SHA256SUMS
```

7. Preview README. Open each PDF, badge and verification link. Check that old links still work and that no credentials, private keys or account recovery data are included.
8. Commit the PDF, optional image, README, evidence notes and checksum changes together.

## Git workflow

```bash
git switch main
git pull --ff-only
git switch -c add-credential
# Copy files and edit the index following the steps above.
git diff --check
git status --short
git add certificates badges notes README.md SHA256SUMS
git diff --cached --stat
git commit -m "Add issuer credential"
git push -u origin add-credential
```

Open a pull request, inspect the rendered README and files, then merge. Use a unique branch name for each addition.

For a browser-only upload, use GitHub **Add file → Upload files** in the target folder, choose a new branch, then edit README and evidence notes on that branch. Calculate the new file's SHA-256 locally (Windows PowerShell: `Get-FileHash -Algorithm SHA256 -LiteralPath 'certificate.pdf'`) and append the hash and repository-relative path to `SHA256SUMS` with two spaces between them. Open and review a pull request before merging.

## Metadata checklist

- Official title and issuer
- Recipient and public credential URL
- Credential type and level
- Issue date and expiry, each with a source
- Status and date it was checked
- Document path, optional badge image and topic tags
- Short factual description; no unsupported expertise claims

Suggested repository topics: `cybersecurity`, `certifications`, `fortinet`, `ibm-skillsbuild`, `security-awareness`, `learning-portfolio`.
Suggested About description: **Cybersecurity credentials and learning records — Fortinet, IBM SkillsBuild and Kaspersky.**
