---
title: Gestire Registrazione inventario software
description: Viene descritto come gestire registrazione inventario Software
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 812173d1-2904-42f4-a9e2-de19effec201
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 095cd2ad799857b789943b4f477aa9e6a8c3ae50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815182"
---
# <a name="manage-software-inventory-logging"></a>Gestire Registrazione inventario software

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Questo documento descrive come gestire registrazione inventario Software, una funzionalità che consente agli amministratori di datacenter a registrare in modo semplice dati di gestione asset software Microsoft per le distribuzioni nel tempo. Questo documento descrive come gestire Registrazione inventario software. Prima di usare registrazione inventario Software con Windows Server 2012 R2, assicurarsi che gli aggiornamenti di Windows [KB 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) sono installati in ogni sistema che devono essere inclusi nell'inventario. Nessun aggiornamento Wndows sono necessario per Windows Server 2016. Questa funzionalità viene eseguita in locale in ogni server di cui deve essere eseguito l'inventario. Non raccoglie dati dai server remoti.  

La funzionalità registrazione inventario Software può essere aggiunto anche a due versioni di Windows Server precedenti a Windows Server 2012 R2. È possibile installare gli aggiornamenti seguenti per aggiungere la funzionalità registrazione inventario Software per Windows Server 2012 e Windows Server 2008 R2 SP1:

- **Windows Server 2012 (Standard o Datacenter Edition)** 

> [!NOTE] 
> Verificare che [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) sia installato prima di applicare il pacchetto di aggiornamento seguente.

