---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Ora esatta per Windows Server 2016
description: Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, garantendo contemporaneamente all'indietro NTP la compatibilità con le versioni precedenti di Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 1399ed6a50085baa37f06c09b8c3e18ca8bca98b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395711"
---
# <a name="accurate-time-for-windows-server-2016"></a>Ora esatta per Windows Server 2016

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

Il servizio ora di Windows è un componente che utilizza un modello di plug-in per i provider di sincronizzazione dell'ora di client e server.  Sono disponibili due provider client predefiniti in Windows e sono disponibili plug-in di terze parti. Un provider USA [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) per sincronizzare l'ora di sistema locale con un server di riferimento compatibile con NTP e/o MS-NTP. L'altro provider per Hyper-V e sincronizza le macchine virtuali (VM) per l'host Hyper-V.  Quando esistono più provider, Windows sceglierà il provider migliore usando prima di tutto il livello di strato, seguito da ritardo radice, dispersione radice e offset ora finale.

> [!NOTE]
> Per una rapida panoramica del servizio ora di Windows, vedere questo [video introduttivo di alto livello](https://aka.ms/WS2016TimeVideo).

In questo argomento verrà illustrato... questi argomenti sono correlati all'abilitazione dell'ora esatta: 

- Miglioramenti
- Misurazioni
- Procedure consigliate

> [!IMPORTANT]
> [Qui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)è possibile scaricare un addendum a cui fa riferimento l'articolo relativo all'ora esatta di Windows 2016.  Questo documento fornisce ulteriori informazioni sulle metodologie di test e misurazione.

> [!NOTE] 
> È il modello di plug-in di windows time provider [documentata in TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Gerarchia di domini
Le configurazioni di dominio e autonoma funzionano in modo diverso.

- I membri del dominio utilizzano un protocollo NTP protetto che utilizza l'autenticazione per garantire la sicurezza e l'autenticità del riferimento all'ora.  I membri del dominio sincronizzare con un orologio master determinato dalla gerarchia di domini e un sistema di punteggio.  In un dominio, esiste un livello gerarchico di stratums di tempo, in base al quale ogni controller di dominio punta a un controller di dominio padre con un più accurato strato di tempo.  La gerarchia viene risolto il PDC o un controller di dominio nella foresta radice o un controller di dominio con il flag GTIMESERV di dominio, che indica un Server valido per il dominio.  Vedere il [specificare un locale affidabile ora servizio utilizzando GTIMESERV](#GTIMESERV) sezione riportata di seguito.

- Computer autonomi sono configurati per utilizzare time.windows.com per impostazione predefinita.  Questo nome viene risolto dal server DNS, che dovrebbe puntare a una risorsa di proprietà di Microsoft.  Come tutti i riferimenti di tempo in modalità remota, interruzioni della rete possono impedire la sincronizzazione.  Carica il traffico di rete e i percorsi di rete asimmetrico possono ridurre la precisione della sincronizzazione del tempo.  Per un'accuratezza di 1 MS, non è possibile dipendere da origini temporali remote.

Poiché gli utenti guest Hyper-V disporrà di almeno due provider ora di Windows tra cui scegliere l'host ora NTP, è possibile visualizzare diversi comportamenti con dominio o autonomo durante l'esecuzione come guest.

> [!NOTE] 
> Per ulteriori informazioni sulla gerarchia del dominio e sul sistema di assegnazione dei punteggi, vedere "informazioni sul [servizio ora di Windows".](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Post di Blog.

> [!NOTE]
> Strato è un concetto utilizzato nel provider di NTP e Hyper-V e il relativo valore indica la posizione di orologi nella gerarchia.  Lo strato 1 è riservato per l'orologio di livello più alto, mentre lo strato 0 è riservato per l'hardware presumendo che sia accurato e a cui non è associato un ritardo.  2 strato di comunicare con server strato 1, strato 3 per strato 2 e così via.  Mentre un strato inferiore indica spesso un orologio più accurato, è possibile trovare le discrepanze.  Inoltre, W32time accetta solo ora dalla strato 15 o di sotto.  Per visualizzare lo strato di un client, utilizzare *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fattori critici per l'ora esatta
In ogni caso per ora esatta, esistono tre fattori importanti:

1. **Tinta unita orologio di origine** -l'orologio di origine nelle esigenze di dominio sia stabile e accurato. Questo significa in genere l'installazione di un dispositivo GPS o fa riferimento a un'origine strato 1, tenendo conto #3. L'analogia passa, se si dispone di due evolveranno in acqua e si sta tentando di misurare l'altitudine di uno rispetto a altro, l'accuratezza è consigliabile se la barca di origine è molto stabile e non lo spostamento. Lo stesso vale per il tempo e se l'orologio di origine non è stabile, l'intera catena di orologi sincronizzati viene interessata e ingrandita in ogni fase. Inoltre, deve essere accessibile perché le rotture nella connessione interferiscono con la sincronizzazione dell'ora. Infine, deve essere protetto. Se il riferimento all'ora non viene mantenuto correttamente o gestito da una parte potenzialmente dannosa, è possibile esporre il dominio a attacchi basati sul tempo.
2. **Orologio client stabile** -orologi un client stabile assicura che sia containable naturale sfasamento dell'oscillatore di.  Il server NTP utilizza più campioni da potenzialmente più server NTP condizione e disciplina l'orologio del computer locale.  Non esegue il passaggio delle modifiche temporali, bensì rallenta o accelera il clock locale in modo da avvicinarsi rapidamente al tempo esatto e rimanere accurato tra le richieste NTP.  Tuttavia, se l'oscillatore del clock del computer client non è stabile, potrebbero verificarsi più fluttuazioni tra le rettifiche e gli algoritmi usati da Windows per condizionare l'orologio non funzionano in modo accurato.  In alcuni casi, gli aggiornamenti del firmware potrebbero essere necessaria per l'ora esatta.
3. **Comunicazione simmetrico NTP** -è fondamentale che la connessione per la comunicazione NTP è simmetrica.  Il server NTP utilizza i calcoli per regolare il tempo che presuppongono che la patch di rete è simmetrica.  Se il percorso di pacchetto NTP ha sarà il server richiede una quantità diversa di tempo per restituire, l'accuratezza è interessato.  Ad esempio possibile modificare il percorso a causa di modifiche nella topologia di rete o i pacchetti vengono inoltrati tramite i dispositivi dotati di interfaccia diverse velocità.

Per i dispositivi di alimentazione a batteria, sia per dispositivi mobili e portatile, è necessario considerare diverse strategie.  In base al Consiglio, mantenendo l'ora esatta richiede l'orologio per essere disciplinato al secondo, che correla la frequenza di aggiornamento dell'orologio. Queste impostazioni utilizzerà alimentazione della batteria maggiore di quella prevista e può interferire con la modalità di risparmio disponibili in Windows per tali dispositivi. I dispositivi a batteria hanno anche determinate modalità di risparmio energia che arrestano l'esecuzione di tutte le applicazioni, interferisce con la possibilità di W32time's di disciplinare l'orologio e di mantenere l'ora esatta. Inoltre, gli orologi di dispositivi mobili non siano molto precisi per cominciare.  Un dispositivo mobile può passare da una condizione di ambiente a quello successivo che può interferire con la possibilità di mantenere in modo accurato in fase di condizioni ambientali ambiente influisce sulla precisione di orologio.  Pertanto, è consigliabile non configurare i dispositivi portatili di alimentazione a batteria con le impostazioni di accuratezza. 

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
