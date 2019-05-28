---
title: Canali di manutenzione
description: 'Descrizione dei canali del servizio Windows Server: Configurazione superficie di attacco e LTSC'
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 625d71edd00ce404cee9525e06a2237d8be4cfcb
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976464"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Windows Server i canali di manutenzione: Configurazione superficie di attacco e LTSC

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

I clienti di Windows Server hanno a disposizione due canali di rilascio principali: Long-Term Servicing Channel e il nuovo Canale semestrale.

Puoi mantenere i server sul canale Long-Term Servicing Channel (LTSC), spostarli nel nuovo Canale semestrale o disporre di alcuni server in una delle tracce in base alle tue esigenze.

## <a name="long-term-servicing-channel-ltsc"></a>Long-Term Servicing Channel (LTSC)

Questo modello di rilascio, in precedenza denominato "Long-Term Servicing *Branch*", è già noto agli utenti e offre il rilascio di una nuova versione principale di Windows Server ogni 2 o 3 anni. Gli utenti hanno diritto a cinque anni di supporto mainstream e cinque anni di supporto esteso. Questo canale è appropriato per i sistemi che richiedono tempi di manutenzione più lunghi e una maggiore stabilità funzionale. Le distribuzioni di Windows Server 2016 e delle versioni precedenti di Windows Server non verranno influenzate dai nuovi rilasci di Canale semestrale. Il Long-Term Servicing Channel continuerà a ricevere sia aggiornamenti della sicurezza sia aggiornamenti non relativi alla sicurezza, ma non riceverà le nuove funzioni e funzionalità.

> [!Note]  
> **Il prodotto LTSC corrente è Windows Server 2019**. Se desideri mantenere questo canale, ti consigliamo di installare (o continuare a usare) Windows Server 2019, che può essere installato nell'opzione di installazione dei componenti di base del server o nell'opzione di installazione Server con Esperienza desktop.

## <a name="semi-annual-channel"></a>Canale semestrale

Canale semestrale è perfetto per i clienti che sono l'innovazione rapida possa sfruttare i vantaggi delle nuove funzionalità del sistema operativo a un ritmo più veloce, entrambi nelle applicazioni, in particolare quelli basato su microservizi e contenitori, nonché in definita dal software Data Center ibridi. Per i prodotti Windows Server in Canale semestrale saranno disponibili nuovi rilasci due volte l'anno, in primavera e in autunno. Ogni versione rilasciata in questo canale verrà supportata per 18 mesi a partire dal rilascio iniziale.

La maggior parte delle funzionalità rilasciate nel Canale semestrale verrà introdotta nel successivo rilascio del Long-Term Servicing Channel di Windows Server. Le edizioni, le funzionalità e il contenuto di supporto possono variare da rilascio a rilascio in base al feedback dei clienti.

È disponibile per i clienti con contratto multilicenza con il canale semestrale [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), nonché tramite Azure Marketplace o altri cloud/host provider di servizi e la fidelizzazione tramite programmi quali come le sottoscrizioni di Visual Studio.

> [!Note]  
> **La versione corrente di canale semestrale è Windows Server, versione 1903**. Se si desidera porre i server in questo canale, è necessario installare Windows Server, versione 1903, che può essere installata in modalità Server Core o Nano Server eseguito in un contenitore. Gli aggiornamenti sul posto da una versione di canale di manutenzione a lungo termine non sono supportati in quanto sono inclusi **i canali di rilascio diversi**. Versioni canale semestrale non sono gli aggiornamenti: l'importante è la prossima versione di Windows Server nel canale semestrale.

In questo modello, le versioni di Windows Server sono identificate dall'anno e dal mese di rilascio: ad esempio, nel 2017 una versione del mese 9 (settembre) viene identificata come **versione 1709**. I nuovi rilasci di Windows Server nel Canale semestrale verranno eseguiti due volte l'anno. Il ciclo di vita del supporto per ogni versione è 18 mesi.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>Devi mantenere i server nel canale LTSC o spostarli nel Canale semestrale?

Ecco le due principali differenze da prendere in considerazione:

- Devi eseguire rapidamente l'innovazione? Hai bisogno di accesso in anteprima alle funzionalità di Windows Server più recenti? Hai bisogno del supporto per infrastrutture Hyper-V, DevOps e applicazioni ibride a rilascio rapido? Se, pertanto, è necessario considerare **che accede al canale semestrale** installando **Windows Server, versione 1903**. Come descritto in questo argomento, riceverai nuove versioni due volte l'anno, con 18 mesi di supporto in produzione Mainstream per ogni versione, da ottenere tramite servizi di sottoscrizione di Visual Studio, Azure o contratti multilicenza. Attualmente, le versioni del Canale semestrale richiedono contratti multilicenza e Software Assurance, se prevedi di eseguire il prodotto nell'ambiente di produzione.
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

- **Windows Update (autonomo)** : Questa opzione è disponibile solo per i server che sono connessi a Internet e con aggiornamento di Windows abilitato.
- **Windows Server Update Services (WSUS)** consente un controllo esteso degli aggiornamenti di Windows 10 e Windows Server ed è disponibile in modalità nativa nel sistema operativo Windows Server. Oltre alla possibilità di rinviare gli aggiornamenti, le organizzazioni possono aggiungere un livello di approvazione per gli aggiornamenti e scegliere di distribuirli in computer o gruppi di computer specifici quando sono pronti.
- **System Center Configuration Manager** offre il massimo controllo sulla manutenzione. I professionisti IT possono rinviare gli aggiornamenti, approvarli e hanno a disposizione più opzioni per decidere la destinazione delle distribuzioni e gestire l'utilizzo della larghezza di banda e i tempi di distribuzione.

