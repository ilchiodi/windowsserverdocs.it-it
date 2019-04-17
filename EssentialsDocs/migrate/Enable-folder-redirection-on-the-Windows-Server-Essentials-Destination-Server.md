---
title: Enable folder redirection on the Windows Server Essentials Destination Server1
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: Enable folder redirection on the Windows Server Essentials Destination Server
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f93d7b28177f96725f2e62c40f9c81cbf186ee6d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>Enable folder redirection on the Windows Server Essentials Destination Server1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

You can perform this task if folder redirection is enabled on the Source Server.  
  
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
  
3.  Right-click **W7PVP Folder Redirection**, and then click **Delete**.  
  
4.  Leggere l'avviso, quindi fare clic su **Sì**.  
  
5.  Chiudi **Gestione criteri di gruppo**.  
  
 Per applicare la modifica al reindirizzamento cartelle, gli utenti della rete devono disconnettere i computer e quindi riconnettersi. In questo modo il trasferimento di tutte le cartelle reindirizzate al Server di destinazione.
