# gerrit-review
A wrapper for the [review cmd](https://gerrit-review.googlesource.com/Documentation/cmd-review.html) of Gerrit Command Line Tools.

## Introduction

`gerrit-review` wrapper automatically parses the connection info of Gerrit server and project path from `.git/config` to save reviewers time entering repeating commands.
The wrapper supports only widely used options of the original [review cmd](https://gerrit-review.googlesource.com/Documentation/cmd-review.html), which includes:
* Score changes
* Write comments
* Submit/rebase/abandon changes

## Usage

```bash
$ gerrit-review [--remote REMOTE] [-v VERIFY] [-c REVIEW] [-m MESSAGE] [-s] [-R] [-a] CHANGE_SPECIFIER
```

### Options

```
positional arguments:
  CHANGE_SPECIFIER                Commit SHA-1 or ChangeID,PatchSet

optional arguments:
  --remote REMOTE                 Git remote hosting Gerrit review server, default 'origin'
  -v SCORE, --verified SCORE      Verified score (-1/0/+1)
  -c SCORE, --code-review SCORE   Code review score (-2/-1/0/+1/+2)
  -m MESSAGE, --message MESSAGE   Review Message
  -s, --submit                    Submit change
  -R, --rebase                    Rebase change
  -a, --abandon                   Abandon change
  --verbose                       Display command send to Gerrit review server
  -h, --help                      Show help message
```

### Remote Configuration
If Gerrit review server is hosted on remote other `origin`, you can set the default remote parsed by `gerrit-review`.
```bash
$ git config gerritcli.remote REMOTE_NAME
```

## Examples

### Scoring commit a5782bf

Note that you must add "+/-" with scores.
```bash
$ gerrit-review -c +2 -s a5782bf
```

### Scoring change 1234, patchset 5 and write comments

Note that patchset number must be specified.
```bash
$ gerrit-review -v -1 -m "Build failed" 1234,5
```

### Scoring commit a5782bf and submit change

Note that submit/abandon/rebase options are mutually exclusive.
```bash
$ gerrit-review -v +1 -c +2 -s a5782bf
```

### Rebase change 1234, patchset 5

Note that the change specifier (patchset number and commit SHA-1) are renewed after a sucessful rebase, and the new change specifier must be used in the follow-up commands.
You can fetch new change specifier from Gerrit review server by `git review -d CHANGE_ID`.
```bash
$ gerrit-review --rebase 1234
```

### Specify remote manually

Note that `gerrit-review` chooses the remote to parse connection info based on the following priority: (1) remote specifed by `--remote`, (2) remote specified in `gerritcli.remote`, (3) the `origin` remote.
```bash
$ gerrit-review --remote gerrit -v +1 -c +2 5432,1
```

## License

This project is licensed under the terms of Apache License 2.0.

