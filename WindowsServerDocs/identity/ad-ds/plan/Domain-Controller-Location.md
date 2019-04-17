---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Individuazione dei Controller di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 198453099336954ee44447a79fec267266caa99a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="domain-controller-location"></a>Individuazione dei Controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Clients use Domain Name System (DNS) to locate domain controllers to complete operations such as processing logon requests or searching the directory for published resources. Domain controllers register a variety of records in DNS to help clients and other computers locate them. These records are collectively referred to as the locator records.  
  
Domain controllers also use DNS to locate other domain controllers and to perform tasks such as replication. The process by which domain controllers locate other domain controllers is the same as the process by which clients locate domain controllers.  
  


