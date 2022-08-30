# Right Way to download python on Mac M1

```bash
brew install pyenv

##### ~/.zprofile #####
eval "$(pyenv init --path)"

##### ~/.zshrc #####
if command -v pyenv 1>/dev/null 2>&1; then
    eval "$(pyenv init -)"
fi

pyenv install -l # list all available python versions
pyenv install 3.9.7
pyenv global 3.9.7
```



## References
- [How to Manage Multiple Python Versions on an Apple Silicon M1 Mac](https://towardsdatascience.com/how-to-use-manage-multiple-python-versions-on-an-apple-silicon-m1-mac-d69ee6ed0250)
