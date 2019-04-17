---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: 'Appendice: amministrazione semplificata'
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>Appendice: amministrazione semplificata

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Aggiunta di Server Manager finestra di dialogo Server (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Stato del Server remoto di Server Manager](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Caricamento del modulo di PowerShell di Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Aggiornamenti rapidi di rilascio di RID per i sistemi operativi precedenti](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Cambia Ntdsutil.exe installa da supporto](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Aggiunta di Server Manager finestra di dialogo Server (Active Directory)  

Il **Aggiungi server** finestra di dialogo consente di ricerca in Active Directory per i server, sistema operativo, usando i caratteri jolly e posizione. La finestra di dialogo consente inoltre di utilizzo delle query DNS con il nome di dominio completo o il nome prefisso. Le ricerche usano protocolli DNS e LDAP nativi implementati tramite .NET, non il AD Windows PowerShell sul gateway di gestione di Active Directory tramite SOAP - vale a dire che i controller di dominio contattati da Server Manager possono anche eseguire Windows Server 2003. È anche possibile importare un file con nomi di server per scopi di provisioning.  
  
La ricerca di Active Directory utilizza i seguenti filtri LDAP:  
  
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
Server Manager verifica accessibilità server remoto utilizzando il protocollo di Routing degli indirizzi. Tutti i server non risponde alle richieste ARP non sono elencati, anche se sono nel pool.  
  
Se ARP risponde, quindi DCOM e WMI connessioni al server per restituire informazioni sullo stato. Se non sono raggiungibili RPC, DCOM e WMI, Gestione server non può gestire completamente i server.  
  
## <a name="BKMK_PSLoadModule"></a>Caricamento del modulo di PowerShell di Windows  
Windows PowerShell 3.0 implementa il caricamento di modulo dinamico. Utilizzando il **Import-Module** cmdlet in genere non è più necessario; al contrario, semplicemente richiamare il cmdlet, alias o la funzione automaticamente carica il modulo.  
  
Per visualizzare i moduli caricati, usare il **Get-Module** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Per visualizzare tutti i moduli installati con i cmdlet e le funzioni esportate, usare:  
  
```  
Get-Module -ListAvailable  
  
```  
  
Il caso principale per l'uso di **import-module** comando è quando è necessario accedere al "AD:" unità virtuale di Windows PowerShell e nient'altro ha già caricato il modulo. Ad esempio, utilizzando i comandi seguenti:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Aggiornamenti rapidi di rilascio di RID per i sistemi operativi precedenti  
Vedere [per rilevare e prevenire una quantità eccessiva consumo del pool di RID globale in un controller di dominio che esegue Windows Server 2008 R2 è disponibile un aggiornamento](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Cambia Ntdsutil.exe installa da supporto  
Windows Server 2012 aggiunge due opzioni aggiuntive per lo strumento da riga di comando Ntdsutil.exe per il **IFM (creazione di supporti di installazione da supporto)** menu. Questi consentono di creare archivi IFM senza prima eseguire una deframmentazione non in linea di NTDS esportato. File di database DIT. Quando lo spazio su disco non è limitato, ciò consente di risparmiare tempo la creazione di IFM.  
  
Nella tabella seguente vengono descritti due nuove voci di menu:  
  
|||  
|-|-|  
|Voce di menu|Spiegazione|  
|Creare NoDefrag completo %s|Creare un supporto di installazione senza la deframmentazione di un controller di dominio completo di Active Directory o un'istanza di AD LDS/nella cartella %s|  
|Creare Sysvol Full NoDefrag %s|Creare un supporto di installazione con SYSVOL e senza la deframmentazione di un controller di dominio completo di Active Directory nella cartella %s|  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Amministrazione semplificata](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


