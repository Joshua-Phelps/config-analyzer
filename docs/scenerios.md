# AZA - Configuration Analyzer Scenarios

## What is it?
A CLI application that can scan Azure Resources for configuration issues.

## Behavior
It returns Resources that break a rule along with documentation on how to fix it.    
A single rule can be run against all supported scenarios for that type of rule without changing the code. For instance, a rule for an exported template should also work for a user supplied template.  
A user can author their own rules for supported scenarios.  
Rule support multiple evaluations.    

## Scenarios

### ARM Templates 

#### Exported ARM Templates - MVP
- A User should be able to author and run rules against existing Azure Resources
- User must supply the Subscription ID to use ResourceManagementClient
- Scopes 
    - Resource Group - Scans across the Resource Group
        - Group name
    - Deployment - Scans across the resources in the deployment
        - Resource Group
            - Group name
            - Deployment name
        - Subscription
            - Subscription Id
            - Deployment name
        - Managment Group
            - Group Id
            - Deployment name
        - Tenant
            - Deployment name
        - Scope
            - scope
                - /subscriptions/{subscriptionId}
                - /providers/Microsoft.Management/managementGroups/{managementGroupName}
                - /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}
            - Deployment name  

#### User Supplied ARM Templates
- A User should be able to author and run rules against a user supplied template
- Challenges
    - user defined functions
    - functions across scopes
    - linked templates
    - nested templates 
    - parameters/variables
    - template validation?
    - "copy"
- Scopes - "$schema"
    - Resource group => https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#
    - Subscription => https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#
    - Management => https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
    - Tenant => https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#    

#### Bicep to JSON ARM Templates
- presents same challenges as user supplied templates 
- other challenges may be presented upon further inspection


### Resource Graph
- scans existing Azure resources
- User can write and execute Resource Graph queries
- Scopes
    - Resource Groups - one or multiple in a single subscription
    - Subscriptions - one or multiple 
#### Limitations 
- Throttling
    - We haven't run into this issue yet, but if more rules were written then this would need to be addressed
    - [guidance](https://docs.microsoft.com/en-us/azure/governance/resource-graph/concepts/guidance-for-throttled-requests)
- Unsupported Resource Types 
    - unsupported types we've needed for specific rules
        - Microsoft.EventHub/namespaces/networkrulesets  
- Can only scan existing Resources   