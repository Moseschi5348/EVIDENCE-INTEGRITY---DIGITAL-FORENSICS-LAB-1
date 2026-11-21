# CYB 305 | Digital Forensics Lab 01

## Student preliminary forensic report

- **Case:** CYB305-001
- **Examination:** Evidence integrity, authentication, payment-portal activity, and a suspicious email
- **Time zone:** West Africa Time (WAT, UTC+01:00)

> **Training simulation.** The institution, accounts, payment references, email, and event records in this case are fictional. The IP addresses and `.example` domains are reserved for documentation. Do not contact them or treat this exercise as evidence of a real incident.

This report summarizes the supplied case material. The [original four-page report](<COLLECTED  EVIDENCE/01_Report/CYB305-001 Student Preliminary Forensic Report (1).pdf>) is available in the evidence bundle.

### 1. Integrity checks

`SHA256_SOURCE_MANIFEST.csv` records the SHA-256 reference hash for each of the nine source files. Verify a working copy against that manifest before analysis. The supplied E06 working copy does **not** match its source: it is one byte longer, and its beneficiary reference differs. Preserve the source and document the discrepancy; do not silently substitute the altered copy. A matching hash establishes byte-for-byte agreement with the listed reference, not provenance or authorship.

### 2. Key observations

- **Approved change:** Ticket CHG-217 authorizes a portal-banner update during 09:00–09:30 WAT; it explicitly excludes changes to the beneficiary reference. E03 PORT-003 records the approved banner change at 09:10:03 WAT.
- **Out-of-scope change:** E02 AUTH-004 and AUTH-005 record two failed `bursary.ops` login attempts from the simulated address `203.0.113.45`; AUTH-006 records a success and session `sess-b927` at 09:14:03 WAT. E03 PORT-006 / E04 WEB-007 record a beneficiary-reference change at 09:17:28 WAT, from `PAYEE-REF-UNIV-110` to `PAYEE-REF-ALT-909`. The change ticket does not authorize it.
- **Email:** E01 message `msg-843` asks a student to verify tuition-payment details. Its training headers record SPF and DMARC failures; E08 MAIL-003 records delivery at 09:24:16 WAT. The header note identifies the message as a classroom simulation, not an independently authenticated communication.
- **Custody:** E09 CUST-002 records a transfer from ICT Triage to the Forensic Desk at 10:19 WAT without a receiving acknowledgement. The other two listed transfers have acknowledgements.
- **Logging limits:** Authentication records show MFA as `NOT_CAPTURED`. The supplied records do not independently establish who controlled an account or performed an action.

### 3. Event timeline (WAT)

| Time | Recorded event | Evidence |
| --- | --- | --- |
| 09:10:03 | Approved portal banner update | E03 PORT-003; E04 WEB-003; CHG-217 |
| 09:14:03 | `bursary.ops` login succeeds from `203.0.113.45`; session `sess-b927` | E02 AUTH-006 |
| 09:17:28 | Beneficiary reference changes to `PAYEE-REF-ALT-909` | E03 PORT-006; E04 WEB-007; E06 |
| 09:24:12–09:24:16 | Email `msg-843` accepted and delivered despite recorded SPF/DMARC failures | E08 MAIL-002–003; E01 |
| 09:37:05 | Recipient reports `msg-843` as suspicious | E08 MAIL-006 |
| 10:19:00 | Evidence set transferred without a receiving acknowledgement | E09 CUST-002 |

UTC authentication and web-access timestamps have been converted to WAT (UTC+01:00) for this timeline. The supplied records are exports; clock accuracy and the completeness of the underlying logs have not been independently established.

### 4. Findings and limits

**Observed:** The supplied portal audit records a beneficiary-reference change outside the scope stated in CHG-217. A related successful login and web request share a session and timestamp sequence. The email export records failed sender-authentication checks and delivery.

**Not established:** These records alone do not establish an individual’s identity, intent, account compromise, payment diversion, email authenticity, or the real-world occurrence of any event. The case is expressly synthetic.

**Further evidence:** Preserve and hash the source exports and configuration snapshots; obtain the relevant original server, identity-provider, proxy, mail-gateway, and change-management records; document clock sources and custodians; and corroborate authorization and payment outcomes through independently preserved records.

### 5. Evidence handling and recommendations

- Retain the supplied sources unchanged. Create verified, write-protected working copies and record hashes, acquisition details, timestamps, and every custodian transfer.
- Resolve and document the missing E09 CUST-002 acknowledgement and the period without a recorded accountable recipient.
- Preserve relevant raw logs and snapshots. Record MFA events and synchronize system clocks.
- Limit access to account, email, and address data to the investigation’s needs, and handle any real-world records under applicable Nigerian evidence and data-protection requirements.

## Evidence bundle

- The public evidence bundle omits two personal-detail screenshots.
- [Original preliminary report](<COLLECTED  EVIDENCE/01_Report/CYB305-001 Student Preliminary Forensic Report (1).pdf>)
- [Source records](SOURCE_EVIDENCE/)
- [Working copies](WORKING_COPIES/)
- [SHA-256 source manifest](SHA256_SOURCE_MANIFEST.csv)
- [Report template](STUDENT_REPORT_TEMPLATE.txt)
- [Case walkthrough](https://moseschi5348.github.io/EVIDENCE-INTEGRITY---DIGITAL-FORENSICS-LAB-1/)

The case walkthrough is hosted with GitHub Pages. Updates pushed to `main` are published by the [Pages workflow](.github/workflows/pages.yml).
