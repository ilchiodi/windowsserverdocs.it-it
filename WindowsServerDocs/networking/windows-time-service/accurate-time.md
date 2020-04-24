---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Ora esatta per Windows Server 2016
description: Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, garantendo contemporaneamente all'indietro NTP la compatibilità con le versioni precedenti di Windows.
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 0486033ee52432191cb35f2ce38c44d5b7728a2e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861684"
---
# <a name="accurate-time-for-windows-server-2016"></a>Ora esatta per Windows Server 2016

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versioni successive

Il servizio Ora di Windows è un componente che usa un modello di plug-in per i provider di sincronizzazione dell'ora client e server.  Sono disponibili due provider client incorporati in Windows e tre plug-in di terze parti. Un provider usa [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS NTP](https://msdn.microsoft.com/library/cc246877.aspx) per sincronizzare l'ora di sistema locale su un server di riferimento conforme a NTP e/o MS-NTP. L'altro provider per Hyper-V e sincronizza le macchine virtuali (VM) per l'host Hyper-V.  Quando sono presenti più provider, Windows seleziona il provider migliore usando prima il livello strato, quindi il ritardo della radice, la dispersione della radice e infine l'offset temporale.

> [!NOTE]
> Per una rapida panoramica del servizio Ora di Windows, guarda questo [video di panoramica generale](https://aka.ms/WS2016TimeVideo).

In questo argomento vengono fornite informazioni sull'attivazione dell'ora esatta: 

- Miglioramenti
- Misurazioni
- Procedure consigliate

> [!IMPORTANT]
> Puoi scaricare un supplemento a cui viene fatto riferimento nell'articolo relativo all'ora esatta di Windows 2016 [qui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Questo documento fornisce informazioni più dettagliate sulle metodologie di test e misurazione.

> [!NOTE] 
> È il modello di plug-in di windows time provider [documentata in TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Gerarchia dei domini
Le configurazioni di dominio e autonoma funzionano in modo diverso.

- I membri del dominio utilizzano un protocollo NTP protetto che utilizza l'autenticazione per garantire la sicurezza e l'autenticità del riferimento all'ora.  I membri del dominio sincronizzare con un orologio master determinato dalla gerarchia di domini e un sistema di punteggio.  In un dominio, esiste un livello gerarchico di stratums di tempo, in base al quale ogni controller di dominio punta a un controller di dominio padre con un più accurato strato di tempo.  La gerarchia viene risolto il PDC o un controller di dominio nella foresta radice o un controller di dominio con il flag GTIMESERV di dominio, che indica un Server valido per il dominio.  Vedere la sezione Specificare un servizio locale Ora affidabile tramite GTIMESERV riportata di seguito.

- Computer autonomi sono configurati per utilizzare time.windows.com per impostazione predefinita.  Questo nome viene risolto dal server DNS, che deve fare riferimento a una risorsa di proprietà di Microsoft.  Come tutti i riferimenti di tempo in modalità remota, interruzioni della rete possono impedire la sincronizzazione.  Carica il traffico di rete e i percorsi di rete asimmetrico possono ridurre la precisione della sincronizzazione del tempo.  Per verificare l'accuratezza di 1 ms, non è possibile fare affidamento su origini di ora remote.

Poiché gli utenti guest Hyper-V disporrà di almeno due provider ora di Windows tra cui scegliere l'host ora NTP, è possibile visualizzare diversi comportamenti con dominio o autonomo durante l'esecuzione come guest.

> [!NOTE] 
> Per altre informazioni sulla gerarchia di domini e sul sistema di punteggio, vedi il post di blog ["What is Windows Time Service?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) (Che cos'è il servizio Ora di Windows?) .

> [!NOTE]
> Strato è un concetto utilizzato nel provider di NTP e Hyper-V e il relativo valore indica la posizione di orologi nella gerarchia.  Il livello Strato 1 è riservato all'orologio di livello più elevato, mentre lo strato 0 è riservato all'hardware, presupponendo che ad esso sia associato un ritardo minimo o non sia associato alcun ritardo.  2 strato di comunicare con server strato 1, strato 3 per strato 2 e così via.  Mentre un strato inferiore indica spesso un orologio più accurato, è possibile trovare le discrepanze.  Inoltre, W32time accetta solo ora dalla strato 15 o di sotto.  Per visualizzare lo strato di un client, utilizzare *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fattori critici per l'ora esatta
In ogni caso per ora esatta, esistono tre fattori importanti:

1. **Tinta unita orologio di origine** -l'orologio di origine nelle esigenze di dominio sia stabile e accurato. Questo significa in genere l'installazione di un dispositivo GPS o fa riferimento a un'origine strato 1, tenendo conto #3. L'analogia passa, se si dispone di due evolveranno in acqua e si sta tentando di misurare l'altitudine di uno rispetto a altro, l'accuratezza è consigliabile se la barca di origine è molto stabile e non lo spostamento. Lo stesso vale per l'ora e, se l'orologio di origine non è stabile, l'intera catena di orologi sincronizzati viene interessata e ingrandita in ogni fase. Deve inoltre essere accessibile, poiché eventuali interruzioni di connessione interferirebbero con la sincronizzazione dell'ora. Infine, deve essere protetto. Se il riferimento dell'ora non viene gestito correttamente oppure è gestito da un'entità potenzialmente dannosa, è possibile che il dominio sia esposto ad attacchi basati sul tempo.
2. **Orologio client stabile** -orologi un client stabile assicura che sia containable naturale sfasamento dell'oscillatore di.  Il server NTP utilizza più campioni da potenzialmente più server NTP condizione e disciplina l'orologio del computer locale.  Non esegue il passaggio delle modifiche temporali, bensì rallenta o accelera il clock locale in modo da avvicinarsi rapidamente al tempo esatto e rimanere accurato tra le richieste NTP.  Tuttavia, se l'oscillatore del clock del computer client non è stabile, potrebbero verificarsi più fluttuazioni tra le rettifiche e gli algoritmi usati da Windows per condizionare l'orologio non funzionano in modo accurato.  In alcuni casi, gli aggiornamenti del firmware potrebbero essere necessaria per l'ora esatta.
3. **Comunicazione simmetrico NTP** -è fondamentale che la connessione per la comunicazione NTP è simmetrica.  Il server NTP utilizza i calcoli per regolare il tempo che presuppongono che la patch di rete è simmetrica.  Se il percorso di pacchetto NTP ha sarà il server richiede una quantità diversa di tempo per restituire, l'accuratezza è interessato.  Ad esempio possibile modificare il percorso a causa di modifiche nella topologia di rete o i pacchetti vengono inoltrati tramite i dispositivi dotati di interfaccia diverse velocità.

Per i dispositivi di alimentazione a batteria, sia per dispositivi mobili e portatile, è necessario considerare diverse strategie.  In base al Consiglio, mantenendo l'ora esatta richiede l'orologio per essere disciplinato al secondo, che correla la frequenza di aggiornamento dell'orologio. Queste impostazioni utilizzerà alimentazione della batteria maggiore di quella prevista e può interferire con la modalità di risparmio disponibili in Windows per tali dispositivi. I dispositivi a batteria hanno anche alcune modalità di risparmio energico che arrestano l'esecuzione di tutte le applicazioni, interferendo con la possibilità di W32time di disciplinare l'orologio e mantenere l'ora esatta. Inoltre, gli orologi di dispositivi mobili non siano molto precisi per cominciare.  Un dispositivo mobile può passare da una condizione di ambiente a quello successivo che può interferire con la possibilità di mantenere in modo accurato in fase di condizioni ambientali ambiente influisce sulla precisione di orologio.  Pertanto, è consigliabile non configurare i dispositivi portatili di alimentazione a batteria con le impostazioni di accuratezza. 

## <a name="why-is-time-important"></a>Perché è importante ora?  
Esistono molte ragioni diverse, che potrebbe essere necessario ora esatta.  Il caso tipico per Windows è Kerberos, che richiede 5 minuti di accuratezza tra il client e server.  Tuttavia, esistono molte altre aree che possono essere influenzati dal tempo accuratezza inclusi:


- Normative come:
    - accuratezza 50 ms per FINRA negli Stati Uniti
    - 1 ms ESMA (MiFID II) dell'Unione europea.
- Algoritmi di crittografia
- Sistemi distribuiti come Cluster/SQL/Exchange e di database di documenti
- Blockchain framework per le transazioni bitcoin
- I log distribuiti e l'analisi delle minacce 
- Replica di Active Directory
- PCI (Payment Card settore), attualmente 1 secondo accuratezza



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
