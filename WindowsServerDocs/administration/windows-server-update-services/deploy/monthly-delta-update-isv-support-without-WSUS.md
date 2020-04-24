---
title: Supporto ISV per l'aggiornamento delta mensile senza WSUS
description: "Argomento di Windows Server Update Services (WSUS): Informazioni su come i fornitori di software indipendente possono usare temporaneamente l'aggiornamento delta mensile anziché il recapito dell'aggiornamento rapido di WSUS per ridurre le dimensioni del pacchetto"
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 3ccddd3bfd55ae340dc5273905bb475e7d2cb98a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828744"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Supporto ISV per l'aggiornamento delta mensile senza WSUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

I download degli aggiornamenti di Windows 10 possono avere dimensioni elevate perché ogni pacchetto contiene tutte le correzioni rilasciate in precedenza per garantire coerenza e semplicità.  

Dalla versione 7, Windows è stato in grado di ridurre le dimensioni dei download di Windows Update con una funzionalità denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2) e, sebbene i dispositivi consumer la supportino per impostazione predefinita, i dispositivi Windows 10 Enterprise richiedono Windows Server Update Services (WSUS) per sfruttare i vantaggi offerti da Express. Se WSUS è disponibile, vedi [Supporto ISV per il recapito dell'aggiornamento rapido](express-update-delivery-ISV-support.md). Consigliamo di usarlo per abilitare il recapito dell'aggiornamento rapido. 

Se WSUS non è attualmente installato, ma nel frattempo sono necessarie dimensioni del pacchetto di aggiornamento minori, puoi usare l'aggiornamento delta mensile. L'aggiornamento delta riduce notevolmente le dimensioni del pacchetto, anche se non allo stesso modo del recapito dell'aggiornamento rapido di WSUS. Consigliamo di distribuire l'aggiornamento rapido di WSUS, laddove possibile, per la massima riduzione delle dimensioni del pacchetto. Di seguito è riportato un grafico che confronta le dimensioni dei download delta, cumulativo e rapido per Windows 10 versione 1607:

![Confronto dimensioni download](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Che cos'è l'aggiornamento delta mensile?

Esistono due varianti dell'aggiornamento della sicurezza mensile: delta e cumulativo.

L'aggiornamento delta mensile è una novità e rappresenta una soluzione provvisoria per i fornitori di software indipendente che non dispongono di WSUS per ridurre le dimensioni del pacchetto di aggiornamento.

>[!IMPORTANT]
>**L'aggiornamento delta è disponibile per la manutenzione di Windows 10, versione 1607 (Aggiornamento dell'anniversario), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni successive alla 1709, dovrai implementare un'infrastruttura di distribuzione che supporti il [recapito dell'aggiornamento rapido](express-update-delivery-ISV-support.md), in modo da continuare a sfruttare i vantaggi degli aggiornamenti incrementali.

Con l'aggiornamento delta mensile, i pacchetti conterranno solo gli aggiornamenti relativi a un mese. L'aggiornamento cumulativo mensile contiene tutti gli aggiornamenti fino alla versione di aggiornamento corrente. Si tratta quindi di un file di grandi dimensioni che cresce ogni mese. Sia gli aggiornamenti delta che quelli mensili vengono rilasciati il secondo martedì di ogni mese, servizio noto anche come Patch Tuesday. La tabella seguente confronta gli aggiornamenti delta e cumulativi:

|                    | Aggiornamento **delta** mensile                                                                                                                                                                                                       | Aggiornamento **cumulativo** mensile                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ambito**          | Aggiornamento singolo contenente **solo le nuove correzioni per il mese corrente**                                                                                                                                                                           | Aggiornamento singolo contenente tutte le nuove correzioni per il mese corrente e per tutti i mesi precedenti                                                                                                                                                   |
| **Applicazione**    | Può essere applicato solo se è stato installato l'aggiornamento del mese precedente (cumulativo o delta)                                                                                                                                           | Può essere applicato in qualsiasi momento                                                                                                                                                                                                |
| **Recapito**       | **Pubblicato solo in Catalogo di Windows Update** dove può essere scaricato per l'uso con altri strumenti o processi. Non offerto ai computer connessi a Windows Update                                                         | Pubblicato in Windows Update (da cui può essere installato in tutti i computer consumer), WSUS e nel Catalogo di Windows Update                                                                                                                |

Gli aggiornamenti delta e cumulativo hanno lo stesso numero di KB, con la stessa classificazione e vengono rilasciati nello stesso momento. Gli aggiornamenti possono essere distinti dal titolo dell'aggiornamento nel catalogo oppure dal nome del file con estensione msu:

- 02-2017 *\***Aggiornamento delta**\**  per Windows 10 versione 1607 per sistemi basati su processore x64 (KB1234567)
- 02-2017 *\***Aggiornamento cumulativo**\**  per Windows 10 versione 1607 per sistemi basati su processore x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usare l'aggiornamento delta mensile

Se le dimensioni dell'aggiornamento per il dispositivo client rappresentano un problema, consigliamo di usare l'aggiornamento delta nei dispositivi che dispongono dell'aggiornamento del mese precedente e l'aggiornamento cumulativo nei dispositivi meno aggiornati. In questo modo, sarà sufficiente un solo aggiornamento. Questa operazione richiede un piccolo adattamento nel processo generale di gestione degli aggiornamenti, in quanto dovrai distribuire aggiornamenti diversi in base a quanto sono recenti quelli installati nei dispositivi dell'organizzazione:

![Confronto dimensioni download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Evitare la distribuzione di aggiornamenti delta e cumulativi nello stesso mese

Poiché l'aggiornamento delta e quello cumulativo sono disponibili nello stesso momento, è importante comprendere le conseguenze della distribuzione di entrambi gli aggiornamenti nello stesso mese.

Se approvi e distribuisci la stessa versione dell'aggiornamento delta e di quello cumulativo, non solo genererai traffico di rete aggiuntivo, poiché entrambi verranno scaricati nel computer, ma potresti non riuscire a riavviare il computer in Windows.

Se gli aggiornamenti delta e cumulativo vengono installati inavvertitamente e il computer non è più in fase di avvio, puoi eseguire il ripristino con i passaggi seguenti:

1. Avvia il prompt dei comandi di WinRE
2. Elenca i pacchetti con stato in sospeso:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Esempio**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Apri il file di testo in cui hai inviato tramite pipe **get-packages**. Se visualizzi eventuali patch di installazione in sospeso, esegui **remove-package** per ogni nome di pacchetto:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Esempio**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Non rimuovere la disinstallazione delle patch in sospeso.

>[!IMPORTANT]
>**L'aggiornamento delta è disponibile per la manutenzione di Windows 10, versione 1607 (Aggiornamento dell'anniversario), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni successive alla 1709, dovrai implementare un'infrastruttura di distribuzione che supporti il [recapito dell'aggiornamento rapido](express-update-delivery-ISV-support.md), in modo da continuare a sfruttare i vantaggi degli aggiornamenti incrementali.
