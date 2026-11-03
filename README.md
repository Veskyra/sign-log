# Veskyra Sign public log

Every document sealed by [Veskyra Sign](https://sign.veskyra.com) adds one leaf to a Merkle tree. Once a day the
tree's root is committed here as `roots/<yyyy>/<mm>/<dd>.json`. Because this history is public and append-only, a
sealed document's place in the log can't be changed or removed afterwards without it showing.

**Nothing here identifies anyone.** Leaves and roots are hashes only, with no names, emails, IP addresses,
organizations or document contents.

## Checking a document

Upload the signed PDF at https://sign.veskyra.com/verify. The PDF is hashed in your browser and only the hash is
looked up. The page returns the leaf, its inclusion proof and the root it belongs to, then checks the proof against
the root locally. You can compare that root with the file here for the same day.

## Formats

- **Leaf data:** `SHA-256("veskyra-sign leaf v1\n" + envelope id + "\n" + final digest + "\n" + chain head + "\n" + seal time)`, hex.
- **Tree:** RFC 6962 hashing. A leaf is `SHA-256(0x00 || data)` and a node is `SHA-256(0x01 || left || right)`.
- **Root file:** `{ "v": 1, "day": "YYYY-MM-DD", "treeSize": n, "root": "<hex>", "hash": "RFC 6962, SHA-256" }`.

## Seal certificate

Veskyra Sign seals with its own certificate: ECDSA P-256, extended key usage id-kp-documentSigning. It isn't on
Adobe's trusted list, so Acrobat may show the seal's validity as "unknown". That doesn't affect the signature's legal
effect. The production certificate is [`seal-cert.pem`](seal-cert.pem).

Design: `Veskyra/veskyra-sign` `docs/crypto-design.md`, section 7.
