# ForceMajuer
### How Observing a Running System Can Minimize Your Risks

In the pursuit of a safer digital environment, observing and
understanding the functioning of a running system can provide invaluable
insights. Tools like file system analysis and code coverage analysis
enable us to get a clear view of our system's interaction boundaries and
take active steps to reduce them, making our systems leaner, more
efficient, and ultimately, more secure.

The classical approach says that you should only enable the features
that you need. This is in fact a great approach; how do you prove it
correct?

How can you audit a running system to make sure that you are only
shipping the code that you need?

Easy right :%) . Test all of the features you need and track what parts
of the system were exercised and the lines of code has a use case. That
is what I hope to prove by instrumenting a system file system monitoring
and then for code coverage analysis.

### And, there is the package level view

Before we dive into the techniques, let's clarify why this is essential.
Every installed software package, each running service, and every open
port on a system increases its exposure points. By minimizing these
components, we reduce potential points of entry for malicious actors,
thus making the system safer. So how can we observe a running system in
order to determine what components it contains that it never uses?

### The Path of Least Resistance: File System Analysis conditions. A thorough investigation is a prerequisite to avoid breaking any system functionality.
Going a Step Further: Code Coverage Analysis
While file system analysis can effectively identify unused binaries, code coverage analysis can delve deeper, identifying unused blocks of code within those binaries. Code coverage tools like Gcov (for C/C++), JaCoCo (for Java), and Coverage.py (for Python) can be instrumental in this process. These tools analyze the execution path of a binary and report on which lines of code are never executed.
Combined with file system analysis, this gives a deeper view of the system’s interaction boundaries, showing not only unused binaries but also unused code within the active binaries. While refactoring these binaries to remove unused code blocks might be more involved, it’s a step towards further reducing exposure points.
Final Thoughts
Prove that we can do “Code Coverage Analysis”—record what code we run when we test insights. For this, a simple test is fine. Determine what code we can record coverage for by trying to cover most of the code. Document anything we don’t know how to test/record coverage for. Identify what parts of the installed system we do not think we are using. Remove something we don’t think we are using and make sure everything still works. Remove all of the things we are pretty sure we aren’t using. Remove some of the things we don’t seem to be using. Remove all of the things we don’t seem to use.

After an initial investigation of auditd and several other real-time
auditing tools such as SELinux and eBPF, there was considerable pushback
regarding their potential to disturb the behavior of the system under
test. Therefore, the go-forward method utilized was to analyze the file
system data that records file reads and writes. This approach allows us
to generate the initial report of what is and is not accessed without
interfering with the system's operation.

### Identifying Unused Binaries

To begin, file system logs should be configured to track file reads and
writes. Analyzing these logs will show us which binaries are actually
used. We can then compare this data against the list of installed
binaries in the system.

To streamline this process, we developed a Python script that creates an
SQLite database of all installed packages and their associated binaries,
then cross-references this with the file system data. The result is a
clear report on which binaries (and their corresponding packages) are
never executed and thus, potential candidates for removal.

However, be careful. Before removing any packages, ensure that they are
genuinely not needed. Sometimes a package might be used infrequently or
only under specific conditions. A thorough investigation is a
prerequisite to avoid breaking any system functionality.

### Going a Step Further: Code Coverage Analysis

While file system analysis can effectively identify unused binaries,
code coverage analysis can delve deeper, identifying unused blocks of
code within those binaries. Code coverage tools like Gcov (for C/C++),
JaCoCo (for Java), and Coverage.py (for Python) can be instrumental in
this process. These tools analyze the execution path of a binary and
report on which lines of code are never executed.

Combined with file system analysis, this gives a deeper view of the
system's interaction boundaries, showing not only unused binaries but
also unused code within the active binaries. While refactoring these
binaries to remove unused code blocks might be more involved, it's a
step towards further reducing exposure points.

### Final Thoughts

Prove that we can do "Code Coverage Analysis"---record what code we run
when we test insights. For this, a simple test is fine. Determine what
code we can record coverage for by trying to cover most of the code.
Document anything we don't know how to test/record coverage for.
Identify what parts of the installed system we do not think we are
using. Remove something we don't think we are using and make sure
everything still works. Remove all of the things we are pretty sure we
aren't using. Remove some of the things we don't seem to be using.
Remove all of the things we don't seem to use.
