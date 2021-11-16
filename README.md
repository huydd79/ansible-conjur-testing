# ansible-conjur-testing
These are some samples to compare ansible playbook running with and without using conjur as secrets manager.

To run secure playbook, deploy Conjur Ansible collection and grant the conjur id role to ansible host.
For more detail of environment setting up, refer to below online document
https://docs.conjur.org/Latest/en/Content/Integrations/ansible.html

#### Conjur Ansible collection setup

`$ansible-galaxy install cyberark.conjur-host-identity`

`$ansible-galaxy collection install cyberark.conjur`

#### Granting conjur role to ansible host
- Getting host factory token from conjur environment (using conjur cli docker version or generating from Conjur Enterprise Admin GUI)
- Exporting host factory token as environment parameter
`export HFTOKEN=<host_factory_token_value>`
- Generating conjur certificate file 
`$sudo echo | openssl s_client -connect https://<conjur-host> 2>&1 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' >/etc/conjur.pem`
- Running grant_conjur_id playbook `run_ansible-grant_conjur_id.sh`
- Checking for result:
  + New hostID created in conjur environment
  + New configuration file created:
     - /etc/conjur.conf
     - /etc/conjur.identity

#### Testing secure playbook
Double check the serect paths in Conjur and playbook file

Run the script `run_ansible-secure.sh` and check for result
