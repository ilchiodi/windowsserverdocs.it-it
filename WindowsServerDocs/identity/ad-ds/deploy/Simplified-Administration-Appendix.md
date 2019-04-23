---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 'Appendice: Amministrazione semplificata'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858262"
---
# <a name="simplified-administration-appendix"></a>Appendice: Amministrazione semplificata

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Server Manager aggiungere server finestre di dialogo (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Stato del Server remoto di Server Manager](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Caricamento del modulo PowerShell di Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Aggiornamenti rapidi di rilascio di RID per sistemi operativi precedenti](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe installa da supporto modifiche](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Server Manager aggiungere server finestre di dialogo (Active Directory)  

Il **Aggiungi server** finestra di dialogo consente la ricerca di Active Directory per i server, dal sistema operativo, usando caratteri jolly e dall'indirizzo. La finestra di dialogo consente anche con le query DNS da nome di dominio completo o prefisso del nome. Queste ricerche usano protocolli LDAP e DNS nativi implementati tramite .NET, non AD Windows PowerShell con il Gateway di gestione di Active Directory tramite SOAP - vale a dire che i controller di dominio contattati dal Server di gestione possono anche eseguire Windows Server 2003. È anche possibile importare un file con nomi di server per scopi di provisioning.  
  
La ricerca di Active Directory Usa i filtri LDAP seguenti:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
La ricerca di Active Directory restituisce gli attributi seguenti:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>Stato del Server remoto di Server Manager  
Server Manager verifica accessibilità server remoto utilizzando il protocollo di Routing degli indirizzi. Tutti i server non risponde alle richieste ARP non sono elencati, anche se sono in pool.  
  
Se risponde ARP, le connessioni DCOM e WMI vengono apportate al server per restituire informazioni sullo stato. Se RPC, DCOM e WMI non sono raggiungibili, Gestione server non è possibile gestire completamente i server.  
  
## <a name="BKMK_PSLoadModule"></a>Caricamento del modulo PowerShell di Windows  
Windows PowerShell 3.0 implementa il caricamento del modulo dinamico. Usando il **Import-Module** cmdlet in genere non è più necessario; in alternativa, è sufficiente richiamare il cmdlet, alias o funzione automaticamente carica il modulo.  
  
Per visualizzare i moduli caricati, usare il **Get-Module** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Per visualizzare tutti i moduli installati con le funzioni esportate e i cmdlet, usare:  
  
```  
Get-Module -ListAvailable  
  
```  
  
Il caso principale per l'uso di **import-module** comando è quando è necessario accedere al "AD:" Il disco virtuale di Windows PowerShell e nessun altro elemento è già caricato il modulo. Ad esempio, usando i comandi seguenti:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Aggiornamenti rapidi di rilascio di RID per sistemi operativi precedenti  
Visualizzare [è disponibile un aggiornamento per rilevare e prevenire eccessivo consumo del pool di RID globale in un controller di dominio che esegue Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe installa da supporto modifiche  
Windows Server 2012 aggiunge due opzioni aggiuntive per lo strumento da riga di comando Ntdsutil.exe per il **IFM (creazione di supporti di installazione da supporto)** menu. Questi consentono di creare gli archivi di installazione da supporto senza prima eseguire una deframmentazione non in linea del file NTDS esportato. File di database DIT. Quando lo spazio su disco non è un piano premium, ciò consente di risparmiare tempo creando l'installazione da supporto.  
  
La tabella seguente descrive due nuove voci di menu:  
  
|||  
|-|-|  
|Voce di menu|Spiegazione|  
|Creare NoDefrag completo %s|Creare supporti di installazione da supporto senza effettuare la deframmentazione di un controller di dominio completo di Active Directory o un'istanza di AD LDS/nella cartella %s|  
|Creare Full Sysvol NoDefrag %s|Creare supporti di installazione da supporto con SYSVOL e senza la deframmentazione di un controller di dominio completo di Active Directory nella cartella %s|  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


