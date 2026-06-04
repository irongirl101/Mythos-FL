# My Research and Understanding of Mythos Claude 

### Starting in Sept 29, 2025 
- With the release of Sonnet 4.5, it started to outperform humans in multiple cases, especially in cybersecurity and finding vulnerabilites (ref DARPA summer competition 2025)
Also distrupted a malicious attacker/vibe hacker 
Possible Inflection point for cybersec

### Feb 5, 2026 
- Zero Day Vulnerability (an undisclosed security flaw in software, hardware, or firmware that is unknown to the vendor or developers. Because the creators are unaware of the flaw, they have had "zero days" to create a patch or defense, giving attackers a critical window to exploit it.)

Claude was provided all the tools like fuzzers, and started utils 

(fuzzers - an automated testing technique that discovers bugs, crashes, and security vulnerabilities by feeding random, malformed, or invalid inputs into your software - forces edge cases that would usual be missed)

GhostScript - no results from fuzzing and manual analysis. 
- eventually, ended up looking at Git commit history - found a security related commit 
- found a function that had checks added, then tested where this function was called (before the commit was made) to check for similar vulnerabilities that were *left* unpatched. 
- found a line, and made a POC file that passed it to the code, and proved it to be correct 

OpenSC - nothing from fuzzing, and manual analysis. 
- started to search the repository for function class that are  frequently vulnerable - like consecutive calls of strcat 
- realized chance of output buffer overflow 
- fuzzers not as useful in this case due to the many number of preconditions 
- was able to reason about which code fragments were interesting and focus its effort there, instead of indiscriminately studying all lines with equal effort.

### Mar 6, 2026 
- 
