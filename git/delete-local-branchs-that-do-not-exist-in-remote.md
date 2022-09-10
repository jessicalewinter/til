# Delete local branchs that do not exist in remote

```bash
BRANCH_NAME=main
git branch --merged ${BRANCH_NAME} | grep -v '^[ *]*${BRANCH_NAME}$' | xargs git branch -d
```


## Resources
- [StackOverflow](https://stackoverflow.com/a/16906759)

