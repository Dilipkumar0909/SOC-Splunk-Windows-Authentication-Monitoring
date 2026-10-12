# Windows Authentication Monitoring and Investigation Using Splunk

## Project Overview

This project demonstrates a basic Security Operations Center (SOC) workflow using Splunk Enterprise and Windows Security logs. I configured log forwarding from a Windows virtual machine to Splunk, investigated successful and failed authentication events, and prepared a basic detection query for repeated failed logons.

## Objectives

- Collect Windows Security events in Splunk.
- Search and investigate failed logons (Event ID 4625).
- Examine successful logons (Event ID 4624).
- Understand Windows logon types and authentication failure codes.
- Develop a basic SPL query to identify repeated failed logons.
- Document investigation findings with screenshots.

## Lab Architecture

- **Windows 11 VM:** Generates Windows Security events.
- **Splunk Universal Forwarder:** Forwards selected Windows logs.
- **Ubuntu VM:** Runs Splunk Enterprise.
- **Receiving port:** TCP 9997.
- **Index:** `main`.

## Tools Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 11
- Ubuntu Linux
- VirtualBox
- Splunk Search Processing Language (SPL)

## Investigation Highlights

### Event ID 4625 — Failed Logon

During controlled testing, I entered an incorrect password three times and investigated the resulting Windows Security events.

Observed event details included:

- Logon Type: `2` — Interactive
- Status: `0xC000006D` — General logon failure
- Substatus: `0xC000006A` — Incorrect password
- Source address reported by the event: `127.0.0.1`

These were deliberately generated lab events, not a confirmed real-world attack.

### Event ID 4624 — Successful Logon

I searched for successful logon events and examined the available event details. Logon type and surrounding activity must be considered before deciding whether an event is expected or suspicious.

## Detection

A basic SPL query was prepared to count failed logons by computer, account, and logon type in five-minute time buckets. The threshold is three or more failures.

This detection identifies activity for further investigation; it does not independently prove brute-force activity.

See [`queries/authentication-investigation.spl`](queries/authentication-investigation.spl) for the queries.

## Repository Structure

```text
evidence/
    Screenshots of lab configuration and investigations
queries/
    authentication-investigation.spl
report/
    investigation-report.md
```

## Evidence

The `evidence` directory contains screenshots of the receiver configuration, forwarder service, Windows Security log ingestion, failed and successful logon investigations, and detection results where captured.

## Key Learning Outcomes

- Configuring a basic Windows-to-Splunk log collection workflow.
- Searching Windows Security events using SPL.
- Interpreting Event IDs 4624 and 4625.
- Understanding interactive logons and authentication failure codes.
- Documenting findings and distinguishing controlled testing from confirmed malicious activity.

## Limitations

This is a small home lab using controlled test events. The detection query has not been validated against a production environment and may require field extraction adjustments.

## Conclusion

This project demonstrates foundational practical skills relevant to a junior SOC analyst role, including log ingestion, authentication monitoring, event investigation, basic detection logic, and technical documentation.