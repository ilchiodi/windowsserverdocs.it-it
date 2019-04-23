---
title: Canali di manutenzione
description: 'Descrizione dei canali del servizio Windows Server: Configurazione superficie di attacco e LTSC'
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: c4329339e37acbb0b589e767a570073f4ae1363f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848732"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Windows Server i canali di manutenzione: Configurazione superficie di attacco e LTSC

>Si applica a: Windows Server 2019, Windows Server 2016


**Esistono due canali di rilascio principale disponibile ai clienti di Windows Server, il canale di manutenzione a lungo termine e il canale semestrale.** 

Puoi mantenere i server sul canale Long-Term Servicing Channel (LTSC), spostarli nel nuovo Canale semestrale o disporre di alcuni server in una delle tracce in base alle tue esigenze.


## <a name="long-term-servicing-channel-ltsc"></a>Long-Term Servicing Channel (LTSC)
Questo modello di rilascio, in precedenza denominato "Long-Term Servicing *Branch*", è già noto agli utenti e offre il rilascio di una nuova versione principale di Windows Server ogni 2 o 3 anni. Gli utenti hanno diritto a cinque anni di supporto mainstream e cinque anni di supporto esteso. Questo canale è appropriato per i sistemi che richiedono tempi di manutenzione più lunghi e una maggiore stabilità funzionale. Le distribuzioni di Windows Server 2016 e delle versioni precedenti di Windows Server non verranno influenzate dai nuovi rilasci di Canale semestrale. Il Long-Term Servicing Channel continuerà a ricevere sia aggiornamenti della sicurezza sia aggiornamenti non relativi alla sicurezza, ma non riceverà le nuove funzioni e funzionalità.

> [!Note]  
> **Il prodotto LTSC corrente è Windows Server 2019**. Se desideri mantenere questo canale, ti consigliamo di installare (o continuare a usare) Windows Server 2019, che può essere installato nell'opzione di installazione dei componenti di base del server o nell'opzione di installazione Server con Esperienza desktop.

## <a name="semi-annual-channel"></a>Canale semestrale 
Canale semestrale è perfetto per i clienti che sono l'innovazione rapida possa sfruttare i vantaggi delle nuove funzionalità del sistema operativo a un ritmo più veloce, entrambi nelle applicazioni, in particolare quelli basato su microservizi e contenitori, nonché in definita dal software Data Center ibridi. Per i prodotti Windows Server in Canale semestrale saranno disponibili nuovi rilasci due volte l'anno, in primavera e in autunno. Ogni versione rilasciata in questo canale verrà supportata per 18 mesi a partire dal rilascio iniziale.

La maggior parte delle funzionalità rilasciate nel Canale semestrale verrà introdotta nel successivo rilascio del Long-Term Servicing Channel di Windows Server. Le edizioni, le funzionalità e il contenuto di supporto possono variare da rilascio a rilascio in base al feedback dei clienti.

Canale semestrale sarà disponibile per i clienti con contratti multilicenza con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), nonché tramite Azure Marketplace o altri provider di servizi cloud/di hosting e programmi fedeltà come Sottoscrizioni di Visual Studio.

> [!Note]  
> **La versione corrente di semi-annuale canale è Windows Server, versione 1809**. Se desideri inserire i server in questo canale, devi installare Windows Server versione 1809, che può essere installato in modalità Server Core o eseguito come Nano Server in un contenitore. Gli aggiornamenti sul posto da Windows Server 2016 a Windows Server, versione 1809 non sono supportati, in quanto sono inclusi in **canali di rilascio differenti**. Windows Server versione 1809 non è un aggiornamento a Windows Server 2016 – è la prossima versione di Windows Server nel canale semi-annuale.



In questo modello, le versioni di Windows Server sono identificate dall'anno e dal mese di rilascio: ad esempio, nel 2017 una versione del mese 9 (settembre) viene identificata come **versione 1709**. I nuovi rilasci di Windows Server nel Canale semestrale verranno eseguiti due volte l'anno. Il ciclo di vita del supporto per ogni versione è 18 mesi.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>Devi mantenere i server nel canale LTSC o spostarli nel Canale semestrale?
Ecco le due principali differenze da prendere in considerazione:

