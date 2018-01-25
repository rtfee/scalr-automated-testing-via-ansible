# scalr-automated-testing-via-ansible
## Setup instructions

The instructions below are written for Ubuntu 16.04. Adapt as necessary for other distributions.

-Scalr Setup
1. Log into Scalr and create a farm with one farm role
2. Make sure the scaling is set to automatic
3. Save farm (do not click "save and launch", part of the test is automatically launching the farm)

## Setup on your testing server
This can be run from any server/workstation. Generally this is used in a scheduling tool like Jenkins or can be simple as using Cron. This playbook will take care of launching/terminating a farm and recording the result, it will be up to you to monitor those results and send to your monitoring tool.

- Install the required packages:
```
apt-get install ansible python-pip -y
pip install scalr-ctl
```

- Update the testing_creds.yml file, example:
```
API_HOST: scalr.example.com
API_KEY_ID: enter_api_key_id_here
API_SCHEME: https
API_SECRET_KEY: enter_api_secret_key_here
SSL_VERIFY_PEER: true
accountId: '1'
colored_output: true
envId: '1'
view: tree
```

- Update the vars.yml, example:
```
---
env: 2
farm: 10
scalrsuccess: '{"name": "scalr_operations", "output": "Succesfully Launched Scalr Farm", "status": 0}'
scalrfailure: '{"name": "scalr_operations", "output": "Unsuccesfully Launched Scalr Farm", "status": 2}'
```

- Run a test: 
```
ansible-playbook test-playbook.yml
```

If the above test is successful then you are ready to set this on a schedule.
