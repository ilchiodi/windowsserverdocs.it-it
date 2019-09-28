---
title: Supporto per ISV con aggiornamento delta mensile senza WSUS
description: "Argomento Windows Server Update Service (WSUS): come i fornitori di software indipendenti (ISV) possono utilizzare temporaneamente l'aggiornamento delta mensile anziché il recapito di aggiornamenti di WSUS Express per ridurre le dimensioni del pacchetto"
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 4607827d73c34f50f721a2774fa498eb95f9dbb8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361735"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Supporto per ISV con aggiornamento delta mensile senza WSUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

I download degli aggiornamenti di Windows 10 possono essere di grandi dimensioni perché ogni pacchetto contiene tutte le correzioni rilasciate in precedenza per garantire coerenza e semplicità.  

Dalla versione 7, Windows è stato in grado di ridurre le dimensioni dei download di Windows Update con una funzionalità denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)e, sebbene i dispositivi consumer lo supportino per impostazione predefinita, i dispositivi Windows 10 enterprise richiedono Windows Server Update Services (WSUS) vantaggio di Express. Se è disponibile WSUS, vedere [supporto ISV per la distribuzione di aggiornamenti rapidi](express-update-delivery-ISV-support.md). È consigliabile usarlo per abilitare il recapito di aggiornamenti rapidi. 

Se WSUS non è attualmente installato, ma sono necessarie dimensioni del pacchetto di aggiornamento minori nel frattempo, è possibile usare l'aggiornamento delta mensile. L'aggiornamento differenziale riduce notevolmente le dimensioni dei pacchetti, ma non tanto quanto il recapito degli aggiornamenti di WSUS Express. Si consiglia di distribuire WSUS Express Update laddove possibile per la massima riduzione delle dimensioni dei pacchetti. Di seguito è riportato un grafico che confronta le dimensioni Delta, cumulative ed Express per Windows 10 versione 1607:

![Confronto dimensioni download](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Che cos'è l'aggiornamento delta mensile?

Esistono due varianti dell'aggiornamento mensile della sicurezza: Delta e cumulativi.

Aggiornamento delta mensile è una novità e una soluzione provvisoria per ISV che non dispongono di WSUS disponibile per ridurre le dimensioni dei pacchetti di aggiornamento.

>[!IMPORTANT]
>**L'aggiornamento Delta è disponibile per la manutenzione di Windows 10, versione 1607 (aggiornamento dell'anniversario), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni successive alla versione 1709, è necessario implementare un'infrastruttura di distribuzione che supporta il [recapito di aggiornamenti rapidi](express-update-delivery-ISV-support.md) per continuare a sfruttare gli aggiornamenti incrementali.

Con l'aggiornamento delta mensile, i pacchetti conterranno solo gli aggiornamenti di un mese. Cumulativa mensile contiene tutti gli aggiornamenti fino alla versione di aggiornamento, ottenendo un file di grandi dimensioni che cresce ogni mese. Sia gli aggiornamenti delta che quelli mensili vengono rilasciati il secondo martedì di ogni mese, noto anche come "Update Tuesday". Nella tabella seguente vengono confrontati gli aggiornamenti delta e cumulativi:

|                    | Aggiornamento **Delta** mensile                                                                                                                                                                                                       | Aggiornamento **cumulativo** mensile                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ambito**          | Singolo aggiornamento con **solo nuove correzioni per il mese**                                                                                                                                                                           | Aggiornamento singolo con tutte le nuove correzioni per quel mese e per tutti i mesi precedenti                                                                                                                                                   |
| **Applicazione**    | Può essere applicato solo se è stato applicato l'aggiornamento del mese precedente (cumulativo o Delta)                                                                                                                                           | Può essere applicato in qualsiasi momento                                                                                                                                                                                                |
| **Consegna**       | **Pubblicata solo per Windows Update Catalogo** , dove può essere scaricato per l'uso con altri strumenti o processi. Non offerto ai PC connessi a Windows Update                                                         | Pubblicato per Windows Update (dove tutti i PC consumer lo installeranno), WSUS e il catalogo Windows Update                                                                                                                |

Delta e cumulativa hanno lo stesso numero di KB, con la stessa classificazione e rilascio nello stesso momento. Gli aggiornamenti possono essere distinti dal titolo dell'aggiornamento nel catalogo oppure dal nome del MSU:

- 2017-02 *\*aggiornamentoDelta\*perWindows 10*versione1607persistemibasati su x64 (KB1234567)
- 2017-02 *\*aggiornamentocumulativo\*perWindows 10*versione1607persistemibasati su x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usare l'aggiornamento delta mensile

Se le dimensioni dell'aggiornamento per il dispositivo client rappresentano un problema, è consigliabile usare l'aggiornamento Delta nei dispositivi che hanno l'aggiornamento del mese precedente e l'aggiornamento cumulativo sui dispositivi che stanno per rimanere indietro. In questo modo, tutti i dispositivi richiedono un solo aggiornamento per aggiornarli. Questa operazione richiede un piccolo adattamento nel processo di gestione degli aggiornamenti globale, in quanto sarà necessario distribuire aggiornamenti diversi in base alla modalità di aggiornamento dei dispositivi nell'organizzazione:

![Confronto dimensioni download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedisci la distribuzione di aggiornamenti delta e cumulativi nello stesso mese

Poiché l'aggiornamento Delta e l'aggiornamento cumulativo sono disponibili allo stesso tempo, è importante comprendere cosa accade se si distribuiscono entrambi gli aggiornamenti nello stesso mese.

Se si approva e si distribuisce la stessa versione del Delta e dell'aggiornamento cumulativo, non verrà generato solo il traffico di rete aggiuntivo poiché entrambi verranno scaricati nel computer, ma potrebbe non essere possibile riavviare il computer in Windows dopo il riavvio.

Se gli aggiornamenti delta e cumulativi vengono installati inavvertitamente e il computer non è più in fase di avvio, è possibile eseguire il ripristino con i passaggi seguenti:

1. Avviare il prompt dei comandi di WinRE
2. Elencare i pacchetti in uno stato in sospeso:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Esempio**:` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Aprire il file di testo in cui è stato inviato tramite pipe **Get-Packages**. Se vengono visualizzate le patch di installazione in sospeso, eseguire **Remove-Package** per ogni nome di pacchetto:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Esempio**:`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Non rimuovere la disinstallazione delle patch in sospeso.

>[!IMPORTANT]
>**L'aggiornamento Delta è disponibile per la manutenzione di Windows 10, versione 1607 (aggiornamento dell'anniversario), versione 1703 (Creators Update) e versione 1709 (Fall Creators Update).** Per le versioni successive alla versione 1709, è necessario implementare un'infrastruttura di distribuzione che supporta il [recapito di aggiornamenti rapidi](express-update-delivery-ISV-support.md) per continuare a sfruttare gli aggiornamenti incrementali.
