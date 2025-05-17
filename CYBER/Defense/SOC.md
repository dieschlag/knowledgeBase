SOC = Security Operations Center

- What they detect:	
	- Vulnerabilities: discovery =>patch/measures
	- Policy violations
	- Unauthorized Activity
	- Network intrusions
- Reponse: 3 pillars
	- People
	- Process
	- Technology
- SOC helps Incident response team carry out first steps

- People: help identify harmful activities and enable prompt response (avoid false positives)
	- Organization Chart:
![[Pasted image 20250311230743.png]]
- SOC Level 1: first responders to any detection receive everything, perform triage, report detections through proper channels 
- SOC Level 2: help to dive deeper into alerts and correlate data from multiple sources
- SOC Level 3: experiences professionals, look for threat indicators and support in incidence response
- Security Engineer: work on security solutions, need deployment and config
- Detection Engineer: define security rules to detect harmful activities, level 2 and 3 can also create rules
- SOC Manager: manages processes followed by SOC team, remains in contact with organization's CISO (Chief Information Security Officer)

- Process: each role has its own processes
	- Alert triage: determine severity of incident and help prioritize, **5W**
		- What
		- When
		- Where
		- Who 
		- Why
	- Reporting: escalate alert to higher-level analyst, report should discuss 5W and should collect evidence (screenshots, etc.) 
	- Incident Response and Forensics: for events that are critical, high-level teams initiate reponse, forensic help determine root cause

- Technology: refers to security solutions
	- [[SIEM]]
	- EDR: End Detection and Response: detailed real-time visibility of device activities,  is used on endpoint level and can carry out automatic responses with a few clicks
	- [[Firewall]]
	- Antivirus
