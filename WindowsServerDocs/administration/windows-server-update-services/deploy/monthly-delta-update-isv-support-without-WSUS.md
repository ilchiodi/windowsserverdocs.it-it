---
title: Supporto di ISV senza WSUS per gli aggiornamenti mensile Delta
description: Argomento Windows Server Update Service (WSUS) - come indipendente dal Software fornitori (ISV) può usare temporaneamente aggiornamento mensile Delta invece di recapito di aggiornamento WSUS Express per ridurre le dimensioni del pacchetto
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: c89d5eb754685fb8000ac2025af391057e77654c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848612"
---
#<a name="monthly-delta-update-isv-support-without-wsus"></a>Supporto di ISV senza WSUS per gli aggiornamenti mensile Delta

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Download aggiornamento Windows 10 può essere estesa perché tutti i pacchetti contiene tutte le correzioni rilasciate in precedenza per garantire la coerenza e la semplicità.  

Fin dalla versione 7, Windows è stato in grado di ridurre le dimensioni del download degli aggiornamenti di Windows con una funzione denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), e anche se i dispositivi consumer supportano per impostazione predefinita, i dispositivi Windows 10 enterprise è richiesto Windows Server Update Services (WSUS) per sfruttare i vantaggi di Express. Se si dispone di WSUS disponibili, vedere [supporto per il recapito ISV gli aggiornamenti Express](express-update-delivery-ISV-support.md). È consigliabile usarlo per abilitare la distribuzione di aggiornamento rapido. 

Se non attualmente è installato WSUS, ma è necessario aggiornare le dimensioni dei pacchetti più piccoli nel frattempo, è possibile utilizzare aggiornamenti differenziali mensile. Aggiornamento delta consente di ridurre le dimensioni dei pacchetti in modo sostanziale, ma non tutte quelle con la distribuzione di aggiornamento WSUS Express. È consigliabile distribuire aggiornamenti rapidi di Windows Server Update Services ogni volta che è possibile che la riduzione massima di dimensioni del pacchetto. Seguito è riportato un grafico che confronta le dimensioni di download Delta, cumulativi ed Express per Windows 10 versione 1607:

![Confronto tra dimensione download](../../media/express-update-delivery-isv-support/delta-1.png)

##<a name="what-is-monthly-delta-update"></a>Che cos'è aggiornamento Delta mensile?

Esistono due varianti di aggiornamento della sicurezza mensili: Delta e aggiornamento cumulativo.

Aggiornamento mensile del Delta è nuova e una soluzione temporanea per gli ISV che non dispongono di WSUS per ridurre le dimensioni dei pacchetti di aggiornamento.

>[!IMPORTANT]
>**Aggiornamento delta è disponibile per la manutenzione di Windows 10, versione 1607 (Anniversary Update), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni dopo la versione 1709, è necessario implementare un'infrastruttura di distribuzione che supporta [recapito update Express](express-update-delivery-ISV-support.md) per continuare a sfruttare gli aggiornamenti incrementali.

Tramite l'aggiornamento mensile Delta, pacchetti conterrà solo gli aggiornamenti di un mese. Cumulativi mensili inclusi tutti gli aggiornamenti fino a tale versione di aggiornamento, risultante in un file di grandi dimensioni che cresce ogni mese. Delta sia gli aggiornamenti mensili vengono rilasciati il secondo martedì di ogni mese, noto anche come "aggiornamento Tuesday". La tabella seguente confronta differenziali e gli aggiornamenti cumulativi:

|                    | Mensile **Delta** aggiornare                                                                                                                                                                                                       | Mensile **cumulativi** aggiornare                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ambito**          | Unico aggiornamento con **solo nuove correzioni per il mese**                                                                                                                                                                           | Unico aggiornamento con tutte le nuove correzioni di tale mese e tutti i precedenti mesi                                                                                                                                                   |
| **Applicazione**    | Può essere applicato solo se l'aggiornamento del mese precedente è stato applicato (cumulativo o Delta)                                                                                                                                           | Può essere applicato in qualsiasi momento                                                                                                                                                                                                |
| **Recapito**       | **Pubblicato solo in Windows Update Catalog** dove possono essere scaricata per l'uso con altri strumenti o processi. Non disponibile per i PC sono connessi a Windows Update                                                         | Pubblicato in Windows Update (in cui tutti i consumer PC verrà installato), WSUS e il catalogo di Windows Update                                                                                                                |

Delta e Cumulative hanno lo stesso numero KB, con la stessa classificazione e versione nello stesso momento. Gli aggiornamenti possono essere distinti da entrambi il titolo dell'aggiornamento nel catalogo, o dal nome del file msu:

- 2017-02 *\***aggiornamento delta**\** per Windows 10 versione 1607 per sistemi basati su x64 (KB1234567)
- 2017-02 *\***aggiornamento cumulativo**\** per Windows 10 versione 1607 per sistemi basati su x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usare aggiornamento mensile del Delta

Se le dimensioni dell'aggiornamento nel dispositivo client rappresenta un problema, è consigliabile utilizzare aggiornamenti differenziali nei dispositivi che dispongono di aggiornamenti del mese precedente e nei dispositivi che sono in ritardo aggiornamento cumulativo. In questo modo, tutti i dispositivi richiedono solo un singolo aggiornamento per riportarle aggiornati. Questa operazione richiede una piccola regolazione nel processo di gestione di aggiornamento complessivo, poiché sarà necessario distribuire diversi aggiornamenti in base sullo stato di aggiornamento dei dispositivi nell'organizzazione sono:

![Confronto tra dimensione download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedire la distribuzione degli aggiornamenti cumulativi e differenziali nello stesso mese

Poiché Delta e aggiornamento cumulativo sono disponibili nello stesso momento, è importante sapere cosa accade se si distribuiscono entrambi gli aggiornamenti dello stesso mese.

Se si approvare e distribuisce la stessa versione del Delta e aggiornamento cumulativo, è non genererà solo il traffico di rete aggiuntive poiché entrambe verrà scaricati nel computer, ma potrebbe non essere in grado di riavviare il computer a Windows dopo il riavvio.

Se i Delta e gli aggiornamenti cumulativi inavvertitamente siano installati e non è più avvio del computer, è possibile ripristinare con i passaggi seguenti:

1. Eseguire l'avvio in ambiente ripristino Windows il prompt dei comandi
2. Elencare i pacchetti in sospeso:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Esempio**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Aprire il file di testo in cui è reindirizzato **get-packages**. Se viene visualizzato qualsiasi installazione in sospeso di patch, eseguire **remove-package** per ogni nome del pacchetto:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Esempio**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Non rimuovere la disinstallazione in attesa di patch.

>[!IMPORTANT]
>**Aggiornamento delta è disponibile per la manutenzione di Windows 10, versione 1607 (Anniversary Update), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni dopo la versione 1709, è necessario implementare un'infrastruttura di distribuzione che supporta [recapito update Express](express-update-delivery-ISV-support.md) per continuare a sfruttare gli aggiornamenti incrementali.
