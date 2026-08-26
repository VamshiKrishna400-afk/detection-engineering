## Detection Quality and Validation Criteria

### 1. Response Feedback

Response Feedback means evaluating whether a detection provides an alert that contains enough information and logic for the responder to understand and act on it. If an alert is missing important information, the analyst may have to spend unnecessary time investigating or may not know what action to take. For example, a weak alert says **“Suspicious Activity Detected.”** A better alert says **“Suspicious PowerShell Activity — User: John, Host: WORKSTATION-01, Process: powershell.exe, Command: DownloadFile(...), Time: 10:32 AM, Reason: PowerShell downloaded an executable from an external domain.”** The second alert gives the responder useful context and makes the alert easier to investigate and act upon. In simple terms, **Response Feedback means: “Does the alert provide enough information and logic for the responder to act?”**

### 2. False Positives Ratios / Fidelity

False Positives Ratios / Fidelity means evaluating whether a detection generates a high percentage of true positives and a low number of false positives. If an alert triggers constantly but rarely represents real malicious activity, analysts will waste time investigating it and may eventually start ignoring the alert. For example, suppose an alert triggers 100 times and produces **98 false positives and 2 true positives**. That is a poor detection because most of the alerts are not genuinely malicious. A better detection might produce **100 alerts, 80 true positives, and 20 false positives**, providing much stronger fidelity. In simple terms, **Fidelity means: “Does this alert usually indicate something genuinely suspicious?”**

### 3. Timelines

Timelines means evaluating whether a detection triggers early enough for the response team to investigate and contain the threat. A detection that identifies an attack only after the attacker has completed their objective may not be useful. For example, imagine an attacker is attempting to steal credentials. The attacker gains access at 10:00, credential dumping begins at 10:05, the detection triggers at 10:06, the SOC investigates at 10:08, and the account is disabled at 10:10. This gives the SOC an opportunity to contain the attack before the attacker can complete their objective. However, if the attack occurs on Day 1, data is stolen on Day 2, and the alert only triggers on Day 3, the detection is much less valuable. In simple terms, **Timeliness means: “Does the alert trigger early enough to allow effective response?”**

### 4. Specificity

Specificity means evaluating whether a detection identifies the relevant suspicious activity without unnecessarily matching unrelated activity. Overly broad detections generate noise and false positives. For example, a weak rule might detect **“Any PowerShell execution.”** However, PowerShell is commonly used for legitimate administrative tasks. A more specific rule could detect **PowerShell + Encoded command + Download from external IP + Suspicious parent process**. This combination focuses the detection on activity that is more likely to be malicious instead of alerting on normal PowerShell usage. In simple terms, **Specificity means: “Does the alert focus on the activity we actually care about?”**

### 5. Testability

Testability means ensuring that a detection can be tested so that you can verify whether it actually works as intended. Without testing, you don't know whether the detection will trigger correctly, whether it produces false positives, or whether changes to the environment will break it. For example, suppose your detection is designed to identify **PowerShell + Encoded command**. You can create a controlled test event, run the detection, verify that the expected condition is matched, and confirm that an alert is generated. You can also test normal PowerShell activity and confirm that it doesn't trigger unnecessarily. This allows the detection engineer to verify that the detection behaves correctly under both suspicious and legitimate conditions. In simple terms, **Testability means: “Can we reliably test and confirm that the detection works?”**

### 6. Compensating Controls

Compensating Controls means evaluating a detection in the context of other security controls and visibility gaps. Sometimes a detection exists because another security tool or control does not provide the required visibility. For example, suppose your organization doesn't have a security tool that detects a particular type of endpoint activity. You can create a detection using available logs, such as Windows event logs, to provide visibility into that behavior. In this situation, the custom detection acts as a compensating control for the identified security gap. For example: **“Our endpoint tool doesn't detect this behavior, but Windows event logs provide enough visibility to create a detection.”** In simple terms, **Compensating Controls means: “Is this detection filling an identified gap in our existing security tooling?”**

