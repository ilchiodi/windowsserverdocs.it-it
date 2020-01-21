---
title: Canali di manutenzione
description: 'Spiegazione dei canali di manutenzione di Windows Server: LTSC e Canale semestrale'
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 3d443ff123cc041196f59d93d156415c34bdf70f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947875"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Canali di manutenzione di Windows Server: LTSC e Canale semestrale

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

I clienti di Windows Server hanno a disposizione due canali di rilascio principali: Long-Term Servicing Channel (LTSC) e il nuovo Canale semestrale o SAC.

Puoi mantenere i server sul canale Long-Term Servicing Channel (LTSC), spostarli nel nuovo Canale semestrale o distribuire i server tra i due canali, in base alle tue esigenze.

## <a name="long-term-servicing-channel-ltsc"></a>Long-Term Servicing Channel (LTSC)

Questo modello di rilascio già noto, in precedenza denominato "Long-Term Servicing *Branch*", prevede il rilascio di una nuova versione principale di Windows Server ogni 2 o 3 anni. Gli utenti hanno diritto a 5 anni di supporto Mainstream e 5 anni di supporto Extended. Questo canale è appropriato per i sistemi che richiedono tempi di manutenzione più lunghi e una maggiore stabilità funzionale. Le distribuzioni di Windows Server 2016 e delle versioni precedenti di Windows Server non verranno influenzate dalle nuove versioni del Canale semestrale. Il Long-Term Servicing Channel continuerà a ricevere sia aggiornamenti della sicurezza sia aggiornamenti non relativi alla sicurezza, ma non riceverà le nuove funzioni e funzionalità.

> [!Note]  
> **Il prodotto LTSC corrente è Windows Server 2019**. Se vuoi restare in questo canale, ti consigliamo di installare (o continuare a usare) Windows Server 2019, che può essere installato con l'opzione di installazione dei componenti di base del server o l'opzione di installazione Server con Esperienza desktop.

## <a name="semi-annual-channel"></a>Canale semestrale

Il Canale semestrale è ideale per i clienti che desiderano introdurre le innovazioni rapidamente, con l'opportunità di sfruttare le nuove funzionalità del sistema operativo, sia nelle applicazioni, in particolare quelle basate su contenitori e micro servizi, sia in data center ibridi software-defined. Per i prodotti Windows Server nel Canale semestrale saranno disponibili nuove versioni due volte l'anno, in primavera e in autunno. Ogni versione in questo canale verrà supportata per 18 mesi a partire dal rilascio iniziale.

La maggior parte delle funzionalità introdotte nel Canale semestrale verrà inclusa nella versione successiva del Long-Term Servicing Channel di Windows Server. Le edizioni, le funzionalità e il contenuto di supporto possono variare da una versione all'altra in base al feedback dei clienti.

Il Canale semestrale sarà disponibile per i clienti con contratti multilicenza con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx), nonché tramite Azure Marketplace o altri provider di servizi cloud/di hosting e programmi fedeltà come Sottoscrizioni di Visual Studio.

> [!Note]  
> **La versione corrente del Canale semestrale è Windows Server versione 1903**. Se vuoi inserire i server in questo canale, devi installare Windows Server versione 1903, che può essere installato in modalità componenti di base del server o come Nano Server in un contenitore. Gli aggiornamenti sul posto da una versione Long-Term Servicing Channel non sono supportati in quanto sono inclusi in **canali di rilascio diversi**. Le versioni del Canale semestrale non sono aggiornamenti, ma corrispondono alla versione successiva di Windows Server.

In questo modello, le versioni di Windows Server sono identificate dall'anno e dal mese di rilascio: ad esempio, nel 2017 una versione del mese 9 (settembre) viene identificata come **versione 1709**. I nuovi rilasci di Windows Server nel Canale semestrale verranno eseguiti due volte l'anno. Il ciclo di vita del supporto per ogni versione è 18 mesi.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>Devi mantenere i server nel canale LTSC o spostarli nel Canale semestrale?

Ecco le principali differenze da prendere in considerazione:

