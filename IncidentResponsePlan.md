
Incident Response Plan

NIST SP 800-61-Aligned Security Incident Response Plan

1. Background

The organization has detected unusual activity indicating a possible cybersecurity incident involving unauthorized access to sensitive organizational data. The activity may indicate that an unauthorized individual has obtained access to one or more systems, user accounts, applications, databases, or network resources.

Potential indicators may include unusual authentication activity, unexpected access to sensitive files, abnormal network connections, privilege escalation, suspicious administrative actions, or data transfers to unauthorized destinations.

Because the incident may involve sensitive information, the organization must respond quickly while preserving evidence and minimizing business disruption.

This Incident Response Plan establishes a structured approach for identifying, analyzing, containing, eradicating, and recovering from the incident. It is aligned with the incident-handling principles described in NIST SP 800-61.


---

2. Purpose and Scope

2.1 Purpose

The primary purpose of this Incident Response Plan is to provide the organization with a consistent and controlled process for responding to cybersecurity incidents.

The plan aims to:

Detect and analyze security incidents quickly.

Limit the impact and scope of an incident.

Protect sensitive information and critical systems.

Preserve relevant digital evidence.

Remove the root cause of the compromise.

Restore affected systems securely.

Ensure appropriate internal and external communication.

Document the incident and response activities.

Identify lessons learned and improve future security operations.


2.2 Scope

This plan applies to:

Employees and contractors.

IT and cybersecurity personnel.

Servers and workstations.

Network infrastructure.

Cloud services.

Applications and APIs.

Databases.

Identity and access management systems.

Security monitoring systems.

Sensitive and confidential information.

Third-party systems where they affect organizational security.


The plan applies to incidents such as:

Unauthorized account access.

Malware infections.

Phishing and credential theft.

Data breaches.

Unauthorized data disclosure.

Privilege escalation.

Ransomware.

Web application compromise.

Insider threats.

Denial-of-service attacks.

Suspicious network activity.



---

3. Incident Classification

Before beginning the response, the incident should be classified according to its severity and potential impact.

Severity	Description	Example

Critical	Major compromise affecting critical systems or highly sensitive data	Large-scale data breach or ransomware
High	Significant unauthorized access or compromise	Compromised privileged account
Medium	Limited security incident with contained impact	Malware on one workstation
Low	Minor security event with limited impact	Repeated failed login attempts


For this scenario, the suspected unauthorized access to sensitive information should initially be treated as a High-Severity incident until investigation determines the actual scope and impact.


---

4. Incident Response Process

The response process should follow a structured lifecycle consisting of:

1. Preparation


2. Detection and Analysis


3. Containment


4. Eradication


5. Recovery


6. Post-Incident Activity



The containment, eradication, and recovery activities should be performed based on the organization's incident severity, business requirements, and available evidence.


---

5. Phase 1 — Preparation

Preparation is the foundation of effective incident response. The organization should establish the necessary people, processes, technologies, and documentation before an incident occurs.

5.1 Preparation Activities

The organization should:

Establish an Incident Response Team (IRT).

Define roles and responsibilities.

Develop incident response procedures.

Maintain updated contact information.

Establish communication channels.

Configure centralized logging and monitoring.

Deploy endpoint detection and response capabilities where appropriate.

Maintain secure backups.

Develop asset inventories.

Identify critical systems and sensitive data.

Establish network diagrams.

Maintain an up-to-date list of administrators and system owners.

Define incident severity levels.

Establish escalation procedures.

Conduct regular incident response exercises.

Train employees on security incidents and reporting procedures.


5.2 Technical Preparation

Security monitoring should collect relevant logs from:

Firewalls.

VPN systems.

Active Directory/identity providers.

Windows and Linux systems.

Web servers.

Databases.

Cloud platforms.

Endpoint security solutions.

Authentication systems.

Email security systems.

Network monitoring systems.


Logs should be centrally collected and protected against unauthorized modification.

For example, a SIEM such as ELK Stack, Wazuh, Microsoft Sentinel, or Splunk can be used to correlate security events and identify suspicious behavior.

5.3 Evidence Preparation

The organization should establish procedures for preserving digital evidence.

Evidence may include:

Authentication logs.

Network traffic.

System logs.

Application logs.

Database logs.

Firewall logs.

Endpoint telemetry.

Memory captures.

Disk images.

Suspicious files.

Email messages.

Screenshots.


Evidence should be stored securely and access should be restricted.


---

6. Phase 2 — Detection and Analysis

The detection and analysis phase begins when suspicious activity is identified.

6.1 Initial Detection

Possible indicators include:

Login from an unusual geographic location.

Multiple failed authentication attempts.

Successful login after numerous failed attempts.

Login outside normal working hours.

Unexpected privilege escalation.

Unusual database queries.

Large data transfers.

Suspicious processes.

Unexpected administrative account activity.

Access to sensitive files.

Unknown devices connecting to the network.


For this scenario, security monitoring may identify an unusual authentication event followed by access to sensitive information.

