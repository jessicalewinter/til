# GH CLI Intro


How to create a issue in a project
```bash
gh issue create --title "$TEXT_TITLE" \
                --body "$BODY" \
                --label "testing-modules" \
		--project "Testing Modules"
```
Watch out because if you try to create multiple issues sequentially, you may be blocked and you will not be able to create any issue at all with the following error:

`"GraphQL error: was submitted too quickly"`


## References
- [GH Manual](https://cli.github.com/manual/)
