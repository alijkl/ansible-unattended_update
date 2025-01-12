# Unattended Update
* Servers Unattended Update

Configure a server for unattended security update and optionally allow to run the update live + reboot

# Deploy
## singles hosts
```
ansible-playbook playbook-unattended_update.yaml -i inventory/PreProd.yaml -l my_single_instance_name
```
Will run for a hosts set using the '-l' switch

## All
```
ansible-playbook playbook-unattended_update.yaml -i inventory/PreProd.yaml
```
Will run for all the hosts set in the inventory by batch of 3


# Recommendation
Do multiple run for better result.

 * fist run with `run_immediately: false` and `reboot: false` until all the task are OK
 * run with `run_immediately: true` and `reboot: false` with the `-v` verbose option to get an idea of the outcome
 * be patient some host needs to rebuilt initrd if their is a kernel update
 * run  with `run_immediately: true` and `reboot: false` until OK, it should get better and better as more
 packages will incrementally be updated, the task will have less and less to do.
 * run  with `run_immediately: false` and `reboot: true` ansible will wait 3 minutes for the host to come back before marking the task as failed.

# TODO
* implement run_immediately for RedHat like OS
* implement `security_only: true` variable to allow to do simple update (not security related) as well.
* document how to pass the option on the command line
* better implementation of last update End-Date
* implement no install but check only the End-Date and assert pached or not
* use to_datetime filter
* https://docs.ansible.com/ansible/latest/collections/ansible/builtin/to_datetime_filter.html
* maybe implement a master task loop to do all the stage from a single ansible command
