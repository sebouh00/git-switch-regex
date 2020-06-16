# git-switch-regex

| ⚠️ This script is written for macOS. |
| --- |

<br/>

This script switches branches using regex matching. When more than one match is found, it prompts a 
choice menu so you can pick the desired branch. If the regex filter is omitted, it prompts the most 
recently switched branches. These are retreived from the repository's `.git/recent-branches` file, 
which is populated by a post-checkout hook.

## Switching with regex

Partial branch name:

```bash
> git switch-regex devel
```
```
Switched to branch 'develop'
Your branch is up to date with 'origin/develop'.
```

With metacharacters:
```bash
> git switch-regex ^ma[r-t].*r$
```
```
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

Multiple matches:
```bash
> git switch-regex feature/coo
```
```
[1] feature/coolBeans
[2] feature/cookie

Make your choice:
```

Remote-only branches are colored <span style="color:red">red</span>. The script uses the hard-coded 'origin' as the remote name.


## Switching to a recent branch

```bash
> git switch-regex
```
```
[1] master
[2] develop
[3] my_feature

Make your choice:
```

## Copying the branch name

Intead of switching to the matching branch, you can copy it to the clipboard by specifying using
the `-c` flag:

```bash
> git switch-regex -c devel
```
```
Copied 'develop' to the clipboard
```

## Installing the script

You need to add the script directory to your `PATH` inside `~/.bashrc` for Bash and `~/.zshrc` 
for Z Shell.

```bash
> echo 'PATH="~/git-switch-regex:$PATH"' > ~/.bashrc
```

## Setting up the hook

To save recent branches to your repository's `.git/recent-branches` file, you need to set up a global
post-checkout hook. If you don't have a 'hooks' directory yet (check with 
`git config -l | grep core.hookspath`), do the following:

```bash
> mkdir ~/.githooks && git config --global core.hooksPath ~/.githooks
```

Now assuming `~/.githooks` is your hooks directory, copy the `post-checkout` file from this repository
to `~/.githooks/` or alternatively, you can source it directly with:

```bash
echo "source ~/git-switch-regex/post-checkout" >> ~/.githooks/post-checkout
```
