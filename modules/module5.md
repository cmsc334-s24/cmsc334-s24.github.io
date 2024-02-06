# Module 5: Software Vulnerabilities

* Put your answers in the README.md file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/cGM5us4_](https://classroom.github.com/a/cGM5us4_)


### Vulnerabilities Enumerated

__CWE__ (Common Weakness Enumeration) and __CVE__ (Common Vulnerabilities and Exposures) are two important concepts in the field of computer security, each serving a distinct purpose in the identification and management of security vulnerabilities.

#### CWE (Common Weakness Enumeration)

* __Purpose:__ CWE is a category system for software weaknesses and vulnerabilities. It provides a unified, measurable set of criteria for assessing the severity of software vulnerabilities. CWE aims to serve as a common language for describing software security weaknesses in architecture, design, or code.

* __Details:__ CWE entries describe types of vulnerabilities or weaknesses in a more general sense than CVEs. For example, a CWE might describe "SQL Injection" or "Buffer Overflow" as categories of vulnerabilities.

* __Use:__ CWE is used by developers, security researchers, and tools as a basis for understanding potential vulnerabilities in software. It helps in the development of automated tools that can identify or prevent vulnerabilities. CWE entries can be associated with one or more CVEs if those CVEs exemplify the weakness described by the CWE.

#### CVE (Common Vulnerabilities and Exposures)

* __Purpose:__ CVE is a list of publicly disclosed cybersecurity vulnerabilities and exposures. Each entry in this list is uniquely identified by a CVE ID (for example, CVE-2021-44228), which provides a standard reference point for a specific vulnerability or exposure.

* __Details:__ A CVE entry includes an identification number, a description of the vulnerability, and at least one public reference. It is focused on specific vulnerabilities in software and hardware.
    
* __Use:__ CVEs are used by security professionals to ensure that they are discussing the same issue. Security tools and services also use CVE IDs to index vulnerabilities, making it easier to cross-reference and fix specific problems.

---

In summary, __CVE__ and __CWE__ serve complementary purposes in the cybersecurity ecosystem: CVEs focus on specific, identified vulnerabilities, while CWEs categorize types of weaknesses that could lead to vulnerabilities. Together, they provide a comprehensive approach to identifying, understanding, and mitigating security risks in software and hardware systems.


## Exercise 1: Exploring CWEs
    
1. Visit the MITRE CWE website: [https://cwe.mitre.org/data/index.html](https://cwe.mitre.org/data/index.html)
2. Explore the various CWE Lists to find a weakness that interests you. It could be because of its prevalence, its impact, or simply because it's related to a topic you're curious about.
3. Write a short paragraph on the chosen CWE that includes:
    * The CWE ID and name.
    * A brief description of the weakness.
    * Why it interests you.
    * Examples of vulnerabilities that could result from this weakness (if available).
    * Any prevention or mitigation strategies mentioned.


## Exercise 2: Exploring CVEs

1. Visit the MITRE CVE website: [https://cve.mitre.org/cve/search_cve_list.html](https://cve.mitre.org/cve/search_cve_list.html)

2. Use the CVE Search tool to find a recent CVE entry that captures your attention. For example if you are interested in recent Java vulnerabilities you could enter [`Java 2024`](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Java+2024)

3. This could be based on its severity, the popularity of the affected software, or any other criteria you find relevant.

4. Write a short paragraph on the chosen CVE that includes:
    * The CVE ID.
    * A description of the vulnerability.
    * The impact it has or could have on affected systems.
    * The software or systems it affects.
    * Any available patches or workarounds.


