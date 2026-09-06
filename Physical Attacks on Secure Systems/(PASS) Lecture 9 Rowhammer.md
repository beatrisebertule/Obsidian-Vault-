guest lecture

software-based fault attacks

[[claude summary rowhammer]]
### DRAM organization
dynamic random access memory

**Dynamic random-access memory** (**dynamic RAM** or **DRAM**) is a type of [random-access](https://en.wikipedia.org/wiki/Random-access_memory "Random-access memory") [semiconductor memory](https://en.wikipedia.org/wiki/Semiconductor_memory "Semiconductor memory") that stores each [bit](https://en.wikipedia.org/wiki/Bit "Bit") of data in a [memory cell](https://en.wikipedia.org/wiki/Memory_cell_\(computing\) "Memory cell (computing)"). A DRAM memory cell usually consists of a microscopic [capacitor](https://en.wikipedia.org/wiki/Capacitor "Capacitor") and a [transistor](https://en.wikipedia.org/wiki/Transistor "Transistor"), both typically based on [metal–oxide–semiconductor](https://en.wikipedia.org/wiki/Metal%E2%80%93oxide%E2%80%93semiconductor "Metal–oxide–semiconductor") (MOS) technology.

![[Pasted image 20260511173629.png]]

![[Pasted image 20260511173821.png]]


### Rowhammer
**Rowhammer** (also written as **row hammer** or **RowHammer**) is a computer security exploit that takes advantage of an unintended and undesirable side effect in [dynamic random-access memory](https://en.wikipedia.org/wiki/Dynamic_random-access_memory "Dynamic random-access memory") (DRAM) in which [memory cells](https://en.wikipedia.org/wiki/Memory_cell_\(computing\) "Memory cell (computing)") interact electrically between themselves by leaking their charges, possibly changing the contents of nearby [memory rows](https://en.wikipedia.org/wiki/Memory_row "Memory row") that were not [addressed](https://en.wikipedia.org/wiki/Memory_address "Memory address") in the original memory access. This circumvention of the isolation between DRAM memory cells results from the high cell density in modern DRAM, and can be triggered by specially crafted [memory access patterns](https://en.wikipedia.org/wiki/Memory_access_pattern "Memory access pattern") that rapidly activate the same memory rows numerous times.[[1]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-isca14-paper-1)[[2]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-arstechnica-2)[[3]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-sophos-3)



The Rowhammer effect has been used in some [privilege escalation](https://en.wikipedia.org/wiki/Privilege_escalation "Privilege escalation") computer security [exploits](https://en.wikipedia.org/wiki/Exploit_\(computer_security\) "Exploit (computer security)"),[[2]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-arstechnica-2)[[4]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-googleprojectzero-4)[[5]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-5)[[6]](https://en.wikipedia.org/wiki/Row_hammer#cite_note-6) and network-based attacks are also theoretically possible.

https://en.wikipedia.org/wiki/Row_hammer

![[Pasted image 20260511174213.png]]

![[Pasted image 20260511174409.png]]





each data bit is stored in a small component - capacitor
bucket that holds electricicty
full bucket - one
empty - zero

the memory control refreshes capacitors
as electricity cannot be stored forever - ones fade into zeros over time - capacitors leak charge over time if left alone
so memory control reads and write data to "top up" the charge



if a malicious program reads the same memory over and over again million times per second it create a stress test
this is called hammering

the row being accesses id called agressor row

the rapid electric switching create electromagnetic inference with neighbour rows - victim rows
this causes a charge to leak from neighbor rows

![[Pasted image 20260511180440.png]]

if row hammer happens fast enough the memory value gets drained before refresh 
0 flips to 1 or
1 flips to 0


attack
flip a bit for access control - privelage escalation

countermeasure
target row refresh
proactively refresh target row


![[Pasted image 20260511182445.png]]

**What is Rowhammer?**  
Rowhammer is a memory attack against **DRAM** where repeatedly accessing (“hammering”) certain memory rows can cause electrical interference in nearby rows, leading to unintended bit flips (0→1 or 1→0). An attacker may exploit those flips to corrupt memory, escalate privileges, or bypass security protections.

### Left side: the attack code

The assembly snippet shows a simplified hammering loop:

- `mov (X), %eax` → read from memory address **X**
- `mov (Y), %ebx` → read from memory address **Y**
- `clflush (X)` and `clflush (Y)` → flush X and Y from CPU cache
- `mfence` → memory fence; ensures operations complete in order
- `jmp code1a` → jump back and repeat forever

Why this works:

- Normally, repeated reads might hit CPU cache, which is fast and doesn’t touch DRAM much.
- `clflush` forces those cache lines out of cache, so the next read must fetch from DRAM again.
- Repeating this very quickly causes constant activation of DRAM rows X and Y.
- This repeated activation can disturb neighboring rows and trigger bit flips.

So the loop is basically:

> access memory → evict from cache → access again → repeat thousands/millions of times

### Right side: defenses / mitigations

The slide lists ways defenders reduce or block Rowhammer:

- **Forbid `clflush`**
    - Restrict or disable use of the `clflush` instruction, since attackers use it to force DRAM accesses.
    - Not a perfect defense, because newer attacks may use cache eviction patterns instead.
- **Refresh victim rows**
    - DRAM cells naturally lose charge over time and need refreshing.
    - Refreshing adjacent “victim” rows more often reduces the chance of disturbance errors.
- **Target Row Refresh (TRR)** (highlighted)
    - A hardware defense implemented in many modern DRAM chips.
    - Memory controllers detect rows being activated excessively and proactively refresh neighboring rows.
    - Idea: “this row is getting hammered—refresh nearby rows before bit flips happen.”
- **Error Correction Code (ECC)**
    - ECC memory stores extra bits to detect/correct some memory errors.
    - Can correct single-bit errors and detect some multi-bit errors.
    - Helps against accidental flips, but sophisticated Rowhammer attacks may sometimes bypass ECC.

**The voltage fluctuations on the row selection lines** cause capacitors in _adjacent_ rows to discharge faster than normal

### Big picture

The slide’s message is:

1. Understand how Rowhammer repeatedly forces DRAM accesses.
2. Break that mechanism with hardware/software protections like:
    - cache flush restrictions,
    - extra refreshes,
    - **TRR**,
    - **ECC**.

In short: **Rowhammer abuses DRAM physics; mitigations either stop the hammering pattern or repair/prevent the resulting bit flips.**



`clflush` cash line flush
`clflush` = **“kick this memory block out of cache so the CPU has to go back to RAM next time.”**


- The attacker repeatedly accesses **aggressor rows** (for example X and Y).
- After each access, they use `clflush` to evict X and Y from cache.
- This forces repeated DRAM accesses to X and Y.

Because DRAM rows are physically adjacent, repeatedly opening/closing the aggressor rows can electrically disturb a **nearby victim row**, causing bit flips there.

So the flow is more like:

1. Read aggressor row X
2. Read aggressor row Y
3. `clflush(X)` and `clflush(Y)`
4. Repeat millions of times

This means:

- X and Y are constantly fetched from DRAM (not cache)
- DRAM repeatedly activates those rows
- Neighboring victim row gets disturbed