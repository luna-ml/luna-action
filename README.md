# Luna ML Github action

This Github action helps to verify [Luna ML Leaderboard](https://github.com/luna-ml/luna) projects files, evaluate and score locally on pullrequest. So it can be verified before when a new project or a new model is submitted.

After pullrequest merge, this action sync the changes with Luna ML server.

If you're staring a new leaderboard project, you can start from [Luna project template](https://github.com/luna-ml/luna-project-template).


## Usage

Basic

```yaml
name: Verify, evaluate and score
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
    - name: Setup KIND # This action requires local kubernetes cluster when 'server' parameter is not configured.
      uses: engineerd/setup-kind@v0.5.0
      with:
        version: "v0.9.0"
    - uses: luna-ml/luna-action@main
      with:
        server: 'https://luna-server'
        version: 'main'
```

Optional input parameters

| parameter | Description |
| ------- | --------- |
| server | [Luna ML](https://github.com/luna-ml/luna)  server address. e.g. `https://luna-ml.org`. remote server address to sync changes. When the 'server' parameter is not configured, then this action locally run a server and sync with it. Local sync is useful for evaluating new module in pull request |
| version | [Luna ML](https://github.com/luna-ml/luna) client version to use. Defaults to `main`. |
