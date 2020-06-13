---
title: SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709, Windows Server versione 1709 e versioni successive
description: Viene illustrato il comportamento del protocollo SMBv1 in Windows 10 Fall Creators Update e Windows Server, versione 1709 e versioni successive.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: a1e1a30530f937289770bcef9e71189bf69719ce
ms.sourcegitcommit: 7200143aa787c7ac05ae0e012263b1c9a95b87ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721759"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709, Windows Server versione 1709 e versioni successive

## <a name="summary"></a>Summary

In Windows 10 Fall Creators Update e Windows Server, versione 1709 (RS3) e versioni successive, il protocollo di rete Server Message Block versione 1 (SMBv1) non viene più installato per impostazione predefinita. È stata sostituita da SMBv2 e dai protocolli successivi a partire da 2007. Microsoft ha deprecato pubblicamente il protocollo SMBv1 in 2014. 

SMBv1 presenta il comportamento seguente in Windows 10 Fall Creators Update e Windows Server, versione 1709 (RS3): 
 
- SMBv1 dispone ora di sottofunzionalità client e server che possono essere disinstallate separatamente.    
- Windows 10 Enterprise e Windows 10 Education non contengono più il client o il server SMBv1 per impostazione predefinita dopo un'installazione pulita.    
- Windows Server 2016 non contiene più il client o il server SMBv1 per impostazione predefinita dopo un'installazione pulita.    
- Windows 10 Home e Windows 10 Professional non contengono più il server SMBv1 per impostazione predefinita dopo un'installazione pulita.    
- Windows 10 Home e Windows 10 Professional contengono ancora il client SMBv1 per impostazione predefinita dopo un'installazione pulita. Se il client di SMBv1 non viene usato per 15 giorni in totale (escluso il computer disattivato), viene automaticamente disinstallato.    
- Gli aggiornamenti sul posto e i voli Insider di Windows 10 Home e Windows 10 Professional non rimuovono automaticamente SMBv1 inizialmente. Se il client o il server di SMBv1 non viene usato per 15 giorni in totale (escluso il tempo durante il quale il computer è spento), ognuno si disinstalla automaticamente.     
- Gli aggiornamenti sul posto e i voli Insider delle edizioni Windows 10 Enterprise e Windows 10 Education non rimuovono automaticamente SMBv1. Un amministratore deve decidere di disinstallare SMBv1 in questi ambienti gestiti. In Windows 10, versione 1809 (RS5) e versioni successive, un amministratore può attivare la rimozione automatica di SMBv1 accendendo la funzionalità "SMB 1.0/CIFS Automatic Removal".    
- La rimozione automatica di SMBv1 dopo 15 giorni è un'operazione una sola volta. Se un amministratore installa nuovamente SMBv1, non verrà eseguito alcun tentativo di disinstallazione.
- Le funzionalità SMB versione 2,02, 2,1, 3,0, 3,02 e 3.1.1 sono ancora completamente supportate e sono incluse per impostazione predefinita come parte dei file binari SMBv2.    
- Poiché il servizio browser del computer si basa su SMBv1, il servizio viene disinstallato se il client o il server SMBv1 è stato disinstallato. Ciò significa che la rete di Explorer non può più visualizzare i computer Windows tramite il metodo legacy di esplorazione del datagramma NetBIOS.    
- SMBv1 può comunque essere reinstallato in tutte le edizioni di Windows 10 e Windows Server 2016.    
 
  > [!NOTE]
  > Windows 10, versione 1803 (RS4) Professional gestisce SMBv1 in modo analogo a Windows 10, versione 1703 (RS2) e Windows 10, versione 1607 (RS1). Questo problema è stato risolto in Windows 10, versione 1809 (RS5). È comunque possibile disinstallare manualmente SMBv1. Tuttavia, Windows non disinstallerà automaticamente SMBv1 dopo 15 giorni negli scenari seguenti: 
 
-  Si esegue un'installazione pulita di Windows 10, versione 1803.     
-  Si aggiorna Windows 10, versione 1607 o Windows 10, versione 1703 a Windows 10, versione 1803 direttamente senza prima eseguire l'aggiornamento a Windows 10, versione 1709.     
 
Se si prova a connettersi ai dispositivi che supportano solo SMBv1 o se questi dispositivi tentano di connettersi all'utente, è possibile che venga visualizzato uno dei messaggi di errore seguenti:     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

Gli eventi seguenti vengono visualizzati quando un server remoto richiede una connessione SMBv1 dal client, ma SMBv1 viene disinstallato o disabilitato nel client.

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

Questi dispositivi non eseguono probabilmente Windows.È più probabile che eseguano versioni precedenti di Linux, Samba o altri tipi di software di terze parti per fornire servizi SMB. Spesso queste versioni di Linux e Samba non sono più supportate. 

> [!NOTE]
> Windows 10, versione 1709 è noto anche come "Fall Creators Update".   

## <a name="more-information"></a>Altre informazioni

Per risolvere questo problema, contattare il produttore del prodotto che supporta solo SMBv1 e richiedere un aggiornamento del software o del firmware che supporti SMBv 2.02 o una versione successiva. Per un elenco aggiornato dei fornitori noti e dei rispettivi requisiti di SMBv1, vedere l'articolo del Blog del team di progettazione dell'archiviazione Windows e Windows Server seguente: 

