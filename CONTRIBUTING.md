# Feedback and Participation in PadLab

Language: [简体中文](https://github.com/Murphy605/PadLab/blob/main/CONTRIBUTING.md) | **English**

Thank you for your interest in PadLab.

PadLab is an open-source project currently focused on **PC motion control, calibration, configuration, and related tools for the EasySMX D10**.

## Mainline Contribution Policy

**PadLab is open source, but the official mainline is currently not open to external code contributions.**

We do not accept pull requests from external contributors, including changes to:

- program code;
- documentation;
- examples;
- build scripts;
- generated files;
- images, assets, or other project files.

If you find a problem, have an idea for an improvement, or have already implemented a fix in your own fork, please open an issue instead of preparing a pull request.

This policy is intended to keep the official mainline simple and traceable, and to ensure that changes entering official releases go through the same implementation, testing, and physical-hardware validation workflow.

It does not restrict anyone from studying, modifying, or maintaining their own version within the rights granted by the project license.

## What We Welcome

Although the official mainline does not accept external code, the following contributions are valuable and very welcome:

- bug reports;
- game or system compatibility reports;
- reproducible failure cases;
- logs and diagnostic information;
- physical hardware test results;
- behavioral differences between D10 hardware or firmware revisions;
- observations about USB, HID, IMU, 2.4 GHz, or other protocol behavior;
- feature requests and real-world use cases;
- root-cause analysis;
- technical analysis, design ideas, and possible solution directions.

For PadLab, **a clear description of the problem with reproducible evidence is often more useful than a ready-made code change.**

## If You Have Already Fixed the Problem

If you have implemented a fix in your own fork, you do not need to turn the code into a pull request.

It is more useful to explain in an issue:

- what the original problem was;
- how it can be reproduced reliably;
- what you believe the root cause is;
- what behavior changed after your fix;
- what testing you actually performed;
- what remains untested;
- what logs, screenshots, videos, or other evidence support the conclusion.

PadLab may use publicly shared problem descriptions, test results, technical observations, and solution ideas as input and then **implement an appropriate solution independently** according to the architecture and validation requirements of the official mainline.

Code from an external fork does not automatically become part of the official mainline simply because it solves the same problem.

## Writing a Useful Bug or Compatibility Report

Please provide as much relevant information as practical.

For D10-related issues, useful information will often include:

- PadLab / D10 tool version or build;
- Windows version;
- wired or 2.4 GHz mode;
- controller firmware version, if known;
- receiver firmware version, if known;
- affected game or application;
- clear reproduction steps;
- expected behavior;
- actual behavior;
- whether the issue reproduces consistently;
- logs, diagnostics, screenshots, or video where useful.

If testing has been performed, clearly distinguish **what was actually tested** from **what is still only a hypothesis**.

For example:

- Automated tests: performed
- Physical D10 test: performed
- 2.4 GHz mode: not tested
- Reproduced only in one game
- Root cause: unconfirmed hypothesis

Passing automated tests does not by itself establish that hardware-facing behavior has been physically validated.

Before posting logs, screenshots, or other diagnostics, remove account information, tokens, keys, personal information, and other sensitive data.

## Feature Requests

Feature requests do not need to include code.

Please explain, where possible:

- the real-world use case;
- what is currently inconvenient or missing;
- the behavior you would like to see;
- the specific problem the feature would solve.

Whether a feature is implemented, how it is implemented, and when it is implemented remain project decisions based on scope, complexity, compatibility risk, and long-term maintenance cost.

## AI-Assisted Analysis

PadLab itself uses an **AI-assisted engineering** workflow, so the use of AI for analysis, research, or organizing technical information is welcome.

What matters is **whether the information can be verified**.

When AI tools are involved, please distinguish where possible between:

- directly observed facts;
- conclusions supported by logs or data;
- unverified hypotheses;
- possible explanations suggested by AI.

Do not present unverified AI-generated output as physical hardware test results or confirmed facts.

## Forks, Modifications, and Independent Versions

Original PadLab source code is provided under the **GPL-3.0-only** terms stated in this repository.

Under that license, you may:

- fork the project;
- study the source code;
- modify your own version;
- build and use it yourself;
- maintain your own branch or derivative version;
- distribute modified versions in compliance with the applicable license terms.

You are not required to submit modifications that you make only for your own use back to PadLab.

If you publicly distribute a modified version, you must comply with the applicable GPL-3.0-only requirements.

Modified versions, forks, and third-party distributions must not be presented as **official PadLab releases** or imply official endorsement, maintenance, or approval by PadLab.

Nothing in this contribution policy reduces or restricts any rights already granted to you under GPL-3.0-only.

## Third-Party Material

Do not submit or publicly upload material that you do not have the right to redistribute, including:

- official EasySMX firmware files;
- proprietary third-party software;
- leaked source code;
- private keys, tokens, or account credentials;
- restricted third-party assets or data.

When an issue involves such material, provide the necessary behavioral description, logs, hashes, version information, or other publicly shareable technical observations instead of uploading restricted original files.

## Security Issues

If you discover a potential security vulnerability, sensitive-data exposure, device-damage risk, or exploitable behavior, please do not publish complete exploitation details in a public issue.

Once PadLab provides a dedicated security-reporting channel, follow the repository's `SECURITY.md` or GitHub Private Vulnerability Reporting instructions.

## Final Note

PadLab welcomes community help in finding problems, providing evidence, sharing test results, and proposing new ideas.

**Open source does not require the official mainline to be open to external writes.**

For now, PadLab prioritizes a simple, traceable, and verifiable maintenance path, so official implementations remain within the project's existing maintenance workflow.

Thank you for testing, feedback, and research.