Probabilmente si è già scelto di utilizzare almeno una di queste opzioni in base alle risorse, al personale e alle competenze. Lo stesso processo può essere utilizzato per i rilasci del Canale semestrale: ad esempio, se già si utilizza System Center Configuration Manager per gestire gli aggiornamenti, è possibile continuare a usarlo. In modo analogo, se si usa WSUS, è possibile continuare a usarlo.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>Rilascia la posizione in cui ottenere il canale semestrale

Versioni canale semestrale devono essere installate come un'installazione pulita.

- Volume Licensing Service Center (VLSC): I clienti con contratto multilicenza con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) può ottenere questa versione visitando il [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e fare clic su **Accedi**. Quindi possono fare clic su **Download e chiavi** e cercare questa versione. 

- Sono anche disponibili in versioni canale semestrale [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Sottoscrizioni di Visual Studio: Sottoscrittori di Visual Studio è possibile ottenere versioni canale semestrale per scaricarli dal [pagina di download di Visual Studio sottoscrittore](https://my.visualstudio.com/downloads?pid=2347). Se non si è già un sottoscrittore, passare a [Sottoscrizioni di Visual Studio](https://www.visualstudio.com/subscriptions/) per registrarsi e quindi visitare la [pagina di download del Sottoscrittore di Visual Studio](https://my.visualstudio.com/downloads?pid=2347) come indicato in precedenza. Le versioni ottenute tramite le sottoscrizioni di Visual Studio sono esclusivamente a scopo di sviluppo e test.

- Ottenere le versioni di anteprima tramite il programma di Windows Insider: Testare le build preliminari di Windows Server è utile sia a Microsoft sia ai suoi clienti in quanto permette di individuare eventuali problemi prima del rilascio. Offre inoltre ai clienti un'opportunità unica per ottimizzare direttamente le funzionalità del prodotto.   
Microsoft tiene in grande considerazione il feedback ricevuto durante il processo di sviluppo in quanto permette di apportate al più presto le modifiche necessarie. Il test preliminare e il feedback che ne consegue sono essenziali per il modello di rilascio rapido. Per ottenere coinvolti con il programma di Insider di Windows, vedere la [Windows Insider Program per docs Server](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Rilascia l'attivazione di canale semestrale

- Se si usa Microsoft Azure, deve attivare automaticamente questa versione.
- Se è stata ottenuta questa versione sono disponibili il Volume Licensing Service Center o sottoscrizioni di Visual Studio, è possibile attivarla usando il csvlk contenuto 2019 di Windows Server con l'ambiente di sistema di gestione delle chiavi (KMS). Per altre informazioni, vedi [chiavi di configurazione client KMS](../get-started/kmsclientkeys.md).

Versioni canale semestrale che sono state rilasciate prima di Windows Server 2019 usano csvlk contenuto 2016 di Server di Windows.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>Il motivo per cui versioni canale semestrale sono disponibile solo l'opzione di installazione Server Core?

Uno degli approcci più importanti adottati nella pianificazione di ogni versione di Windows Server implica l'ascolto del feedback dei clienti: in che modo viene utilizzato Windows Server? Quali nuove funzionalità avranno l'impatto maggiore sulle distribuzioni di Windows Server e, di conseguenza, sulle attività quotidiane? Il feedback dei clienti indica che la distribuzione di innovazioni nel modo più veloce ed efficiente possibile è una priorità chiave. Allo stesso tempo, per i clienti strada all'innovazione più velocemente, si è indicato che si sta principalmente tramite riga di comando script con PowerShell per gestire i Data Center e di conseguenza non hanno un nome sicuro necessario per l'interfaccia utente grafica desktop disponibile nell'installazione di Windows Server con esperienza Desktop, soprattutto adesso che [Windows Admin Center](../manage/windows-admin-center/overview.md) è disponibile per la gestione remota dei server.

Concentrandosi sull'opzione di installazione Server Core, è possibile dedicare più risorse alle innovazioni, pur mantenendo la compatibilità con le tradizionali applicazioni e funzionalità della piattaforma Windows Server. I commenti e suggerimenti su questo o altri problemi relativi a Windows Server e alle versioni future possono essere inviati tramite l'[Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>Informazioni su Nano Server

Nano Server è disponibile come sistema operativo contenitore nel canale semestrale. Vedere [Modifiche a Nano Server nel Canale semestrale di Windows](../get-started/nano-in-semi-annual-channel.md) per informazioni dettagliate.

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>Come stabilire se un server è in esecuzione una versione LTSC o Configurazione superficie di attacco

In generale, Long-Term Servicing Channel rilascia, ad esempio Windows Server 2019 vengono rilasciati contemporaneamente come nuova versione del canale semestrale, ad esempio, Windows Server, versione 1809. Ciò può rendere un po' difficoltoso determinare se un server è in esecuzione release canale semestrale. Invece di esaminare il numero di build, è necessario esaminare il nome del prodotto: Versioni canale semestrale usare il nome del prodotto "Windows Server Standard" o "Windows Server Datacenter", senza un numero di versione, anche se rilascia Long-Term Servicing Channel includono il numero di versione, ad esempio, "Windows Server Datacenter 2019".

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

## <a name="see-also"></a>Vedere anche

[Modifiche in Nano Server in Windows Server come canale semestrale](../get-started/nano-in-semi-annual-channel.md)

[Ciclo di vita del supporto per Windows Server](https://support.microsoft.com/lifecycle)

[Determinare se è in esecuzione Server Core](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Funzione GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlet di registrazione inventario software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