## Security Tooling

Security tooling refers to the **security products, platforms, and technologies used by a SOC to collect data, detect suspicious activity, investigate alerts, and respond to security incidents**. Different tools provide different types of visibility, and detection engineers often use multiple tools together to build reliable detections.

### SIEM — Security Information and Event Management

A **SIEM** collects and centralizes logs from different sources such as endpoints, servers, firewalls, applications, authentication systems, and network devices. It allows analysts and detection engineers to search, correlate, and analyze security events and create detection rules and alerts.

**In simple terms:** **SIEM = Collect, search, correlate, and alert on security logs.**

### EDR — Endpoint Detection and Response

**EDR** monitors endpoints such as laptops, desktops, and servers. It provides visibility into processes, command lines, files, users, network connections, and other endpoint activity. EDR can also help investigate and respond to suspicious activity.

**In simple terms:** **EDR = Monitor and respond to activity on endpoints.**

### NDR — Network Detection and Response

**NDR** monitors network traffic and communication between systems. It can help identify suspicious connections, unusual network behavior, command-and-control activity, and other network-based threats.

**In simple terms:** **NDR = Detect suspicious activity in network traffic.**

### Firewall

A **firewall** controls network traffic based on security rules. It can allow or block connections between networks, systems, IP addresses, ports, and services. Firewall logs can also provide useful data for detection engineering.

**In simple terms:** **Firewall = Control and monitor network connections.**

### IDS/IPS — Intrusion Detection and Prevention System

**IDS/IPS** monitors network traffic for known attack patterns or suspicious behavior. An IDS can generate alerts, while an IPS can additionally block or prevent certain malicious traffic.

**In simple terms:** **IDS/IPS = Detect and potentially block network attacks.**

### Email Security

**Email security tools** protect organizations from phishing, malicious attachments, malicious URLs, spam, and other email-based threats. Their logs can be used to create detections for suspicious emails and user interactions.

**In simple terms:** **Email Security = Detect and protect against email-based threats.**

### Threat Intelligence Platform — TIP

A **Threat Intelligence Platform (TIP)** collects, organizes, and manages threat intelligence such as malicious IP addresses, domains, URLs, file hashes, malware information, and attacker techniques. This information can be used by detection engineers and threat hunters to improve detections.

**In simple terms:** **TIP = Manage and use threat intelligence.**

### Vulnerability Management

**Vulnerability management tools** identify and track vulnerabilities in systems, applications, and infrastructure. This information can provide useful context when investigating alerts and determining the risk associated with affected systems.

**In simple terms:** **Vulnerability Management = Find and manage security weaknesses.**

### SOAR — Security Orchestration, Automation and Response

**SOAR** platforms help automate repetitive security tasks and coordinate actions between different security tools. For example, when a malicious IP is detected, SOAR may automatically enrich the indicator, create a ticket, notify analysts, or isolate an affected endpoint.

**In simple terms:** **SOAR = Automate and coordinate security response.**

### Security Tooling in Detection Engineering

Detection engineers use these tools as **sources of data, detection logic, investigation context, and response capabilities**. For example:

| Tool Type | Tool Names | Purpose |
|---|---|---|
| **SIEM** | Splunk, Elastic, Microsoft Sentinel, QRadar | Centralize logs and generate alerts |
| **EDR** | CrowdStrike, Tanium, Cybereason, Windows Defender | Provide endpoint visibility |
| **NDR** | Suricata, Zeek, ExtraHop, Darktrace | Provide network visibility |
| **Firewall** | Palo Alto, Cisco, Check Point, pfSense | Provide network control and logs |
| **Email Security** | Proofpoint, Microsoft 365, Mimecast, FortiMail | Provide email visibility |
| **TIP** | CloudSEK, Recorded Future, CrowdStrike Falcon X, ThreatConnect, MISP | Provide threat intelligence |
| **Automation** | XSOAR, Splunk SOAR, TheHive, GitHub/Python | Automate response |

