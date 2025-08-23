# MyData DID-based Consent Template v0.1

Scope: User-controlled consent for health-data access using DID + Verifiable Credentials.

Actors:
- Data Subject (patient) with DID
- Data Controller (practitioner/system) with DID
- Verifier (service requesting access)

Artifacts:
- Consent VC (JSON-LD)
- Presentation (OID4VP) with selective disclosure
- Audit hash to public chain (transaction hash only; no PHI)

Principles:
- Human-centric
- Revocable
- Purpose-bound
- Time-bound
- Minimal disclosure

Revocation:
- StatusList2021 entry + on-chain "revoked" marker (hash only).
