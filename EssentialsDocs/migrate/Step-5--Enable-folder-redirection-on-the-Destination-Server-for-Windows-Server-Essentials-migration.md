---
title: 'Passaggio 5: Abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials'
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 613ff4c80a80ed4f3207cb0c1ead6db12c723e85
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Passaggio 5: Abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Se il reindirizzamento cartelle è abilitato nel Server di origine, è possibile abilitare il reindirizzamento cartelle nel Server di destinazione e quindi eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle.  
  
 Prima di tutto, usare il Dashboard di Windows Server Essentials per abilitare il reindirizzamento cartelle nel Server di destinazione. Quindi, eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Per abilitare il reindirizzamento cartelle nel Server di destinazione  
  
1.  Nel Server di destinazione, aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento, fare clic su **dispositivi**.  
  
3.  Nel **attività dispositivi** riquadro, fare clic su **implementa criteri di gruppo**.  
  
4.  Nel **Abilita criteri di gruppo reindirizzamento cartelle** pagina, selezionare le cartelle da reindirizzare, quindi fare clic su **Avanti**.  
  
5.  Nel **attiva le impostazioni di criteri di sicurezza** pagina, fare clic su **fine**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Per eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle  
  
1.  Nel Server di destinazione, aprire il **Gestione criteri di gruppo** strumento di amministrazione.  
  
2.  In **Gestione criteri di gruppo**, espandere **foresta:***YourNetworkDomainName*, espandere **domini**, espandere *YourNetworkDomainName*, quindi espandere **oggetti Criteri di gruppo**.  
  
3.  Fare doppio clic su criteri che si desidera eliminare e quindi fare clic su **eliminare**.  
  
4.  Leggere l'avviso, quindi fare clic su **Sì**.  
  
5.  Chiudi **Gestione criteri di gruppo**.  
  
 Per applicare la modifica del reindirizzamento cartelle, gli utenti della rete devono disconnettere i computer e quindi riconnettersi. In questo modo il trasferimento di tutte le cartelle reindirizzate al Server di destinazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato abilitato il reindirizzamento cartelle nel Server di destinazione. Passare quindi a [passaggio 6: abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Per visualizzare tutti i passaggi, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