6.2 Validate the Incident

The response team should determine whether the alert represents a genuine security incident or a false positive.

The team should answer:

What happened?

When did it happen?

Which account was involved?

Which system was accessed?

What data was accessed?

Was the access authorized?

Is the attacker still present?

How did the attacker obtain access?

Has data been modified or exfiltrated?


6.3 Establish a Timeline

Investigators should create a chronological timeline.

Example:

Time	Event

08:42	Suspicious login detected
08:45	Account accesses sensitive application
08:51	Large database query observed
08:56	Unusual outbound traffic detected
09:05	SOC escalates incident
09:15	Incident Commander activated
09:25	Account disabled


The timeline should be continuously updated throughout the investigation.

6.4 Determine Scope

The team should identify:

Affected users.

Affected endpoints.

Affected servers.

Affected applications.

Affected databases.

Compromised accounts.

Potentially exposed information.

Source and destination of suspicious traffic.


The team should determine whether the incident is isolated or part of a larger compromise.


---

7. Phase 3 — Containment

The objective of containment is to prevent the attacker from continuing the compromise while preserving evidence and minimizing business disruption.

Containment should normally occur at two levels:

Short-Term Containment

Immediate actions may include:

Disable compromised accounts.

Revoke active sessions.

Reset compromised credentials.

Block malicious IP addresses where appropriate.

Isolate compromised endpoints.

Restrict suspicious network connections.

Disable compromised API keys or tokens.

Block malicious domains.

Increase monitoring.

Restrict access to affected systems.


For example, if an administrator account is suspected of compromise, the organization may immediately disable the account and revoke its active sessions.

Long-Term Containment

Longer-term measures may include:

Moving affected systems to isolated network segments.

Applying temporary firewall rules.

Implementing additional authentication controls.

Restricting privileged access.

Increasing logging.

Deploying additional endpoint monitoring.

Applying temporary application-level restrictions.


7.1 Evidence Preservation

Containment should not unnecessarily destroy evidence.

Before shutting down or rebuilding systems, investigators should consider collecting:

Volatile memory.

Relevant logs.

Network connections.

Running processes.

Suspicious files.

Disk images.

Authentication information.


Evidence collection should follow organizational procedures and applicable legal requirements.


---

8. Phase 4 — Eradication

After containment, the organization should identify and eliminate the root cause of the incident.

8.1 Identify the Root Cause

Investigators should determine:

Initial access method.

Vulnerability exploited.

Compromised credentials.

Malware involved.

Misconfiguration.

Phishing activity.

Privilege escalation method.

Persistence mechanism.

Systems accessed by the attacker.


For example, investigation may determine that an employee's credentials were stolen through phishing and subsequently used to access an internal application.

8.2 Remove the Threat

Eradication activities may include:

Removing malware.

Deleting unauthorized accounts.

Removing persistence mechanisms.

Resetting compromised credentials.

Rotating API keys and secrets.

Patching exploited vulnerabilities.

Removing malicious scheduled tasks.

Removing unauthorized applications.

Blocking malicious infrastructure.

Correcting security misconfigurations.


If there is uncertainty about system integrity, affected systems should be rebuilt from known-good sources rather than simply deleting suspicious files.


---

9. Phase 5 — Recovery

Recovery restores normal business operations while ensuring that systems are secure.

9.1 Recovery Activities

The organization should:

1. Restore systems from trusted backups or clean images.


2. Apply security patches.


3. Reset credentials where necessary.


4. Validate security configurations.


5. Reconnect systems gradually.


6. Monitor systems closely.


7. Verify application functionality.


8. Confirm data integrity.


9. Monitor for signs of attacker re-entry.



9.2 Enhanced Monitoring

For a period following recovery, security teams should increase monitoring.

They should look for:

Repeated authentication failures.

New suspicious accounts.

Unexpected administrative activity.

Unusual network traffic.

Reappearance of malware.

Unexpected outbound connections.

Abnormal database activity.


Systems should only be considered fully recovered when the organization has reasonable confidence that the attacker no longer has access.


---

10. Phase 6 — Post-Incident Activity

Post-incident activity focuses on understanding what happened and improving future security.

The organization should conduct a formal lessons-learned review after the incident.

The review should answer:

What happened?

How was the incident detected?

What was the initial attack vector?

How long did the attacker have access?

What systems were affected?

What data was exposed?

What containment measures worked?

What actions were ineffective?

Were there communication problems?

Were logs sufficient?

Were response procedures followed?

What could have prevented the incident?

What security controls need improvement?



---

11. Team Roles and Responsibilities

A clearly defined Incident Response Team prevents confusion during an incident.

11.1 Incident Commander

The Incident Commander coordinates the overall response.

Responsibilities:

Declare and classify the incident.

Coordinate response activities.

Set priorities.

Approve major containment decisions.

Coordinate different teams.

Escalate the incident to management.

Maintain situational awareness.

Ensure decisions are documented.

Coordinate the transition between response phases.


The Incident Commander should not necessarily perform technical investigations directly. Their primary responsibility is coordination and decision-making.


