---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Esempi di valutazione della strategia di distribuzione di Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3d04530c53150a3222b609a80938d7fdfcdfeff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402587"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Esempi di valutazione della strategia di distribuzione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si consideri l'esempio seguente di una società fittizia, Contoso Pharmaceuticals, che è la distribuzione di servizi di dominio Active Directory (AD DS) nel proprio ambiente. L'ambiente di Contoso è costituito da quattro domini. Il livello funzionale della foresta è Windows Server 2003. Nella figura seguente mostra la struttura di dominio corrente per l'organizzazione Contoso.  
  
![Strategia di distribuzione di Active Directory](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Dopo aver esaminato il proprio ambiente esistente e identificare gli obiettivi di distribuzione, Contoso stabilito la strategia di distribuzione di Active Directory seguente:  
  
-   Eseguire l'aggiornamento di domini di Windows Server 2003 a domini di Windows Server 2008.  
  
-   Abilitare le funzionalità avanzate di dominio Active Directory aumentando i livelli di funzionalità dominio e foresta a Windows Server 2008.  
  
-   Ristrutturazione africa.concorp.contoso.com all'interno della foresta per consolidare tale dominio con il dominio emea.concorp.contoso.con.  
  
Aumento del livello funzionale di foresta a Windows Server 2008 consentirà Contoso sfruttare al meglio le nuove funzionalità di dominio Active Directory. Ristrutturazione dei domini all'interno dell'insieme di strutture, come illustrato nella figura seguente, consente di ridurre la quantità di amministrazione che è necessario per gestire i domini.  
  
![Strategia di distribuzione di Active Directory](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


