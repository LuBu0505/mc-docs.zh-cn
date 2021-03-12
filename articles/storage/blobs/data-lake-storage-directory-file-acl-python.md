---
title: 使用 Python 管理 Azure Data Lake Storage Gen2 中的数据
description: 使用 Python 在启用了分层命名空间的存储帐户中管理目录和文件。
author: WenJason
ms.service: storage
origin.date: 02/17/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.custom: devx-track-python
ms.openlocfilehash: ca7046357fb6b57fe1755ea6ccd9ca081d70ddd0
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196312"
---
# <a name="use-python-to-manage-directories-and-files-in-azure-data-lake-storage-gen2"></a>使用 Python 管理 Azure Data Lake Storage Gen2 中的目录和文件

本文介绍了如何使用 Python 在具有分层命名空间的存储帐户中创建和管理目录与文件。

若要了解如何获取、设置和更新目录与文件的访问控制列表 (ACL)，请参阅[使用 Python 管理 Azure Data Lake Storage Gen2 中的 ACL](data-lake-storage-acl-python.md)。

[包（Python 包索引）](https://pypi.org/project/azure-storage-file-datalake/) | [示例](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples) | [API 参考](https://docs.microsoft.com/python/api/azure-storage-file-datalake/azure.storage.filedatalake) | [提供反馈](https://github.com/Azure/azure-sdk-for-python/issues)

## <a name="prerequisites"></a>先决条件

- Azure 订阅。 请参阅[获取 Azure 试用版](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

- 一个已启用分层命名空间的存储帐户。 按[这些](create-data-lake-storage-account.md)说明创建一个。

## <a name="set-up-your-project"></a>设置项目

使用 [pip](https://pypi.org/project/pip/) 安装适用于 Python 的 Azure Data Lake Storage 客户端库。

```
pip install azure-storage-file-datalake
```

将这些 import 语句添加到代码文件的顶部。

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

## <a name="connect-to-the-account"></a>连接到帐户

若要使用本文中的代码片段，需创建一个表示存储帐户的 **DataLakeServiceClient** 实例。 

### <a name="connect-by-using-an-account-key"></a>使用帐户密钥进行连接

这是连接到帐户的最简单方法。 

此示例使用帐户密钥创建 DataLakeServiceClient 实例。

```python
def initialize_storage_account(storage_account_name, storage_account_key):
    
    try:  
        global service_client

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.chinacloudapi.cn".format(
            "https", storage_account_name), credential=storage_account_key)
    
    except Exception as e:
        print(e)
```
 
- 将 `storage_account_name` 占位符值替换为存储帐户的名称。

- 将 `storage_account_key` 占位符值替换为存储帐户访问密钥。

### <a name="connect-by-using-azure-active-directory-azure-ad"></a>使用 Azure Active Directory (Azure AD) 进行连接

可以使用[适用于 Python 的 Azure 标识客户端库](https://pypi.org/project/azure-identity/)，通过 Azure AD 对应用程序进行身份验证。

此示例使用客户端 ID、客户端密码和租户 ID 创建 DataLakeServiceClient 实例。  若要获取这些值，请参阅[从 Azure AD 获取用于请求客户端应用程序授权的令牌](../common/storage-auth-aad-app.md)。

```python
def initialize_storage_account_ad(storage_account_name, client_id, client_secret, tenant_id):
    
    try:  
        global service_client

        credential = ClientSecretCredential(tenant_id, client_id, client_secret)

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.chinacloudapi.cn".format(
            "https", storage_account_name), credential=credential)
    
    except Exception as e:
        print(e)
```

> [!NOTE]
> 有关更多示例，请参阅[适用于 Python 的 Azure 标识客户端库](https://pypi.org/project/azure-identity/)文档。

## <a name="create-a-container"></a>创建容器

容器充当文件的文件系统。 可以通过调用 **FileSystemDataLakeServiceClient.create_file_system** 方法来创建一个。

此示例创建一个名为 `my-file-system` 的容器。

```python
def create_file_system():
    try:
        global file_system_client

        file_system_client = service_client.create_file_system(file_system="my-file-system")
    
    except Exception as e:
        print(e) 
```


## <a name="create-a-directory"></a>创建目录

通过调用 **FileSystemClient.create_directory** 方法来创建目录引用。

此示例将名为 `my-directory` 的目录添加到容器中。 

```python
def create_directory():
    try:
        file_system_client.create_directory("my-directory")
    
    except Exception as e:
     print(e)
```

## <a name="rename-or-move-a-directory"></a>重命名或移动目录

通过调用 **DataLakeDirectoryClient.rename_directory** 方法来重命名或移动目录。 以参数形式传递所需目录的路径。 

此示例将子目录重命名为 `my-subdirectory-renamed` 的名称。

```python
def rename_directory():
    try:
       
       file_system_client = service_client.get_file_system_client(file_system="my-file-system")
       directory_client = file_system_client.get_directory_client("my-directory")
       
       new_dir_name = "my-directory-renamed"
       directory_client.rename_directory(rename_destination=directory_client.file_system_name + '/' + new_dir_name)

    except Exception as e:
     print(e)
```

## <a name="delete-a-directory"></a>删除目录

通过调用 **DataLakeDirectoryClient.delete_directory** 方法来删除目录。

此示例删除名为 `my-directory` 的目录。  

```python
def delete_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")
        directory_client = file_system_client.get_directory_client("my-directory")

        directory_client.delete_directory()
    except Exception as e:
     print(e)
```

## <a name="upload-a-file-to-a-directory"></a>将文件上传到目录

首先，通过创建 **DataLakeFileClient** 类的实例，在目标目录中创建文件引用。 通过调用 **DataLakeFileClient.append_data** 方法上传文件。 确保通过调用 **DataLakeFileClient.flush_data** 方法完成上传。

此示例将文本文件上传到名为 `my-directory` 的目录。   

```python
def upload_file_to_directory():
    try:

        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.create_file("uploaded-file.txt")
        local_file = open("C:\\file-to-upload.txt",'r')

        file_contents = local_file.read()

        file_client.append_data(data=file_contents, offset=0, length=len(file_contents))

        file_client.flush_data(len(file_contents))

    except Exception as e:
      print(e)
```

> [!TIP]
> 如果文件很大，则代码必须多次调用 DataLakeFileClient.append_data 方法。 请考虑改用 DataLakeFileClient.upload_data 方法。 这样就可以在单个调用中上传整个文件。 

## <a name="upload-a-large-file-to-a-directory"></a>将大型文件上传到目录

使用 DataLakeFileClient.upload_data 方法上传大型文件，无需多次调用 DataLakeFileClient.append_data 方法 。

```python
def upload_file_to_directory_bulk():
    try:

        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.get_file_client("uploaded-file.txt")

        local_file = open("C:\\file-to-upload.txt",'r')

        file_contents = local_file.read()

        file_client.upload_data(file_contents, overwrite=True)

    except Exception as e:
      print(e)
```

## <a name="download-from-a-directory"></a>从目录下载 

打开用于写入的本地文件。 然后，创建一个 **DataLakeFileClient** 实例，该实例表示要下载的文件。 调用 **DataLakeFileClient.read_file**，以便从文件读取字节，然后将这些字节写入本地文件。 

```python
def download_file_from_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        local_file = open("C:\\file-to-download.txt",'wb')

        file_client = directory_client.get_file_client("uploaded-file.txt")

        download = file_client.download_file()

        downloaded_bytes = download.readall()

        local_file.write(downloaded_bytes)

        local_file.close()

    except Exception as e:
     print(e)
```

## <a name="list-directory-contents"></a>列出目录内容

通过调用 **FileSystemClient.get_paths** 方法列出目录内容，然后枚举结果。

此示例输出名为 `my-directory` 的目录中的每个子目录和文件的路径。

```python
def list_directory_contents():
    try:
        
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        paths = file_system_client.get_paths(path="my-directory")

        for path in paths:
            print(path.name + '\n')

    except Exception as e:
     print(e)
```

## <a name="see-also"></a>另请参阅

- [API 参考文档](https://docs.microsoft.com/python/api/azure-storage-file-datalake/azure.storage.filedatalake)
- [包（Python 包索引）](https://pypi.org/project/azure-storage-file-datalake/)
- [示例](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
- [已知问题](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
- [提供反馈](https://github.com/Azure/azure-sdk-for-python/issues)