---

12. Security/Technical Lead

The Technical Lead manages the technical investigation.

Responsibilities:

Analyze security alerts.

Investigate affected systems.

Review logs.

Identify indicators of compromise.

Analyze suspicious files.

Determine the attack vector.

Identify compromised systems.

Recommend containment measures.

Support eradication and recovery.



---

13. SOC / Security Analyst

The SOC Analyst is usually responsible for initial detection and triage.

Responsibilities:

Monitor security alerts.

Validate suspicious activity.

Collect initial evidence.

Analyze authentication and network logs.

Identify indicators of compromise.

Escalate confirmed incidents.

Maintain incident documentation.



---

14. IT / System Administrator

The IT or System Administrator supports containment and recovery.

Responsibilities:

Disable compromised accounts.

Isolate systems.

Apply firewall changes.

Reset credentials.

Patch affected systems.

Restore systems.

Validate system functionality.

Assist with infrastructure recovery.



---

15. Communications Lead

The Communications Lead manages internal and external communication.

Responsibilities:

Prepare management updates.

Coordinate employee communications.

Ensure consistent messaging.

Prevent unauthorized disclosure of incident information.

Coordinate approved external communications.


Communication should follow the organization's approved communication and escalation procedures.


---

16. Legal and Compliance Representative

Legal and compliance personnel determine whether regulatory, contractual, or legal obligations apply.

Responsibilities:

Assess notification requirements.

Determine regulatory obligations.

Advise on evidence handling.

Coordinate with external authorities when necessary.

Review communications before external disclosure.

Ensure compliance with applicable data protection requirements.



---

17. Management / Business Owner

Management provides business-level decisions and resources.

Responsibilities:

Approve major business decisions.

Allocate resources.

Determine business priorities.

Support system shutdowns when necessary.

Coordinate business continuity activities.



---

18. Responsibility Transfer and Escalation

Incident response responsibilities should be transferred based on expertise and incident severity.

For example:

SOC Analyst → Incident Commander → Technical Lead → IT/Infrastructure → Management/Legal

An incident may begin as a routine security alert handled by the SOC. If evidence indicates unauthorized access to sensitive information, the SOC escalates it to the Incident Commander.

The Incident Commander then activates the appropriate technical, legal, management, and communication resources.

All major decisions and transfers should be documented with:

Timestamp.

Responsible person.

Decision.

Reason for decision.

Supporting evidence.



---

19. Incident Communication Plan

Effective communication is essential during a security incident.

Communication should follow the principle of need-to-know.

Internal Communication

Relevant stakeholders may include:

Incident Response Team.

IT department.

Senior management.

Legal/compliance.

Data owners.

Business continuity teams.


External Communication

Depending on the incident, external communication may involve:

Customers.

Partners.

Service providers.

Regulatory authorities.

Law enforcement.

Cybersecurity vendors.


External communication should only be performed by authorized personnel.


---

20. Evidence and Documentation

Every significant incident-response activity should be documented.

The incident record should contain:

Incident ID.

Date and time detected.

Detection source.

Incident severity.

Affected systems.

Affected accounts.

Indicators of compromise.

Timeline.

Evidence collected.

Actions performed.

People involved.

Decisions made.

Recovery activities.

Final impact assessment.


Sensitive information should be anonymized before being used in reports, screenshots, training materials, or public documentation.

For example:

Instead of:

john.smith@company.com

use:

user001@company.example

Instead of:

192.168.10.45

use:

10.0.0.XX

when exact addresses are not necessary for the report.


---

21. Post-Incident Analysis

A formal post-incident review should be performed after recovery.

21.1 Incident Timeline Review

The response team should reconstruct the complete sequence of events:

Initial access → Discovery → Privilege escalation → Data access → Detection → Containment → Eradication → Recovery

This helps identify delays and weaknesses.

21.2 Root Cause Analysis

The organization should determine the underlying reason the incident occurred.

For example:

Root Cause: Compromised employee credentials.

Contributing factors:

Phishing attack.

Lack of phishing-resistant MFA.

Excessive account privileges.

Insufficient authentication monitoring.

Limited security awareness.


The goal is not simply to identify the attacker's actions but to understand why organizational controls failed to prevent or detect the attack.


---

22. Lessons Learned

The team should identify both successful and unsuccessful actions.

What Worked?

For example:

SIEM detected unusual authentication activity.

SOC escalated the alert quickly.

Compromised accounts were disabled rapidly.

Backups were available.

Communication channels worked effectively.


What Did Not Work?

For example:

Some systems lacked centralized logging.

Detection occurred several hours after initial compromise.

Contact information was outdated.

The incident escalation procedure was unclear.

Critical systems lacked MFA.



---

23. Improvement Plan

Lessons learned should be converted into measurable security improvements.

Finding	Improvement	Responsible Team	Priority

