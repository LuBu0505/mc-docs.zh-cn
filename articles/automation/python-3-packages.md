---
title: 在 Azure 自动化中管理 Python 3 程序包
description: 本文介绍了如何在 Azure 自动化中管理 Python 3 程序包（预览版）。
services: automation
ms.subservice: process-automation
origin.date: 12/22/2020
ms.date: 02/22/2021
ms.topic: conceptual
ms.openlocfilehash: 5538a51c51e2b41269a9c2196eafabd7cebde278
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751905"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>在 Azure 自动化中管理 Python 3 程序包（预览版）

通过 Azure 自动化，可以在 Azure 和 Linux 混合 Runbook 辅助角色上运行 Python 3 runbook（预览版）。 为了帮助简化 runbook，可以使用 Python 包导入所需的模块。 本文介绍了如何在 Azure 自动化中管理和使用 Python 3 程序包（预览版）。

## <a name="import-packages"></a>导入程序包

在你的自动化帐户中，在“共享资源”下选择“Python 程序包”。 选择“+ 添加 Python 程序包”。

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="“Python 3 程序包”页的屏幕截图显示了左侧菜单中的“Python 3 程序包”，并突出显示了“添加 Python 2 程序包”。":::

在“添加 Python 程序包”页面上，选择“Python 3”作为版本，然后选择要上传的本地程序包。 包可以是 .whl 或 .tar.gz 文件 。 选择程序包后，选择“确定”以上传程序包。

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="屏幕截图显示了“添加 Python 3 程序包”页面，其中选择了已上传的 tar.gz 文件。":::

导入程序包之后，该程序包将在自动化帐户的“Python 2 程序包”页中列出，位于“Python 3 程序包(预览版)”选项卡下。如果需要删除某个程序包，请选择该程序包并单击“删除”。

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="屏幕截图显示了导入程序包后的“Python 3 程序包”页。":::

## <a name="import-packages-with-dependencies"></a>导入具有依赖项的包

Azure 自动化在导入过程中不会解析 Python 程序包的依赖项。 但是，有一种方法可以导入程序包及其所有依赖项。

### <a name="manually-download"></a>手动下载

在安装了 [Python 3.8](https://www.python.org/downloads/release/python-380/) 和 [pip](https://pip.pypa.io/en/stable/) 的 Windows 64 位计算机上运行以下命令，以便下载程序包及其所有依赖项：

```cmd
C:\Python38\Scripts\pip3.8.exe download -d <output dir> <package name>
```

下载程序包后，可以将其导入到你的自动化帐户中。

## <a name="use-a-package-in-a-runbook"></a>在 runbook 中使用包

导入程序包后，可以在 runbook 中使用它。 添加以下代码以列出 Azure 订阅中的所有资源组。

```python
import os  
import azure.mgmt.resource  
import automationassets  

def get_automation_runas_credential(runas_connection):  
    from OpenSSL import crypto  
    import binascii  
    from msrestazure import azure_active_directory  
    import adal 

    # Get the Azure Automation RunAs service principal certificate  
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")  
    pks12_cert = crypto.load_pkcs12(cert)  
    pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())  

    # Get run as connection information for the Azure Automation service principal 
    application_id = runas_connection["ApplicationId"]  
    thumbprint = runas_connection["CertificateThumbprint"]  
    tenant_id = runas_connection["TenantId"]  

    # Authenticate with service principal certificate  
    resource ="https://management.core.chinacloudapi.cn/"  
    authority_url = ("https://login.partner.microsoftonline.cn/"+tenant_id)  
    context = adal.AuthenticationContext(authority_url)  
    return azure_active_directory.AdalAuthentication(  
    lambda: context.acquire_token_with_client_certificate(  
            resource,  
            application_id,  
            pem_pkey,  
            thumbprint) 
    ) 

# Authenticate to Azure using the Azure Automation RunAs service principal  
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")  
azure_credential = get_automation_runas_credential(runas_connection)  

# Intialize the resource management client with the RunAs credential and subscription  
resource_client = azure.mgmt.resource.ResourceManagementClient(  
    azure_credential,  
    str(runas_connection["SubscriptionId"]),
    "2017-05-10",
    "https://management.chinacloudapi.cn"))  

# Get list of resource groups and print them out  
groups = resource_client.resource_groups.list()  
for group in groups:  
    print(group.name) 
```

## <a name="next-steps"></a>后续步骤

要准备 Python runbook，请参阅[创建 Python runbook](learn/automation-tutorial-runbook-textual-python-3.md)。
