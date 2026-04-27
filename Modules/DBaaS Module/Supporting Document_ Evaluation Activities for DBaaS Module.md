= Supporting Document: Evaluation Activities for DBaaS Module
:showtitle:
:toc: macro
:toclevels: 7
:sectnums:
:sectnumlevels: 7
:revnumber: 0.1
:revdate: 2026-01-25
:iTC-shortname: DBMS-iTC
:iTC-longname: Database Management Systems
:pp-module-name: collaborative PP-Module for Database-as-a-Service
:pp-module-version: 0.1

toc::[]

= Supporting Document: DBaaS Module
:showtitle:
:toc: macro
:toclevels: 7
:sectnums:
:sectnumlevels: 7
:revnumber: 0.1
:revdate: 2026-01-25

:iTC-longname: Database Management Systems
:iTC-shortname: DBMS-iTC
:base-pp: collaborative Protection Profile for Database Management Systems
:pp-module: collaborative PP-Module for Database-as-a-Service

toc::[]

== Introduction

=== Technology Area and Scope of Supporting Document

This Supporting Document (SD) defines Evaluation Activities (EAs) for the *{pp-module}* (DBMS_MOD_DBAAS).

=== The "Black Box" Evaluation Constraint
Unlike the "Lift-and-Shift" Cloud Module, the DBaaS TOE is a managed service. The evaluator typically does **not** have access to:
*   The underlying Operating System (OS).
*   The hypervisor or physical hardware.
*   Service Provider administrative accounts.

Therefore, Evaluation Activities in this SD are designed to be performed via:
1.  **Tenant Interfaces:** Standard SQL clients, Web Consoles, and APIs accessible to the customer.
2.  **Documentation Review:** Analysis of the Service Provider's architectural descriptions to verify assumptions.

=== Relationship to Other Documents

This SD is used in conjunction with:
*   **cPP_DBMS SD**: Base requirements.
*   **DBMS Crypto Module SD**: Cryptographic testing (Mandatory).
*   **DBaaS PP-Module**: The requirements definition.

== General Guidance for Evaluators

=== Verifying the Trusted Platform Assumption
The DBaaS Module relies on `A.PLATFORM_ISOLATION` (e.g., Exadata, Hardened Hypervisors).
The evaluator cannot technical test this isolation (no physical access). Instead, the evaluator shall:
. Review the ST's description of the underlying platform.
. Verify the Service Provider holds a valid certification for the infrastructure (e.g., FedRAMP High, ISO 27001, or a specific Hypervisor PP certification) *OR* rely on the scheme's recognition of the platform.
. Document this reliance in the ETR.

=== Tenant Isolation Testing Strategy
Testing for logical isolation requires the evaluator to provision **at least two distinct tenant instances** (or schemas within a container database, depending on the claim) and attempt cross-tenant access.

== FCS Class: Cryptographic Support

=== FCS_CKM_DBAAS_EXT.1 Customer Controlled Keys

==== Evaluation Activities

===== TSS Activities
The evaluator shall verify the TSS describes:
*   The Key Management architecture.
*   If **External/Managed** keys are selected: The protocol used (e.g., OCI Vault, AWS KMS) and the authentication mechanism.
*   Confirmation that the key material is never stored in the database's local storage in plaintext.

===== Test Activities

*Test 1: Provider Inaccessibility Verification*
. Configure the TOE to use a Customer Managed Key (CMK).
. Attempt to access the encrypted data as a valid Tenant Admin.
. **Expected Result:** Access succeeds (TOE can use key via reference).
. Attempt to export or view the Master Key material via any database interface (SQL, Console).
. **Expected Result:** Operation denied. The TOE may reference the key but cannot reveal the key material.

*Test 2: Key Revocation Behavior*
. Revoke the TOE's permission to access the External Key Vault (simulate a customer revoking CSP access).
. Attempt to access data in the TOE.
. **Expected Result:** Access is denied (Decrypt failure). This confirms the TOE relies on the external key and does not cache it indefinitely in plaintext.

---

== FDP Class: User Data Protection

=== FDP_RIP.1 Residual Information Protection (Memory)

==== Evaluation Activities

===== Test Activities
*Note:* Direct memory inspection is often impossible in DBaaS. Use logical deduction tests.

*Test 1: Temporal Table/Memory Cache Test*
. As Tenant A, create a large table and populate it with a unique, distinct pattern (e.g., string "TENANT_A_SECRET").
. Run queries to force the data into memory/buffer cache.
. Drop the table and purge it completely.
. As Tenant B (or a new user on a shared infrastructure if supported), attempt to allocate large amounts of memory and scan for the pattern.
. **Note:** This is a best-effort test. If the evaluator cannot inspect memory directly, they rely on the TOE's architecture description in the TSS verifying standard clearing protocols.

=== FDP_ACC.2(1) Tenant Isolation Policy

==== Evaluation Activities

===== Test Activities

