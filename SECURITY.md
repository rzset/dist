# Security Policy

## Security Vulnerabilities Found

### Critical Issues

#### 1. Windows Installer Zero-Length File (CRITICAL)
**File:** `appcast.xml` line 47  
**Issue:** The Windows installer has `length="0"` which disables file size validation.  
**Impact:** HIGH - Users could download corrupted or incomplete files without detection  
**Recommendation:** Set the correct file size for the Windows installer or remove the entry if the file is not ready for distribution.

```xml
<!-- VULNERABLE -->
<enclosure url="https://github.com/rzset/dist/releases/download/278/z_set-2.7.8+278-windows-setup.exe" 
           type="application/octet-stream" 
           sparkle:os="windows" 
           sparkle:dsaSignature="MDwCHAuFavkRuqmnqvSGQrujnCYFWFBwFDdLX8zrSxUCHDomhgHBrdInOu1g3gvFaUIH9snyYvybxlbvhLU=" 
           length="0"/>
```

### High Priority Issues

#### 2. DSA Signature Algorithm (Deprecated)
**File:** `appcast.xml` line 47  
**Issue:** Windows installer uses deprecated DSA signature (`sparkle:dsaSignature`) instead of Ed25519 (`sparkle:edSignature`)  
**Impact:** MEDIUM - DSA is cryptographically weaker than Ed25519  
**Recommendation:** Migrate to Ed25519 signatures for all platforms. All macOS releases already use Ed25519.

**Why Ed25519 is better:**
- Stronger cryptographic security
- Resistant to timing attacks
- Smaller signature size
- Recommended by Sparkle 2.x

## Recommended Actions

### Immediate Actions Required

1. **Fix or Remove Windows Installer Entry**
   - If the file is ready: Update `length="0"` to the actual file size in bytes
   - If the file is not ready: Remove the entire Windows installer `<item>` entry from appcast.xml
   - Verify the file exists and is not corrupted

2. **Migrate to Ed25519 Signatures**
   - Generate Ed25519 key pair for code signing
   - Re-sign Windows installer with Ed25519
   - Update appcast.xml to use `sparkle:edSignature` instead of `sparkle:dsaSignature`

### Security Best Practices

1. **Always Specify File Length**
   - Never use `length="0"` in production
   - Validate file size matches before publishing

2. **Use Strong Signatures**
   - Use Ed25519 (edSignature) for all releases
   - Avoid DSA signatures (deprecated and weaker)

3. **HTTPS for Downloads**
   - All download URLs use HTTPS ✓ (already implemented)

4. **Verify Before Publishing**
   - Always verify files are complete before adding to appcast
   - Test update process in staging environment
   - Validate signatures before publishing

## Reporting Security Issues

If you discover a security vulnerability, please email security@[your-domain].com instead of using the issue tracker.

## Version History

- **2024-09-30**: Initial security audit completed
  - Identified zero-length Windows installer vulnerability
  - Identified deprecated DSA signature usage