- Devi adottare le novità rapidamente? Hai bisogno di accesso in anteprima alle funzionalità di Windows Server più recenti? Hai bisogno del supporto per infrastrutture Hyper-V, DevOps e applicazioni ibride a rilascio rapido? In tal caso, dovresti valutare la possibilità di **accedere al Canale semestrale** installando **Windows Server versione 1903**. Come descritto in questo argomento, riceverai nuove versioni due volte l'anno, con 18 mesi di supporto in produzione Mainstream per ogni versione, da ottenere tramite servizi di sottoscrizione di Visual Studio, Azure o contratti multilicenza. Attualmente, le versioni del Canale semestrale richiedono contratti multilicenza e Software Assurance, se prevedi di eseguire il prodotto in ambiente di produzione.
- Hai bisogno di stabilità e prevedibilità? Hai bisogno di eseguire le macchine virtuali e i carichi di lavoro tradizionali in server fisici? In tal caso, ti consigliamo di prendere in considerazione la possibilità di **mantenere tali server nel Long-Term Servicing Channel**. La versione LTSC corrente è **Windows Server 2019**. Come descritto in questo argomento, avrai accesso alle nuove versioni ogni 2-3 anni, con 5 anni di supporto Mainstream, seguiti da 5 anni di supporto Extended per ogni versione. Le versioni LTSC sono disponibili tramite tutti i meccanismi di rilascio. Le versioni nel canale LTSC sono disponibili per tutti gli utenti indipendentemente dal modello di gestione licenze in uso. 

Nella tabella seguente sono riepilogate le differenze principali tra i canali:


