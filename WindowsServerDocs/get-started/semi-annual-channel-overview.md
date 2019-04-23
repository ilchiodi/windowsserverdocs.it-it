---
title: Panoramica di Canale semestrale di Windows Server
description: Microsoft intende semplificare la manutenzione di Windows Server per facilitare testing, gestione e distribuzione degli aggiornamenti del sistema operativo.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/07/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 2995fca3085d6611ecce083685dca0e587913f17
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841242"
---
# <a name="windows-server-semi-annual-channel-overview"></a>Panoramica di Canale semestrale di Windows Server

>Si applica a: Windows Server (Canale semestrale)

Il modello di rilascio di Windows Server offre una nuova possibilità di aggiornamento che ne permette l'allineamento a versioni e modelli di manutenzione simili per [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) e [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Se hai già usato Windows 10 o Office 365 ProPlus, questi miglioramenti potrebbero essere familiari.

**Esistono due canali di rilascio principale disponibile ai clienti di Windows Server, il canale di manutenzione a lungo termine e il canale semestrale.** Puoi mantenere i server sul canale Long-Term Servicing Channel (LTSC), spostarli nel nuovo Canale semestrale o disporre di alcuni server in una delle tracce in base alle tue esigenze.


## <a name="long-term-servicing-channel-ltsc"></a>Long-Term Servicing Channel (LTSC)
Questo modello di rilascio, in precedenza denominato "Long-Term Servicing *Branch*", è già noto agli utenti e offre il rilascio di una nuova versione principale di Windows Server ogni 2 o 3 anni. Gli utenti hanno diritto a cinque anni di supporto mainstream e cinque anni di supporto esteso. Questo canale è appropriato per i sistemi che richiedono tempi di manutenzione più lunghi e una maggiore stabilità funzionale. Le distribuzioni di Windows Server 2016 e delle versioni precedenti di Windows Server non verranno influenzate dai nuovi rilasci di Canale semestrale. Il Long-Term Servicing Channel continuerà a ricevere sia aggiornamenti della sicurezza sia aggiornamenti non relativi alla sicurezza, ma non riceverà le nuove funzioni e funzionalità.

> [!Note]  
> **Il prodotto LTSC corrente è Windows Server 2016**. Se desideri mantenere questo canale, devi installare (o continuare a usare) Windows Server 2016, che può essere installato con l'opzione dei componenti di base del server o con l'opzione Server con Esperienza desktop. Vedi [Introduzione a Windows Server 2016](server-basics.md) per informazioni dettagliate. 



## <a name="semi-annual-channel"></a>Canale semestrale 
Il Canale semestrale è ideale per i clienti che desiderano attuare le innovazioni rapidamente, con l'opportunità di sfruttare le nuove funzionalità del sistema operativo, sia nelle applicazioni, in particolare quelle basate su contenitori e micro servizi, sia in data center ibridi software-defined. Per i prodotti Windows Server in Canale semestrale saranno disponibili nuovi rilasci due volte l'anno, in primavera e in autunno. Ogni versione rilasciata in questo canale verrà supportata per 18 mesi a partire dal rilascio iniziale.

La maggior parte delle funzionalità rilasciate nel Canale semestrale verrà introdotta nel successivo rilascio del Long-Term Servicing Channel di Windows Server. Le edizioni, le funzionalità e il contenuto di supporto possono variare da rilascio a rilascio in base al feedback dei clienti.

Canale semestrale sarà disponibile per i clienti con contratti multilicenza con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), nonché tramite Azure Marketplace o altri provider di servizi cloud/di hosting e programmi fedeltà come Sottoscrizioni di Visual Studio.

> [!Note]  
> **La versione corrente di semi-annuale canale è Windows Server, versione 1803**. Se desideri inserire i server in questo canale, devi installare Windows Server versione 1803, che può essere installato in modalità Server Core o come Nano Server in un contenitore. Vedi [Introduzione a Windows Server, versione 1803](get-started-with-1803.md) per informazioni su come ottenere e attivare Windows Server, versione 1803. Gli aggiornamenti sul posto da Windows Server 2016 a Windows Server, versione 1803 non sono supportati, in quanto sono inclusi in **canali di rilascio differenti**. Windows Server versione 1803 non è un aggiornamento a Windows Server 2016 – è la prossima versione di Windows Server nel canale semi-annuale.