*Test 1: Data Cross-Access Prevention*
. Provision Tenant A and Tenant B on the same infrastructure (or logical cluster).
. Create sensitive data in Tenant A.
. As Tenant B, attempt to access Tenant A's objects via standard SQL (requires guessing names/IDs).
. Attempt to access Tenant A's objects via system views/metadata tables.
. **Expected Result:** All attempts fail.

*Test 2: Metadata Isolation*
. As Tenant A, create an object with a distinct name.
. As Tenant B, query the system catalog/views (e.g., `ALL_TABLES`, `INFORMATION_SCHEMA`).
. **Expected Result:** Tenant B sees only their own objects and generic system objects. They cannot see Tenant A's object metadata.

=== FDP_RSG_EXT.1 Resource Governance

==== Evaluation Activities

===== TSS Activities
The evaluator shall verify the TSS describes the mechanism for Resource Governance (e.g., Resource Manager, Quotas).

===== Test Activities

*Test 1: CPU/Query Throttling*
. Configure a resource quota for the Tenant (e.g., max 50% CPU or max concurrent queries).
. Initiate a workload that exceeds this limit.
. **Expected Result:** Queries are queued, delayed, or terminated based on the configured policy. The TOE does not crash or impact other tenants (observability of other tenants may be simulated via timing analysis or distinct log timestamps).

---

== FIA Class: Identification and Authentication

=== FIA_USB.2 User-Subject Binding

==== Evaluation Activities

===== Test Activities

*Test 1: Tenant ID Binding Verification*
. Authenticate as a Tenant User.
. Execute a query that returns the current session context (e.g., `SELECT CURRENT_TENANT_ID` or equivalent).
. Verify the returned ID matches the authenticated tenant.

*Test 2: Audit Correlation*
. Perform an action as Tenant A.
. Query the audit trail (if accessible).
. Verify the audit record is stamped with the correct Tenant ID.
. Authenticate as Tenant B.
. Attempt to access the audit trail.
. **Expected Result:** Tenant B sees only Tenant B's records.

---

== FMT Class: Security Management

=== FMT_MOF.1 Management of Security Functions Behavior

==== Evaluation Activities

===== Test Activities

*Test 1: Immutability Verification (Audit)*
. Authenticate as a Tenant Administrator.
. Attempt to disable the audit function.
. **Expected Result:** Operation denied. The Audit function is not disable-able by the tenant.

*Test 2: Immutability Verification (Encryption)*
. Authenticate as a Tenant Administrator.
. Attempt to disable Data-at-Rest Encryption.
. **Expected Result:** Operation denied.

*Test 3: Role Restriction Verification*
. Attempt to grant "Service Provider" or "Root" administrative privileges to a standard user.
. **Expected Result:** Operation denied.

---

== FAU Class: Security Audit

=== FAU_SEL.1 Selective Audit (Tenant Separation)

==== Evaluation Activities

===== Test Activities

*Test 1: Audit Scope Leakage*
. Generate audit events in Tenant A and Tenant B.
. As Tenant A Admin, query the audit logs.
. **Expected Result:** Only Tenant A records are returned.
. Analyze the query result to ensure no Tenant B data (username, table names, query text) is present.

=== FAU_STG_EXT.2 Immutable Audit Stream

==== Evaluation Activities

===== Test Activities

*Test 1: Deletion Prevention*
. As Tenant Admin, identify an audit record.
. Attempt to delete or modify that record via SQL DELETE or UPDATE commands on the audit table.
. **Expected Result:** Operation denied.

*Test 2: Audit Export Verification*
. Configure audit export to an external sink (e.g., OCI Object Storage).
. Generate audit events.
. Verify the external sink receives the events.
. Attempt to stop the audit export pipeline as Tenant Admin.
. **Expected Result:** Operation denied.

---

== FPT Class: Protection of the TSF

=== FPT_TUD_EXT.2 Autonomous Trusted Update

==== Evaluation Activities

===== TSS Activities
The evaluator shall verify the TSS describes the autonomous update mechanism:
*   How the update source is authenticated.
*   Whether updates are applied automatically or require a schedule.

===== Test Activities

*Note:* Testing live patching in a production DBaaS environment is high risk. Testing should be conducted on a non-production instance if possible, or verified via documentation/audit logs.

*Test 1: Update Provenance*
. Review the audit logs or system event logs for a recent patch application.
. Verify the log indicates the source of the update.
. Verify the signature verification step is logged (if claimed in TSS).

*Test 2: Tenant Non-Interference*
. Verify that the Tenant Admin cannot block or delay security patches via configuration settings.
. **Expected Result:** Security patches are applied per Service Provider policy, not Tenant policy.

[appendix]
== Document References

[bibliography]
* [#PP_MOD]#[PP_MOD]# collaborative PP-Module for Database-as-a-Service.
* [#Crypto_Catalog]#[Crypto_Catalog]# Common Criteria Cryptographic Catalogue.