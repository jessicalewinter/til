# Stop writing bash scripts
Shell scripts are fantastic… for small, throwaway analysis or just stringing together a few commands. But to build a maintainable, understandable, and scalable piece of software, just do the world a favor and start with Python(or another programming language for scripting) from the beginning.
 I’ll close by repeating my rule of thumb:

If it requires more than 1 function or any type of control structure (like a loop, or if/then/else statement), you should probably use Python instead of shell.

## When not to use shell scripts
1. Resource-intensive tasks, especially where speed is a factor (sorting, hashing, recursion [1] ...)
2. Procedures involving heavy-duty math operations, especially floating point arithmetic, arbitrary precision calculations, or complex numbers(use a high-level programming language instead)
3. Cross-platform portability required
4. Complex applications, where structured programming is a necessity (type-checking of variables, function prototypes, etc.)
5. Mission-critical applications upon which you are betting the future of the company


## References
- [Stop writing shell scripts!](https://databio.org/posts/shell_scripts.html)
- [Don't Use Bash for Scripting (All the Time)](https://dev.to/nikoheikkila/don-t-use-bash-for-scripting-all-the-time-2kci)
- [Shell Programming](https://tldp.org/LDP/abs/html/why-shell.html#FTN.AEN87)

