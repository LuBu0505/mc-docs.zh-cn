---
title: 管理身份的最佳做法
titleSuffix: Azure Kubernetes Service
description: 了解有关为 Azure Kubernetes 服务 (AKS) 中的群集管理身份验证和授权的群集操作员最佳做法
services: container-service
ms.topic: conceptual
origin.date: 07/07/2020
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
author: rockboyfor
ms.openlocfilehash: d00dfb86a4a1f938357c11844592d6d9d93fe8f8
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054355"
---
# <a name="best-practices-for-authentication-and-authorization-in-azure-kubernetes-service-aks"></a>有关 Azure Kubernetes 服务 (AKS) 中的身份验证和授权的最佳做法

在 Azure Kubernetes 服务 (AKS) 中部署和维护群集时，需要实施相应的方法来管理对资源和服务的访问。 如果不采用这些控制措施，帐户可能会有权访问它们不需要访问的资源和服务。 此外，可能难以跟踪用于做出更改的凭据集。

本最佳做法文章重点介绍群集操作员如何管理 AKS 群集的访问和标识。 在本文中，学习如何：

> [!div class="checklist"]
>
> * 使用 Azure Active Directory 对 AKS 群集用户进行身份验证
> * 使用 Kubernetes 基于角色的访问控制 (Kubernetes RBAC) 控制对资源的访问权限
> * 使用 Azure RBAC 大规模精细控制对 AKS 资源和 Kubernetes API 的访问权限，以及对 kubeconfig 的访问权限。
> * 使用托管标识在其他服务中对 Pod 本身进行身份验证

## <a name="use-azure-active-directory"></a>使用 Azure Active Directory

**最佳做法指导** - 使用 Azure AD 集成部署 AKS 群集。 使用 Azure AD 可以集中化标识管理组件。 访问 AKS 群集时，用户帐户或组状态的任何更改会自动更新。 根据下一部分所述，使用角色或群集角色和绑定，将用户或组的权限范围限定为所需的最少量权限。

Kubernetes 群集的开发人员和应用程序所有者需要访问不同的资源。 Kubernetes 不提供标识管理解决方案来控制哪些用户可与哪些资源交互。 通常，你会将群集与现有的标识解决方案相集成。 Azure Active Directory (AD) 提供企业就绪的标识管理解决方案，并可与 AKS 群集相集成。

使用 AKS 中与 Azure AD 集成的群集，创建角色或群集角色用于定义对资源的访问权限。  然后，从 Azure AD 将角色绑定到用户或组。 下一部分将介绍这些 Kubernetes 基于角色的访问控制 (Kubernetes RBAC)。 下图显示了 Azure AD 集成，以及如何控制对资源的访问：

:::image type="content" source="media/operator-best-practices-identity/cluster-level-authentication-flow.png" alt-text="与 AKS 集成的 Azure Active Directory 的群集级身份验证":::

1. 开发人员在 Azure AD 中进行验证身份。
1. Azure AD 令牌颁发终结点颁发访问令牌。
1. 开发人员使用 Azure AD 令牌执行操作，例如 `kubectl create pod`
1. Kubernetes 使用 Azure Active Directory 验证令牌，并提取开发人员的组成员身份。
1. 应用 Kubernetes 基于角色的访问控制 (Kubernetes RBAC) 和群集策略。
1. 开发人员的请求成功或失败，具体取决于前面的 Azure AD 组成员身份和 Kubernetes RBAC 验证以及策略。

若要创建使用 Azure AD 的 AKS 群集，请参阅[将 Azure Active Directory 与 AKS 集成][aks-aad]。

## <a name="use-kubernetes-role-based-access-control-kubernetes-rbac"></a>使用 Kubernetes 基于角色的访问控制 (Kubernetes RBAC)

**最佳做法指导** - 使用 Kubernetes RBAC 定义用户或组对群集中的资源拥有的权限。 创建角色和绑定，用于分配所需的最少量权限。 与 Azure AD 集成，使用户状态或组成员身份的任何更改可自动更新，并使群集资源访问权限保持最新状态。

