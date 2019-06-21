---
title: Prerequisiti per la distribuzione di DirectAccess
description: In questo argomento illustra i prerequisiti per la distribuzione di DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f77df438ba2b282101a031b4ff4d145286cf3e94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283621"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Prerequisiti per la distribuzione di DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Nella tabella seguente elenca i prerequisiti necessari per utilizzare le procedure guidate di configurazione per distribuire DirectAccess.  
  
|||  
|-|-|  
|Scenario|Prerequisiti|  
|[Distribuire un Server DirectAccess singolo con attività iniziali guidate](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows Firewall deve essere abilitato in tutti i profili<br /><br />-Per i client che eseguono Windows 10 solo supportata&reg;, <br />              Windows&reg; 8 e Windows&reg; 8.1 Enterprise.<br /><br />-Un'infrastruttura a chiave pubblica non è obbligatorio.<br /><br />-Non supportato per la distribuzione dell'autenticazione a due fattori. Le credenziali di dominio sono necessarie per l'autenticazione.<br /><br />-Distribuisce automaticamente DirectAccess a tutti i computer portatili nel dominio corrente.<br /><br />-Il traffico a Internet non passa attraverso DirectAccess. La configurazione del tunneling forzato non è supportata.<br /><br />-Server DirectAccess si trova il server dei percorsi di rete.<br /><br />-Network Access Protection (NAP) non è supportato.<br /><br />-Modifica i criteri mediante una funzionalità diversa dalla console Gestione DirectAccess o i cmdlet di Windows PowerShell non è supportata.<br /><br />-Per una configurazione multisito, ora o in futuro, prima di tutto seguire le indicazioni fornite in [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Distribuire un singolo server di DirectAccess con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Un'infrastruttura a chiave pubblica deve essere distribuita.<br />    Per altre informazioni, vedere [mini-modulo di Test Lab Guide: Base PKI per Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Windows Firewall deve essere abilitato in tutti i profili.<br /><br />I seguenti sistemi operativi server supportano DirectAccess.<br /><br />-È possibile distribuire tutte le versioni di Windows Server 2016 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 R2 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2008 R2 come un client DirectAccess o un server DirectAccess.<br /><br />I seguenti sistemi operativi client supportano DirectAccess.<br /><br />-   Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 Long Term Servicing Branch (LTSB)<br />-Windows&reg; 8 e 8.1 Enterprise<br />-   Windows&reg; 7 Ultimate<br />-   Windows&reg; 7 Enterprise<br /><br />-Configurazione del tunneling forzato non è supportato con l'autenticazione KerbProxy.<br /><br />-Modifica i criteri mediante una funzionalità diversa dalla console Gestione DirectAccess o i cmdlet di Windows PowerShell non è supportata.<br /><br />-Separazione NAT64 o DNS64 e ruoli del server IPHTTPS in un altro server non è supportata.|  
  