In questo modello, le versioni di Windows Server sono identificate dall'anno e dal mese di rilascio: ad esempio, nel 2017 una versione del mese 9 (settembre) viene identificata come **versione 1709**. I nuovi rilasci di Windows Server nel Canale semestrale verranno eseguiti due volte l'anno. Il ciclo di vita del supporto per ogni versione è 18 mesi.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>Devi mantenere i server nel canale LTSC o spostarli nel Canale semestrale?
Ecco le due principali differenze da prendere in considerazione:

- Devi eseguire rapidamente l'innovazione? Hai bisogno di accesso in anteprima alle funzionalità di Windows Server più recenti? Hai bisogno del supporto per infrastrutture Hyper-V, DevOps e applicazioni ibride a rilascio rapido? In tal caso, è consigliabile considerare di **accedere al Canale semiannuale** installando [Windows Server, versione 1803](get-started-with-1803.md). Come descritto in questo argomento, riceverai nuove versioni due volte l'anno, con 18 mesi di supporto in produzione Mainstream per ogni versione, da ottenere tramite servizi di sottoscrizione di Visual Studio, Azure o contratti multilicenza. Attualmente, le versioni del Canale semestrale richiedono contratti multilicenza e Software Assurance, se prevedi di eseguire il prodotto nell'ambiente di produzione.
- Hai bisogno di stabilità e prevedibilità? Hai bisogno di eseguire le macchine virtuali e i carichi di lavoro tradizionali in server fisici? In tal caso, ti consigliamo di considerare il fatto di **mantenere tali server sul Long-Term Servicing Channel**. La versione LTSC corrente è [Windows Server 2016](server-basics.md). Come descritto in questo argomento, avrai accesso alle nuove versioni ogni 2-3 anni, con 5 anni di supporto mainstream, seguiti da cinque anni di supporto esteso per ogni versione. Le versioni LTSC sono disponibili tramite tutti i meccanismi di rilascio. Le versioni nel canale LTSC sono disponibili per tutti gli utenti indipendentemente dal modello di gestione licenze in uso. 

Nella tabella seguente sono riepilogate le differenze principali tra i canali, a partire da Windows Server, versione 1803:

|  | Long-Term Servicing Channel (Windows Server 2016) |Canale semestrale (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Scenari consigliati | Generici file server, Microsoft e carichi di lavoro non Microsoft, App tradizionale, ruoli dell'infrastruttura, definiti software Datacenter e infrastruttura hyper convergenza | Applicazioni in contenitori, host contenitore e scenari di applicazioni che traggono vantaggio da un'innovazione più rapida |
| Nuove versioni | Ogni 2-3 anni |Ogni 6 mesi |
| Supporto |5 anni di supporto "Mainstream", più 5 anni di supporto "Extended" | 18 mesi |
| Edizioni | Tutte le edizioni di Windows Server disponibili | Edizione standard e Datacenter |
| Chi può utilizzare | Tutti i clienti attraverso tutti i canali | Software Assurance e cloud i clienti |
| Opzioni di installazione | Server Core e Server con esperienza Desktop | Server Core di host contenitore e immagine e Server aspetti contenitore immagine |                |


## <a name="device-compatibility"></a>Compatibilità dei dispositivi
Se non diversamente comunicato, i requisiti hardware minimi per eseguire i rilasci di Canale semestrale corrisponderanno a quelli del rilascio più recente di Windows Server del Long-Term Servicing Channel. Ad esempio, il **corrente rilascio Long-Term Servicing Channel è Windows Server 2016**. La maggior parte dei driver hardware continuerà a funzionare con questi rilasci.

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
# <a name="related-topics"></a>Argomenti correlati
[Windows Server. 2019 canali di manutenzione: Configurazione superficie di attacco e LTSC](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19)

[Modifiche in Nano Server in Windows Server come canale semestrale](nano-in-semi-annual-channel.md)

[Ciclo di vita del supporto per Windows Server](https://support.microsoft.com/en-us/lifecycle)

[Requisiti di sistema di Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/system-requirements) 




