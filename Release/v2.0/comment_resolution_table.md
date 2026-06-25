# DBMS CC:2022 Comment Resolution Table

Generated from the annotated PDFs in `Documents/Comments` after the first resolution pass.

Status key:

* `Fixed`: source was changed and regenerated into HTML/PDF.
* `Verified`: source already addressed the comment or the comment was a context highlight.

| # | Document / page | Reviewer comment or marked text | Resolution so far | Status | Source / output location |
|---|---|---|---|---|---|
| 1 | cPP p.6 | "Should the CCMB versions be included? Could it be copyright issues with the reference the the ISO versions?" | Updated the cPP and SD references to follow the NDcPP v4 CCMB-style citations for CC1-CC5, CCE, and CEM. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:81`, `:314`; `Documents/SD/DBMS_SD-working_copy.adoc:729` |
| 2 | cPP p.7 | "Duplicate" on the related-documents table. | Removed `%header` from the reference/related-document tables so the first data row does not repeat as a table header across a page break. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:79`, `:99`, `:108`; regenerated PDF p.7 |
| 3 | cPP p.7 | "Errata v1.2 to CC and CEM should be included." | Added the NDcPP-style `[CCE]` errata and interpretation reference for CC:2022 and CEM:2022, and referenced it in the cPP conformance text. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:91`, `:314`; `Documents/SD/DBMS_SD-working_copy.adoc:739` |
| 4 | cPP p.8 | Highlighted "This is an application note." | Treated as context for the next sticky note. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:137` |
| 5 | cPP p.8 | "Application Note 1:" | Changed the example text to `Application Note 1: This is an application note.` | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:137` |
| 6 | cPP p.10 | "The next 3 bullets should be indented." | Converted the three user-data examples into nested bullets under the first data type. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:243`, `:246` |
| 7 | cPP p.11 | "The 3 next paragraphs should be a bullet list." | Converted the external IT entity paragraphs into a three-item bullet list. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:272`, `:274` |
| 8 | cPP p.13 | "Errata v1.2 should be included." | Same NDcPP-style `[CCE]` errata/interp fix as rows 1 and 3. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:91`, `:314`; `Documents/SD/DBMS_SD-working_copy.adoc:739` |
| 9 | cPP p.20 | "The text \"Application Note x:\" is missing for the application notes later in the document." | Added numbered `Application Note n:` prefixes to the SFR-level note paragraphs in the cPP. | Fixed | Examples: `Documents/cPP/DBMS_cPP-working_copy.adoc:537`, `:631`, `:659`, `:836`, `:1650` |
| 10 | cPP p.22 | "Refinement: and any identified groups" | Added group association language to FAU_GEN.2.1. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:612`, `:614` |
| 11 | cPP p.22 | "Refinement: [selection: \"user\", \"user and group\"]" | Added `[selection: user, user and group]` to FAU_GEN.2.1. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:614` |
| 12 | cPP p.22 | Highlighted `user identity` in FAU_SEL.1.1. | Made the mandatory `user identity` attribute bold to show the DBMS refinement. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:627` |
| 13 | cPP p.22 | "Refinement: Crossed out, bold text" on FAU_SEL.1.1. | Retained `user identity` in the selection list as bold, crossed-out text because it is now the mandatory first bullet. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:628` |
| 14 | cPP p.22 | Highlighted FDP_ACC.1.1 text "all subjects, all DBMS-controlled objects, and all operations among them." | Treated as context for assignment operation comment. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:644`, `:646` |
| 15 | cPP p.22 | "Assignment: [all subjects, all DBMS-controlled objects, and all operations among them]" | Wrapped the FDP_ACC.1.1 scope in brackets as the completed assignment. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:646` |
| 16 | cPP p.25 | Highlighted `the [` in FMT_MSA.3.2. | Treated as context for the next sticky note. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:762`, `:764` |
| 17 | cPP p.25 | "Refinement: Crossed out, bold text" on FMT_MSA.3.2. | Marked deleted `the` before `[no user]` as bold, crossed-out text. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:764` |
| 18 | cPP p.25 | "(Users)" near FMT_REV.1(1). | Changed the title to `FMT_REV.1(1) Revocation (Users)`. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:781`; SD aligned at `Documents/SD/DBMS_SD-working_copy.adoc:342` |
| 19 | cPP p.25 | Highlighted `(DAC)` near FMT_REV.1(2). | Treated as context for the object-label comment. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:794` |
| 20 | cPP p.25 | "(Objects)" near FMT_REV.1(2). | Changed the title to `FMT_REV.1(2) Revocation (Objects)`. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:794`; SD aligned at `Documents/SD/DBMS_SD-working_copy.adoc:364` |
| 21 | cPP p.26 | `[assignment: any additional security management functions required to configure the claimed security]` | Confirmed the assignment is present in FMT_SMF.1.1. No source change needed. | Verified | `Documents/cPP/DBMS_cPP-working_copy.adoc:832` |
| 22 | cPP p.34 | "Should be more like FIA_USB_EXT" on FTA_MCS_EXT component leveling. | Reworked the FTA_MCS_EXT component leveling text/diagram to follow the family-to-component style used by FIA_USB_EXT and added hierarchy text. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:1146` |
| 23 | cPP p.34 | Highlighted `FIA_UID.1` dependency. | Treated as context for the next sticky note. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:1185` |
| 24 | cPP p.34 | "Should it be FIA_UID.2 because it is exact conformance?" | Changed the FTA_MCS_EXT dependency in the extended component definition and dependency rationale from FIA_UID.1 to FIA_UID.2. | Fixed | `Documents/cPP/DBMS_cPP-working_copy.adoc:1185`; rationale at `:1582` |
| 25 | SD p.8 | "other" near FAU_GEN.1 testing note. | Changed "the testing of the security mechanisms" to "the testing of other security mechanisms." | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:136` |
| 26 | SD p.9 | "The testing ..." near FAU_SEL.1 testing note. | Changed "The following testing" to "This testing" for clearer wording. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:174` |
| 27 | SD p.12 | "(User)" near FMT_REV.1(1). | Changed the section title to `FMT_REV.1(1) Revocation (Users)`. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:342` |
| 28 | SD p.13 | Highlighted `(iii)` in FMT_REV.1(1) note. | Treated as context for the wording change. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:360` |
| 29 | SD p.13 | "\"...before completing the test\". Or something like that." | Replaced "before completing (iii)" with "before completing the test." | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:360`, `:382` |
| 30 | SD p.13 | Highlighted `(DAC)` near FMT_REV.1(2). | Treated as context for the object-label comment. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:364` |
| 31 | SD p.13 | "(Object)" near FMT_REV.1(2). | Changed the section title to `FMT_REV.1(2) Revocation (Objects)`. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:364` |
| 32 | SD p.27 | "Should 7.1 - 7.7 be Appendix A.1- A7?" | Converted the Vulnerability Analysis block into Appendix A, so subsections now render as A.1 through A.7. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:748`, `:750`, `:816`; regenerated PDF p.28 |
| 33 | SD p.31 | "Should this be a heading?" near Glossary. | Converted `Glossary` into a proper `== Glossary` heading. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:898`; regenerated PDF p.33 |
| 34 | SD follow-on cleanup | Glossary table had stale AVA table caption visible after heading fix. | Renamed glossary table captions to `Terms and Definitions` and `Acronyms used in this SD`. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:904`, `:921` |
| 35 | SD follow-on cleanup | SAR mapping table captions had copied component names. | Corrected table captions for ALC_FLR.3, ATE_IND.2, and AVA_VAN.2 mappings. | Fixed | `Documents/SD/DBMS_SD-working_copy.adoc:602`, `:655`, `:692` |

## Verification performed

* Regenerated `Documents/cPP/DBMS_cPP-working_copy.html`
* Regenerated `Documents/cPP/DBMS_cPP-working_copy.pdf`
* Regenerated `Documents/SD/DBMS_SD-working_copy.html`
* Regenerated `Documents/SD/DBMS_SD-working_copy.pdf`
* Spot-checked rendered PDF pages covering the reference table, FAU/FDP/FMT/FTA changes, Appendix A numbering, and Glossary tables.