Compromised credentials	Implement MFA	IAM/IT	High
Insufficient logging	Centralize logs	SOC	High
Slow detection	Improve SIEM rules	SOC	High
Excessive privileges	Implement least privilege	IT/IAM	High
Weak employee awareness	Security awareness training	HR/Security	Medium
Outdated IR contacts	Update contact list	Management	Medium
Incomplete procedures	Update IR playbook	Security	Medium


Each improvement should have an owner and target completion date.


---

24. Updating Policies and Procedures

Following the incident, the organization should review and update:

Incident Response Policy.

Access Control Policy.

Password Policy.

MFA Policy.

Logging and Monitoring Policy.

Data Protection Policy.

Backup Policy.

Business Continuity Plan.

Disaster Recovery Plan.

Security Awareness Program.


Updated procedures should be communicated to relevant employees.


---

25. Training and Testing

The organization should regularly test its incident response capabilities.

Recommended exercises include:

Tabletop Exercises

A simulated incident is presented to management and technical teams.

Example:

> "An employee's account has been compromised and sensitive customer information may have been accessed."



The team discusses what actions it would take.

Technical Simulations

Security teams can simulate:

Compromised credentials.

Malware infection.

Phishing attacks.

Data exfiltration.

Privilege escalation.

Ransomware.


Recovery Testing

The organization should periodically test:

Backup restoration.

System recovery.

Account recovery.

Emergency communication.

Incident escalation.


Testing should identify weaknesses before a real incident occurs.


---

26. Recommended Incident Response Metrics

Organizations should measure incident response performance.

Useful metrics include:

Mean Time to Detect (MTTD)
How long it takes to detect an incident.

Mean Time to Respond (MTTR)
How long it takes to begin responding.

Mean Time to Contain (MTTC)
How long it takes to prevent further attacker activity.

Additional metrics:

Number of incidents.

Number of false positives.

Number of compromised accounts.

Number of affected systems.

Percentage of incidents detected internally.

Percentage of incidents successfully contained.

Time required for recovery.

Number of recurring incidents.


These metrics help determine whether the organization's incident response capability is improving.


---

27. Example Response Workflow

For the current unauthorized-access scenario, the response could follow this workflow:

Suspicious Activity Detected
            ↓
Initial SOC Triage
            ↓
Validate Security Incident
            ↓
Classify Severity
            ↓
Activate Incident Response Team
            ↓
Collect & Preserve Evidence
            ↓
Identify Affected Accounts/Systems
            ↓
Short-Term Containment
            ↓
Determine Root Cause
            ↓
Eradicate Threat
            ↓
Restore Systems
            ↓
Enhanced Monitoring
            ↓
Incident Closure
            ↓
Post-Incident Review
            ↓
Lessons Learned
            ↓
Update Policies, Controls & Training


---

28. Conclusion

A well-structured incident response plan provides the organization with a repeatable and controlled method for responding to cybersecurity incidents.

For the suspected unauthorized-access incident, the organization should prioritize rapid detection, evidence preservation, containment of compromised accounts and systems, identification of the root cause, secure recovery, and continuous monitoring.

The response should not end when systems are restored. A formal post-incident review is essential for identifying weaknesses and converting lessons learned into concrete improvements.

A NIST-aligned incident response program should therefore be treated as a continuous process rather than a one-time document. Policies, procedures, technologies, contact information, and training should be regularly reviewed and tested to ensure that the organization remains prepared for future incidents.


---

29. References

NIST SP 800-61

National Institute of Standards and Technology (NIST), Computer Security Incident Handling Guide, SP 800-61.

