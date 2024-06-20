# backport-tool-server

Contains instructions and configuration for the Server Docs team backport tool.

## Setup

### 1. Install the Backport command-line tool

```
npm install backport
```

### 2. Configure a GitHub access token

During installation backport will create an empty configuration file in `/Users/<username>/.backport/config.json`. You must update this file with a Github Access Token.

Access tokens can be created here: https://github.com/settings/tokens

Please select the necessary access scopes:

**For public and private repos (recommended)**
![image](https://user-images.githubusercontent.com/209966/67081197-fe93d380-f196-11e9-8891-c6ba8c4686a4.png)

<img width="971" alt="image" src="https://user-images.githubusercontent.com/7416358/226398066-54cd918e-7d5a-420b-9f84-bb34f9f43dd6.png">

Add the personal access token to [global config](https://github.com/sorenlouv/backport/blob/main/docs/config-file-options.md#global-config-backportconfigjson):

```js
// ~/.backport/config.json
{
  "accessToken": "ghp_very_secret"
}
```

**Configure your token with SSO**

On the https://github.com/settings/tokens screen, click `Configure SSO`, then click `Authorize` and `Continue` twice to authorize the token for SSO. Do this for both `10gen` and `mongodb` orgs:

![Screenshot 2024-06-10 at 10 58 15 AM](https://github.com/mmaville-mdb/backport-tool-server/assets/85948430/27c092dc-bec3-4c53-8228-86f5dd4ee576)


## Use

1. Pull the latest changes for `master`.

```
docs-mongodb-internal git:(master) ✗ git pull
```

2. Select the commit you want to backport:

```
docs-mongodb-internal git:(master) ✗ backport
repo: 10gen/docs-mongodb-internal • maxNumber: 15

? Select commit (Use arrow keys)
  1. DOCSP-36576 adding snapshot limitation to views and time series (#7819)
❯ 2. DOCSP-39616-push-aggregation-missing-behavior (#7857) v7.3
  3. DOCSP-39285 Fixes duplicate Bulk method entries (#7859)
  4. DOCSP-39984 Fix SVG Build Warning (#7852)
  5. DOCSP-39985 fixing snapshot 404 (#7855)
```


 > [!IMPORTANT]  
 > If you get an error when running the `backport` command from terminal, try to install the cli with `npm install -g backport`.


3. Select the branch(es) you want to backport to:

```
? Select branch (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)
 ◯ v7.3
 ◉ v7.0
 ◉ v6.0
❯◉ v5.0
```

Once you press <enter>, the selected backports will commence in list order. The tool pauses and opens VSCode if a merge conflict is encountered during the cherry-pick, allowing you to resolve it before proceeding to the next queued backport.

```
Backporting to v7.0:
✔ Pulling latest changes
✔ Cherry-picking: DOCSP-39616-push-aggregation-missing-behavior (#7857)
✔ Pushing branch "mmaville-mdb:DOCSP-39616-push-aggregation-missing-behavior-v7.0-backport"
✔ Creating pull request
View pull request: https://github.com/10gen/docs-mongodb-internal/pull/7870

Backporting to v6.0:
✔ Pulling latest changes
✔ Cherry-picking: DOCSP-39616-push-aggregation-missing-behavior (#7857)
✔ Pushing branch "mmaville-mdb:DOCSP-39616-push-aggregation-missing-behavior-v6.0-backport"
✔ Creating pull request
View pull request: https://github.com/10gen/docs-mongodb-internal/pull/7871

Backporting to v5.0:
✔ Pulling latest changes
✔ Cherry-picking: DOCSP-39616-push-aggregation-missing-behavior (#7857)
✔ Pushing branch "mmaville-mdb:DOCSP-39616-push-aggregation-missing-behavior-v5.0-backport"
✔ Creating pull request
View pull request: https://github.com/10gen/docs-mongodb-internal/pull/7872
```

4. Manually edit the PR description, adding a link to the build log so others can verify there are no residual build errors.

```
## Build Log
https://workerpool-boxgs.mongodbstitch.com/pages/job.html?collName=queue&jobId=665dfa69cf5bec1c2905c3b3
```

## Limitations

- PR headings cannot contain spaces or certain other characters. See character considerations in [item #4](https://git-scm.com/docs/git-check-ref-format#_description).

## Troubleshooting

Some issues you might run into when the sailing isn't so smooth.

### Merge conflict during cherry-pick

You must resolve all merge conflicts in the temp backport repo and stage the changes before resuming backporting.

```
Backporting to v5.0:
✔ Pulling latest changes
x Cherry-picking: DOCSP-39616-push-aggregation-missing-behavior (#7857)

The commit could not be backported due to conflicts

Please fix the conflicts in /Users/mattmaville/.backport/repositories/10gen/docs-mongodb-internal
? Fix the following conflicts manually:


Unstaged files:
 - /Users/mattmaville/.backport/repositories/10gen/docs-mongodb-internal/config/changelog_conf.yaml

Press ENTER when the conflicts are resolved and files are staged
```

### VSCode does not open automatically

**Solution**: Set VSCode as the default editor for all relevant filetypes on your OS.

You can also resolve merge conflicts by hand and stage the changes before resuming the script. Use the file refs provided by the script to ensure you're working on the temporary backport repo and not your local source repo.
