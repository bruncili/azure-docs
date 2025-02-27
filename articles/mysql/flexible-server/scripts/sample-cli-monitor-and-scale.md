---
title: CLI script - Monitor and scale an Azure Database for MySQL - Flexible Server (Preview)
description: This Azure CLI sample script shows how to monitor and scale a single Azure Database for MySQL - Flexible server up or down to allow for changing performance needs. 
author: shreyaaithal
ms.author: shaithal
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 09/15/2021
---

# Monitor and scale an Azure Database for MySQL - Flexible Server (Preview) using Azure CLI

This sample CLI script scales compute, storage and IOPS for a single Azure Database for MySQL - Flexible server after querying the corresponding metrics. Compute and IOPS can be scaled up or down, while storage can only be scaled up. 

[!INCLUDE [flexible-server-free-trial-note](../../includes/flexible-server-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment](../../../../includes/azure-cli-prepare-your-environment.md)]

- This article requires version 2.0 or later of the Azure CLI. If using Azure Cloud Shell, the latest version is already installed. 

## Sample script

Edit the highlighted lines in the script with your values for variables.

[!code-azurecli-interactive[main](../../../../cli_scripts/mysql/flexible-server/monitor-and-scale/monitor-and-scale.sh?highlight=8,11-12 "Monitor your Flexible Server and scale Compute, Storage and IOPS.")]

## Clean up deployment

After the sample script has been run, the following code snippet can be used to clean up the resources.

[!code-azurecli-interactive[main](../../../../cli_scripts/mysql/flexible-server/monitor-and-scale/clean-up-resources.sh?highlight=4 "Clean up resources.")]


## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| **Command** | **Notes** |
|---|---|
|[az group create](/cli/azure/group#az_group_create)|Creates a resource group in which all resources are stored|
|[az mysql flexible-server create](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_create)|Creates a Flexible Server that hosts the databases.|
|[az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list)|Lists the Azure Monitor metric value for the resources.|
|[az mysql flexible-server update](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_update)|Updates properties of the Flexible Server.|
|[az mysql flexible-server delete](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_delete)|Deletes a Flexible Server.|
|[az group delete](/cli/azure/group#az_group_delete) | Deletes a resource group including all nested resources.|

## Next steps

- Try additional scripts: [Azure CLI samples for Azure Database for MySQL - Flexible Server (Preview)](../sample-scripts-azure-cli.md)
- For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure).