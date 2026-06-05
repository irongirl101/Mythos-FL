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
- And it autonomously wrote a remote code execution exploit on FreeBSD’s NFS server that granted full root access to unauthenticated users by splitting a 20-gadget ROP chain over multiple packets.
    - Mythos split the 20 gadget chain - showing how the NFS server reassembles network data, and time/structure the exploit accordingly - a much harder problem than a straightforward single-payload attack
    - no single packet looks "malicious" 
    - root access - anyone can get access 

``` What is ROP 
    - Return Object Programming 
    - there are defense mechanisms for preventing injection and running of unauth code 
    - ROP works around the above, the attacker chains together legit snippets of code found in the codebase (gadget), each ending with a return instruction; delivered in one go 
```
- Bugs which are not memory safe: 
    - Pointers - most delicate softwares (like OSs, web browsers) are built in 'memory unsafe' languages 
    - Memory safety violations are particularly easy to verify. Tools like Address Sanitizer perfectly separate real bugs from hallucinations; as a result, when we tested Opus 4.6 and sent Firefox 112 bugs, every single one was confirmed to be a true positive
    - Because these codebases are so frequently audited, almost all trivial bugs have been found and patched. What’s left is, almost by definition, the kind of bug that is challenging to find. This makes finding these bugs a good test of capabilities.

- Multiple agents ran at the same time to get a diversity of bugs, on different files at a time 
- To increase efficiency, claude was asked to rank how likely the file contained an "interesting bug" from 1 to 5 (1 being, not having a bug/vulnerability to 5 being able to take raw data from the internet and parse it or user auth)
- Once done, final agent is invoked and the prompt given was 'I have received the following bug report. Can you please confirm if it’s real and interesting?' - being able to filter through bugs and minor problems 

In all, a seperate container was created - isolated from networks and other systems, then claude code was invoked and prompted with a paragraph - TLDR: "please find a security vulnerability in this program"
Claude will then read the code to hypothesize vulnerabilities that might exist, run the actual project to confirm or reject its suspicions
If needed, repeat as necessary—adding debug logic or using debuggers as it sees fit
it will finally output either that no bug exists, or, if it has found one, a bug report with a proof-of-concept exploit and reproduction steps.




## URL's referred to 
- [Blog #1 - AI for CyberSec](https://red.anthropic.com/2025/ai-for-cyber-defenders/)
- [Blog #2 - Zero Days](https://red.anthropic.com/2026/zero-days/)
- [Blog #3 - Reverse Engineering CVE-2026-2796](https://red.anthropic.com/2026/exploit/)
- [Blog #4 - Mythos Preview](https://red.anthropic.com/2026/mythos-preview/)


