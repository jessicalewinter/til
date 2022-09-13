## How to grep recursively

Example:
```bash
grep --inclue="*.md" -nRHi "how" *
```

Read all files under each directory, recursively; this is equivalent to the -d recurse option.

```bash
-R, -r, --recursive
```

Prefix each line of output with the line number within its input file.

> Note: -n decreases performance a lot so I just use it when really needed 

```bash
-n, --line-number
```

Process a binary file as if it did not contain matching data. It's case insensitive, instead you can use *-I* to return case sentitive results
```bash
-i
```

## References

- [How do I recursively grep all directories and subdirectories?](https://stackoverflow.com/a/14871646/8679480)
- [How does grep run so fast?](https://stackoverflow.com/a/12630617/8679480)