|                       |                                                              Long-Term Servicing Channel (Windows Server 2019)                                                               |                                   Canale semestrale (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Scenari consigliati | File server per utilizzo generico, carichi di lavoro Microsoft e non Microsoft, app tradizionali, ruoli dell'infrastruttura, data center software-defined e infrastruttura iperconvergente | Applicazioni in contenitori, host contenitore e scenari di applicazioni che traggono vantaggio dall'introduzione più rapida delle innovazioni |
|     Nuove versioni      |                                                                               Ogni 2-3 anni                                                                                |                                              Ogni 6 mesi                                              |
|        Supporto        |                                                       5 anni di supporto Mainstream, più 5 anni di supporto Extended                                                        |                                                18 mesi                                                 |
|       Edizioni        |                                                                    Tutte le edizioni di Windows Server disponibili                                                                     |                                     Edizioni Standard e Datacenter                                     |
|      Destinatari      |                                                                      Tutti i clienti attraverso tutti i canali                                                                      |                               Solo clienti Software Assurance e cloud                                |
| Opzioni di installazione  |                                                                Componenti di base del server e Server con Esperienza desktop                                                                |                 Componenti di base del server per host e immagine del contenitore e immagine del contenitore Nano Server                 |

## <a name="device-compatibility"></a>Compatibilità dei dispositivi

Se non diversamente comunicato, i requisiti hardware minimi per eseguire le versioni del Canale semestrale corrisponderanno a quelli della versione più recente di Windows Server del Long-Term Servicing Channel. Ad esempio, la **versione corrente del Long-Term Servicing Channel è Windows Server 2019**. La maggior parte dei driver hardware continuerà a funzionare in queste versioni.

## <a name="servicing"></a>Manutenzione

Sia le versioni del Long-Term Servicing Channel sia quelle del Canale semestrale verranno supportate da aggiornamenti della sicurezza e aggiornamenti non relativi alla sicurezza. La differenza consiste nella durata del supporto della versione, come descritto in precedenza.

### <a name="servicing-tools"></a>Strumenti di manutenzione

Esistono molti strumenti con cui i professionisti IT possono eseguire la manutenzione di Windows Server. Ogni opzione presenta vantaggi e svantaggi, sia a livello di funzionalità e controllo sia a livello di semplicità e requisiti di amministrazione non elevati. Ecco alcuni esempi di strumenti di manutenzione disponibili per gestire gli aggiornamenti di manutenzione:

- **Windows Update (autonomo)** : questa opzione è disponibile solo per i server connessi a Internet e in cui è abilitato Windows Update.
- **Windows Server Update Services (WSUS)** consente un controllo esteso degli aggiornamenti di Windows 10 e Windows Server ed è disponibile in modalità nativa nel sistema operativo Windows Server. Oltre alla possibilità di rinviare gli aggiornamenti, le organizzazioni possono aggiungere un livello di approvazione per gli aggiornamenti e scegliere di distribuirli in computer o gruppi di computer specifici quando sono pronti.
- **System Center Configuration Manager** offre il massimo controllo sulla manutenzione. I professionisti IT possono rinviare gli aggiornamenti, approvarli e hanno a disposizione più opzioni per decidere la destinazione delle distribuzioni e gestire l'utilizzo della larghezza di banda e i tempi di distribuzione.

Probabilmente hai già scelto di usare almeno una di queste opzioni in base alle risorse, al personale e alle competenze. Puoi continuare a usare lo stesso processo per le versioni del Canale semestrale: ad esempio, se usi già System Center Configuration Manager per gestire gli aggiornamenti, puoi continuare a farlo. In modo analogo, se usi WSUS, puoi continuare a usarlo.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>Dove ottenere le versioni del Canale semestrale

Le versioni del Canale semestrale devono essere installate con un'installazione pulita.

- Volume Licensing Service Center (VLSC): i clienti che dispongono di contratti multilicenza con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) possono ottenere questa versione da [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e facendo clic su **Sign In** (Accedi). Possono quindi fare clic su **Downloads and Keys** (Download e chiavi) e cercare questa versione. 

- Le versioni del Canale semestrale sono disponibili anche in [Microsoft Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Sottoscrizioni di Visual Studio: i sottoscrittori di Visual Studio possono ottenere le versioni del canale semestrale scaricandole dalla [pagina di download per i sottoscrittori di Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Se non sei già un sottoscrittore, passa a [Sottoscrizioni di Visual Studio](https://www.visualstudio.com/subscriptions/) per registrarti e quindi visita la [pagina di download per i sottoscrittori di Visual Studio](https://my.visualstudio.com/downloads?pid=2347) come indicato in precedenza. Le versioni ottenute tramite le sottoscrizioni di Visual Studio sono esclusivamente a scopo di sviluppo e test.

- Ottenere le versioni di anteprima tramite il Programma Windows Insider: testare le build preliminari di Windows Server è utile sia a Microsoft sia ai suoi clienti in quanto permette di individuare eventuali problemi prima del rilascio. Offre anche ai clienti un'opportunità unica per influire direttamente sulle funzionalità del prodotto.   
Microsoft tiene in grande considerazione il feedback ricevuto durante il processo di sviluppo in quanto permette di apportare al più presto le modifiche necessarie. Il test preliminare e il feedback che ne consegue sono essenziali per il modello di rilascio rapido. Per altre informazioni su come partecipare al Programma Windows Insider, vedi la [documentazione del Programma Windows Insider per i server](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Attivazione delle versioni del Canale semestrale

- Se usi Microsoft Azure questa versione viene automaticamente attivata.
- Se hai ottenuto questa versione dal Volume Licensing Service Center o tramite le sottoscrizioni di Visual Studio, puoi attivarla tramite Windows Server 2019 CSVLK con l'ambiente del sistema di gestione delle chiavi (KMS). Per altre informazioni, vedi [Chiavi di configurazione di client del Servizio di gestione delle chiavi](../get-started/kmsclientkeys.md).

Le versioni del Canale semestrale rilasciate prima di Windows Server 2019 usano Windows Server 2016 CSVLK.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>Perché le versioni del Canale semestrale offrono solo l'opzione di installazione dei componenti di base del server?

Uno degli approcci più importanti adottati per la pianificazione di ogni versione di Windows Server implica l'ascolto del feedback dei clienti: in che modo viene usato Windows Server? Quali nuove funzionalità avranno l'impatto maggiore sulle distribuzioni di Windows Server e, di conseguenza, sulle attività quotidiane? Il feedback dei clienti indica che la distribuzione di innovazioni nel modo più veloce ed efficiente possibile è una priorità chiave. Allo stesso tempo, i clienti che introducono più rapidamente le innovazioni indicano di usare principalmente script dalla riga di comando con PowerShell per gestire i data center e di conseguenza non hanno grande necessità di avere a disposizione un'interfaccia utente grafica desktop nell'installazione di Windows Server con Esperienza desktop, in particolare ora che è disponibile [Windows Admin Center](../manage/windows-admin-center/overview.md) per la gestione remota dei server.

Concentrandosi sull'opzione di installazione Server Core, è possibile dedicare più risorse alle innovazioni, pur mantenendo la compatibilità con le applicazioni e funzionalità tradizionali della piattaforma Windows Server. I commenti e suggerimenti su questo o altri problemi relativi a Windows Server e alle versioni future possono essere inviati tramite l'[Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>Informazioni su Nano Server

Nano server è disponibile come sistema operativo contenitore nel Canale semestrale. Vedi [Modifiche a Nano Server nel Canale semestrale di Windows](../get-started/nano-in-semi-annual-channel.md) per informazioni dettagliate.

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>Come stabilire se un server esegue una versione LTSC o del Canale semestrale

In generale, le versioni del Long-Term Servicing Channel come Windows Server 2019 vengono rilasciate in contemporanea a una nuova versione del Canale semestrale, ad esempio, Windows Server versione 1809. Può quindi essere un po' difficoltoso determinare se un server esegue la versione del Canale semestrale. Invece di controllare il numero di build, dovrai verificare il nome del prodotto: Le versioni del Canale semestrale usano il nome di prodotto "Windows Server Standard" o "Windows Server Datacenter", senza un numero di versione, mentre le versioni del Long-Term Servicing Channel includono il numero di versione, ad esempio "Windows Server 2019 Datacenter".

>[!Note]  
> Le indicazioni seguenti sono progettate per identificare e differenziare LTSC e il Canale semestrale solo a scopi di inventario e del ciclo di vita generali.  Non sono ideate per la compatibilità delle applicazioni o per rappresentare una superficie API specifica.  Gli sviluppatori di app devono usare le linee guida pubblicate altrove per garantire la compatibilità di componenti, API e funzionalità, che possono essere aggiunti durante il ciclo di vita di un sistema o non ancora aggiunti. [Versione del sistema operativo](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) è un punto di partenza migliore per gli sviluppatori di app.

Apri Powershell e usa il cmdlet Get-ItemProperty o Get-ComputerInfo per controllare queste proprietà nel Registro di sistema.  Insieme al numero di build troverai indicazione del canale (LTSC o Canale semestrale) in base alla presenza o assenza dell'anno, ad esempio 2019.  Il canale LTSC lo include, diversamente dal Canale semestrale.  Il risultato indicherà anche la tempistica di rilascio tramite ReleaseId o WindowsVersion, ad esempio 1809, e indicherà se si tratta di un'installazione dei componenti di base del server o Server con Esperienza desktop. 

**Esempio di Windows Server 2019 Datacenter Edition (LTSC) con Esperienza desktop:**

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

**Esempio di Windows Server versione 1809 (SAC) Standard Edition con componenti di base del server:**

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

**Esempio di Windows Server 2019 Standard Edition (LTSC) con componenti di base del server:**


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

Per eseguire una query e scoprire se la nuova [funzionalità su richiesta di compatibilità delle app per i componenti di base del server](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) è presente in un server, usa il cmdlet [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) e cerca:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>Vedere anche

[Modifiche apportate a Nano Server nel Canale semestrale di Windows Server](../get-started/nano-in-semi-annual-channel.md)

[Ciclo di vita del supporto per Windows Server](https://support.microsoft.com/lifecycle)

[Determinare se è in esecuzione l'installazione dei componenti di base del server](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Funzione GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlet di Registrazione inventario software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
