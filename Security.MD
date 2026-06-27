## Security for Desktop Applications

Desktop applications face unique security challenges: they run with user privileges, can persist malware, and distribution requires trust signals to prevent SmartScreen warnings and malware detection.

### Code Signing

**Purpose:** Digitally sign executables and installers to prove authenticity and build user trust

**Why it matters:**
- Windows SmartScreen warnings without signatures (scares users)
- Malware detection: unsigned binaries flagged as suspicious
- Extension integrity: verify extensions haven't been tampered with
- User reputation/trust: signed = "legitimate company"

### For Open Source Projects

**Challenge:** Limited budget, but need to establish trust

#### Option 1: GitHub Actions + Free Certificate (Simple)
**Cost:** $0 (but limited trust)

**Process:**
- Generate self-signed certificate (free, Windows certutil or OpenSSL)
- Sign binaries in GitHub Actions workflow
- Self-signed = users see warning, but at least consistent signing
- Better than nothing, but not ideal for distribution

**Setup:**
```yaml
# In GitHub Actions
- name: Sign executable
  run: signtool sign /f certificate.pfx /p ${{ secrets.CERT_PASSWORD }} /t http://timestamp.digicert.com MyApp.exe
```

**Limitation:** Users still see "Unknown publisher" warning on first run

#### Option 2: Extended Validation (EV) Certificate (Best for open source)
**Cost:** $200-400/year (Sectigo, DigiCert)
**Setup time:** 1-2 weeks (company verification)

**Why:** 
- Establishes organizational identity
- Windows SmartScreen recognizes it immediately
- No "Unknown publisher" warning
- Builds reputation over time

**Process:**
- Purchase from Sectigo, DigiCert, or similar
- Verify company/project identity
- Download .pfx file
- Store in GitHub Secrets
- Sign in CI/CD pipeline (see code above)

**For small open source projects:**
- Some CAs offer discounts (check Sectigo for open source)
- Consider per-version signing (not every build)
- Combine with strong versioning/release process

#### Option 3: Windows Store Distribution
**Cost:** $19 one-time (Microsoft developer account)
**Trust:** Highest (Windows Store verification)

**Process:**
- Package as MSIX
- Submit to Microsoft Store
- Microsoft scans for malware
- Signed by Microsoft
- No SmartScreen warnings

**Trade-offs:**
- Requires MSIX packaging (more complex)
- Microsoft review process (slower releases)
- Restrictions on what you can do
- Best for mature, stable projects

### For Small Companies

**Recommended approach:** Extended Validation (EV) Certificate

**Why:**
- Professional appearance
- Cost-justified for commercial product
- Long-term reputation building
- Automated in CI/CD pipeline
- Applies to all future builds

**Certificate providers:**
- DigiCert (premium, reliable)
- Sectigo (cost-effective)
- Comodo (budget option)

**Typical cost:** $100-300/year

**Setup in CI/CD:**
1. Purchase certificate, get .pfx file
2. Encode to base64: `base64 -i cert.pfx`
3. Store in GitHub Secrets
4. Decode and sign in workflow
5. Use timestamp server (ensures signature doesn't expire)

### Timestamp Servers

**Critical:** Always use timestamp servers when signing

**Why:** Without timestamp, signature expires when certificate expires
**With timestamp:** Signature remains valid forever (verified at signing time)

**Public timestamp servers:**
- http://timestamp.digicert.com
- http://timestamp.sectigo.com
- http://time.certum.pl

### NuGet Package Signing

If distributing via NuGet:
```
nuget sign MyPackage.nupkg -CertificatePath cert.pfx -CertificatePassword $PASSWORD -Timestamper http://timestamp.digicert.com
```

**Why sign NuGet packages:**
- Verifies package hasn't been tampered with
- Proves authenticity (you published this, not a fake)
- Protects consumers from supply chain attacks
- nuget.org and private feeds support signature verification

**Process:**
1. Use same certificate as executable signing (cost-effective)
2. Sign in CI/CD pipeline when publishing package
3. Use timestamp server (same as executable signing)
4. Upload signed .nupkg to nuget.org or private feed

**Publishing signed packages to nuget.org:**
```bash
nuget push MyPackage.nupkg -ApiKey YOUR_NUGET_API_KEY -Source https://api.nuget.org/v3/index.json
```

**Verification:**
- Consumers can verify: `nuget verify -Signature MyPackage.nupkg`
- Most package managers auto-verify signatures
- Rejected if signature invalid (requires -AllowUnsigned flag)

**For your extension architecture:**
- Sign all extension NuGet packages
- Store certificate securely in GitHub Secrets
- Automate signing in CI/CD release workflow
- Consumers trust your extensions (verified at download)

**Integration with code signing:**
You can reuse the same EV certificate for:
- Executable/MSI signing
- NuGet package signing
- Update package signing

This creates consistent, verifiable chain of trust across all deliverables.

### Extension Security

When using [extension architecture](FromMicroServicesToExtensions.MD):

**Verify extension authenticity:**
- Check extension binary signatures before loading
- Maintain trusted publisher whitelist
- Reject unsigned or invalid extensions at runtime

**For NuGet extensions:**
- Use NuGet package signing
- Verify signatures on download
- Store public key for verification

### Update Security

Integrating with [channel-based updates](FromContinuousDeploymentToChannelBasedUpdate.MD):

**Squirrel.Windows:**
- Sign update packages
- Verify signatures on download
- Use HTTPS for update endpoint
- Implement delta update verification

**Requirements:**
- HTTPS-only update URLs (no HTTP)
- Signed packages
- Version monotonicity (prevent downgrades)

### Certificate Storage Best Practices

**Never commit certificates to Git:**
```bash
# .gitignore
*.pfx
*.p12
certificate.pem
```

**Store in:**
- GitHub Secrets (for CI/CD)
- Azure Key Vault (for production)
- Local machine cert store (for local development)

**Rotate regularly:**
- Annual certificate renewal
- Plan key rotation if compromised
- Keep backup of old certificates for verification

### Summary for Your Setup

**Open source on GitHub:**
1. Start with self-signed + GitHub Actions
2. When ready, upgrade to EV certificate ($200-400/year)
3. Sign in CI/CD pipeline automatically
4. Use timestamp servers
5. Distribute via GitHub Releases + winget

**Small company product:**
1. Purchase EV certificate immediately (worth it)
2. Automate signing in CI/CD
3. Store secrets securely
4. Consider Windows Store for additional trust
