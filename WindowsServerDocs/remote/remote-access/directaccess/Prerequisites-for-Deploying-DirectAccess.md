---
title: Prerequisiti per la distribuzione di DirectAccess
description: In questo argomento vengono forniti i prerequisiti per la distribuzione di DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5916c5bb87d7bdb10bcc32e24647923d434de2e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857448"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Prerequisiti per la distribuzione di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nella tabella seguente sono elencati i prerequisiti necessari per l'utilizzo delle procedure guidate di configurazione per la distribuzione di DirectAccess.  
  
|||  
|-|-|  
|Scenario|Prerequisiti|  
|[Distribuire un server DirectAccess singolo tramite la procedura guidata di Introduzione](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows Firewall deve essere abilitato in tutti i profili<p>-Supportato solo per i client che eseguono Windows 10&reg;, <br />              Windows&reg; 8 e Windows&reg; 8,1 Enterprise Edition.<p>-Un'infrastruttura a chiave pubblica non è obbligatoria.<p>-Non supportato per la distribuzione dell'autenticazione a due fattori. Le credenziali di dominio sono necessarie per l'autenticazione.<p>-Distribuisce automaticamente DirectAccess a tutti i computer portatili nel dominio corrente.<p>-Il traffico verso Internet non passa attraverso DirectAccess. La configurazione del tunneling forzato non è supportata.<p>-Il server DirectAccess è il server dei percorsi di rete.<p>-Protezione accesso alla rete (NAP) non è supportato.<p>-La modifica dei criteri utilizzando una funzionalità diversa dalla console di Gestione DirectAccess o dai cmdlet di Windows PowerShell non è supportata.<p>-Per una configurazione multisito, ora o in futuro, seguire prima le istruzioni riportate in [distribuire un server DirectAccess singolo con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Distribuire un singolo server di DirectAccess con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-È necessario distribuire un'infrastruttura a chiave pubblica.<br />    Per altre informazioni, vedere [Guida al Lab di test: modulo PKI di base per Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<p>-Windows Firewall deve essere abilitato in tutti i profili.<p>I sistemi operativi server seguenti supportano DirectAccess.<p>-È possibile distribuire tutte le versioni di Windows Server 2016 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 R2 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2012 come un client DirectAccess o un server DirectAccess.<br />-È possibile distribuire tutte le versioni di Windows Server 2008 R2 come un client DirectAccess o un server DirectAccess.<p>I sistemi operativi client seguenti supportano DirectAccess.<p>-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 Long Term Servicing Branch (LTSB)<br />-Windows&reg; 8 e 8,1 Enterprise<br />-Windows&reg; 7 Ultimate<br />-Windows&reg; 7 Enterprise<p>-La configurazione del tunneling forzato non è supportata con l'autenticazione KerbProxy.<p>-La modifica dei criteri utilizzando una funzionalità diversa dalla console di Gestione DirectAccess o dai cmdlet di Windows PowerShell non è supportata.<p>-La separazione dei ruoli del server NAT64/DNS64 e IPHTTPS in un altro server non è supportata.|  
  


