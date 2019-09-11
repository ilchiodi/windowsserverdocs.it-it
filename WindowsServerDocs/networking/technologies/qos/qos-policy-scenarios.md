---
title: Scenari di criteri QoS
description: Questo argomento fornisce scenari di criteri di qualità del servizio (QoS), che illustrano come usare Criteri di gruppo per definire la priorità del traffico di rete di applicazioni e servizi specifici in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0968157532c0b3bd926acbaff4291e27a71de31
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871870"
---
# <a name="qos-policy-scenarios"></a>Scenari di criteri QoS

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per esaminare scenari ipotetici che dimostrano come, quando e perché utilizzare i criteri QoS.

I due scenari in questo argomento sono i seguenti:

1. Definire la priorità del traffico di rete per un'applicazione line-of-business
2. Definire la priorità del traffico di rete per un'applicazione server HTTP

>[!NOTE]
>Alcune sezioni di questo argomento contengono i passaggi generali che è possibile eseguire per eseguire le azioni descritte. Per istruzioni più dettagliate sulla gestione dei criteri QoS, vedere [gestire i criteri QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scenario 1: Definire la priorità del traffico di rete per un'applicazione line-of-business

In questo scenario, un reparto IT presenta diversi obiettivi che è possibile eseguire usando i criteri QoS:

- Fornire migliori prestazioni di rete per\-le applicazioni mission-critical.
- Fornire migliori prestazioni di rete per un set di chiavi di utenti durante l'uso di un'applicazione specifica.
- Assicurarsi che l'applicazione\-di backup dei dati a livello aziendale non impedisca le prestazioni di rete con troppa larghezza di banda alla volta.

Il reparto IT decide di configurare i criteri QoS per assegnare la priorità alle applicazioni specifiche usando i valori \(DSCP\) del servizio di differenziazione per classificare il traffico di rete e configurare i relativi router per fornire i privilegi trattamento per il traffico con priorità più elevata. 

>[!NOTE]
>Per ulteriori informazioni su DSCP, vedere la sezione **definire la priorità QoS tramite un punto di codice servizi differenziati** nell'argomento [criteri QoS (Quality of Service)](qos-policy-top.md).

Oltre ai valori DSCP, i criteri QoS possono specificare una velocità di limitazione. La limitazione ha l'effetto di limitare tutto il traffico in uscita che corrisponde ai criteri QoS a una velocità di invio specifica.

### <a name="qos-policy-configuration"></a>Configurazione dei criteri QoS

Con tre obiettivi distinti da completare, l'amministratore IT decide di creare tre diversi criteri QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Criteri QoS per i server app LOB

La prima applicazione\-mission-critical per la quale il reparto IT crea un criterio QoS è\-un'applicazione ERP\) aziendale \(per la pianificazione delle risorse aziendali. L'applicazione ERP è ospitata in più computer che eseguono Windows Server 2016. In Active Directory Domain Services, questi computer sono membri di un'unità organizzativa \(unità\) organizzativa creata per i server applicazioni line-of \(-\) business LOB. Il componente\-lato client per l'applicazione ERP è installato nei computer che eseguono Windows 10 e Windows 8.1.

In criteri di gruppo, un amministratore it Seleziona l'oggetto criteri \(\) di gruppo dell'oggetto Criteri di gruppo su cui verranno applicati i criteri QoS. Utilizzando la creazione guidata criteri QoS, l'amministratore IT crea un criterio QoS denominato "criterio LOB server" che specifica un valore\-DSCP con priorità alta 44 per tutte le applicazioni, qualsiasi indirizzo IP, TCP e UDP e numero di porta.

Il criterio QoS viene applicato solo ai server LOB collegando l'oggetto Criteri di gruppo all'unità organizzativa che contiene solo questi server, tramite lo \(strumento\) di gestione criteri di gruppo console Gestione criteri di gruppo. Questo criterio LOB del server iniziale applica il\-valore DSCP con priorità alta ogni volta che il computer invia il traffico di rete. Questo criterio QoS può essere modificato \(in un secondo momento nello strumento\) Editor oggetti Criteri di gruppo per includere i numeri di porta dell'applicazione ERP, che limitano l'applicazione dei criteri solo quando viene usato il numero di porta specificato.

#### <a name="qos-policy-for-the-finance-group"></a>Criteri QoS per il gruppo Finance

Sebbene molti gruppi all'interno dell'azienda accedano all'applicazione ERP, il gruppo Finance dipende da questa applicazione quando si gestiscono i clienti e il gruppo richiede prestazioni costantemente elevate dall'app.

Per garantire che il gruppo Finance possa supportare i clienti, i criteri QoS devono classificare il traffico di questi utenti come priorità alta. Tuttavia, i criteri non devono essere applicati quando i membri del gruppo Finance usano applicazioni diverse dall'applicazione ERP. 

Per questo motivo, il reparto IT definisce un secondo criterio QoS denominato "criterio LOB client" nello strumento Editor oggetti Criteri di gruppo che applica il valore DSCP 60 quando il gruppo di utenti Finance esegue l'applicazione ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Criteri QoS per un'app di backup

In tutti i computer è in esecuzione un'applicazione di backup separata. Per assicurarsi che il traffico dell'applicazione di backup non usi tutte le risorse di rete disponibili, il reparto IT crea un criterio di backup dei dati. Questo criterio di backup specifica il valore DSCP 1 in base al nome dell'eseguibile per l'app di backup, ovvero **backup. exe**. 

Viene creato e distribuito un terzo oggetto Criteri di gruppo per tutti i computer client nel dominio. Ogni volta che l'applicazione di backup invia dati, viene applicato il valore DSCP con priorità bassa, anche se ha origine dai computer del reparto finanziario.
  
>[!NOTE]
>Il traffico di rete senza un criterio QoS invia con un valore DSCP pari a 0.

### <a name="scenario-policies"></a>Criteri di scenario

Nella tabella seguente sono riepilogati i criteri QoS per questo scenario.
  
|Nome criterio|Valore DSCP|Velocità di limitazione|Applicato alle unità organizzative|Descrizione|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nessun criterio]|0|Nessuna|[Nessuna distribuzione]|Trattamento ottimale (impostazione predefinita) per il traffico non classificato.|  
|Dati di backup|1|Nessuna|Tutti i client|Applica un valore DSCP con priorità bassa per questi dati in blocco.|  
|LOB server|44|Nessuna|Unità organizzativa del computer per i server ERP|Applica il DSCP con priorità alta per il traffico del server ERP|  
|LOB client|60|Nessuna|Gruppo di utenti Finance|Applica il DSCP con priorità alta per il traffico client ERP|  

