# glab

#### links

- https://gitlab.com/gitlab-org/cli
- https://gitlab.com/gitlab-org/cli/-/releases changelog

## Helper

```shell
# Auth & config

glab auth status --all                  # which hosts, token source, endpoints
glab auth login --hostname gitlab.loc   # writes host block + keyring credential
glab config get --host gitlab.loc user
glab config set editor "zed --wait"

# MRs

glab mr list                            # open MRs in current repo
glab mr list --assignee=@me
glab mr list --reviewer=@me --state=opened
glab mr view 1234                       # metadata, description, pipeline status
glab mr view 1234 --web
glab mr diff 1234
glab mr checkout 1234                   # aliased: glab co 1234
glab mr create --fill --yes             # title/body from commits, no prompts
glab mr create --fill --draft --yes
glab mr note 1234 -m "comment"
glab mr approve 1234
glab mr merge 1234 --when-pipeline-succeeds --remove-source-branch
glab mr update 1234 --ready

# Pipelines & CI

glab ci list
glab ci status 
glab ci get --pipeline-id 98765
glab ci trace <job-id>
glab ci retry <job-id>
glab ci cancel <job-id>
glab ci run --branch feature/x

glab repo view --web
```

### config

Resolution order

`get` without flags resolves environment variable → local (repo) → global. Local config is per-clone in .git/glab-cli/config.yml; --global forces the user-level file.

```shell
glab config path                   # where the live config actually is
glab config get <key>
glab config get <key> --host gitlab.loc
glab config get <key> --global     # skip env/local, read global only
glab config set <key> <value>
glab config set <key> <value> --host gitlab.loc
glab config set <key> <value> --global
glab config edit                   # opens $EDITOR on the config file

glab config path is the first thing to run when settings seem ignored — it prints the file glab is really reading.
```