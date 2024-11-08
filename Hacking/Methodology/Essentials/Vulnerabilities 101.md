List of things you will learn about:
-  What vulnerabilities are
-  Why you should learn about them
-  How are vulns rated
-  Databases for vuln research
-  A show case of how vuln research is used on ACKme's engagement

## Introduction to Vulns

Vulnerabilities are points of weakness in the system.
They come in many shapes and forms, here are some categories.

|                             |                                                                                                                                                                                                                                                    |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vulnerability**           | **Description**                                                                                                                                                                                                                                    |
| Operating System            | These types of vulnerabilities are found within Operating Systems (OSs) and often result in privilege escalation.                                                                                                                                  |
| (Mis)Configuration-based    | These types of vulnerability stem from an incorrectly configured application or service. For example, a website exposing customer details.                                                                                                         |
| Weak or Default Credentials | Applications and services that have an element of authentication will come with default credentials when installed. For example, an administrator dashboard may have the username and password of "admin". These are easy to guess by an attacker. |
| Application Logic           | These vulnerabilities are a result of poorly designed applications. For example, poorly implemented authentication mechanisms that may result in an attacker being able to impersonate a user.                                                     |
| Human-Factor                | Human-Factor vulnerabilities are vulnerabilities that leverage human behaviour. For example, phishing emails are designed to trick humans into believing they are legitimate.                                                                      |

## Scoring Vulns (CVSS & VPR)

**Common Vulnerability Scoring System**

The score is determined by some of the following factors.
 1. How easy is it to exploit the vulnerability?
  2. Do exploits exist for this?
  3. How does this vulnerability interfere with the CIA triad?

In fact, there are so many variables that you have to use a [calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator) to figure out the score using this framework. A vulnerability is given a classification (out of five) depending on the score that is has been assigned.

|            |            |
| ---------- | ---------- |
| **Rating** | **Score**  |
| None       | 0          |
| Low        | 0.1 - 3.9  |
| Medium     | 4.0 - 6.9  |
| High       | 7.0 - 8.9  |
| Critical   | 9.0 - 10.0 |

However, CVSS is not a magic bullet. Let's analyze some of the advantages and disadvantages of CVSS in the table below:

|                                                                                  |                                                                                                                                                                                                |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Advantages of CVSS**                                                           | **Disadvantages of CVSS**                                                                                                                                                                      |
| CVSS has been around for a long time.                                            | CVSS was never designed to help prioritise vulnerabilities, instead, just assign a value of severity.                                                                                          |
| CVSS is popular in organisations.                                                | CVSS heavily assesses vulnerabilities on an exploit being available. However, only 20% of all vulnerabilities have an exploit available ([Tenable., 2020](https://www.tenable.com/research)) . |
| CVSS is a free framework to adopt and recommended by organisations such as NIST. | Vulnerabilities rarely change scoring after assessment despite the fact that new developments such as exploits may be found.                                                                   |

**Vulnerability Priority Rating (VPR)**

Is a more modern framework in vuln management - developed by Tenable (Nessus devs).
Framework is risk driven; meaning meaning that vulns are scored with a focus on the risk a vuln poses to the organization, rather than factors such as impact like in CVSS.

Unlike CVSS, VPR scoring accounts the relevancy of vuln.
For example, the risk is not considered if that vuln does not apply to the organization.
(i.e. they do not use the software that is vulnerable).

VPR uses a similar scoring range as CVSS, which I have also put into the table below. However, two notable differences are that VPR does not have a _"None/Informational"_ category, and because VPR uses a different scoring method, the same vulnerability will have a different score using VPR than when using CVSS.

|   |   |
|---|---|
|**Rating**|**Score**|
|Low|0.0 - 3.9|
|Medium|4.0 - 6.9|
|High|7.0 - 8.9|
|Critical|9.0 - 10.0|

Let's recap some of the advantages and disadvantages of using the VPR framework in the table below.

|   |   |
|---|---|
|**Advantages of VPR**|**Disadvantages of VPR**|
|VPR is a modern framework that is real-world.|VPR is not open-source like some other vulnerability management frameworks.|
|VPR considers over 150 factors when calculating risk.|VPR can only be adopted apart of a commercial platform.|
|VPR is risk-driven and used by organisations to help prioritise patching vulnerabilities.|VPR does not consider the CIA triad to the extent that CVSS does; meaning that risk to the confidentiality, integrity and availability of data does not play a large factor in scoring vulnerabilities when using VPR.|
|Scorings are not final and are very dynamic, meaning the priority a vulnerability should be given can change as the vulnerability ages.|_Intentionally left blank._|

## Vulnerability Databases

**NVD - National Vulnerability Database**

**Exploit-DB**

Before we dive into these two resources, let's ensure that our understanding of some fundamental key terms is on the same page:

|   |   |
|---|---|
|**Term**|**Definition**|
|Vulnerability|A vulnerability is defined as a weakness or flaw in the design, implementation or behaviours of a system or application.|
|Exploit|An exploit is something such as an action or behaviour that utilises a vulnerability on a system or application.|
|Proof of Concept (PoC)|A PoC is a technique or tool that often demonstrates the exploitation of a vulnerability.|

--------------------------------------------------------------------


The National Vulnerability Database is a website that lists all publicly categorized vulnerabilities. In cybersecurity, vulnerabilities are classified under 
**"Common Vulnerabilities and Exposures”** (Or CVE for short).

These CVEs have the formatting of `CVE-YEAR-IDNUMBER`. For example, the vulnerability that the famous malware WannaCry used was `CVE-2017-0144.`

NVD allows you to see all the CVEs that have been confirmed, using filters by category and month of submission. For example, it is three days into August; there have already been 223 new CVEs submitted to this database.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/aa86c1cce478d6c357f5507d927c9e88.png)

While this website helps keep track of new vulnerabilities, it is not great when searching for vulnerabilities for a specific application or scenario.

 Exploit-DB

[Exploit-DB](https://www.exploit-db.com/) is a resource that we, as hackers, will find much more helpful during an assessment. Exploit-DB retains exploits for software and applications stored under the name, author and version of the software or application.

We can use Exploit-DB to look for snippets of code (known as Proof of Concepts) that are used to exploit a specific vulnerability.

![](https://assets.tryhackme.com/additional/vulnerability-module/vulnerabilities101/exploitdb1.png)


## Conclusion

Now this is why good enumeration is the key to success.
Expanding the attack surface and all that good stuff so we can utilize these databases.