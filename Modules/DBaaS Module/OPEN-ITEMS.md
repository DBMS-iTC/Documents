# DBaaS Module — Open Items

Design items identified during v0.4 development and deliberately deferred. Each needs its own design pass before it enters the module; none blocks the current draft.

## OI-1: Tenant lifecycle termination (deferred 2026-07-15)

Instance stop, instance delete, and tenant offboarding are control-plane operations with no requirements coverage in the module. The key-unavailable enforcement state is currently the only service-state transition the module addresses.

Uncovered questions:

- Disposition of protected data and **backups** after offboarding. Backups outlive the tenancy: who can restore them after the tenant is gone, and under whose key authorization? A post-offboarding restore by the provider is the same disclosure vector FDP_BKP_EXT.1 closes for live tenancies.
- Whether deprovisioning triggers destruction of tenant key material under the Crypto Module's FCS_CKM.6, and what happens to customer-controlled key references held by the service.
- Audit of destruction. The final records of a tenancy are generated after the tenant's data plane no longer exists — necessarily service event stream records — and the tenant who would want to verify destruction no longer has a pane to view them from. Evidence delivery (final export, attestation, or destruction certificate) needs a defined mechanism.

Candidate shape: a new extended component (FDP class) for tenant deprovisioning — bounded data/backup disposition, key-destruction linkage, and a tenant-deliverable destruction record — plus auditable-events rows (service event stream) and a P.* or OE.* hook for post-tenancy retention obligations.

Expected to draw vendor comment: offboarding assurance is a known hard problem in cloud CC. Do not fold into a routine editorial pass.
