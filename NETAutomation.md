# Network Automation

Tools used to automate network tasks:
 - SDN
 - Ansible
 - Puppet
 - Python scripts


### Network Automation Tools
Name| Client-SRV| Op-Model| Comms-Protocal| Port| Written-by| Config-file-format| Key Components| 
----|-----------|---------|---------------|-----|-----------|-------------------|---------------|
Ansible| agentless| push model| SSH| 22| Python| YAML | Control Node: Inventory, Template, Variable, Playbook|
Puppet| agent-based; 'agentless'| pull model| HTTP(s)| 8140| Ruby| Custom declarative language | Puppet Master: Manifest , Templates|
Chef| agent-based| pull model| 10002| Ruby| HTTP(s)|  | Resources, Recipes, Cookbooks, Run-list|
