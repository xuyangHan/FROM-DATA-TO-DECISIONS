# The SFTP Feed Broke After Key Rotation

## The Situation

A partner delivers a file over SFTP every night. After the ingestion team rotates its client key, authentication begins failing. The network path and port `22` test are healthy, so someone proposes restoring the old private key and trying again.

That may restore service, but it does not explain whether the server rejected the client's identity, the client rejected the server's identity, or the two systems could not agree on a cryptographic algorithm.

## Why It Is Not Simple

SFTP is the **SSH File Transfer Protocol**. It is a file-transfer subsystem carried over SSH, usually on TCP `22`. It is not FTP, and it is not FTPS, which is FTP protected with TLS.

An SSH connection establishes encrypted transport and authenticates parties in separate steps:

```mermaid
sequenceDiagram
  participant C as SFTP client
  participant S as SFTP server
  C->>S: Propose SSH and key algorithms
  C<->>S: Key exchange and derive session keys
  S->>C: Present host key
  C->>C: Verify known host identity
  C->>S: Prove possession of client private key
  S->>S: Match authorized public key
```

## Build the Mental Model

**Asymmetric encryption** uses a public/private key pair and supports identity proof and secure key establishment. The private key remains secret; the public key can be shared. **Symmetric encryption** uses a shared session key and is fast enough for bulk data transfer. SSH uses both ideas: asymmetric operations establish trust and session secrets, then symmetric keys protect the session.

A **host key** identifies the SFTP server and helps prevent man-in-the-middle attacks. A **client key** authenticates the user or application to the server. Replacing one does not replace the other.

Common SSH key algorithms include RSA, ECDSA, and ED25519. DSA is deprecated. ED25519 is modern and compact, but older servers or restrictive crypto policies may not support it. In `ssh-keygen -t rsa -b 2048`, `-b 2048` selects a 2,048-bit RSA key size.

## Investigate the Problem

After a rotation failure:

1. Confirm the pipeline uses the intended new private key.
2. Confirm the matching public key was installed for the correct server user.
3. Check `authorized_keys` ownership, permissions, line breaks, and key format.
4. Check whether client and server support the chosen algorithm.
5. Determine whether the error concerns the server host key or client authentication.
6. Compare verbose SSH logs from before and after rotation.
7. Confirm no secret-store version or cached configuration still points to the old key.

## Choose a Solution

Rotate without downtime by creating a new pair, adding the new public key alongside the old one, testing from the real runtime, switching the client, monitoring successful transfers, and only then removing the old public key.

Use the strongest algorithm supported by both organizations' approved policies. Compatibility is a constraint to plan, not a reason to keep weak algorithms indefinitely.

Related reading: [Encryption Worked, but Trust Failed](06-tls-trust-failed.md) explains certificate-based trust, while [Why Should Nobody Know the Production Password?](07-passwordless-production-access.md) covers secure private-key storage.

## Production Checklist

- Keep private keys in a secret store and restrict access.
- Record owner, purpose, algorithm, fingerprint, and expiry or rotation date.
- Verify server host keys through a controlled process.
- Support an overlap period during rotation.
- Test from the actual ingestion runtime.
- Remove old keys after evidence confirms the new path works.

## Interview Takeaways

- SFTP runs over SSH and inherits its encrypted transport.
- FTPS is FTP over TLS and has different behavior.
- Host keys authenticate servers; client keys authenticate clients.
- Public keys are shared; private keys must remain secret.
- SSH negotiates algorithms, performs key exchange, derives symmetric session keys, verifies the server, and authenticates the client.
- ED25519 may fail against older systems; DSA should not be used.

## Official References

- [RFC 4251: The Secure Shell Protocol Architecture](https://www.rfc-editor.org/rfc/rfc4251)
- [RFC 4253: SSH Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253)
- [OpenSSH key management](https://www.openssh.com/manual.html)
