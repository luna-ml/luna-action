# Luna ML Github action

This Github action helps to verify [Luna ML](https://github.com/luna-ml/luna) projects files, evaluate and score locally on Pull Request. So it can be verified before when a new project or a new model is submitted.

After pullrequest merges, this action syncs the changes with the Luna ML server.

If you're starting a new leaderboard project, you can start from [Luna project template](https://github.com/luna-ml/luna-project-template). See the [Quickstart](https://luna-ml.github.io/luna/quickstart/index.html) guide for more details.


## Usage

On pull request build and evaluate in Github action without submitting to remote luna server

```yaml
name: Evaluate locally
on: [pull_request]
jobs:
  luna-ml:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout latest code
      uses: actions/checkout@v2
    - name: Set up Python # This action requires python 3.6 or later
      uses: actions/setup-python@v2
      with:
        python-version: "3.6"
    - name: Setup KIND # Local kubernetes cluster to validate evaluation
      uses: engineerd/setup-kind@v0.5.0
      with:
        version: "v0.9.0"
    - uses: luna-ml/luna-action@main
```

On sync changes with remote luna server. Remote luna server will automatically trigger evaluation when necessary.

```yaml
name: Sync changes
on:
  push:
    branches:
      - main
jobs:
  luna-ml:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout latest code
      uses: actions/checkout@v2
    - name: Set up Python # This action requires python 3.6 or later
      uses: actions/setup-python@v2
      with:
        python-version: "3.6"
    - uses: luna-ml/luna-action@main
      with:
        access-token: '' # github access token with permissions to the current repository. Can not use ${{ secrets.GITHUB_TOKEN }}
```

Optional input parameters

| parameter | Description |
| ------- | --------- |
| server | Optional, [Luna ML](https://github.com/luna-ml/luna)  server address. e.g. `https://luna-ml.org`. remote server address to sync changes. When the 'server' parameter is not configured, then this action locally run a server and sync with it. Local sync is useful for evaluating new module in pull request |
| version | Optinal, [Luna ML](https://github.com/luna-ml/luna) client version to use.|
| access-token | github access token with permissions to the current repository. This token will be transfered to luna server to validate if request from this github action has access permission to the repository. Therefore, can not use `${{ secrets.GITHUB_TOKEN }}` since it only works inside github action |

