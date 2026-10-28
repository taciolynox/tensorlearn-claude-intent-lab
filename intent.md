# Intent: results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved PRD N-17, 2026-09-18), `inputs/02-current-meeting.md`
(discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17
and is history only: nothing in this intent is approved on its authority.

## Problem

- **P-1** The fictional clinic receives avoidable calls from patients checking whether their results
  are ready. (01: "receives avoidable calls from patients checking whether results are ready")

## Proposed outcome

- **I-1** Notify the patient when a result is ready for their review in the existing authenticated
  portal. (01)
- **I-2** "Ready" means a doctor has reviewed and released the result. A notification is never
  eligible before review and release; a finished laboratory upload alone does not make a result
  ready. (01: "A doctor must review and release the result before any notification is eligible";
  02, product owner: "'Ready' must not mean that the laboratory upload merely finished.")
- **I-3** The message only says that an update is available and directs the patient to the existing
  authenticated portal. (01)

### How success will be observed

- **S-1** Results-status calls, counted separately from clinical questions and compared against a
  measured baseline. (01: "Measure reduction in results-status calls"; 02, support lead)
- **S-2** Delivery failures. (01: "Measure reduction in ... delivery failures")
- **S-3** No numeric target is approved. The baseline and the target will be agreed during discovery
  (01). The "90% call reduction in one week" in the superseded request is not a target.

## Affected users and systems

- **U-1** Patients: the only approved recipients of the notification. (01)
- **U-2** Doctors: review and release results before any notification becomes eligible. (01)
- **U-3** Support staff: field the status calls and own the call measurement. (02)
- **SYS-1** The existing authenticated portal and its existing result route. (01, Scope; 02,
  engineering lead: "the existing portal path is adequate")
- **SYS-2** The notification delivery channel, which is still undecided (OPEN-1), and the
  notification and operational logs.

## Scope

In scope (01):
- Notification through the existing portal result route.
- Duplicate prevention for the same released result version.
- Delivery-failure handling that does not send duplicate patient messages.

## Out of scope

Excluded (01, Scope; 02):
- **X-1** Portal redesign.
- **X-2** A new or rebuilt patient dashboard: a separate project (02, engineering lead). The
  superseded request asked for it; it is not approved.
- **X-3** Appointment scheduling.
- **X-4** Clinical interpretation.
- **X-5** Notifying third parties. The referring doctor is not approved as a recipient and is
  handled only as an open question (OPEN-2).
- **X-6** Sending results automatically on lab upload, and including the test result in an SMS:
  both come only from the superseded request.

## Constraints

- **C-1** Doctor review and release must happen before any notification. (01; 02)
- **C-2** No diagnosis, test result, clinical measurement or treatment detail in the message or in
  notification and operational logs. (01; 02, privacy owner: "no sensitive content in messages or
  operational logs")
- **C-3** Access is authenticated in the existing portal. No attached results and no public result
  link. (01)
- **C-4** At most one notification per released result version, and delivery failures are handled
  without duplicate patient messages. (01)
- **C-5** Patients are the only recipients. Every other recipient needs explicit authorization by
  its owners (OPEN-2). (01; 02, clinical owner)

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: product owner and privacy owner. They
  decide after the consent and delivery review. (01; 02, privacy owner: "Consent and the actual
  channel are still under review.") No channel is assumed; the SMS in the superseded request is
  not a decision.
- **OPEN-2 Referring-doctor notification.** Owners: clinical owner and privacy owner, who must
  decide on recipient authorization. Not approved by PRD N-17. (01; 02, clinical owner: "do not
  infer approval for referring-doctor notifications from the old request.")
- **OPEN-3 Baseline and numeric target** for status-only calls. To be agreed during discovery (01);
  no target until then.

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
