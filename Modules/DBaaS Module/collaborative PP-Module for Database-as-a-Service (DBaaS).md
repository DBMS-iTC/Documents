= collaborative PP-Module for Database-as-a-Service (DBaaS)
:showtitle:
:toc: macro
:toclevels: 7
:sectnums:
:sectnumlevels: 7
:imagesdir: images
:icons: font
:doctype: book
:revnumber: 0.1
:revdate: 2026-01-25

:iTC-longname: Database Management Systems
:iTC-shortname: DBMS-iTC
:iTC-email: cm-itc-mailing-list@gmail.com
:iTC-website: https://github.com/DBMS-iTC
:iTC-GitHub: https://github.com/DBMS-iTC/DBMS-iTC.github.io
:base-pp: collaborative Protection Profile for Database Management Systems

:sectnums!:

== Acknowledgements
This collaborative Protection Profile Module (PP-Module) was developed by the {iTC-longname} international Technical Community (iTC) also known as {iTC-shortname}.

*INDUSTRY*

_Vendors_

*COMMON CRITERIA TESTING LABORATORIES*

_ITSEF Name_

*GOVERNMENT AGENCIES*

_Government Agency Names_

=== Revision History

.Revision history
[%header,cols=".^1,.^2,.^4"]
|===

|Version
|Date
|Description

|0.1
|2026-01-25
|Initial Draft for DBaaS deployments (Oracle Autonomous Database, etc.)

|===

toc::[]

== Preface

This PP-Module, the *Database-as-a-Service (DBaaS) Module*, extends the collaborative Protection Profile for Database Management Systems (cPP_DBMS). It applies to database services where the cloud provider manages the infrastructure, availability, and maintenance of the database (e.g., Oracle Autonomous Database, AWS Aurora, Azure SQL Database).

This module addresses the unique security challenges of the DBaaS model where:
*   The **Service Provider** controls the infrastructure, patching, and availability.
*   The **Customer (Tenant)** is strictly a database user with no administrative access to the underlying OS or hardware.

This module is designed for "Trust No One" architectures, assuming the service provider's administrators are part of the threat environment, necessitating strict cryptographic isolation and logical tenant separation.

=== Objectives of Document
This document expresses the security functional requirements for DBaaS deployments, focusing on logical isolation, resource governance, and customer-managed cryptographic controls.

=== Related Documents