在 Kubernetes 中，可以针对群集中的资源提供精细的访问控制。 在群集级别或者针对特定的命名空间定义权限。 可以定义能够使用哪些权限管理哪些资源。 然后，将这些角色通过绑定应用于用户或组。 有关角色、群集角色和绑定的详细信息，请参阅 [Azure Kubernetes 服务 (AKS) 的访问和标识选项][aks-concepts-identity]。  

例如，可以创建一个角色，用于授予对名为 *finance-app* 的命名空间中的资源的完全访问权限，如以下示例 YAML 清单中所示：

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role
  namespace: finance-app
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
```

然后创建 RoleBinding，用于将 Azure AD 用户 *developer1\@contoso.com* 绑定到该 RoleBinding，如以下 YAML 清单所示：

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role-binding
  namespace: finance-app
subjects:
- kind: User
  name: developer1@contoso.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: finance-app-full-access-role
  apiGroup: rbac.authorization.k8s.io
```

*developer1\@contoso.com* 通过 AKS 群集进行身份验证后，便对 *finance-app* 命名空间中的资源拥有了完全权限。 这样，即可以逻辑方式隔离和控制对资源的访问权限。 应根据上一部分中所述，将 Kubernetes RBAC 与 Azure AD 集成结合使用。

若要了解如何使用 Azure AD 组通过 Kubernetes RBAC 来控制对 Kubernetes 资源的访问，请参阅[在 AKS 中使用基于角色的访问控制和 Azure Active Directory 标识来控制对群集资源的访问][azure-ad-rbac]。

## <a name="use-azure-rbac"></a>使用 Azure RBAC 
**最佳做法指导** - 使用 Azure RBAC 来定义用户或组对一个或多个订阅中 AKS 资源拥有的所需最低权限。

完全操作 AKS 群集需要一个级别的访问权限： 
1. 访问 Azure 订阅上的 AKS 资源。 具有此访问级别，可以使用 AKS API 来控制群集的缩放或升级，还可以拉取 kubeconfig。
    要查看如何控制对 AKS 资源和 kubeconfig 的访问权限，请参阅[限制对群集配置文件的访问](control-kubeconfig-access.md)。

<!--NOT AVAILABLE ON [Use Azure RBAC for Kubernetes authorization](manage-azure-rbac.md)-->

<!--NOT AVAILABLE ON  ## Use Pod-managed Identities-->
<!--NOT AVAILABLE ON https://docs.azure.cn/aks/use-azure-ad-pod-identity-->
<!--NOT AVAILABLE ON  az feature register --name EnablePodIdentityPreview --namespace Microsoft.ContainerService-->
## <a name="next-steps"></a>后续步骤

本最佳做法文章重点介绍了群集和资源的身份验证与授权。 若要实施其中的某些最佳做法，请参阅以下文章：

* [将 Azure Active Directory 与 AKS 集成][aks-aad]

<!--NOT AVAILABLE ON https://docs.azure.cn/aks/use-azure-ad-pod-identity-->

有关 AKS 中的群集操作的详细信息，请参阅以下最佳做法：

* [多租户和群集隔离][aks-best-practices-cluster-isolation]
* [基本 Kubernetes 计划程序功能][aks-best-practices-scheduler]
* [高级 Kubernetes 计划程序功能][aks-best-practices-advanced-scheduler]

<!-- EXTERNAL LINKS -->

[aad-pod-identity]: https://github.com/Azure/aad-pod-identity

<!-- INTERNAL LINKS -->

[aks-concepts-identity]: concepts-identity.md
[aks-aad]: azure-ad-integration-cli.md

<!--Not Available on [managed-identities]: ../active-directory/managed-identities-azure-resources/overview.md-->

[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[azure-ad-rbac]: azure-ad-rbac.md

<!--Update_Description: update meta properties, wording update, update link-->