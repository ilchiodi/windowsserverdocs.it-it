---
title: Configurare il reindirizzamento cartelle nel Server di destinazione Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7cc1207f36d3a921b49cc3ecd02acf3fe4fa243c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurare il reindirizzamento cartelle nel Server di destinazione Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Eseguire questa attività se il reindirizzamento cartelle è abilitato nel Server di origine.  
  
 Eliminare prima la vecchia impostazione di criteri di gruppo reindirizzamento cartelle. Quindi utilizzare il Dashboard di Windows Server Essentials per abilitare il reindirizzamento cartelle nel Server di destinazione.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Per eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle  
  
1.  Nel Server di destinazione, aprire il **Gestione criteri di gruppo** strumento di amministrazione.  
  
2.  In **Gestione criteri di gruppo**, espandere **foresta:***YourNetworkDomainName*, espandere **domini**, espandere *YourNetworkDomainName*, quindi espandere **oggetti Criteri di gruppo**.  
  
3.  Fare doppio clic su **reindirizzamento cartelle Criteri di gruppo SBS**, quindi fare clic su **eliminare**.  
  
4.  Fare doppio clic su **modelli di sicurezza Criteri di gruppo SBS**, quindi fare clic su **eliminare**.  
  
5.  Leggere l'avviso, quindi fare clic su **Sì**.  
  
6.  Chiudi **Gestione criteri di gruppo**.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Per abilitare il reindirizzamento cartelle nel Server di destinazione  
  
1.  Nel Server di destinazione, aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento, fare clic su **dispositivi**.  
  
3.  Nel **attività dispositivi** riquadro, fare clic su **implementa criteri di gruppo**.  
  
4.  Nel **Abilita criteri di gruppo reindirizzamento cartelle** pagina, selezionare le cartelle da reindirizzare, quindi fare clic su **Avanti**.  
  
5.  Nel **attiva le impostazioni di criteri di sicurezza** pagina, fare clic su **fine**.  
  
 Per applicare la modifica al reindirizzamento cartelle, gli utenti della rete devono disconnettere i computer e quindi riconnettersi. In questo modo il trasferimento di tutte le cartelle reindirizzate al Server di destinazione.