>[!NOTE]
>I valori DSCP sono rappresentati in formato decimale.

Con i criteri QoS definiti e applicati tramite Criteri di gruppo, il traffico di rete in uscita riceve il valore DSCP specificato dal criterio. I router forniscono quindi un trattamento differenziale basato su questi valori DSCP usando l'accodamento. Per questo reparto IT, i router sono configurati con quattro code: priorità alta, priorità intermedia, massimo sforzo e priorità bassa.

Quando il traffico arriva al router con valori DSCP da "criteri LOB server" e "criterio LOB client", i dati vengono inseriti in code con priorità alta. Il traffico con un valore DSCP pari a 0 riceve un livello di servizio ottimale. I pacchetti con un valore DSCP pari a 1 (dall'applicazione di backup) ricevono un trattamento con priorità bassa.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Prerequisiti per la definizione delle priorità di un'applicazione line-of-business

Per completare questa attività, verificare che siano soddisfatti i requisiti seguenti:

- I computer interessati eseguono sistemi operativi\-compatibili con QoS.

- I computer interessati sono membri di un dominio \(di servizi\) di dominio Active Directory Active Directory Domain Services in modo che possano essere configurati utilizzando criteri di gruppo.

- Le reti TCP/IP sono configurate con i router configurati\)per DSCP \(RFC 2474. Per ulteriori informazioni, vedere [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Sono soddisfatti i requisiti di credenziali amministrative.

#### <a name="administrative-credentials"></a>Credenziali amministrative

Per completare questa attività, è necessario essere in grado di creare e distribuire Criteri di gruppo oggetti.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurazione dell'ambiente di test per la definizione delle priorità di un'applicazione line-of-business

Per configurare l'ambiente di test, completare le seguenti attività.

- Creare un dominio di servizi di dominio Active Directory con client e utenti raggruppati in unità organizzative. Per istruzioni sulla distribuzione di servizi di dominio Active Directory, vedere la [Guida alla rete core](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurare i router per la coda differenziale in base ai valori DSCP. Ad esempio, il valore DSCP 44 immette una coda "Platinum" e tutte le altre sono ponderate-Fair-queued.

>[!NOTE]
> È possibile visualizzare i valori DSCP usando le acquisizioni di rete con strumenti come Network Monitor. Dopo aver eseguito un'acquisizione di rete, è possibile osservare il campo TOS nei dati acquisiti.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Passaggi per la definizione delle priorità di un'applicazione line-of-business

Per definire la priorità di un'applicazione line-of-business, completare le attività seguenti:

1. Creare e collegare un oggetto criteri \(\) di gruppo criteri di gruppo oggetto con criteri QoS.

2. Configurare i router in modo da trattare in modo differenziale un'applicazione line-of-business (usando l'accodamento) in base ai valori DSCP selezionati. Le procedure di questa attività variano a seconda del tipo di router disponibili.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scenario 2: Definire la priorità del traffico di rete per un'applicazione server HTTP

In Windows Server 2016, QoS basata su criteri include i criteri basati su URL funzionalità. I criteri URL consentono di gestire la larghezza di banda per i server HTTP.

Molte applicazioni aziendali sono sviluppate e ospitate in \(Internet Information Services\) server Web IIS e le app Web sono accessibili dai browser nei computer client.

In questo scenario, si supponga di gestire un set di server IIS che ospitano i video di formazione per tutti i dipendenti dell'organizzazione. L'obiettivo è garantire che il traffico proveniente da questi server video non sovraccarica la rete e che il traffico video sia differenziato dal traffico voce e dati sulla rete. 

L'attività è simile all'attività nello scenario 1. Verranno progettate e configurate le impostazioni di gestione del traffico, ad esempio il valore DSCP per il traffico video e la frequenza di limitazione delle richieste per le applicazioni line-of-business. Tuttavia, quando si specifica il traffico, anziché fornire il nome dell'applicazione, si immette solo l'URL a cui l'applicazione server HTTP risponderà: ad https://hrweb/training esempio,.
  
> [!NOTE]
>Non è possibile usare i criteri QoS basati su URL per definire la priorità del traffico di rete per i computer che eseguono sistemi operativi Windows che sono stati rilasciati prima di Windows 7 e Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regole di precedenza per i criteri basati su URL

Tutti gli URL seguenti sono validi e possono essere specificati nei criteri QoS e applicati contemporaneamente a un computer o a un utente:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

Ma quale riceverà la precedenza? Le regole sono semplici. I criteri basati su URL hanno priorità in ordine di lettura da sinistra a destra. Quindi, dalla priorità più alta alla priorità più bassa, i campi URL sono:
  
[1. Schema URL](#bkmk_QoS_UrlScheme)

[2. Host URL](#bkmk_QoS_UrlHost)

[3. Porta URL](#bkmk_QoS_UrlPort)

[4. Percorso URL](#bkmk_QoS_UrlPath)

I dettagli sono i seguenti:

####  <a name="bkmk_QoS_UrlScheme"></a>1. Schema URL

 `https://`ha una priorità più alta `https://`di.

####  <a name="bkmk_QoS_UrlHost"></a>2. Host URL

 Dalla priorità più alta alla più bassa, sono:

1. Hostname

2. Indirizzo IPv6

3. Indirizzo IPv4

4. Carattere jolly

Nel caso di hostname, un nome host con più elementi punteggiati (maggiore profondità) ha una priorità più alta rispetto a un nome host con meno elementi tratteggiati. Ad esempio, tra i nomi host seguenti:

- video.internal.training.hr.mycompany.com (profondità = 6)
  
- selfguide.training.mycompany.com (Depth = 4)
  
- Training (profondità = 1)
  
- Library (depth = 1)
  
  **video.Internal.Training.hr.mycompany.com** ha la priorità più elevata e **selfguide.Training.mycompany.com** ha la priorità più alta successiva. Il **Training** e la **libreria** condividono la stessa priorità più bassa.  
  
####  <a name="bkmk_QoS_UrlPort"></a>3. Porta URL

Un numero di porta specifico o implicito ha una priorità più alta rispetto a una porta con caratteri jolly.

####  <a name="bkmk_QoS_UrlPath"></a>4. Percorso URL

Analogamente a un nome host, un percorso URL può essere costituito da più elementi. Quello con più elementi ha sempre una priorità più alta rispetto a quello con minore. Ad esempio, i percorsi seguenti sono elencati per priorità:  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Se un utente sceglie di includere tutte le sottodirectory e i file che seguono un percorso URL, il percorso dell'URL avrà una priorità più bassa rispetto a quando non è stata effettuata la scelta.

Un utente può anche scegliere di specificare un indirizzo IP di destinazione in un criterio basato su URL. La priorità dell'indirizzo IP di destinazione è inferiore a quella dei quattro campi URL descritti in precedenza.
  
### <a name="quintuple-policy"></a>Criteri di quintupla

Un criterio quintupla viene specificato in base all'ID protocollo, all'indirizzo IP di origine, alla porta di origine, all'indirizzo IP di destinazione e alla porta di destinazione. Un criterio quintupla ha sempre una precedenza superiore rispetto a qualsiasi criterio basato su URL. 

Se un criterio quintupla è già applicato per un utente, i nuovi criteri basati su URL non provocheranno conflitti in nessuno dei computer client di tale utente.

Per l'argomento successivo di questa guida, vedere [gestire i criteri QoS](qos-policy-manage.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).
