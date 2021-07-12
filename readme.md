# Monitoring Implementation

## Log Analytics Workspace

Customer should confirm if new workspace needs to be created or an existing one can be used? 

Please review [Workspace Consideratiions](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/design-logs-deployment#important-considerations-for-an-access-control-strategy ) when deciding to create new or existing workspace and who can access it

The workspace will be used to collect any relevent event and log data from the various sources we need to monitoring in Azure.

Please follow the [Create a Log Analytics Workspace](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace) if a new workpsace needs to be created.

## Action Groups

Following KPI review the relevant action groups should be set up for the alerts. These will be the relevant mail teams for each compent that has been discussed in the monitoring disuccsion workshop. An example arm template can be found in the tempaltes section.  

## Service Health

Service health alerts should be set up in adherence to the the KPI disucssion session. Exmaple templates for setting up a service health alert can be found 


## Virtual Machines

To monitor Virtual Machines we will use the **VM Insights**. This gives us a standardized and recommend set of data collection and builtin visualisations to help indentify performance bottlenecks and dependencies 

Please follow [Add VM Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-configure-workspace?tabs=CLI#add-vminsights-solution-to-workspace) to add the solution to the workspace. 

The virtual machines will require the agents to be installed. It is recommended to use Azure Policy to deploy the agents. Please use the [Enable VM Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-enable-policy) initiative to deploy the Log analytics and Dependency agent to the targetted VMs at the relevant scope. 


## Recovery Vaults

Recovery vaults diagnostics should be set up using policy. Please refer to [configure Vault Diagnostics at scale](https://docs.microsoft.com/en-gb/azure/backup/azure-policy-configure-diagnostics) to set this up. For alerting we can configure alet within the vault itself abd configure alerts, These can be set to summarise failure in an hourly digest to avoid alert fatigue for similar issues. 














