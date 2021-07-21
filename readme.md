# Key Performance Indicators

Following mapping out of the Service we now need to determine what are the key performance indicators are for each component, how they are recorded, how we can monitor them and if applicable what threshold should be set for alerting.  These will need to be recored so we can implement the alerts and dashboards following KPI disucussion.  An individual  [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx) sheet should be saved for customer recording what thresholds and key performance indciators have been set to be configured later. For a list of supported metrics that can be used for each resource type please refer to the [platform metrics](https://docs.microsoft.com/en-us/azure/azure-monitor/essentials/metrics-supported) documentation.  


## Actions and Notifications

Discussion needs to made on the set up action groups. Do different people or teams need to be informed dependent on the components or issue? Does the customer needs alerts sent to a 3rd party system or ITSM tools? **Action Groups will only be set up for email notifcations in this release** . If other actions are required for alerts these can be added subsequently to the action groups created. Action groups carry a short and long name. These should adhere to naming convention agreed and in line with other naming conventions already set within current Azure deployment. 

## Components

In this section we cover off the various components discovered in the workshop to discuss what KPI are for each components and what thresholds for alerts we can set up for them. 

### Service and Resource Health

Review current Service and Resource Health and confirm the criteria for the service and resource health alerts and if customer wants alerts for all service health alerts and the criteria they want for the resource health alerts. Example has been set in the [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx) sheet. Careful consideration should be taken to see if customer wants to be alerted for User Initiated as well as Platform iInitiated resource health events. An example of a user initiated event would be if a virtual machine is gracefully shut down.  It is advised, to ensure you do not get false postives for unknown health events, to filter out unknown health events. The example template in the reposistory filters out these events 

### Activity Log 

Review key components in design to confirm if we want any activity log alerts for for these Examples could include route table or NSG updates or deletion of resources in production. Other examples could be for Autoscale events of VMs or failure to Autoscale.  An example arm template can be found [here](https://github.com/Azure/azure-quickstart-templates/tree/master/demos/monitor-autoscale-failed-alert). The activity log should be also be collected into a Log Analytics workspace and set via policy. The builtin Policy Definition name is "Configure Azure Activity logs to stream to specified Log Analytics workspace". 

### Endpoints

Discuss what endpoints need to be monitored and if they are publically or internally available. These should be recorded in the  [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx) for later reference. 

### Virtual Machines

Discuss what Virtual Machines are in place , would they need the same threshold set for them or would different thresholds need to be set for different VMS in scope? Some example thresholds have been set in the [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx) sheet.  Review current CPU, Memory usage via metric exploror to determine current levels and what threshold should be set initially. 

### Containers and Kubernetes Services 

AKS cluster now supports [predefined alerts](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-metric-alerts) . Review these alerts to confirm if customers happy with enabling them.  

### Key Vault

A key vault availability can be crucial for operation of the application. Example thesholds have been set in in the  [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx) sheet for some key metrics in the operation or Key Vault. 

### Storage

Review the [storage metrics](https://docs.microsoft.com/en-us/azure/storage/blobs/monitor-blob-storage-reference#) available for effecttive monitoring. Example threshold for non-dimension specific alerts have been set in the example [Monitoring Pattern](MonitoringPattern\azuremonitoringpattern.xlsx)

Storage also has a set [dimension for metrics](https://docs.microsoft.com/en-us/azure/storage/blobs/monitor-blob-storage-reference#metrics-dimensions) for more.effective monitoring for known errors depending on the storage type. Review to see if further alerts should be set. 

### App Service 







# Monitoring Implementation

## Log Analytics Workspace

Customer should confirm if new workspace needs to be created or an existing one can be used? 

Please review [Workspace Consideratiions](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/design-logs-deployment#important-considerations-for-an-access-control-strategy ) and [Management and Monitoring](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring) of Enterpise scale documentation  when deciding to create new or existing workspace and who can access it. 

The workspace will be used to collect any relevent event and log data from the various sources we need to monitoring in Azure. This should 

Please follow the [Create a Log Analytics Workspace](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace) if a new workpsace needs to be created.

## Diagnostic Settings

Review of diagnostics settings for all key components. Enab;eIf Diagnostics are required for 

Diagnostics should be set via Policy.  Review each resource type documentnation for policy for the diagnostic and resource logs for each. A policy intiative can be set for diagnostic settings and should be set at a Management Group level if possible. Not all resoure type have builtin policies to set respurce logs and custom polcies can be found in the Enterprise Scale deployment if followed


## Action Groups

Following KPI review the relevant action groups should be set up for the alerts. These will be the relevant mail teams for each compent that has been discussed in the monitoring disuccsion workshop. An example arm template can be found in the [templates section](Templates\ActionGroupTemplate.json) of the repository with relevant [parameter file](Templates\ParameterFiles\ActionGrpParams.json).   

## Service Health

Service health alerts should be set up in adherence to the the KPI disucssion session. Example templates for setting up a service health alert can be found here 

## Resource Health

Resource Health alert should be set up in accordance with KPI discussion. Example arm templates for deployment can be found in the [templates section](Templates\ResourceHealthTemplate.json). The example template only alerts on Unavailable or Degraded health events that have been Platform initiated.  


## Virtual Machines

To monitor Virtual Machines we will use the **VM Insights**. This gives us a standardized and recommend set of data collection and builtin visualisations to help indentify performance bottlenecks and dependencies 

Please follow [Add VM Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-configure-workspace?tabs=CLI#add-vminsights-solution-to-workspace) to add the solution to the workspace. 

The virtual machines will require the agents to be installed. It is recommended to use Azure Policy to deploy the agents. Please use the [Enable VM Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-enable-policy) initiative to deploy the Log analytics and Dependency agent to the targetted VMs at the relevant scope. 


## Recovery Vaults

Recovery vaults diagnostics should be set up using policy. Please refer to [configure Vault Diagnostics at scale](https://docs.microsoft.com/en-gb/azure/backup/azure-policy-configure-diagnostics) to set this up.  If following KPI disucssion we are to use the [Recovery Vault BackUps Alerts](https://docs.microsoft.com/en-gb/azure/backup/backup-azure-monitoring-built-in-monitor#backup-alerts-in-recovery-services-vault). These need to be configured directly on the console as there is  PS/CLI/REST API/Azure Resource Manager Template support for these alerts. If other alerting needs to be set up please refer to the 


## Containers 

For the AKS clusterr please [enable container insights](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-enable-existing-clusters#enable-using-an-azure-resource-manager-template) using one fo the methods listed. For long term supportability using the customer policy would be the recommended method for deployment. 

## Storage

For storage review the Storage Insight which  


## App Service 

Ensure [diagnostic settings are enabled for the app service](https://docs.microsoft.com/en-us/azure/app-service/troubleshoot-diagnostic-logs) and 
https://docs.microsoft.com/en-us/azure/app-service/monitor-app-service