Together, these tools provide the **visibility and capabilities required to build, test, deploy, and improve security detections**.

## Detection Testing and Attack Simulation

The main idea is: **Before deploying a detection, you need a way to generate realistic attack activity and verify that your detection catches it.** The tools shown are different ways to simulate attacks or create test environments. A detection needs realistic logs to test against. Instead of waiting for a real attack, you can safely create attack activity in a controlled environment and verify whether the detection generates an alert.

### 1. Splunk Attack Range

**Splunk Attack Range** provides an environment where you can simulate attacks and generate security data. A detection needs realistic logs to test against, and instead of waiting for a real attack, you can safely create attack activity in a controlled environment. For example, if your detection is supposed to identify PowerShell abuse, you can generate that behavior in the Attack Range and check whether the alert fires. In simple terms, **Splunk Attack Range = A controlled environment for generating attack activity and testing detections.**

### 2. AttackIQ

**AttackIQ** is a security validation and adversary emulation platform used to test whether security controls and detections work against simulated attacks. Instead of simply assuming that your security controls work, you can run known attack techniques and see whether your defenses detect them. For example, you could simulate a credential-access technique and verify whether your endpoint and SIEM detections respond. In simple terms, **AttackIQ = Simulate attacker techniques and validate whether security controls detect them.**

### 3. Atomic Red Team

**Atomic Red Team** provides small, focused tests for individual MITRE ATT&CK techniques. Instead of performing an entire attack, you can test one specific attacker behavior at a time. For example, suppose your detection is designed to detect **PowerShell execution**. You can run an Atomic Red Team test that performs that specific technique and then verify whether the expected logs and alert are generated. This makes it very useful for unit testing individual detections. In simple terms, **Atomic Red Team = Run individual adversary techniques to test whether a specific detection works.**

### 4. BloodHound

**BloodHound** is primarily used to analyze relationships and attack paths in Active Directory environments. It helps security teams understand how an attacker could move through an AD environment and identify privilege escalation or lateral-movement paths. It can also be useful during detection testing when you're investigating or validating behaviors related to AD attacks. For example, you can analyze relationships between users, groups, computers, and administrative privileges to understand potential attack paths and use this knowledge to develop and test detections around suspicious AD activity. In simple terms, **BloodHound = Understand and analyze Active Directory attack paths and relationships.**

### 5. Caldera

**MITRE Caldera** is an automated adversary emulation platform. It allows you to emulate attacker behavior across multiple steps instead of testing only one technique. For example, you can simulate activities across **Initial Access, Execution, Discovery, Credential Access, and Lateral Movement** and then evaluate which parts of the attack chain your security controls detect. In simple terms, **Caldera = Automate realistic adversary behavior to test your defenses and detections.**

### 6. Custom VMs

**Custom VMs** allow you to create your own virtual machines specifically for testing detections. Sometimes existing attack frameworks don't exactly reproduce the environment or behavior you need. A custom VM gives you complete control over the testing environment. For example, you could create a Windows workstation, a Domain Controller, and an attacker machine, then generate controlled activity and observe the resulting logs in the SIEM to verify whether the detection works. In simple terms, **Custom VMs = Build your own controlled environment for generating specific test activity.**

### How This Relates to Unit Testing

Think about a detection like a piece of software. You write a detection such as **“Detect suspicious PowerShell execution,”** but you don't want to simply assume it works. You need to create the behavior and verify that the detection catches it. This means generating the attack activity, generating the required logs, running the detection, and checking whether the expected alert is triggered. If the alert is generated as expected, the test passes; if the alert is not generated, the test fails.

That's essentially what unit testing a detection means. **Testing capabilities are tools or environments that can safely generate attacker behavior so you can prove that your detection actually detects it.**

Follow [Installation](installation_procedure.md) guide for setting the lab environment using custom vm's.
