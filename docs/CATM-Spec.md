# Click-Alignment Threat Model (CATM): A Behavioral Framework for Vulnerability Risk Assessment

## Overview

Traditional vulnerability metrics often prioritize technical attributes—such as exploit complexity or system impact—while overlooking how user behavior mediates the path to exploitation. The **Click-Alignment Threat Model (CATM)** fills this gap by quantifying the *minimum meaningful user interactions* (or “clicks”) needed for an exploit to succeed, modified by how perceptible or frictionful those interactions are.

CATM introduces a simple but powerful behavioral scoring system that complements existing frameworks like CVSS by factoring in human agency and interaction surface.

---

## Core Concepts

### Click Action Score (CAS)

The number of deliberate user actions required to progress an exploit. CAS typically counts direct actions like:

- Clicking links
- Opening attachments
- Running files
- Accepting prompts

CAS does **not** count passive exposure or background activity (e.g., receiving an email).

### Click Severity Modifier (CSM)

Each action is adjusted using a **Click Severity Modifier (CSM)**, which reflects how *noticeable*, *suspicious*, or *disruptive* the action is from a user-awareness perspective.

- **Positive (+)**: The action involves visible friction or prompts that could alert the user.
  - Examples: Security warnings, UAC dialogs, SSL cert errors.
- **Neutral (0)**: The interaction appears typical or expected.
  - Examples: Opening a document, clicking a familiar-looking link.
- **Negative (−)**: The interaction is unusually deceptive or stealthy, mimicking trusted behavior.
  - Examples: Typosquatted URLs, spoofed login screens, silent downloads.

### Total Threat Interaction Score (TTIS)

The combined behavioral score:

TTIS = CAS + sum of CSM values


- **Lower TTIS → higher threat** (exploit is easier or stealthier).
- **Higher TTIS → more friction and detection opportunity.**

---

## Why User Interaction Matters

Security isn’t just technical—it’s behavioral.

Each required interaction gives a user an opportunity to:
- Notice something is wrong
- Pause or reject the action
- Alert others

Exploits that reduce or obscure these interactions (zero-click, typosquatting, UAC bypasses) carry more behavioral risk and deserve special attention in triage and mitigation.

---

## CATM in Practice: Vulnerability Scoring Examples

| CVE ID          | CAS | CSM | TTIS | Notes                                                       |
|-----------------|-----|-----|------|-------------------------------------------------------------|
| CVE-2021-30860  | 0   | -1  | -1   | True zero-click; no user interaction required               |
| CVE-2025-27143  | 1   | -1  | 0    | User redirected on login; standard login page               |
| CVE-2018-8174   | 1   | +1  | 2    | Link or doc triggers script; scripting must be enabled      |
| CVE-2019-0709   | 2   | +1  | 3    | Exploit requires network setup and user-triggered execution |
| UAC-protected exploit | +1 | +1 | +2 | User clicks and sees a UAC prompt                         |

> These scores are illustrative. In practice, TTIS can vary depending on context (e.g., enterprise vs. consumer use, training level, hardened environments).

---

## Defensive Implications

CATM suggests prioritizing vulnerabilities and mitigations with an eye toward user behavior:

- **Low TTIS = high urgency.** Patch or block stealthy, low-friction exploits first.
- **Increase friction.** Use prompts, confirmations, and anomaly detection to slow exploit progression.
- **Protect normal interactions.** Harden everyday actions like file opening, link clicking, and credential input.
- **Train users at real inflection points.** Behavioral defense is most effective when it intercepts typical exploit patterns.

---

## Design Ethics and Limitations

CATM is a framework, not a guarantee. It should be applied carefully with the following caveats in mind:

### 1. Security ≠ Elimination
No model prevents all threats. CATM shifts attacker economics by increasing the cost and complexity of exploitation.

### 2. Respect User Autonomy
Adding friction can backfire if it annoys or fatigues users. CATM should be used to inform *smart design*, not paternalism.

### 3. Avoid Friction Fatigue
Too many popups, warnings, or prompts can cause users to become desensitized, clicking through instinctively.

### 4. Consider Context
Not all environments are the same. A trained sysadmin and a casual user experience risk differently. CATM’s interpretation should reflect that.

---

## Future Work

Areas for development and exploration:

- Tooling for automatic TTIS calculation from CVE or user flow descriptions.
- Integration with existing scoring systems (e.g., CVSS extensions).
- Empirical studies of user behavior and click-path resistance.
- Dataset of TTIS-scored CVEs with crowd input.

---

## Attribution

The CATM framework is developed as an open, extensible approach to combining human behavior with exploit risk modeling. Contributions and critiques are encouraged. See [CONTRIBUTING.md](../CONTRIBUTING.md) for more information.

