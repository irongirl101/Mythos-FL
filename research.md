# My Research and Understanding of Mythos Claude 

### Starting in Sept 29, 2025 
- With the release of Sonnet 4.5, it started to outperform humans in multiple cases, especially in cybersecurity and finding vulnerabilites (ref DARPA summer competition 2025)
Also distrupted a malicious attacker/vibe hacker 
Possible Inflection point for cybersec


### Feb 5, 2026 
- Zero Day Vulnerability (an undisclosed security flaw in software, hardware, or firmware that is unknown to the vendor or developers. Because the creators are unaware of the flaw, they have had "zero days" to create a patch or defense, giving attackers a critical window to exploit it.)

- Claude was provided all the tools like fuzzers, and started utils 

``` (fuzzers - an automated testing technique that discovers bugs, crashes, and security vulnerabilities by feeding random, malformed, or invalid inputs into your software - forces edge cases that would usual be missed) ```

- GhostScript - no results from fuzzing and manual analysis. 
    - eventually, ended up looking at Git commit history - found a security related commit 
    - found a function that had checks added, then tested where this function was called (before the commit was made) to check for similar vulnerabilities that were *left* unpatched. 
    - found a line, and made a POC file that passed it to the code, and proved it to be correct 

- OpenSC - nothing from fuzzing, and manual analysis. 
    - started to search the repository for function class that are  frequently vulnerable - like consecutive calls of strcat 
    - realized chance of output buffer overflow 
    - fuzzers not as useful in this case due to the many number of preconditions 
    - was able to reason about which code fragments were interesting and focus its effort there, instead of indiscriminately studying all lines with equal effort.

### Mar 6, 2026 
- The way claude "exploits" is by giving it a vm and a task verifier, and asking it to make an exploit. then the poc was reverse engineered, to verify result 
- Firefox CVE-2026-2796 
    - gave access to vulnerabilites to claude that was submitted to mozilla and was asked to find exploits
    - exploit a stripped-down version of the js shell (a standalone utility that lets developers use Firefox’s JavaScript engine without the browser) that resembles an unsandboxed content process in the browser, and a task verifier to determine whether the exploit worked. 
    - A separate "verifier" system was set up with a secret file and a target location 
    - claude had to read the file - showing that it broke out of the sandbox 
    - and write than content to another location, proving that it had write access


## Claude Mythos Preview - a Preview on Project Glasswing 

- wrote a complex JIT heap spray that escpaed both rendered and OS sandboxes 
- it obtained local privilige escalation exploits on LInux and other OSs by exploiting subtle race conditions and KASLR-bypasses (Kernel Address Space Layout Randomization (KASLR) is a crucial security defense that randomizes the memory locations of core kernel code and data structures at boot time. )
- And it autonomously wrote a remote code execution exploit on FreeBSD’s NFS server that granted full root access to unauthenticated users by splitting a 20-gadget ROP chain over multiple packets.(look into this, need to understand)


## URL's referred to 
- [Blog #1 - AI for CyberSec](https://red.anthropic.com/2025/ai-for-cyber-defenders/)
- [Blog #2 - Zero Days](https://red.anthropic.com/2026/zero-days/)
- [Blog #3 - Reverse Engineering CVE-2026-2796](https://red.anthropic.com/2026/exploit/)
- [Blog #4 - Mythos Preview](https://red.anthropic.com/2026/mythos-preview/)


