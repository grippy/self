# DID:web Implementation

This repository implements a [DID:web](https://w3c-ccg.github.io/did-method-web/) (Decentralized Identifier) document that can be resolved from GitHub's raw content URL.

## DID Identifier

The DID identifier for this repository is:

```
did:web:raw.githubusercontent.com:grippy:self
```

## How DID:web Resolution Works

The `did:web` method converts a DID into an HTTPS URL to fetch the DID document. The conversion follows these rules:

1. Start with the DID: `did:web:raw.githubusercontent.com:grippy:self`
2. Remove the `did:web:` prefix: `raw.githubusercontent.com:grippy:self`
3. Replace colons (`:`) with slashes (`/`): `raw.githubusercontent.com/grippy/self`
4. Append `/.well-known/did.json` to the path
5. Prepend `https://` to create the final URL

**Result**: `https://raw.githubusercontent.com/grippy/self/main/.well-known/did.json`

## Constructing the DID:web from GitHub Raw URL

If you have a GitHub raw URL like:
```
https://raw.githubusercontent.com/grippy/self/main/.well-known/did.json
```

To construct the DID:web identifier:

1. Remove `https://`: `raw.githubusercontent.com/grippy/self/main/.well-known/did.json`
2. Remove `/main/.well-known/did.json`: `raw.githubusercontent.com/grippy/self`
3. Replace slashes (`/`) with colons (`:`): `raw.githubusercontent.com:grippy:self`
4. Prepend `did:web:`: `did:web:raw.githubusercontent.com:grippy:self`

## Accessing the DID Document

The DID document can be accessed directly at:

- **Main branch**: https://raw.githubusercontent.com/grippy/self/main/.well-known/did.json
- **Any branch**: https://raw.githubusercontent.com/grippy/self/{branch}/.well-known/did.json

## DID Document Structure

The DID document includes:

- **@context**: Standard W3C DID context and security suites
- **id**: The DID identifier
- **verificationMethod**: Public keys for verification (placeholder - replace with actual keys)
- **authentication**: Keys that can be used to authenticate as this DID
- **assertionMethod**: Keys that can be used to make assertions
- **service**: Endpoints associated with this DID (e.g., GitHub repository link)

## Customizing the DID Document

To use this DID document in production:

1. **Replace the public key**: Update the `publicKeyJwk` in `verificationMethod` with your actual public key
2. **Generate a key pair**: Use a tool like `jose` or `openssl` to generate Ed25519 keys
3. **Update services**: Add any additional service endpoints you need

### Example: Generating Ed25519 Keys

Using Node.js and the `jose` library:

```javascript
import { generateKeyPair, exportJWK } from 'jose';

(async () => {
  const { publicKey, privateKey } = await generateKeyPair('EdDSA', {
    crv: 'Ed25519'
  });
  
  const publicJwk = await exportJWK(publicKey);
  console.log('Public Key (for did.json):', JSON.stringify(publicJwk, null, 2));
  
  const privateJwk = await exportJWK(privateKey);
  console.log('Private Key (keep secure!):', JSON.stringify(privateJwk, null, 2));
})();
```

## Verification

To verify your DID document is properly formatted and accessible:

1. **Check JSON validity**: Ensure the JSON is well-formed
2. **Verify URL accessibility**: Test the raw GitHub URL in a browser
3. **Use a DID resolver**: Try resolving your DID using a universal resolver like https://dev.uniresolver.io/

## References

- [DID Core Specification](https://www.w3.org/TR/did-core/)
- [DID:web Method Specification](https://w3c-ccg.github.io/did-method-web/)
- [Universal Resolver](https://dev.uniresolver.io/)
