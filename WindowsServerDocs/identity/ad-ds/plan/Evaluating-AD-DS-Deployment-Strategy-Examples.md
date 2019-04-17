---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Valutazione di esempi strategia di distribuzione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cac1e344cfed078927f2945048807d686deebd1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Valutazione di esempi strategia di distribuzione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si consideri l'esempio seguente di una società fittizia, Contoso Pharmaceuticals, che è la distribuzione di servizi di dominio Active Directory (AD DS) nel proprio ambiente. Ambiente di Contoso è costituito da quattro domini. Il livello funzionale di foresta è Windows Server 2003. Nella figura seguente mostra la struttura di dominio corrente per l'organizzazione Contoso.  
  
![Strategia di distribuzione di Active Directory](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Dopo aver esaminato è ambiente esistente e identificare gli obiettivi di distribuzione, Contoso stabilito la strategia di distribuzione di dominio Active Directory seguenti:  
  
-   Eseguire l'aggiornamento di domini di Windows Server 2003 a domini di Windows Server 2008.  
  
-   Abilitare la funzionalità avanzate di dominio Active Directory per aumentare i livelli di funzionalità dominio e foresta a Windows Server 2008.  
  
-   Ristrutturazione africa.concorp.contoso.com all'interno della foresta per consolidare tale dominio con il dominio emea.concorp.contoso.con.  
  
Aumento del livello funzionale di foresta a Windows Server 2008 consentirà Contoso sfruttare appieno le nuove funzionalità di dominio Active Directory. Ristrutturazione dei domini all'interno della foresta, come illustrato nella figura seguente, consente di ridurre la quantità di amministrazione che è necessario per gestire i domini.  
  
![Strategia di distribuzione di Active Directory](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


