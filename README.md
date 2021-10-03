# Az Monitor Alert View
Get All Azure monitor Alerts  with their description details to Azure Dashboard


Display of alerts screen in Azure is available only on the screen of the Azure monitor, without the ability to PIN the all screan to dashboard 

![image](https://user-images.githubusercontent.com/24368496/135747723-2127fbe5-bfaa-4f58-b495-6a60f7e15165.png)

You can pin the blade and get the link to the alerts screen, the MOM screen is not an option

![image](https://user-images.githubusercontent.com/24368496/135748520-f1b657fd-a587-4eea-8cb8-28f5653187af.png)

What i found best and usefull is to use "Azure Resource Graph Explorer" tha table "AlertsManagementResources" conatin all alerts

![image](https://user-images.githubusercontent.com/24368496/135749912-504dd3eb-ffa9-4b5f-9e74-2823c368b28f.png)

Exmpale of query [KQL], it will list the open and closed alerts (change the query and icons according to your needs) in the last seven days

AlertsManagementResources
| where type =~ 'Microsoft.AlertsManagement/alerts'
| where isnotnull(properties.essentials.description)
| project Description = properties.essentials.description, name, State = properties.essentials.alertState, Severity = properties.essentials.severity, Condition = properties.essentials.monitorCondition, ResourceType = properties.essentials.targetResourceType, Resource = properties.essentials.targetResourceName, LastModifiedDate = todatetime (properties.essentials.lastModifiedDateTime)
| extend Severity = case (Severity == "Sev4","ℹ️", Severity == "Sev3","ℹ️", Severity == "Sev2","⚠️",Severity == "Sev1","⚠️","❌" )
| where LastModifiedDate > ago(7d)
//| project name, Severity, Resource, LastModifiedDate
| order by LastModifiedDate


