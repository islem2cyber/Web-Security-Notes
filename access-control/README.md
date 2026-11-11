# Access Control Vulnerabilities and Privilege Escalation

Personal notes and learning references for access control, privilege escalation, and authorization weakness analysis. This section documents the root causes, common exploitation patterns, and defensive design principles behind broken access control in web applications.

## Learning progress

The foundational stage is complete: **13 labs completed** (9 Apprentice and 4 Practitioner), with no postponed labs. See the [lab tracker](./lab-tracker.md) for the exercise list and status.

## Scope

Access control is the process by which a server decides whether an identity is allowed to access a resource or perform an action. Authentication verifies who someone is; authorization determines what they may do. A user can be authenticated and still be blocked by missing, weak, or bypassable authorization logic.

These notes focus on mechanisms and failures that matter in real application stacks:

- Authentication and session context.
- Authorization checks, permissions, and role data.
- Horizontal and vertical privilege escalation.
- IDOR, BOLA, and BFLA.
- Unprotected administrative and sensitive functionality.
- Parameter tampering, routing bypasses, and workflow abuse.
- Safe testing practices in explicitly authorized environments.

## Repository structure

- [Lab tracker](./lab-tracker.md)
- [Methodology](./methodology.md)
- Fundamentals: [Access control fundamentals](./fundamentals/access-control-fundamentals.md), [Access control models](./fundamentals/access-control-models.md), [Authentication vs authorization](./fundamentals/authentication-vs-authorization.md)
- Vulnerabilities: [Vertical privilege escalation](./vulnerabilities/vertical-privilege-escalation.md), [Horizontal privilege escalation](./vulnerabilities/horizontal-privilege-escalation.md), [IDOR](./vulnerabilities/idor.md), [BOLA and BFLA](./vulnerabilities/bola-and-bfla.md), [Unprotected functionality](./vulnerabilities/unprotected-functionality.md), [Parameter-based access control](./vulnerabilities/parameter-based-access-control.md), [Platform misconfiguration](./vulnerabilities/platform-misconfiguration.md), [URL-matching discrepancies](./vulnerabilities/url-matching-discrepancies.md), [Multi-step process vulnerabilities](./vulnerabilities/multi-step-process-vulnerabilities.md), [Referer-based access control](./vulnerabilities/referer-based-access-control.md), [Context-dependent access control](./vulnerabilities/context-dependent-access-control.md)
- Testing: [Manual testing](./testing/manual-testing.md), [Authorization testing matrix](./testing/authorization-testing-matrix.md), [Burp Suite workflow](./testing/burp-suite-workflow.md)
- Prevention: [Secure access control design](./prevention/secure-access-control-design.md)

## Learning objectives

1. Understand the difference between identity, session state, and authorization decisions.
2. Recognize broken access control in both UI and API flows.
3. Separate direct evidence of an access-control flaw from a mere implementation detail.
4. Evaluate whether a control is enforced server-side and consistent across steps.
5. Recommend mitigations rooted in least privilege, deny-by-default, and explicit policy enforcement.

## Study sequence

1. Start with the [fundamentals](./fundamentals/access-control-fundamentals.md) and [authentication vs authorization](./fundamentals/authentication-vs-authorization.md) notes.
2. Review [access control models](./fundamentals/access-control-models.md) and understand how policy goals differ from implementation style.
3. Examine the main vulnerability classes: [vertical privilege escalation](./vulnerabilities/vertical-privilege-escalation.md), [horizontal privilege escalation](./vulnerabilities/horizontal-privilege-escalation.md), and [IDOR](./vulnerabilities/idor.md).
4. Read the API-specific material on [BOLA and BFLA](./vulnerabilities/bola-and-bfla.md) and the workflow-based issues in [multi-step process vulnerabilities](./vulnerabilities/multi-step-process-vulnerabilities.md).
5. Apply the [manual testing](./testing/manual-testing.md) and [authorization testing matrix](./testing/authorization-testing-matrix.md) in an explicitly authorized environment.
6. Review the [secure design](./prevention/secure-access-control-design.md) guidance and connect it to future API Security work.

## Notes on scope

This collection is a learning reference built from practical web-security reasoning and controlled lab observations. Completing the listed labs does not imply mastery of every access-control topic or guarantee that a specific application is vulnerable; the notes do not replace formal authorization design or secure coding reviews.