[NIST SP 800-61 publication page](https://csrc.nist.gov/pubs/sp/800/61/r2/final?utm_source=chatgpt.com)

NIST Cybersecurity Framework

National Institute of Standards and Technology, Cybersecurity Framework (CSF) 2.0.

[NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework?utm_source=chatgpt.com)

NIST Computer Security Resource Center

Additional NIST cybersecurity guidance and publications:

[NIST Computer Security Resource Center](https://csrc.nist.gov/?utm_source=chatgpt.com)

MITRE ATT&CK

MITRE ATT&CK can be used to map attacker behavior and techniques identified during incident investigation.

[MITRE ATT&CK](https://attack.mitre.org/?utm_source=chatgpt.com)


---

Recommended Google Docs Structure

For the actual submission, I recommend making the document look like a formal security report:

Cover Page

Incident Response Plan

NIST SP 800-61

Organization: [Organization Name]

Prepared by: [Your Name]

Date: September 2026

Classification: Internal Use


Table of Contents

1. Background


2. Purpose and Scope


3. Incident Classification


4. Incident Response Process


5. Preparation


6. Detection and Analysis


7. Containment


8. Eradication


9. Recovery


10. Post-Incident Activity


11. Team Roles and Responsibilities


12. Communication Plan


13. Evidence and Documentation


14. Post-Incident Analysis


15. Lessons Learned


16. Improvement Plan


17. Training and Testing


18. Incident Response Metrics


19. Conclusion


20. References



Important: For your submission, don't include real company names, real IP addresses, real usernames, passwords, customer information, tokens, or screenshots containing sensitive information. Use placeholders such as [REDACTED], user001, and 10.0.0.XX.

One important note: NIST SP 800-61 Revision 2 is the classic source behind the Preparation → Detection & Analysis → Containment/Eradication/Recovery → Post-Incident Activity structure. NIST has since published newer incident-response guidance, so if your instructor specifically requires SP 800-61, use the version specified by the assignment and cite it accordingly.

2. Purpose and Scope

2.1 Purpose

The primary purpose of this Incident Response Plan is to provide the organization with a consistent and controlled process for responding to cybersecurity incidents.

The plan aims to:

Detect and analyze security incidents quickly.

Limit the impact and scope of an incident.

Protect sensitive information and critical systems.

Preserve relevant digital evidence.

Remove the root cause of the compromise.

Restore affected systems securely.

Ensure appropriate internal and external communication.

Document the incident and response activities.

Identify lessons learned and improve future security operations.


2.2 Scope

This plan applies to:

Employees and contractors.

IT and cybersecurity personnel.

Servers and workstations.

Network infrastructure.

Cloud services.

Applications and APIs.

Databases.

Identity and access management systems.

Security monitoring systems.

Sensitive and confidential information.

Third-party systems where they affect organizational security.


The plan applies to incidents such as:

Unauthorized account access.

Malware infections.

Phishing and credential theft.

Data breaches.

Unauthorized data disclosure.

Privilege escalation.

Ransomware.

Web application compromise.

Insider threats.

Denial-of-service attacks.

Suspicious network activity.



---

3. Incident Classification

Before beginning the response, the incident should be classified according to its severity and potential impact.

Severity	Description	Example

Critical	Major compromise affecting critical systems or highly sensitive data	Large-scale data breach or ransomware
High	Significant unauthorized access or compromise	Compromised privileged account
Medium	Limited security incident with contained impact	Malware on one workstation
Low	Minor security event with limited impact	Repeated failed login attempts


For this scenario, the suspected unauthorized access to sensitive information should initially be treated as a High-Severity incident until investigation determines the actual scope and impact.


---

4. Incident Response Process

The response process should follow a structured lifecycle consisting of:

1. Preparation


2. Detection and Analysis


3. Containment


4. Eradication


5. Recovery


6. Post-Incident Activity



The containment, eradication, and recovery activities should be performed based on the organization's incident severity, business requirements, and available evidence.


---

5. Phase 1 — Preparation

Preparation is the foundation of effective incident response. The organization should establish the necessary people, processes, technologies, and documentation before an incident occurs.

5.1 Preparation Activities

The organization should:

Establish an Incident Response Team (IRT).

Define roles and responsibilities.

Develop incident response procedures.

Maintain updated contact information.

Establish communication channels.

Configure centralized logging and monitoring.

Deploy endpoint detection and response capabilities where appropriate.

Maintain secure backups.

Develop asset inventories.

Identify critical systems and sensitive data.

Establish network diagrams.

Maintain an up-to-date list of administrators and system owners.

Define incident severity levels.

Establish escalation procedures.

Conduct regular incident response exercises.

Train employees on security incidents and reporting procedures.


5.2 Technical Preparation

Security monitoring should collect relevant logs from:

Firewalls.

VPN systems.

Active Directory/identity providers.

Windows and Linux systems.

Web servers.

Databases.

Cloud platforms.

Endpoint security solutions.

Authentication systems.

Email security systems.

Network monitoring systems.


Logs should be centrally collected and protected against unauthorized modification.

For example, a SIEM such as ELK Stack, Wazuh, Microsoft Sentinel, or Splunk can be used to correlate security events and identify suspicious behavior.

5.3 Evidence Preparation

The organization should establish procedures for preserving digital evidence.

Evidence may include:

Authentication logs.

Network traffic.

System logs.

Application logs.

Database logs.

Firewall logs.

Endpoint telemetry.

Memory captures.

Disk images.

Suspicious files.

Email messages.

Screenshots.


Evidence should be stored securely and access should be restricted.


---

6. Phase 2 — Detection and Analysis

The detection and analysis phase begins when suspicious activity is identified.

6.1 Initial Detection

Possible indicators include:

Login from an unusual geographic location.

Multiple failed authentication attempts.

Successful login after numerous failed attempts.

Login outside normal working hours.

Unexpected privilege escalation.

Unusual database queries.

Large data transfers.

Suspicious processes.

Unexpected administrative account activity.

Access to sensitive files.

Unknown devices connecting to the network.


For this scenario, security monitoring may identify an unusual authentication event followed by access to sensitive information.

6.2 Validate the Incident

The response team should determine whether the alert represents a genuine security incident or a false positive.

The team should answer:

What happened?

When did it happen?

Which account was involved?

Which system was accessed?

What data was accessed?

Was the access authorized?

Is the attacker still present?

How did the attacker obtain access?

Has data been modified or exfiltrated?


6.3 Establish a Timeline

Investigators should create a chronological timeline.

Example:

Time	Event

08:42	Suspicious login detected
08:45	Account accesses sensitive application
08:51	Large database query observed
08:56	Unusual outbound traffic detected
09:05	SOC escalates incident
09:15	Incident Commander activated
09:25	Account disabled


The timeline should be continuously updated throughout the investigation.

6.4 Determine Scope

The team should identify:

Affected users.

Affected endpoints.

Affected servers.

Affected applications.

Affected databases.

Compromised accounts.

Potentially exposed information.

Source and destination of suspicious traffic.


The team should determine whether the incident is isolated or part of a larger compromise.


---

7. Phase 3 — Containment

The objective of containment is to prevent the attacker from continuing the compromise while preserving evidence and minimizing business disruption.

Containment should normally occur at two levels:

Short-Term Containment

Immediate actions may include:

Disable compromised accounts.

Revoke active sessions.

Reset compromised credentials.

Block malicious IP addresses where appropriate.

Isolate compromised endpoints.

Restrict suspicious network connections.

Disable compromised API keys or tokens.

Block malicious domains.

Increase monitoring.

Restrict access to affected systems.


For example, if an administrator account is suspected of compromise, the organization may immediately disable the account and revoke its active sessions.

Long-Term Containment

Longer-term measures may include:

Moving affected systems to isolated network segments.

Applying temporary firewall rules.

Implementing additional authentication controls.

Restricting privileged access.

Increasing logging.

Deploying additional endpoint monitoring.

Applying temporary application-level restrictions.


7.1 Evidence Preservation

Containment should not unnecessarily destroy evidence.

Before shutting down or rebuilding systems, investigators should consider collecting:

Volatile memory.

Relevant logs.

Network connections.

Running processes.

Suspicious files.

Disk images.

Authentication information.


Evidence collection should follow organizational procedures and applicable legal requirements.


---

8. Phase 4 — Eradication

After containment, the organization should identify and eliminate the root cause of the incident.

8.1 Identify the Root Cause

Investigators should determine:

Initial access method.

Vulnerability exploited.

Compromised credentials.

Malware involved.

Misconfiguration.

Phishing activity.

Privilege escalation method.

Persistence mechanism.

Systems accessed by the attacker.


For example, investigation may determine that an employee's credentials were stolen through phishing and subsequently used to access an internal application.

8.2 Remove the Threat

Eradication activities may include:

Removing malware.

Deleting unauthorized accounts.

Removing persistence mechanisms.

Resetting compromised credentials.

Rotating API keys and secrets.

Patching exploited vulnerabilities.

Removing malicious scheduled tasks.

Removing unauthorized applications.

Blocking malicious infrastructure.

Correcting security misconfigurations.


If there is uncertainty about system integrity, affected systems should be rebuilt from known-good sources rather than simply deleting suspicious files.


---

9. Phase 5 — Recovery

Recovery restores normal business operations while ensuring that systems are secure.

9.1 Recovery Activities

The organization should:

1. Restore systems from trusted backups or clean images.


2. Apply security patches.


3. Reset credentials where necessary.


4. Validate security configurations.


5. Reconnect systems gradually.


6. Monitor systems closely.


7. Verify application functionality.


8. Confirm data integrity.


9. Monitor for signs of attacker re-entry.



9.2 Enhanced Monitoring

For a period following recovery, security teams should increase monitoring.

They should look for:

Repeated authentication failures.

New suspicious accounts.

Unexpected administrative activity.

Unusual network traffic.

Reappearance of malware.

Unexpected outbound connections.

Abnormal database activity.


Systems should only be considered fully recovered when the organization has reasonable confidence that the attacker no longer has access.


---

10. Phase 6 — Post-Incident Activity

Post-incident activity focuses on understanding what happened and improving future security.

The organization should conduct a formal lessons-learned review after the incident.

The review should answer:

What happened?

How was the incident detected?

What was the initial attack vector?

How long did the attacker have access?

What systems were affected?

What data was exposed?

What containment measures worked?

What actions were ineffective?

Were there communication problems?

Were logs sufficient?

Were response procedures followed?

What could have prevented the incident?

What security controls need improvement?



---

11. Team Roles and Responsibilities

A clearly defined Incident Response Team prevents confusion during an incident.

11.1 Incident Commander

The Incident Commander coordinates the overall response.

Responsibilities:

Declare and classify the incident.

Coordinate response activities.

Set priorities.

Approve major containment decisions.

Coordinate different teams.

Escalate the incident to management.

Maintain situational awareness.

Ensure decisions are documented.

Coordinate the transition between response phases.


The Incident Commander should not necessarily perform technical investigations directly. Their primary responsibility is coordination and decision-making.


---

12. Security/Technical Lead

The Technical Lead manages the technical investigation.

Responsibilities:

Analyze security alerts.

Investigate affected systems.

Review logs.

Identify indicators of compromise.

Analyze suspicious files.

Determine the attack vector.

Identify compromised systems.

Recommend containment measures.

Support eradication and recovery.



---

13. SOC / Security Analyst

The SOC Analyst is usually responsible for initial detection and triage.

Responsibilities:

Monitor security alerts.

Validate suspicious activity.

Collect initial evidence.

Analyze authentication and network logs.

Identify indicators of compromise.

Escalate confirmed incidents.

Maintain incident documentation.



---

14. IT / System Administrator

The IT or System Administrator supports containment and recovery.

Responsibilities:

Disable compromised accounts.

Isolate systems.

Apply firewall changes.

Reset credentials.

Patch affected systems.

Restore systems.

Validate system functionality.

Assist with infrastructure recovery.



---

15. Communications Lead

The Communications Lead manages internal and external communication.

Responsibilities:

Prepare management updates.

Coordinate employee communications.

Ensure consistent messaging.

Prevent unauthorized disclosure of incident information.

Coordinate approved external communications.


Communication should follow the organization's approved communication and escalation procedures.


---

16. Legal and Compliance Representative

Legal and compliance personnel determine whether regulatory, contractual, or legal obligations apply.

Responsibilities:

Assess notification requirements.

Determine regulatory obligations.

Advise on evidence handling.

Coordinate with external authorities when necessary.

Review communications before external disclosure.

Ensure compliance with applicable data protection requirements.



---

17. Management / Business Owner

Management provides business-level decisions and resources.

Responsibilities:

Approve major business decisions.

Allocate resources.

Determine business priorities.

Support system shutdowns when necessary.

Coordinate business continuity activities.



---

18. Responsibility Transfer and Escalation

Incident response responsibilities should be transferred based on expertise and incident severity.

For example:

SOC Analyst → Incident Commander → Technical Lead → IT/Infrastructure → Management/Legal

An incident may begin as a routine security alert handled by the SOC. If evidence indicates unauthorized access to sensitive information, the SOC escalates it to the Incident Commander.

The Incident Commander then activates the appropriate technical, legal, management, and communication resources.

All major decisions and transfers should be documented with:

Timestamp.

Responsible person.

Decision.

Reason for decision.

Supporting evidence.



---

19. Incident Communication Plan

Effective communication is essential during a security incident.

Communication should follow the principle of need-to-know.

Internal Communication

Relevant stakeholders may include:

Incident Response Team.

IT department.

Senior management.

Legal/compliance.

Data owners.

Business continuity teams.


External Communication

Depending on the incident, external communication may involve:

Customers.

Partners.

Service providers.

Regulatory authorities.

Law enforcement.

Cybersecurity vendors.


External communication should only be performed by authorized personnel.


---

20. Evidence and Documentation

Every significant incident-response activity should be documented.

The incident record should contain:

Incident ID.

Date and time detected.

Detection source.

Incident severity.

Affected systems.

Affected accounts.

Indicators of compromise.

Timeline.

Evidence collected.

Actions performed.

People involved.

Decisions made.

Recovery activities.

Final impact assessment.


Sensitive information should be anonymized before being used in reports, screenshots, training materials, or public documentation.

For example:

Instead of:

john.smith@company.com

use:

user001@company.example

Instead of:

192.168.10.45

use:

10.0.0.XX

when exact addresses are not necessary for the report.


---

21. Post-Incident Analysis

A formal post-incident review should be performed after recovery.

21.1 Incident Timeline Review

The response team should reconstruct the complete sequence of events:

Initial access → Discovery → Privilege escalation → Data access → Detection → Containment → Eradication → Recovery

This helps identify delays and weaknesses.

21.2 Root Cause Analysis

The organization should determine the underlying reason the incident occurred.

For example:

Root Cause: Compromised employee credentials.

Contributing factors:

Phishing attack.

Lack of phishing-resistant MFA.

Excessive account privileges.

Insufficient authentication monitoring.

Limited security awareness.


The goal is not simply to identify the attacker's actions but to understand why organizational controls failed to prevent or detect the attack.


---

22. Lessons Learned

The team should identify both successful and unsuccessful actions.

What Worked?

For example:

SIEM detected unusual authentication activity.

SOC escalated the alert quickly.

Compromised accounts were disabled rapidly.

Backups were available.

Communication channels worked effectively.


What Did Not Work?

For example:

Some systems lacked centralized logging.

Detection occurred several hours after initial compromise.

Contact information was outdated.

The incident escalation procedure was unclear.

Critical systems lacked MFA.



---

23. Improvement Plan

Lessons learned should be converted into measurable security improvements.

Finding	Improvement	Responsible Team	Priority

Compromised credentials	Implement MFA	IAM/IT	High
Insufficient logging	Centralize logs	SOC	High
Slow detection	Improve SIEM rules	SOC	High
Excessive privileges	Implement least privilege	IT/IAM	High
Weak employee awareness	Security awareness training	HR/Security	Medium
Outdated IR contacts	Update contact list	Management	Medium
Incomplete procedures	Update IR playbook	Security	Medium


Each improvement should have an owner and target completion date.


---

24. Updating Policies and Procedures

Following the incident, the organization should review and update:

Incident Response Policy.

Access Control Policy.

Password Policy.

MFA Policy.

Logging and Monitoring Policy.

Data Protection Policy.

Backup Policy.

Business Continuity Plan.

Disaster Recovery Plan.

Security Awareness Program.


Updated procedures should be communicated to relevant employees.


---

25. Training and Testing

The organization should regularly test its incident response capabilities.

Recommended exercises include:

Tabletop Exercises

A simulated incident is presented to management and technical teams.

Example:

> "An employee's account has been compromised and sensitive customer information may have been accessed."



The team discusses what actions it would take.

Technical Simulations

Security teams can simulate:

Compromised credentials.

Malware infection.

Phishing attacks.

Data exfiltration.

Privilege escalation.

Ransomware.


Recovery Testing

The organization should periodically test:

Backup restoration.

System recovery.

Account recovery.

Emergency communication.

Incident escalation.


Testing should identify weaknesses before a real incident occurs.


---

26. Recommended Incident Response Metrics

Organizations should measure incident response performance.

Useful metrics include:

Mean Time to Detect (MTTD)
How long it takes to detect an incident.

Mean Time to Respond (MTTR)
How long it takes to begin responding.

Mean Time to Contain (MTTC)
How long it takes to prevent further attacker activity.

Additional metrics:

Number of incidents.

Number of false positives.

Number of compromised accounts.

Number of affected systems.

Percentage of incidents detected internally.

Percentage of incidents successfully contained.

Time required for recovery.

Number of recurring incidents.


These metrics help determine whether the organization's incident response capability is improving.


---

27. Example Response Workflow

For the current unauthorized-access scenario, the response could follow this workflow:

Suspicious Activity Detected
            ↓
Initial SOC Triage
            ↓
Validate Security Incident
            ↓
Classify Severity
            ↓
Activate Incident Response Team
            ↓
Collect & Preserve Evidence
            ↓
Identify Affected Accounts/Systems
            ↓
Short-Term Containment
            ↓
Determine Root Cause
            ↓
Eradicate Threat
            ↓
Restore Systems
            ↓
Enhanced Monitoring
            ↓
Incident Closure
            ↓
Post-Incident Review
            ↓
Lessons Learned
            ↓
Update Policies, Controls & Training


---

28. Conclusion

A well-structured incident response plan provides the organization with a repeatable and controlled method for responding to cybersecurity incidents.

For the suspected unauthorized-access incident, the organization should prioritize rapid detection, evidence preservation, containment of compromised accounts and systems, identification of the root cause, secure recovery, and continuous monitoring.

The response should not end when systems are restored. A formal post-incident review is essential for identifying weaknesses and converting lessons learned into concrete improvements.

A NIST-aligned incident response program should therefore be treated as a continuous process rather than a one-time document. Policies, procedures, technologies, contact information, and training should be regularly reviewed and tested to ensure that the organization remains prepared for future incidents.


---

29. References

NIST SP 800-61

National Institute of Standards and Technology (NIST), Computer Security Incident Handling Guide, SP 800-61.

[NIST SP 800-61 publication page](https://csrc.nist.gov/pubs/sp/800/61/r2/final?utm_source=chatgpt.com)

NIST Cybersecurity Framework

National Institute of Standards and Technology, Cybersecurity Framework (CSF) 2.0.

[NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework?utm_source=chatgpt.com)

NIST Computer Security Resource Center

Additional NIST cybersecurity guidance and publications:

[NIST Computer Security Resource Center](https://csrc.nist.gov/?utm_source=chatgpt.com)

MITRE ATT&CK

MITRE ATT&CK can be used to map attacker behavior and techniques identified during incident investigation.

[MITRE ATT&CK](https://attack.mitre.org/?utm_source=chatgpt.com)


---

Recommended Google Docs Structure

For the actual submission, I recommend making the document look like a formal security report:

Cover Page

Incident Response Plan

NIST SP 800-61

Organization: [Organization Name]

Prepared by: [Your Name]

Date: September 2026

Classification: Internal Use


Table of Contents

1. Background


2. Purpose and Scope


3. Incident Classification


4. Incident Response Process


5. Preparation


6. Detection and Analysis


7. Containment


8. Eradication


9. Recovery


10. Post-Incident Activity


11. Team Roles and Responsibilities


12. Communication Plan


13. Evidence and Documentation


14. Post-Incident Analysis


15. Lessons Learned


16. Improvement Plan


17. Training and Testing


18. Incident Response Metrics


19. Conclusion


20. References



Important: For your submission, don't include real company names, real IP addresses, real usernames, passwords, customer information, tokens, or screenshots containing sensitive information. Use placeholders such as [REDACTED], user001, and 10.0.0.XX.

One important note: NIST SP 800-61 Revision 2 is the classic source behind the Preparation → Detection & Analysis → Containment/Eradication/Recovery → Post-Incident Activity structure. NIST has since published newer incident-response guidance, so if your instructor specifically requires SP 800-61, use the version specified by the assignment and cite it accordingly.