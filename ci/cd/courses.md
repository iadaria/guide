github actions:
done: 51/63
next: 52

# 1 Github actions
workflow - рабочий процес

- event (push request, push, )
- workflow (event triggers a workflow) - process
- job (workflow contains one or more jobs)
- step (each jobs performs some step)
- action
- virtual environemnt
- runners
- github-hosted runner
- self-hosted runners

### YAML format
arrays:
  - key: value
    key2: value2
    key3: value3
  - key: value
    key1: value1
  
multiple string: >
  string string2
  string3 string4
  string5

### debug
add secrets: ACTIONS_RUNNER_DEBUG and ACTIONS_STEP_DEBUG

### Marketplace actions
https://github.com/marketplace?type=actions

### cron
https://crontab.guru

### env
https://help.github.com/en/actions/automating-your-workflow-with-github-actions/using-environment-variables

ecrypted env:
- secrets in Settings of github