# Click-Alignment Threat Model (CATM)

**CATM** is a behavioral framework for evaluating the human interaction dimension of software vulnerabilities. Traditional severity scores often overlook how exploits rely on user behavior. CATM introduces the **Total Threat Interaction Score (TTIS)** — a quantifiable way to model how many user actions (and how much friction) are required for an exploit to succeed.

This model helps defenders prioritize vulnerabilities not just by technical risk, but by how stealthy or interactive their exploitation path is.

---

## 🔍 Why Use CATM?

While most vulnerability scoring systems emphasize technical complexity, many attacks hinge on a human clicking a link, opening a file, or ignoring a warning. CATM fills that gap by:

- Capturing user interaction as a measurable component of risk.
- Highlighting stealthy, low-friction exploits (often more dangerous).
- Offering a lightweight, extensible model for behavioral threat analysis.

---

## 📊 Core Metrics

### Click Action Score (CAS)
The minimum number of deliberate user actions (e.g., clicks, confirmations) required to complete the exploit path.

### Click Severity Modifier (CSM)
A friction or stealth score for each interaction step. CSM reflects how noticeable, suspicious, or deceptive a given interaction is:

- **+1** – Action includes warning or friction (e.g., UAC prompt, security popup).
- **0** – Neutral or typical interaction (e.g., opening a document).
- **−1** – Action mimics trusted behavior or hides intent (e.g., typo-squatting, spoofed link).

### Total Threat Interaction Score (TTIS)
**TTIS = CAS + total of CSM values**

Lower TTIS implies higher behavioral risk, because fewer or stealthier steps are required for exploitation.

---

## 🧠 Real-World Examples

| CVE ID          | CAS | CSM | TTIS | Notes                                      |
|-----------------|-----|-----|------|---------------------------------------------|
| CVE-2021-30860  | 0   | 0   | 0    | Zero-click exploit (Image I/O parsing)     |
| CVE-2017-0199   | 1   | 0   | 1    | Malicious DOCX file, opened by user        |
| CVE-2018-8174   | 1   | +1  | 2    | Requires scripting be enabled + trigger    |
| CVE-2019-0709   | 2   | +1  | 3    | Requires authenticated access + trigger    |
| UAC             | +1  | +1  | +2   | Warning dialog creates user friction       |

See the [examples/](./examples) folder for more.

---

## 🛡 Implications for Defense

- Prioritize patching of low-TTIS vulnerabilities.
- Train users to resist common exploit actions (like document opening or blind link clicking).
- Preserve mechanisms that increase CAS or CSM (e.g., popups, sandboxing, UAC).
- Avoid alert fatigue — excessive friction may desensitize users and reduce effectiveness.

---

## 📁 Repository Structure

