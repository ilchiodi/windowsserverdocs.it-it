---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 'Appendice: Amministrazione semplificata'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ffc2849fa5e18f7984814d6187cf83d68566409b
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371549"
---
# <a name="simplified-administration-appendix"></a>Appendice: Amministrazione semplificata

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Finestra di dialogo Server Manager Aggiungi server (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Stato del server remoto Server Manager](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Caricamento del modulo di Windows PowerShell](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Hotfix di rilascio dei RID per i sistemi operativi precedenti](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Installazione di Ntdsutil. exe da supporti modifiche](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Finestra di dialogo Server Manager Aggiungi server (Active Directory)  

La finestra di dialogo **Aggiungi server** consente la ricerca Active Directory per i server, per sistema operativo, per l'utilizzo di caratteri jolly e per percorso. La finestra di dialogo consente inoltre l'utilizzo di query DNS in base al nome di dominio completo o al nome del prefisso. Queste ricerche usano i protocolli DNS e LDAP nativi implementati tramite .NET, non AD Windows PowerShell per il gateway di gestione di Active Directory tramite SOAP, ovvero i controller di dominio contattati da Server Manager possono anche eseguire Windows Server 2003. È anche possibile importare un file con nomi di server a scopo di provisioning.  
  
La ricerca Active Directory usa i filtri LDAP seguenti:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
La ricerca Active Directory restituisce i seguenti attributi:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>Stato del server remoto Server Manager  
Server Manager verifica l'accessibilità del server remoto usando il protocollo di routing degli indirizzi. Eventuali server che non rispondono alle richieste ARP non sono elencati, anche se sono nel pool.  
  
Se ARP risponde, le connessioni DCOM e WMI vengono effettuate al server per restituire informazioni sullo stato. Se RPC, DCOM e WMI non sono raggiungibili, Server Manager non è in grado di gestire completamente il server.  
  
## <a name="BKMK_PSLoadModule"></a>Caricamento del modulo di Windows PowerShell  
Windows PowerShell 3,0 implementa il caricamento dinamico dei moduli. L'uso del cmdlet **Import-Module** in genere non è più necessario. ma semplicemente richiamando il cmdlet, l'alias o la funzione carica automaticamente il modulo.  
  
Per visualizzare i moduli caricati, usare il cmdlet **Get-Module** .  
  
```  
Get-Module  
  
```  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Per visualizzare tutti i moduli installati con le funzioni e i cmdlet esportati, usare:  
  
```  
Get-Module -ListAvailable  
  
```  
  
Il caso principale per l'uso del comando **Import-Module** è quando è necessario accedere all'unità virtuale di Windows PowerShell "ad:" e nessun altro elemento ha già caricato il modulo. Ad esempio, usando i comandi seguenti:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Hotfix di rilascio dei RID per i sistemi operativi precedenti  
Vedere [un aggiornamento è disponibile per rilevare e impedire un consumo eccessivo del pool di RID globale in un controller di dominio che esegue Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Installazione di Ntdsutil. exe da supporti modifiche  
Windows Server 2012 aggiunge due opzioni aggiuntive allo strumento da riga di comando Ntdsutil. exe per il menu **installazione da supporto (installazione da supporto Media Creation)** . Questi consentono di creare archivi installazione da supporto senza prima eseguire una deframmentazione non in linea del NTDS esportato. File di database DIT. Quando lo spazio su disco non è Premium, viene risparmiato tempo per la creazione di installazione da supporto.  
  
Nella tabella seguente vengono descritte le due nuove voci di menu:  
  
|||  
|-|-|  
|Voce di menu|Spiegazione|  
|Crea nodefrag% s completa|Creare supporti installazione da supporto senza deframmentazione per un controller di dominio Active Directory completo o un'istanza di AD/LDS nella cartella% s|  
|Creazione di SYSVOL completa nodefrag% s|Creare un supporto installazione da supporto con SYSVOL e senza deframmentazione per un controller di dominio Active Directory completo nella cartella% s|  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


