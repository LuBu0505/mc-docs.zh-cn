---
title: “一对多”多类分类
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习设计器中的“一对多”多类分类模块创建一组二进制分类模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/13/2020
ms.openlocfilehash: ee7d6fc9ea1f5a936b0f317b18f8280a29265ad9
ms.sourcegitcommit: c2c9dc65b886542d220ae17afcb1d1ab0a941932
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94978050"
---
# <a name="one-vs-all-multiclass"></a>“一对多”多类分类

本文介绍如何使用 Azure 机器学习设计器中的“一对多”多类分类模块。 目的是创建一个分类模型，该模型可以使用“一对多”方法对多类分类进行预测  。

当结果依赖于连续的或分类的预测器变量时，此模块可用于创建预测三个或更多可能结果的模型。 使用此方法还能对需要多个输出类的问题使用二元分类方法。

### <a name="more-about-one-versus-all-models"></a>有关一对多模型的详细信息

某些分类算法允许在设计中使用两个以上的类。 其他分类算法会将可能的结果限制为两个值中的一个（二进制或双类模型）。 但即使是二元分类算法也可以通过各种策略应用于多类分类任务。 

此模块实现一对多方法，此方法为每个输出类创建一个二元模型。 该模块针对各个类的补集（模型中的所有其他类）评估各个类的二元模型，就像处理二元分类一样。 除了它的计算效率（只需要 `n_classes` 分类器）外，这种方法的一个优点是它的可解释性。 由于每个类仅由一个分类器表示，因此可以通过检查其对应的分类器来获取该类的知识。 这是多类分类最常用的策略，也是一个合理的默认选择。 然后该模块会执行预测，运行这些二元分类器并选择可信度最高的预测结果。 

从本质上讲，该模块创建了一组个体模型，然后合并结果，从而创建预测所有类的单一模型。 任何二元分类器均可用作一对多模型的基础。  

例如，假设你配置了一个[双类支持向量机](two-class-support-vector-machine.md)模型，并将其作为输入提供给“一对多”多类分类模块。 该模块将为输出类的所有成员创建双类支持向量机模型。 然后它将应用一对多方法来合并所有类的结果。  

本模块使用 sklearn 的 OneVsRestClassifier，可在[此处](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsRestClassifier.html)了解更多详细信息。

## <a name="how-to-configure-the-one-vs-all-multiclass-classifier"></a>如何配置“一对多”多类分类分类器  

此模块创建一组二元分类模型以分析多个类。 若要使用此模块，需要先配置并训练二元分类模型  。 

将二元模型连接到“一对多”多类分类模块。 然后通过使用带标记的训练数据集的[训练模型](train-model.md)来训练这组模型。

组合模型时，“一对多”多类分类会创建多个二元分类模型，为每个类优化算法，然后合并这些模型。 虽然训练数据集可以有多个类值，但模块还是会执行这些任务。

1. 在设计器中将“一对多”多类分类模块添加到管道。 可以在“机器学习 - 初始化”下的“分类”类别中找到此模块   。

   “一对多”多类分类分类器没有自己的可配置参数。 任何自定义操作都必须在作为输入提供的二元分类模型中完成。

2. 将二元分类模型添加到管道，并配置该模型。 例如，可以使用[双类支持向量机](two-class-support-vector-machine.md)或[双类提升决策树](two-class-boosted-decision-tree.md)。

3. 将[训练模型](train-model.md)模块添加到管道。 连接作为“一对多”多类分类的输出的未训练分类器。

4. 在[训练模型](train-model.md)的其他输入上，连接包含多个类值的带标记的训练数据集。

5. 提交管道。

## <a name="results"></a>结果

训练完成后，可以使用模型进行多类分类预测。

另外，还可以将未训练的分类器传递给[交叉验证模型](cross-validate-model.md)，以针对带标记的验证数据集进行交叉验证。


## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 