-  Pacchetto di aggiornamento di WMF 4.0 per Windows Server 2012: [KB 3119938](https://support.microsoft.com/en-us/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> Verificare che [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) sia installato prima di applicare il pacchetto di aggiornamento seguente.


- Richiede [.NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)


- Pacchetto di aggiornamento di WMF 4.0 per Windows Server 2008 R2: [KB 3109118](https://support.microsoft.com/en-us/kb/3109118)


Ci sono due metodi principali per eseguire l'inventario con questa funzionalità:  
  
1.  Avviare la funzionalità di registrazione SIL per raccogliere da origini dati SIL e inoltrare il payload attraverso la rete a una destinazione (URI) specificata su base oraria.  
  
2.  Eseguire una query manuale dei dati SIL tramite PowerShell o WMI con qualsiasi frequenza.  
  
L'avvio della registrazione SIL richiede una pianificazione e una previsione, ma offre vantaggi significativi rispetto alle query manuali dei dati. L'uso della registrazione SIL offre i seguenti tre vantaggi principali per gli amministratori di datacenter:  
  
-   È possibile raccogliere una cronologia continuativa (registro) nel tempo, per creare report flessibili e completi da un'unica origine.  
  
-   È possibile superare le difficoltà di individuazione dei computer, tipiche in scenari in cui vengono usati molti strumenti di inventario.  
  
-   È possibile superare le difficoltà legate a limiti di trust e necessità di privilegi utente elevati, tipiche in scenari in cui vengono usati molti strumenti di inventario, mantenendo comunque un livello di sicurezza, perché i dati vengono crittografati tramite HTTPS con SSL.  
  
La funzionalità Registrazione inventario software è installata per impostazione predefinita, ma la registrazione non viene avviata per impostazione predefinita. Tutte le configurazioni di Registrazione inventario software vengono eseguite con i cmdlet di PowerShell. Registrazione inventario software prevede solo un numero limitato di opzioni di configurazione. Questo documento descrive queste opzioni e gli scopi previsti, nonché i cmdlet usati per la raccolta dei dati (se si usa il secondo dei metodi descritti sopra).  
  
**In questo documento**  
  
Le opzioni di configurazione descritte in questo documento includono:  
  
-   [Avvio e arresto di Software di registrazione inventario](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [La funzionalità registrazione inventario software nel tempo](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [Visualizzazione dei dati di registrazione inventario Software](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [Eliminazione dei dati registrati da registrazione inventario Software](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [Backup e ripristino dei dati registrati da registrazione inventario Software] gestire-software-inventario-logging.md & BKMK_Step5)  
  
-   [La lettura dei dati registrati e pubblicati da registrazione inventario Software](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [Sicurezza di registrazione inventario software](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [Uso delle impostazioni di data e ora di registrazione inventario Software di Windows Server](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [Abilitazione e configurazione di registrazione inventario Software in un disco rigido virtuale montato](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [Panoramica dell'uso di registrazione inventario Software in Windows Server 2012 R2 senza KB 3000850](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [Usare registrazione inventario Software in un ambiente di Windows Server 2012 R2 Hyper-V senza KB 3000850](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedere Utilizzo di cmdlet.

  
## <a name="BKMK_Step1"></a>Avvio e arresto di Software di registrazione inventario  
Raccolta giornaliera di registrazione inventario software e l'inoltro attraverso la rete deve essere abilitati in un computer che esegue Windows Server 2012 R2 per registrare l'inventario software.  
  
> [!NOTE]  
> È possibile usare il cmdlet **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** di PowerShell per recuperare informazioni sul servizio Registrazione inventario software e determinare, tra le altre cose, se il servizio è in esecuzione o arrestato.  
  
#### <a name="to-start-software-inventory-logging"></a>Per avviare Registrazione inventario software  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Aprire PowerShell come amministratore.  
  
3.  Al prompt di PowerShell digitare **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> È possibile impostare la destinazione senza impostare un'identificazione personale del certificato, ma in tal caso gli inoltri non riusciranno e i dati verranno archiviati in locale per un periodo che può andare fino al valore predefinito di 30 giorni (dopo il quale verranno eliminati). Dopo aver impostato un hash del certificato valido per la destinazione (e aver installato un certificato valido corrispondente nell'archivio Computer locale/Personale), i dati archiviati in locale verranno inoltrati alla destinazione, purché la destinazione sia configurata per accettare questi dati con il certificato (vedere [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) per altre informazioni).  
  
#### <a name="to-stop-software-inventory-logging"></a>Per arrestare Registrazione inventario software  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Aprire PowerShell come amministratore.  
  
3.  Al prompt di PowerShell digitare **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>Configurazione di Registrazione inventario software  
La configurazione di Registrazione inventario software per l'inoltro dei dati a un server di aggregazione nel tempo prevede tre passaggi:  
  
1.  Uso **Set-SilLogging – TargetUri** per specificare l'indirizzo web del server di aggregazione (deve iniziare con "https://?).  
  
2.  Usare **Set SilLogging – CertificateThumbprint** per specificare l'hash di identificazione personale del certificato SSL valido da usare per autenticare le trasmissioni di dati al server di aggregazione (il server di aggregazione dovrà essere configurato per accettare l'hash).  
  
3.  Installare un certificato SSL valido (per la rete) nell' **archivio Computer locale/Personale** (o **/LocalMachine/MY**) del server locale da cui inoltrare i dati  
  
È consigliabile completare questi passaggi prima di usare **Start-SilLogging**.  Se si vuole eseguirli dopo aver usato **Start-SilLogging**, è sufficiente arrestare e avviare nuovamente SIL.  In alternativa, è possibile usare il cmdlet Publish-SilData per assicurarsi che il server di aggregazione disponga di una serie completa dei dati per questo server.  
  
Per una guida completa alla configurazione del framework SIL nel suo complesso, vedere [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).  In particolare, se **Publish-SilData** genera un errore o la registrazione SIL non riesce per altri motivi, vedere la sezione sulla risoluzione dei problemi.  
  
## <a name="BKMK_Step2"></a>La funzionalità registrazione inventario software nel tempo  
Se la funzionalità Registrazione inventario software è stata avviata da un amministratore, la raccolta oraria e l'inoltro dei dati al server di aggregazione (URI di destinazione) hanno inizio. Il primo inoltro sarà un set completo degli stessi dati che [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) recupera e visualizza nella console in un determinato momento. Successivamente, a ogni intervallo, SIL controllerà i dati e inoltrerà solo un piccolo acknowledgment di identificazione al server di aggregazione di destinazione se i dati non sono cambiati rispetto all'ultima raccolta. Se ci sono valori modificati, SIL invierà di nuovo un set di dati completo.  
  
> [!IMPORTANT]  
> Se in un determinato intervallo l'URI di destinazione non è raggiungibile o il trasferimento dei dati attraverso la rete non riesce per qualsiasi motivo, i dati raccolti verranno archiviati in locale per un periodo che può andare fino al valore predefinito di 30 giorni (dopo il quale verranno eliminati). Al successivo inoltro riuscito dei dati al server di aggregazione di destinazione, tutti i dati archiviati in locale verranno inoltrati e i dati memorizzati nella cache locale verranno eliminati.  
  
## <a name="BKMK_Step3"></a>Visualizzazione dei dati di registrazione inventario Software  
Oltre ai cmdlet di PowerShell descritti nella sezione precedente, per raccogliere i dati di Registrazione inventario software è possibile usare altri sei cmdlet:  
  
-   **[Get-SilComputer](https://technet.microsoft.com/library/dn283392.aspx)**: mostra i valori temporizzati per dati specifici correlati al server e al sistema operativo, nonché il nome host o FQDN dell'host fisico, se disponibile.  
  
-   **[Get-SilComputerIdentity (KB 3000850)](https://technet.microsoft.com/library/dn858074.aspx)**: mostra gli identificatori usati da SIL per i singoli server.  
  
-   **[Get-SilData](https://technet.microsoft.com/library/dn283388.aspx)**: mostra una raccolta temporizzata di tutti i dati di Registrazione inventario software.  
  
-   **[Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)**: mostra l'identità temporizzata di tutto il software installato nel computer.  
  
-   **[Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx)**: mostra il numero totale di richieste univoche di dispositivi client e di utenti client del server degli ultimi due giorni.  
  
-   **[Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx)**: mostra l'elenco temporizzato di tutti gli aggiornamenti di Windows installati nel computer.  
  
Uno scenario di caso di utilizzo tipico per i cmdlet di Registrazione inventario software è il caso in cui un amministratore richiede a Registrazione inventario software una raccolta temporizzata di tutti i dati della registrazione con [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx).  
  
**Esempio di output**  
  
```  
PS C:\> Get-SilData   
  
ID                 : 961FF8A1-8549-4BEC-8DF6-3B3E32C26FFA  
UUID               : B49ACB4C-7D9C-4806-9917-AE750BB3DA84  
VMGUID             : E84CCCBD-0D0F-486B-A424-9780C7CF92E4  
Name               : Server01Guest.Test.Contoso.com  
HypervisorHostName : Server01.Test.Contoso.com  
  
ID          : {F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}  
Name        : Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219  
InstallDate : 12/5/2013  
Publisher   : Microsoft Corporation  
Version     : 10.0.40219  
  
ID          : {89F4137D-6C26-4A84-BDB8-2E5A4BB71E00}  
Name        : Microsoft Silverlight  
InstallDate : 3/20/2014  
Publisher   : Microsoft Corporation  
Version     : 5.1.30214.0  
  
ChassisSerialNumber       : 4452-0564-0284-2290-0113-6804-05  
CollectedDateTime         : 10/27/2014 4:01:33 PM  
Model                     : Virtual Machine  
Name                      : Server01Guest.Test.Contoso.com  
NumberOfCores             : 1  
NumberOfLogicalProcessors : 1  
NumberOfProcessors        : 1  
OSName                    : Microsoft Windows Server 2012 R2 Datacenter  
OSSku                     : 8  
OSSuite                   : 400  
OSSuiteMask               : 400  
OSVersion                 : 6.3.9600  
ProcessorFamily           : 179  
ProcessorManufacturer     : GenuineIntel  
ProcessorName             : Intel(R) Xeon(R) CPU           E5440  @ 2.83GHz  
SystemManufacturer        : Microsoft Corporation  
  
```  
  
> [!NOTE]  
> L'output di questo cmdlet equivale a quello di tutti gli altri cmdlet **Get-Sil** combinati per questa funzionalità, ma viene fornito alla console in modo asincrono e quindi l'ordine degli oggetti potrebbe non essere sempre lo stesso.  
>   
> Non è necessario che la funzionalità Registrazione inventario software sia avviata per usare i cmdlet **Get-Sil**.  
  
## <a name="BKMK_Step4"></a>Eliminazione dei dati registrati da registrazione inventario Software  
Registrazione inventario software non è progettato come componente cruciale. È pensato per un impatto minimo sulle operazioni di sistema locali, pur mantenendo un alto livello di affidabilità. Ciò consente inoltre all'amministratore di eliminare manualmente i database di registrazione inventario Software e i file (tutti i file nella directory \Windows\System32\LogFiles\SIL) di supporto per soddisfare le esigenze operative.  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>Per eliminare i dati registrati da Registrazione inventario software  
  
1.  In PowerShell arrestare Registrazione inventario software con il comando **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** .  
  
2.  Aprire Esplora risorse.  
  
3.  Passare a **\Windows\System32\Logfiles\SIL\**  
  
4.  Eliminare tutti i file nella cartella.  
  
## <a name="BKMK_Step5"></a>Backup e ripristino dei dati registrati da registrazione inventario Software  
Registrazione inventario software archivia temporaneamente le raccolte orarie dei dati, se l'inoltro attraverso la rete non riesce. I file di registro sono archiviati nella directory \Windows\System32\LogFiles\SIL\. I backup dei dati di Registrazione inventario software possono essere effettuati con i backup dei server pianificati a intervalli regolari.  
  
> [!IMPORTANT]  
> Se per qualsiasi motivo è necessario un aggiornamento o un'installazione di ripristino del sistema operativo, i file di registro archiviati in locale andranno persi.  Se questi dati sono fondamentali per le operazioni, è consigliabile eseguire il backup prima dell'installazione del nuovo sistema operativo. Dopo il ripristino o l'aggiornamento, è sufficiente ripristinare i dati nello stesso percorso.  
  
> [!NOTE]  
> Se per qualsiasi motivo il periodo di conservazione di gestire la durata dei dati registrati in locale da SIL diventa importante, ciò può essere configurato impostando il valore del Registro di sistema qui: \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. Il valore predefinito è "30", che indica 30 giorni.  
  
## <a name="BKMK_Step6"></a>La lettura dei dati registrati e pubblicati da registrazione inventario Software  
I dati registrati da SIL ma archiviati in locale (se l'inoltro all'URI di destinazione non riesce) o i dati correttamente inoltrati al server di aggregazione di destinazione vengono archiviati in un file binario (per i dati di ogni giorno). Per visualizzare questi dati in PowerShell, usare il cmdlet [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) .  
  
## <a name="BKMK_Step7"></a>Sicurezza di registrazione inventario software  
Per recuperare correttamente i dati dalle API di PowerShell e WMI di Registrazione inventario software sono necessari privilegi amministrativi nel server locale.  
  
Per poter sfruttare correttamente tutte le caratteristiche della funzionalità Registrazione inventario software per inoltrare i dati a un punto di aggregazione in modo continuativo (a intervalli orari) nel tempo, un amministratore deve usare i certificati client per garantire sessioni SSL sicure per il trasferimento dei dati tramite HTTPS. Una panoramica di base dell'autenticazione HTTPS è disponibile qui: [Autenticazione HTTPS](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx).  
  
Tutti i dati archiviati in locale in un server Windows (solo nel caso in cui la funzionalità venga avviata ma la destinazione non sia raggiungibile per qualsiasi motivo) sono accessibili solo con privilegi amministrativi nel server locale.  
  
## <a name="BKMK_Step8"></a>Uso delle impostazioni di data e ora in Windows Server 2012 R2 registrazione inventario Software  
  
-   Quando si usa [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay per impostare l'ora di esecuzione della registrazione SIL, è necessario specificare una data e un'ora. Verrà impostata la data del calendario e la registrazione non verrà eseguita fino a quando non viene raggiunta la data, in base all'ora di sistema locale.  
  
-   Quando si usa [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx), o [Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx), "InstallDate? mostreranno sempre 12:00:00 AM, un valore non significativo.  
  
-   Quando si usa [Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx), "SampleDate? mostreranno sempre 11:59:00 PM, un valore non significativo.  La data corrisponde alla data pertinente per le query dei cmdlet.  
  
## <a name="BKMK_Step10"></a>Abilitazione e configurazione di registrazione inventario Software in un disco rigido virtuale montato  
Registrazione inventario software supporta anche la configurazione e l'abilitazione in macchine virtuali offline. Gli usi pratici per questo scenario comprendono la configurazione di "immagini gold" per la distribuzione su vasta scala nei datacenter, nonché la configurazione delle immagini degli utenti finali da una distribuzione locale a una distribuzione cloud.  
  
Per supportare questi tipi di uso, Registrazione inventario software dispone di voci del Registro di sistema associate a ogni opzione configurabile.  Questi valori del Registro di sistema sono disponibili in \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging.  
  
|||||  
|-|-|-|-|  
|**(Funzione)**|**Nome del valore**|**Dati**|**Cmdlet corrispondente (disponibile solo nel sistema operativo in esecuzione)**|  
|Funzionalità di avvio/arresto|CollectionState|1 o 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx), [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|Specifica il punto di aggregazione di destinazione nella rete|TargetUri|string|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TargetURI|  
|Specifica l'hash o l'identificazione personale del certificato usato per l'autenticazione SSL per il server Web di destinazione|CertificateThumbprint|string|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|Specifica la data e l'ora in cui la funzionalità deve essere avviata (se il valore impostato è nel futuro in base all'ora di sistema locale)|CollectionTime|Default:  2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
Per modificare questi valori in un disco rigido virtuale offline (sistema operativo VM non in esecuzione), il disco rigido virtuale deve prima di tutto essere montato e quindi è possibile usare i comandi seguenti per apportare modifiche:  
  
-   [Carico Reg](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [Reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [REG aggiungere](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [Reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
Registrazione inventario software controllerà questi valori all'avvio del sistema operativo ed eseguirà i comandi di conseguenza.  
  
## <a name="BKMK_Step11"></a>Panoramica dell'uso di registrazione inventario Software in Windows Server 2012 R2 senza KB 3000850  
Con l'aggiornamento [KB 3000850](https://support.microsoft.com/kb/3000850)sono state apportate le modifiche seguenti alla funzionalità e alle impostazioni predefinite di Registrazione inventario software:  
  
-   L'intervallo predefinito per la raccolta e l'inoltro attraverso la rete quando viene avviata la registrazione SIL è stato modificato da ogni giorno a ogni ora (in un momento casuale all'interno di ogni ora).  
  
-   Il payload di dati predefinito è stato ridotto per includere solo gli oggetti di Get-SilComputer, Get-SilComputerIdentity e Get-SilSoftware.  
  
-   La comunicazione nel canale da guest a host in ambienti Hyper-V è stata rimossa.  
  
## <a name="BKMK_Step12"></a>Usare registrazione inventario Software in un ambiente di Windows Server 2012 R2 Hyper-V senza KB 3000850  
  
> [!NOTE]  
> Questa funzionalità è stata rimossa con l'installazione dell'aggiornamento [KB 3000850](https://support.microsoft.com/kb/3000850) .  
  
Quando si usa registrazione inventario Software in un host Windows Server 2012 R2 Hyper-V, è possibile recuperare i dati SIL da utenti guest di Windows Server 2012 R2 che sono in esecuzione in locale, se la registrazione SIL è stata avviata nei nei guest. Tuttavia, questo è solo possibile quando si usa il cmdlet Get-SilData e Publish-SilData di Powershell e solo con WIndows Server 2012 R2 che nel guest e host.  Lo scopo di questa funzionalità è quello di consentire agli amministratori di datacenter che forniscono VM guest ai tenant, o ad altre entità di una grande azienda, di acquisire i dati dell'inventario software nell'host hypervisor e successivamente inoltrare tutti i dati a un aggregatore (o un URI di destinazione).  
  
Di seguito sono riportati due esempi di output in PowerShell (molto abbreviati) console sarà simile a un host Windows Server 2012 R2 Hyper-V che esegue un guest di Windows Server 2012 R2 macchina virtuale con la registrazione SIL avviata.  Si noterà nel primo esempio, che usa solo Get-SilData, che l'output contiene tutti i dati dell'host, come previsto.  Sono inclusi anche tutti i dati SIL del guest, ma in formato compresso.  Per espandere e visualizzare i dati del guest, tagliare e incollare il frammento di codice usato nel secondo esempio riportato di seguito.  Agli oggetti dati SIL del guest sarà sempre associato il GUID di VM.  
  
> [!NOTE]  
> Poiché i dati SIL vengono trasmessi nella console, quando si usa il cmdlet Get-SilData nei flussi di dati, l'ordine degli oggetti nell'output non sarà sempre prevedibile.  Nei due esempi seguenti il testo è stato codificato con colori diversi (blu per i dati dell'host fisico e verde per i dati del guest virtuale) solo come strumento illustrativo per questo documento.  
  
**Output di esempio 1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**Output di esempio 2** (con la funzione Expand-SilData)  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>Vedere anche  
[Introduzione a Software di registrazione inventario](get-started-with-software-inventory-logging.md)  
[Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md)  
[Cmdlet di registrazione inventario software in Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)  
[Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx)  
[Export-BinaryMiLog](https://technet.microsoft.com/library/dn262591.aspx)  
  