- Devi eseguire rapidamente l'innovazione? Hai bisogno di accesso in anteprima alle funzionalità di Windows Server più recenti? Hai bisogno del supporto per infrastrutture Hyper-V, DevOps e applicazioni ibride a rilascio rapido? In tal caso, è consigliabile considerare di **accedere al Canale semiannuale** installando **Windows Server, versione 1809**. Come descritto in questo argomento, riceverai nuove versioni due volte l'anno, con 18 mesi di supporto in produzione Mainstream per ogni versione, da ottenere tramite servizi di sottoscrizione di Visual Studio, Azure o contratti multilicenza. Attualmente, le versioni del Canale semestrale richiedono contratti multilicenza e Software Assurance, se prevedi di eseguire il prodotto nell'ambiente di produzione.
- Hai bisogno di stabilità e prevedibilità? Hai bisogno di eseguire le macchine virtuali e i carichi di lavoro tradizionali in server fisici? In tal caso, ti consigliamo di considerare il fatto di **mantenere tali server sul Long-Term Servicing Channel**. La versione LTSC corrente è **Windows Server 2019**. Come descritto in questo argomento, avrai accesso alle nuove versioni ogni 2-3 anni, con 5 anni di supporto mainstream, seguiti da cinque anni di supporto esteso per ogni versione. Le versioni LTSC sono disponibili tramite tutti i meccanismi di rilascio. Le versioni nel canale LTSC sono disponibili per tutti gli utenti indipendentemente dal modello di gestione licenze in uso. 

Nella tabella seguente sono riepilogate le differenze principali tra i canali:

