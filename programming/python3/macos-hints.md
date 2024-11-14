# Python Setup

## macOS Hints

### python2=>python3 transition

Until we have preinstalled python2 in macOS... Just add two new lines to zsh configuration file

```bash
# check path before changing
echo "alias pip=/usr/bin/pip3" >> ~/.zshrc  
echo "alias python=/usr/bin/python3" >> ~/.zshrc

# open new term. session and check results
python --version && pip --version
```
