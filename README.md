# gerrit-review
A wrapper for the [review cmd](https://gerrit-review.googlesource.com/Documentation/cmd-review.html) of Gerrit Command Line Tools.

## Introduction

gerrit-review wrapper automatically parses the connection info of review server and project path from the `origin` remote in `.git/config` to save time on entering repeating commands.
The wrapper supports only parts of, but widely used, options of the original [review cmd](https://gerrit-review.googlesource.com/Documentation/cmd-review.html), which includes:
* Score changes
* Write comments
* Rebase/abandon/submit changes

## Usage

```bash
$ gerrit-review [--remote REMOTE] [-v VERIFY] [-c REVIEW] [-m MESSAGE] [-s] [-a] [-R] CHANGE
```

### Options

```
positional arguments:
  CHANGE                          Commit or ChangeID,PatchSet

optional arguments:
  --remote REMOTE                 Path to output directory/file
  -v SCORE, --verified SCORE      Verified scoew (-1/0/+1)
  -c REVIEW, --code-review SCORE  Code Review score (-2/-1/0/+1/+2)
  -m MESSAGE, --message MESSAGE   Review Message
  -s, --submit                    Submit change
  -a, --abandon                   Abandon change
  -R, --rebase                    Rebase change
  --verbose                       Show command send to server
  -h, --help                      Show help message
```

### Remote Configuration
If review server is specified in remote other `origin`, you can set up the remote parsed by gerrit-review by default.
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

Note that the change specifier (patchset number and commit ID) are renewed after sucessful rebase, and the new change specifier must be used in the follow-up commands.
(You can fetch new change sprifier from gerrit server by `git review -d CHANGE_ID`.)
```bash
$ gerrit-review --rebase 1234
```

### Specify remote manually

You can manually specify the remote that gerrit-review treats as review server and send commands to.
Note that gerrit-review follows the following priority: (1) remote specifed by `--remote`, (2) remote specified in `gerritcli.remote`, (3) `origin` remote, when parsing connection info of review server.
```bash
$ gerrit-review --remote gerrit -v +1 -c +2 5432,1
```

## License

This project is licensed under the terms of Apache License 2.0.

