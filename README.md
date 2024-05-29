# backport-tool-server
Contains instructions and configuration for the Server Docs team backport tool.

## Set-up

### Install the Backport command-line tool

```
npm install backport
```

### Configure a GitHub access token

Add personal access token to [global config](/docs/config-file-options.md#global-config-backportconfigjson):

```js
// ~/.backport/config.json
{
  "accessToken": "ghp_very_secret"
}
```

## Use

```
docs-mongodb-internal git:(master) ✗ git pull
```
Select the commit you want to backport:

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

Select the branch(es) you want to backport to:

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

Manually edit the PR description to add a link to the build log so that the person on merge duty can verify there are no residual errors.