[bibliography]
* [#CC1]#[CC1]# Common Criteria for Information Technology Security Evaluation, Part 1: Introduction and General Model, CC:2022.
* [#Crypto_Catalog]#[Crypto_Catalog]# Common Criteria Cryptographic Catalogue.
* [#cPP_DBMS]#[cPP_DBMS]# collaborative Protection Profile for Database Management Systems.
* [#DBMS_MOD_CRYPTO]#[DBMS_MOD_CRYPTO]# PP-Module for DBMS Cryptographic Functions.

:sectnums:
== PP-Module Introduction

=== PP-Module Reference Identification

.PP-Module Identification
[cols="1,3"]
|===
|Attribute |Value

|PP-Module Title
|collaborative PP-Module for Database-as-a-Service (DBaaS)

|PP-Module Short Name
|DBMS_MOD_DBAAS

|PP-Module Version
|{revnumber}

|PP-Module Publication Date
|{revdate}

|PP-Module Sponsor
|{iTC-longname} international Technical Community ({iTC-shortname})

|CC Version
|CC:2022

|PP-Module Keywords
|Database, DBaaS, Cloud, Autonomous, Multi-Tenant, Isolation
|===

== PP-Module Base

This PP-Module requires the **collaborative Protection Profile for Database Management Systems (cPP_DBMS)**.

=== Required Modules

This PP-Module must be claimed in conjunction with:

* **PP-Module for DBMS Cryptographic Functions (DBMS_MOD_CRYPTO)** [<<DBMS_MOD_CRYPTO>>]

*Rationale:* In a DBaaS model, the customer does not control the physical infrastructure. Therefore, cryptographic isolation (Data-at-Rest Encryption with Customer-Managed Keys) is mandatory to protect data from unauthorized access by the Service Provider or other tenants.

== TOE Overview

The Target of Evaluation (TOE) in this context is the **Database Service Instance**.

=== TOE Scope

The TOE includes:
*   The Database Engine (Single or Multi-Tenant Architecture).
*   The Autonomous Management Logic (Self-patching, Self-tuning agents *if* functionally part of the DBMS image).
*   Logical Resource Managers (CPU/Memory governance).

The TOE explicitly excludes:
*   The Cloud Control Plane (the web console used to provision the service).
*   The Hypervisor or Physical Infrastructure (Exadata, Generic Cloud Hardware).
*   The Service Provider's administrative personnel operations.

== Security Problem Definition

=== Threats

T.MALICIOUS_SERVICE_ADMIN::
A Service Provider administrator (DBA, Cloud Ops, Hardware Tech) may attempt to access or modify customer data, or inspect tenant queries, abusing their elevated privileges on the underlying platform.

T.TENANT_ISOLATION_FAILURE::
A malicious tenant may attempt to access the data, memory, or audit records of another tenant sharing the same database infrastructure (logical isolation failure).

T.NOISY_NEIGHBOR::
A tenant may consume excessive shared resources (CPU, I/O, Memory), causing a denial of service or performance degradation for other tenants on the shared platform.

T.AUDIT_TAMPERING::
A malicious tenant or compromised service account may attempt to delete or disable audit records to conceal unauthorized activities.

=== Assumptions

A.PLATFORM_ISOLATION::
The underlying cloud platform (Infrastructure) enforces physical and virtual isolation such that the TOE cannot be bypassed via the hypervisor, network fabric, or storage layer. This assumption is typically satisfied by platforms like Oracle Exadata or hardened hyper-visors.

A.SERVICE_AVAILABILITY::
The Service Provider ensures the availability of the infrastructure hosting the TOE.

== Security Objectives

=== Security Objectives for the TOE

O.CUSTOMER_CONTROLLED_KEYS::
The TOE shall ensure that the cryptographic keys used to protect tenant data (Master Keys) are controlled by the Customer (Tenant) or generated/managed in such a way that they are inaccessible to the Service Provider.

O.LOGICAL_ISOLATION::
The TOE shall enforce strict logical isolation between tenants, ensuring that one tenant cannot access another tenant's data, metadata, or audit streams.

O.RESOURCE_GOVERNANCE::
The TOE shall enforce resource quotas on a per-tenant basis to prevent any single tenant from consuming resources that would deny service to other tenants.

O.IMMUTABLE_AUDIT::
The TOE shall ensure that audit records, once generated, cannot be modified or deleted by the Tenant Administrator or the Database Service.

== Security Rationale

=== Threats to Objectives Mapping

[cols="1,2,3",options="header"]
|===
| Threat
| Security Objectives Addressing the Threat
| Rationale

| T.MALICIOUS_SERVICE_ADMIN
| O.CUSTOMER_CONTROLLED_KEYS
| Even if the admin accesses the storage, they cannot decrypt data without the Customer's key.

| T.TENANT_ISOLATION_FAILURE
| O.LOGICAL_ISOLATION
| The TOE enforces separation within the database engine (PDB/Schemas).

| T.NOISY_NEIGHBOR
| O.RESOURCE_GOVERNANCE
| The TOE limits resource consumption per tenant.

| T.AUDIT_TAMPERING
| O.IMMUTABLE_AUDIT
| Prevents the "Fox guarding the henhouse" scenario.
|===

== Security Functional Requirements

=== Conventions

*   [text within square brackets] indicates the completion of a selection.
*   *Bold text* indicates a refinement.

== Security Functional Requirements (Mandatory)

=== FCS: Cryptographic Support (Refinement)

==== FCS_CKM_DBAAS_EXT.1 Customer Controlled Keys

*FCS_CKM_DBAAS_EXT.1.1* The TSF shall support the use of Master Encryption Keys that are:
*   [selection:
    *   **Generated by the Customer**: Imported via a Trusted Channel (`FDP_ITC_EXT.1`).
    *   **Managed by an External Entity**: Referenced via a Key Management Service API (e.g., OCI Vault, AWS KMS) where the key material never leaves the customer-controlled boundary.]

*FCS_CKM_DBAAS_EXT.1.2* The TSF shall ensure that Service Provider administrators cannot access the Master Key material.

*Application Note:* This requirement enforces the "BYOK" or "External Key" model. Storing keys locally in a wallet managed by the Service Provider is insufficient to satisfy this requirement.

=== FDP: User Data Protection

==== FDP_RIP.1 Residual Information Protection (Memory)

*FDP_RIP.1.1* The TSF shall ensure that any previous information content of a resource (specifically memory and buffer cache) is made unavailable upon the allocation of the resource to a different tenant.

*Application Note:* In shared-memory DBaaS architectures, this prevents one tenant from reading "ghost" data from another tenant's recently freed memory blocks.

==== FDP_ACC.2(1) Tenant Isolation Policy

*FDP_ACC.2.1(1)* The TSF shall enforce the **Tenant Isolation SFP** on [all subjects, all objects, and all operations among them].

*FDP_ACC.2.2(1)* The TSF shall ensure that the TSF enforces the following rules for the Tenant Isolation SFP:
*   **Metadata Isolation**: A tenant cannot query metadata (system catalogs) revealing the existence or structure of other tenants' objects.
*   **Data Isolation**: A tenant cannot access data objects (tables, rows) belonging to another tenant unless explicitly granted access by that tenant.

==== FDP_RSG_EXT.1 Resource Governance

*FDP_RSG_EXT.1.1* The TSF shall enforce resource quotas limiting the consumption of [selection: CPU, Memory, I/O bandwidth, Sessions] by individual tenants.

*FDP_RSG_EXT.1.2* The TSF shall terminate or throttle operations that exceed the defined resource quotas.

*Application Note:* This addresses the "Noisy Neighbor" threat.

=== FIA: Identification and Authentication

==== FIA_USB.2 User-Subject Binding (Service Account Restriction)

*FIA_USB.2.1* The TSF shall associate the following user security attributes with subjects acting on the behalf of that user: [Tenant ID, User ID, Service Role].

*FIA_USB.2.2* The TSF shall enforce the following rules on the initial association of user security attributes with subjects: A subject (session) must bind to the Tenant ID of the authenticating user immediately upon authentication.

*Application Note:* This binding is critical for the Database Engine to enforce the logical isolation policies defined in `FDP_ACC.2`.

=== FMT: Security Management

==== FMT_MOF.1 Management of Security Functions Behavior (Tenant Admin Restriction)

*FMT_MOF.1.1* The TSF shall restrict the ability to **disable** the following functions **to no one (immutable)**:
*   Audit generation
*   Data-at-Rest Encryption

*FMT_MOF.1.2* The TSF shall restrict the ability to **modify** the following functions to the **Service Provider Administrator** (or automated autonomous logic):
*   Resource Governor Policies
*   Patching and Updates

*Application Note:* The Tenant Administrator is effectively excluded from managing security-critical configurations. They manage data (DDL/DML) but not security infrastructure.

=== FAU: Security Audit

==== FAU_SEL.1 Selective Audit (Tenant Separation)

*FAU_SEL.1.1* The TSF shall be able to select the set of events to be audited based on the following attributes: **Tenant ID**.

*FAU_SEL.1.2* The TSF shall enforce the following rule: A Tenant Administrator can query audit records generated by their own tenant only. They cannot query audit records of the Service Provider or other tenants.

==== FAU_STG_EXT.2 Immutable Audit Stream

*FAU_STG_EXT.2.1* The TSF shall protect the stored audit records from unauthorized **modification** and **deletion**.

*FAU_STG_EXT.2.2* The TSF shall not provide an interface for the Tenant Administrator to delete audit records.

=== FPT: Protection of the TSF

==== FPT_TUD_EXT.2 Autonomous Trusted Update

*FPT_TUD_EXT.2.1* The TSF shall provide a mechanism to apply updates automatically (autonomously) using trusted sources verified via digital signatures.

*FPT_TUD_EXT.2.2* The TSF shall validate the update package signatures before installation without requiring human intervention.

*Application Note:* This aligns with "Self-Driving" or "Autonomous" database features where patching is handled by the service logic, not a human admin.

[appendix]
== Extended Component Definitions

=== FDP_RSG_EXT: Resource Governance

===== Family Behaviour
Defines requirements for the TOE to limit resource consumption to prevent denial of service attacks in a shared environment.

===== Component Levelling
FDP_RSG_EXT.1 requires the TOE to enforce hard or soft limits on resource usage.

=== FCS_CKM_DBAAS_EXT: Customer Controlled Keys

===== Family Behaviour
Refines key management to specifically address the separation of duties required in a DBaaS environment, ensuring the provider cannot access customer keys.

=== FAU_STG_EXT: Immutable Audit Stream

===== Family Behaviour
Refines audit storage to prevent the deletion of logs, which is critical in managed environments where the customer cannot physically control the storage.

[appendix]
== SFR List

.Security Functional Requirements
[%header,cols=".^3,.^6,.^2"]
|===

|Requirement Class
|Requirement Component
|Type

.2+|Cryptographic Support (FCS)
|FCS_CKM_DBAAS_EXT.1 Customer Controlled Keys
|Mandatory

|FCS_CKM.4 Cryptographic Key Destruction (Referenced from Crypto Module)
|Mandatory

.3+|User Data Protection (FDP)
|FDP_RIP.1 Residual Information Protection (Memory)
|Mandatory

|FDP_ACC.2(1) Tenant Isolation Policy
|Mandatory

|FDP_RSG_EXT.1 Resource Governance
|Mandatory

.1+|Identification and Authentication (FIA)
|FIA_USB.2 User-Subject Binding (Service Account Restriction)
|Mandatory

.2+|Security Management (FMT)
|FMT_MOF.1 Management of Security Functions Behavior (Tenant Admin Restriction)
|Mandatory

.2+|Security Audit (FAU)
|FAU_SEL.1 Selective Audit (Tenant Separation)
|Mandatory

|FAU_STG_EXT.2 Immutable Audit Stream
|Mandatory

.1+|Protection of the TSF (FPT)
|FPT_TUD_EXT.2 Autonomous Trusted Update
|Mandatory

|===