[SMBv1 di compensazione del prodotto](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>Modalità di leasing

Se SMBv1 è necessario per garantire la compatibilità delle applicazioni per il comportamento del software legacy, ad esempio un requisito per disabilitare oplock, Windows fornisce un nuovo flag di condivisione SMB noto come modalità di leasing. Questo flag specifica se una condivisione Disabilita la semantica SMB moderna, ad esempio lease e oplock.

È possibile specificare una condivisione senza usare oplock o leasing per consentire a un'applicazione legacy di lavorare con SMBv2 o una versione successiva. A tale scopo, usare i cmdlet di PowerShell **New-SmbShare** o **set-SmbShare** insieme al parametro **-LeasingMode None**   .

> [!NOTE]
> Usare questa opzione solo per le condivisioni richieste da un'applicazione di terze parti per il supporto legacy se il fornitore dichiara che è necessario. Non specificare la modalità di leasing per le condivisioni dati utente o le condivisioni CA utilizzate dai file server di scalabilità orizzontale. Ciò è dovuto al fatto che la rimozione di oplock e lease causa l'instabilità e il danneggiamento dei dati nella maggior parte delle applicazioni. La modalità di leasing funziona solo in modalità di condivisione. Può essere usato da qualsiasi sistema operativo client.

#### <a name="explorer-network-browsing"></a>Esplorazione della rete di Esplora risorse

Il servizio browser del computer si basa sul protocollo SMBv1 per popolare il nodo di rete di Esplora risorse (noto anche come "Neighborhood di rete"). Questo protocollo legacy è deprecato, non viene indirizzato e ha una sicurezza limitata. Poiché il servizio non può funzionare senza SMBv1, viene rimosso nello stesso momento.

Tuttavia, se è ancora necessario utilizzare Esplora Rete in ingresso ambienti Home e Small Business Workgroup per individuare i computer basati su Windows, è possibile attenersi alla procedura seguente nei computer basati su Windows che non utilizzano più SMBv1: 
 
1. Avviare i servizi "host del provider di individuazione della funzione" e "pubblicazione di risorse di individuazione delle funzioni", quindi impostarli su **automatico (avvio ritardato)**.

2. Quando si apre la rete di Explorer, abilitare l'individuazione della rete quando richiesto.    
 
Tutti i dispositivi Windows presenti nella subnet che dispongono di queste impostazioni verranno visualizzati in rete per l'esplorazione. Viene utilizzato il protocollo WS-DISCOVERY. Se i dispositivi non sono ancora presenti in questo elenco di esplorazione dopo aver visualizzato i dispositivi Windows, contattare gli altri fornitori e produttori. È possibile che questi protocolli siano disabilitati o che supportino solo SMBv1.

> [!NOTE]
> Si consiglia di eseguire il mapping delle unità e delle stampanti invece di abilitare questa funzionalità, che richiede ancora la ricerca e l'esplorazione dei propri dispositivi. Le risorse mappate sono più facili da individuare, richiedono meno formazione e sono più sicure da usare. Ciò è particolarmente vero se queste risorse vengono fornite automaticamente tramite Criteri di gruppo.Un amministratore può configurare le stampanti per la posizione in base a metodi diversi dal servizio browser del computer legacy usando indirizzi IP, Active Directory Domain Services (AD DS), Bonjour, MDN, uPnP e così via.

Se non è possibile utilizzare una di queste soluzioni alternative o se il produttore dell'applicazione non è in grado di fornire versioni supportate di SMB, è possibile riabilitare SMBv1 manualmente attenendosi alla procedura descritta in [come rilevare, abilitare e disabilitare SMBv1, SMBv2 e SMBv3 in Windows](detect-enable-and-disable-smbv1-v2-v3.md).

> [!IMPORTANT]
> Si consiglia vivamente di non reinstallare SMBv1. Questo è dovuto al fatto che questo protocollo precedente presenta problemi di sicurezza noti relativi a ransomware e altro malware.  

#### <a name="windows-server-best-practices-analyzer-messaging"></a>Messaggistica di Windows Server Best Practices Analyzer

I sistemi operativi server Windows Server 2012 e versioni successive contengono Best Practices Analyzer (BPA) per i file server. Se sono state seguite le linee guida in linea corrette per disinstallare SMB1, l'esecuzione di questo BPA restituirà un messaggio di avviso contraddittorio:

    Title: The SMB 1.0 file sharing protocol should be enabled
    Severity: Warning
    Date: 3/25/2020 12:38:47 PM
    Category: Configuration
    Problem: The Server Message Block 1.0 (SMB 1.0) file sharing protocol is disabled on this file server.
    Impact: SMB not in a default configuration, which could lead to less than optimal behavior.
    Resolution: Use Registry Editor to enable the SMB 1.0 protocol.

È consigliabile ignorare le linee guida specifiche della regola BPA, che è deprecata. Si ripete: non abilitare SMB 1,0.

## <a name="references"></a>Riferimenti

[Interrompi uso di SMB1](https://aka.ms/stopusingsmb1)