|  | Long-Term Servicing Channel (Windows Server 2019) |Canale semestrale (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Scenari consigliati | Generici file server, Microsoft e carichi di lavoro non Microsoft, App tradizionale, ruoli dell'infrastruttura, definiti software Datacenter e infrastruttura hyper convergenza | Applicazioni in contenitori, host contenitore e scenari di applicazioni che traggono vantaggio da un'innovazione più rapida |
| Nuove versioni | Ogni 2-3 anni |Ogni 6 mesi |
| Supporto |5 anni di supporto "Mainstream", più 5 anni di supporto "Extended" | 18 mesi |
| Edizioni | Tutte le edizioni di Windows Server disponibili | Edizione standard e Datacenter |
| Chi può utilizzare | Tutti i clienti attraverso tutti i canali | Software Assurance e cloud i clienti |
| Opzioni di installazione | Server Core e Server con esperienza Desktop | Server Core di host contenitore e immagine e Server aspetti contenitore immagine |                |


## <a name="device-compatibility"></a>Compatibilità dei dispositivi
Se non diversamente comunicato, i requisiti hardware minimi per eseguire i rilasci di Canale semestrale corrisponderanno a quelli del rilascio più recente di Windows Server del Long-Term Servicing Channel. Ad esempio, il **corrente rilascio Long-Term Servicing Channel è Windows Server 2019**. La maggior parte dei driver hardware continuerà a funzionare con questi rilasci.

## <a name="servicing"></a>Manutenzione
Sia i rilasci del Long-Term Servicing Channel sia quelli del Canale semestrale verranno supportati da aggiornamenti della sicurezza e aggiornamenti non relativi alla sicurezza. La differenza consiste nella durata del supporto del rilascio, come descritto in precedenza.

### <a name="servicing-tools"></a>Strumenti di manutenzione
Esistono molti strumenti con cui i professionisti IT possono eseguire la manutenzione di Windows Server. Ogni opzione presenta vantaggi e svantaggi, sia a livello di funzionalità che di controllo oppure di semplicità e requisiti limitati per l'amministrazione. Ecco alcuni esempi di strumenti di manutenzione disponibili per gestire gli aggiornamenti di manutenzione:

- **Windows Update (autonomo)**: Questa opzione è disponibile solo per i server che sono connessi a Internet e con aggiornamento di Windows abilitato.
- **Windows Server Update Services (WSUS)** consente un controllo esteso degli aggiornamenti di Windows 10 e Windows Server ed è disponibile in modalità nativa nel sistema operativo Windows Server. Oltre alla possibilità di rinviare gli aggiornamenti, le organizzazioni possono aggiungere un livello di approvazione per gli aggiornamenti e scegliere di distribuirli in computer o gruppi di computer specifici quando sono pronti.
- **System Center Configuration Manager** offre il massimo controllo sulla manutenzione. I professionisti IT possono rinviare gli aggiornamenti, approvarli e hanno a disposizione più opzioni per decidere la destinazione delle distribuzioni e gestire l'utilizzo della larghezza di banda e i tempi di distribuzione.

Probabilmente si è già scelto di utilizzare almeno una di queste opzioni in base alle risorse, al personale e alle competenze. Lo stesso processo può essere utilizzato per i rilasci del Canale semestrale: ad esempio, se già si utilizza System Center Configuration Manager per gestire gli aggiornamenti, è possibile continuare a usarlo. In modo analogo, se si usa WSUS, è possibile continuare a usarlo.

## <a name="obtain-preview-releases-through-the-windows-insider-program"></a>Ottenere le versioni di anteprima tramite il Programma Windows Insider
Testare le build preliminari di Windows Server è utile sia a Microsoft sia ai suoi clienti in quanto permette di individuare eventuali problemi prima del rilascio. Offre inoltre ai clienti un'opportunità unica per ottimizzare direttamente le funzionalità del prodotto. 

Microsoft tiene in grande considerazione il feedback ricevuto durante il processo di sviluppo in quanto permette di apportate al più presto le modifiche necessarie. Il test preliminare e il feedback che ne consegue sono essenziali per il modello di rilascio rapido.

Per altre informazioni su come entrare nel Programma Windows Insider, consulta la sezione [Programma Windows Insider per documenti di Server](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="to-identify-windows-server-2019-and-windows-server-version-1809"></a>Per identificare Windows Server 2019 e Windows Server, versione 1809

>[!Note]  
> Le seguenti indicazioni sono progettate per identificare e differenziare LTSC e SAC solo a scopi di inventario e del ciclo di vita generali.  Non sono ideate per la compatibilità delle applicazioni o per rappresentare una superficie API specifica.  Gli sviluppatori di app devono usare le linee guida altrove per garantire la corretta compatibilità di componenti, API e funzionalità che possono essere aggiunti durante il ciclo di vita di un sistema o non ancora aggiunti. [Versione del sistema operativo](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) è un punto di partenza migliore per gli sviluppatori di app.

Apri Powershell e usa il Cmdlet Get-ItemProperty o Get-ComputerInfo, per controllare queste proprietà nel registro di sistema.  Insieme a numero di build, indicherà LTSC o SAC in base alla presenza o alla relativa mancanza, dell'anno contrassegnato, ad esempio 2019.  LTSC lo indica, SAC non lo indica.  Ciò indicherà anche la tempistica di rilascio tramite ReleaseId o WindowsVersion, ad esempio 1809, e indicherà se l'installazione è Server Core o Server con esperienza Desktop. 

**Windows Server 2019 Datacenter Edition (LTSC) con esempio di esperienza Desktop:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server, ad esempio di base del Server Standard Edition di versione 1809 (SAC):**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Esempio di Windows Server 2019 Standard Edition (LTSC) Server Core:**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

Per eseguire una query e osservare se la nuova [Funzionalità di compatibilità dell'app Server Core su richiesta](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) è presente in un server, utilizzare il Cmdlet [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) e cercare:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

# <a name="related-topics"></a>Argomenti correlati
[Modifiche in Nano Server in Windows Server come canale semestrale](../get-started/nano-in-semi-annual-channel.md)

[Ciclo di vita del supporto per Windows Server](https://support.microsoft.com/lifecycle)

[Determinare se è in esecuzione Server Core](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Funzione GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlet di registrazione inventario software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
