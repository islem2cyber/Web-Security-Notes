# Access Control Lab Tracker

This tracker records the verified PortSwigger Web Security Academy progress for this learning path. All listed labs are completed; there are no postponed labs.

## Labs

| # | Lab name | Difficulty | Vulnerability category | Main concept | Status |
|---:|---|---|---|---|---|
| 1 | Unprotected admin functionality | Apprentice | Vertical privilege escalation; unprotected functionality | Administrative functionality must be protected by server-side authorization. | Completed |
| 2 | Unprotected admin functionality with unpredictable URL | Apprentice | Vertical privilege escalation; unprotected functionality | An undisclosed or unpredictable URL is not an authorization control. | Completed |
| 3 | User role controlled by request parameter | Apprentice | Parameter-based access control; vertical privilege escalation | A client-controlled role value must not determine server-side privileges. | Completed |
| 4 | User role can be modified in user profile | Apprentice | Parameter-based access control; vertical privilege escalation | Privilege changes require trusted authorization, not editable profile data. | Completed |
| 5 | User ID controlled by request parameter | Apprentice | Horizontal privilege escalation; IDOR | Enforce object-level authorization for user-selected identifiers. | Completed |
| 6 | User ID controlled by request parameter, with unpredictable user IDs | Apprentice | Horizontal privilege escalation; IDOR | Unpredictable identifiers do not replace authorization checks. | Completed |
| 7 | User ID controlled by request parameter with data leakage in redirect | Apprentice | Horizontal privilege escalation; IDOR; information disclosure | Review redirect responses and their contents when assessing access to user-specific resources. | Completed |
| 8 | User ID controlled by request parameter with password disclosure | Apprentice | Horizontal privilege escalation; IDOR; sensitive-data disclosure | Access checks must protect sensitive account data as well as ordinary profile data. | Completed |
| 9 | Insecure direct object references | Apprentice | IDOR; horizontal privilege escalation | A valid object reference does not itself authorize access to the referenced object. | Completed |
| 10 | URL-based access control can be circumvented | Practitioner | URL-based access-control bypass; vertical privilege escalation | Inconsistent URL interpretation between access-control and request-handling layers can undermine a path-based restriction. | Completed |
| 11 | Method-based access control can be circumvented | Practitioner | HTTP method-based access-control bypass; vertical privilege escalation | Authorization must cover the operation regardless of the accepted request method. | Completed |
| 12 | Multi-step process with no access control on one step | Practitioner | Multi-step process; context-dependent access control | Enforce authorization at every sensitive workflow step. | Completed |
| 13 | Referer-based access control | Practitioner | Referer-based access-control weakness | The Referer header is client-controlled request metadata and cannot establish authorization. | Completed |

## Progress summary

| Difficulty | Completed | Total |
|---|---:|---:|
| Apprentice | 9 | 9 |
| Practitioner | 4 | 4 |
| **Total** | **13** | **13** |

There are **13 completed labs and no postponed labs**. The main concepts and categories above summarize the exercise topics; they do not assert undocumented solution steps or additional lab observations.

## Documentation principles

- Record the exact route, method, identity, and state when documenting a specific observation.
- Separate a user-controlled parameter from a demonstrated authorization failure.
- Distinguish tests against authorized accounts from tests involving real users or data.
- Capture the root cause and recommended server-side fix, not just the observed symptom.

Use this tracker together with the [methodology](./methodology.md) and testing guides.
