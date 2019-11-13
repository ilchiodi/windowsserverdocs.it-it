---
title: Prerequisiti per la distribuzione di DirectAccess
description: In questo argomento vengono forniti i prerequisiti per la distribuzione di DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e04dbf1576277493ec849c8de82aeab51e97649
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388811"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Prerequisiti per la distribuzione di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nella tabella seguente sono elencati i prerequisiti necessari per l'utilizzo delle procedure guidate di configurazione per la distribuzione di DirectAccess.  
  
|||  
|-|-|  
|Scenario|Prerequisiti|  
|[Distribuire un server DirectAccess singolo tramite la procedura guidata di Introduzione](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows Firewall deve essere abilitato in tutti i profili<br /><br />-Supportato solo per i client che eseguono Windows 10&reg;, <br />              Windows&reg; 8 e Windows&reg; 8,1 Enterprise Edition.<br /><br />-Un'infrastruttura a chiave pubblica non è obbligatoria.<br /><br />-Non supportato per la distribuzione dell'autenticazione a due fattori. Le credenziali di dominio sono necessarie per l'autenticazione.<br /><br />-Distribuisce automaticamente DirectAccess a tutti i computer portatili nel dominio corrente.<br /><br />-Il traffico verso Internet non passa attraverso DirectAccess. La configurazione del tunneling forzato non è supportata.<br /><br />-Il server DirectAccess è il server dei percorsi di rete.<br /><br />-Protezione accesso alla rete (NAP) non è supportato.<br /><br />-La modifica dei criteri utilizzando una funzionalità diversa dalla console di Gestione DirectAccess o dai cmdlet di Windows PowerShell non è supportata.<br /><br />-Per una configurazione multisito, ora o in futuro, seguire prima le istruzioni riportate in [distribuire un server DirectAccess singolo con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Distribuire un singolo server di DirectAccess con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-È necessario distribuire un'infrastruttura a chiave pubblica.<br />    Per altre informazioni, vedere [Guida al Lab di test: modulo PKI di base per Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Windows Firewall deve essere abilitato in tutti i profili.<br /><br />I sistemi operativi server seguenti supportano DirectAccess.<br /><br />-È possibile distribuire tutte le versioni di Windows Server 2016 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 R2 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2008 R2 come un client DirectAccess o un server DirectAccess.<br /><br />I sistemi operativi client seguenti supportano DirectAccess.<br /><br />-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 Long Term Servicing Branch (LTSB)<br />-Windows&reg; 8 e 8,1 Enterprise<br />-Windows&reg; 7 Ultimate<br />-Windows&reg; 7 Enterprise<br /><br />-La configurazione del tunneling forzato non è supportata con l'autenticazione KerbProxy.<br /><br />-La modifica dei criteri utilizzando una funzionalità diversa dalla console di Gestione DirectAccess o dai cmdlet di Windows PowerShell non è supportata.<br /><br />-La separazione dei ruoli del server NAT64/DNS64 e IPHTTPS in un altro server non è supportata.|  
  


