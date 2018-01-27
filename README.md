Run Ansible tasks in parallel
=============================

When you use Ansible to provision hosts, running a task on a set of hosts
in parallel is not a problem. You simply use the `serial` keyword in a playbook.
See: http://docs.ansible.com/ansible/latest/playbooks_delegation.html#rolling-update-batch-size

This Ansible repository is about when you want to run a task several times in parallel, either on each
host, or on a single host. A common use case is when you want to provision infrastructure at a cloud provider,
such as Amazon Web Services (AWS).

Creating a single AWS EC2 instance or a RDS instance takes a long time.
If you want to create several, you can use a loop and wait for each to finish,
but that takes a long time. As a alternative, you can create them all in parallel.

The playbook `run-in-parallel.yml` in this repository is an example of how you
can run any Ansible task in parallel and wait for them all to finish.

One of the plays runs the tasks in parallel, and one runs then in sequence, for comparison.

## How to run example
`ansible-playbook run-in-parallel.yml`

### Example output
```
$ ansible-playbook run-in-parallel.yml 

PLAY [Run tasks in parallel] **********************************************************************************************************

TASK [Pretend to create instances] ****************************************************************************************************
changed: [localhost] => (item=6)
changed: [localhost] => (item=8)
changed: [localhost] => (item=7)

TASK [Wait for creation to finish] ****************************************************************************************************
FAILED - RETRYING: Wait for creation to finish (10 retries left).
changed: [localhost] => (item={'_ansible_parsed': True, '_ansible_item_result': True, '_ansible_no_log': False, u'ansible_job_id': u'957883465297.7487', 'item': 6, u'started': 1, 'changed': True, 'failed': False, u'finished': 0, u'results_file': u'/home/max/.ansible_async/957883465297.7487'})
FAILED - RETRYING: Wait for creation to finish (10 retries left).
changed: [localhost] => (item={'_ansible_parsed': True, '_ansible_item_result': True, '_ansible_no_log': False, u'ansible_job_id': u'450659566541.7513', 'item': 8, u'started': 1, 'changed': True, 'failed': False, u'finished': 0, u'results_file': u'/home/max/.ansible_async/450659566541.7513'})
changed: [localhost] => (item={'_ansible_parsed': True, '_ansible_item_result': True, '_ansible_no_log': False, u'ansible_job_id': u'763185500456.7538', 'item': 7, u'started': 1, 'changed': True, 'failed': False, u'finished': 0, u'results_file': u'/home/max/.ansible_async/763185500456.7538'})

PLAY [Run tasks in sequence] **********************************************************************************************************

TASK [Pretend to create instances] ****************************************************************************************************
changed: [localhost] => (item=6)
changed: [localhost] => (item=8)
changed: [localhost] => (item=7)

PLAY RECAP ****************************************************************************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0   

```

## Notes
* Using Ansible 2.4

## Author
Max Wenzin, Crisp
