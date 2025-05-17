## Main components

- Auxiliary: support modules: scanners, etc
- Encoders: encode exploit/signature so that antivirus not detect
- Evasion: module specified to avoid antivirus
- Exploits: in the name
- NOPs: do nothing, used to achieve consistent payload sizes
- Payloads: 
	- adapters: wraps single payload in different formats
	- singles: self contained payloads, not requiring extra soft to be downloaded
	- stages: downloaded by stager
	- stagers: set up connection between metasploit and target sys, useful when woking with stages
- Post: modules for final stage
	
## Ranking System

- ExcellentRanking: will never crash the service
- GreatRanking: default target + detects appropriate target | version check
- GoodRanking: default target, "common case"
- NormalRanking: reliable but on specific version + not autotarget
- AverageRanking: difficult to exploit /generally unreliable
- LowRanking: nearly impossible to exploit for most platforms
- ManualRanking: DoS or no unless config by user

## Shell prompts:

- regular
- msfconsole prompt
- context prompt
- meterpreter
- shell on target system

## Links

- [[Metasploit Commands|List of Metasploit Commands]]
