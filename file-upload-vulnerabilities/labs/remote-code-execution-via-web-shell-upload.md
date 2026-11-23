# Remote code execution via web shell upload

- **Difficulty:** Apprentice
- **Status:** SOLVED

## Vulnerability concept

An application accepts a server-side script through its upload functionality and serves or stores it in a location where it is interpreted.

## Root cause

The upload boundary does not prevent executable server-side content, or server configuration allows uploaded script files to execute. Generated names, execution isolation, and server-side allowlists may be missing.

## Technical explanation

The uploaded file is server-side code, but upload alone does not execute it. Execution becomes possible when the file is in an interpreter-enabled location and accessible to the application server. This lab's exact payload and HTTP observations have not been supplied, so no specific request or capture is reproduced.

## What the lab teaches

- Uploaded files can cross a validation boundary.
- Storage location and server configuration affect whether upload leads to execution.
- A web-accessible script can become a web shell in a vulnerable setup.

## Security implications

Successful execution can give an attacker server-side privileges under the web application account. Impact includes data access, command execution, and further compromise, subject to server isolation and least privilege.

## Proper remediation

- Generate server-side filenames.
- Store outside executable locations.
- Disable interpreters in upload directories.
- Apply strict allowlists and format validation.
- Run the application with least privilege.
- Block dangerous formats and embedded active content.

## Key takeaways

The complete risk is the combination of accepted content, storage, and execution configuration. Upload validation and server configuration must both prevent script interpretation.

## Personal observations

To be added.
