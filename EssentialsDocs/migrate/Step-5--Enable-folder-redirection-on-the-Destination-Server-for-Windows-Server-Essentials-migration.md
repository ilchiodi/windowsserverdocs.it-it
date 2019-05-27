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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815382"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Passaggio 5: Abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Se il reindirizzamento cartelle è abilitato nel server di origine, è possibile abilitarlo nel server di destinazione e quindi eliminare l'impostazione dei Criteri di gruppo per il reindirizzamento cartelle.  
  
 In primo luogo, usare il Dashboard di Windows Server Essentials per abilitare il reindirizzamento cartelle nel Server di destinazione. Eliminare quindi la vecchia impostazione di Criteri di gruppo per il reindirizzamento cartelle.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Per abilitare il reindirizzamento cartelle sul server di destinazione  
  
1.  Nel Server di destinazione, aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **DISPOSITIVI**.  
  
3.  Nel riquadro **Attività dispositivo** fare clic su **Implementa criteri di gruppo**.  
  
4.  Nella pagina **Abilita criteri di gruppo per reindirizzamento cartelle** selezionare le cartelle da reindirizzare e quindi fare clic su **Avanti**.  
  
5.  Nella pagina **Abilita impostazioni dei criteri di sicurezza** fare clic su **Fine**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Per eliminare la vecchia impostazione di Criteri di gruppo per il reindirizzamento cartelle  
  
1.  Nel server di destinazione aprire lo strumento di amministrazione **Gestione Criteri di gruppo**.  
  
2.  Nelle **Gestione criteri di gruppo**, espandere **foresta:***YourNetworkDomainName*, espandere **domini**, espandere *YourNetworkDomainName* , quindi espandere **oggetti Criteri di gruppo**.  
  
3.  Fare clic con il pulsante destro del mouse sui criteri da eliminare e quindi scegliere **Elimina**.  
  
4.  Leggere l'avviso e quindi fare clic su **Sì**.  
  
5.  Chiudere **Gestione Criteri di gruppo**.  
  
 Per applicare la modifica del reindirizzamento cartelle, gli utenti di rete devono disconnettere i computer e quindi riconnettersi. Questo garantisce il trasferimento di tutte le cartelle reindirizzate al server di destinazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato abilitato il reindirizzamento delle cartelle nel server di destinazione. Passare quindi a [passaggio 6: Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

