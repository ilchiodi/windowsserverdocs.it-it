---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Esempi di valutazione della strategia di distribuzione di Active Directory Domain Services
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 228aec00fab5e1ca4e136a0f75cb5e2294d66700
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822494"
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
  


