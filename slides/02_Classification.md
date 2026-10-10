## Same bug, many names

| Term | Answers | Example |
|------|---------|---------|
| **CVE** | Which specific vulnerability? | CVE-2021-44228 |
| **CWE** | What *kind* of weakness? | CWE-89 SQL Injection |
| **CVSS** | How severe? | 9.8 – Critical |
| **NVD** | Where do I look it up? | nvd.nist.gov |
| **EPSS / KEV** | Is it actually exploited? | cisa.gov KEV catalog |

Note:
- common language: tools, advisories, teams
- CVE = instance, CWE = class
- score ≠ risk (later)
- the three examples are independent (9.8 is the textbook "all-high" vector)

---

## CVE – Common Vulnerabilities and Exposures

- One **unique ID** per publicly known vulnerability
- Format: `CVE-YEAR-NUMBER`
- IDs assigned by CNAs (vendors, projects, CERTs)
- Contains: ID, short description, references
- No severity score of its own
- cve.org

Note:
- ~40,000 new CVEs in H1 2026 (Microsoft Digital Defense Report 2026)
- volume → can't read all, need prioritization
- no CVE ≠ no vulnerability (e.g. Shai-Hulud had none at first)

---

## CWE – Common Weakness Enumeration

- Catalog of **weakness types** = root causes
- Hierarchy: class → base → variant
- A CVE is mapped to one or more CWEs
- Annual **CWE Top 25**
- OWASP Top 10 category = group of CWEs (A01:2025 alone maps 40)
- cwe.mitre.org

Note:
- CWE-79 XSS, CWE-89 SQLi, CWE-22 path traversal
- gets you from "this bug" to "this class of bug"
- → right Cheat Sheet
- Veracode AI study reports in CWEs (Input 3)

---

## NVD – National Vulnerability Database

- Run by NIST (US)
- Takes the CVE and **adds**: CVSS score, CWE mapping, affected products (CPE), references
- Search by CVE, product, keyword; API available
- nvd.nist.gov

**Cross-check:** vendor advisory · GitHub Advisory DB · osv.dev

Note:
- enrichment can lag behind CVE publication
- vendor advisory often more precise on affected versions
- osv.dev good for dependencies

---

## CVSS – Common Vulnerability Scoring System

- Severity score **0.0 – 10.0**, by FIRST
- Two versions in the wild: **3.1** and **4.0**
- Base metrics: attack vector, complexity, privileges, user interaction, impact on C / I / A

| Score | Severity |
|-------|----------|
| 0.0 | None |
| 0.1 – 3.9 | Low |
| 4.0 – 6.9 | Medium |
| 7.0 – 8.9 | High |
| 9.0 – 10.0 | Critical |

Note:
- v4.0 (2023): adds attack requirements, finer impact model
- base = intrinsic; threat / environmental = context
- NVD lists v3.1 and/or v4.0, check which

---

## Reading a CVSS vector

```text
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H   →  9.8 Critical
```

| Part | Meaning here |
|------|--------------|
| AV:N | attackable over the **network** |
| AC:L | **low** complexity |
| PR:N | **no** privileges needed |
| UI:N | **no** user interaction |
| C/I/A:H | **high** impact on all three |

Note:
- "remote, no login, no click, full impact"
- developer view: AV / PR / UI → can it reach *my* app?
- try changing PR:N → PR:L in calculator

---

## Severity is not risk

- CVSS = technical severity, **in a vacuum**
- Ask: Is the code **reachable** in my app? Exposed? Exploit available? What is at stake?
- **EPSS** – probability of exploitation in the next 30 days (first.org/epss)
- **CISA KEV** – vulnerabilities **known to be exploited** in the wild

Note:
- critical in a dev-only dependency vs. medium on internet-facing login
- KEV = fix first
- exploitation windows shrinking → Input 3
- patch policy: KEV / exposed first

---

## Exercise 1 – Look up a real CVE

1. Pick **one** CVE (everyone a different one)
2. Open **nvd.nist.gov**, search the ID
3. Write down: product & versions · **CWE** · **CVSS** score + vector · listed in **KEV**?
4. Which **OWASP Top 10 category** fits?

| | | |
|---|---|---|
| CVE-2021-44228 Log4Shell | CVE-2014-0160 Heartbleed | CVE-2022-22965 Spring4Shell |
| CVE-2024-3094 XZ Utils | CVE-2023-34362 MOVEit | CVE-2014-6271 Shellshock |
| CVE-2017-0144 EternalBlue | CVE-2017-5638 Struts | CVE-2021-41773 Apache httpd |
| CVE-2022-0847 Dirty Pipe | | |

Note:
- ~10 min, solo or pairs
- hint: more than one category can fit
- compare: CWE choices differ between CVEs
- fast ones: also check the vendor advisory

---

## Exercise 2 – Score it yourself

1. Open the **FIRST CVSS calculator** (first.org/cvss/calculator)
2. Set the metrics for **your** CVE from the description
3. Compare with the NVD score – same? why not?
4. Share in 1 minute: **CVE – CWE – score – category**

Note:
- ~10 min incl. sharing
- differences are normal → scoring is judgement
- discuss: what changes if app is not internet-facing?
- bridge: same in the hunt → each finding = CWE + category

---

## Takeaways: a dependency alert lands in your repo

1. Read the **CVE** description
2. Check the **CWE** – what kind of weakness? → Top 10 → Cheat Sheet
3. Read the **vector** – can it reach **us**?
4. Check **KEV / EPSS** – is it exploited?
5. Decide: upgrade · mitigate · document

Note:
- Dependabot / pnpm audit / Renovate
- don't panic on score alone, don't ignore
- define team rule (e.g. KEV = same week)
- break → set up Juice Shop
