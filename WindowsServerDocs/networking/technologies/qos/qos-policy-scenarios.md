---
title: Scenari relativi ai criteri di QoS
description: In questo argomento fornisce scenari relativi ai criteri di qualità del servizio (QoS), che illustrano come usare criteri di gruppo per definire la priorità il traffico di rete di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2617c897ed2ea173d29fc7c4a87e52557154d463
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446989"
---
# <a name="qos-policy-scenarios"></a>Scenari relativi ai criteri di QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare questo argomento per esaminare ipotetici scenari che illustrano come fare, quando e perché usare i criteri QoS.

I due scenari in questo argomento sono:

1. Definire la priorità del traffico di rete per un'applicazione Line-of-Business
2. Definire la priorità del traffico di rete per un'applicazione Server HTTP

>[!NOTE]
>Alcune sezioni di questo argomento contengono generale da seguire per eseguire le azioni descritte. Per istruzioni sulla gestione dei criteri QoS più dettagliate, vedere [gestire i criteri QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scenario 1: Definire la priorità del traffico di rete per un'applicazione Line-of-Business

In questo scenario, un reparto IT ha obiettivi diversi che possono essere eseguite tramite criteri di QoS:

- Offrono migliori prestazioni di rete per la missione\-applicazioni critiche.
- Fornire migliorate prestazioni di rete per un set di chiavi di utenti mentre usano un'applicazione specifica.
- Assicurarsi che la società\-dei dati a livello applicazione di Backup non influire sulle prestazioni di rete usando troppa larghezza di banda in una sola volta.

Il reparto IT decide di configurare i criteri QoS per assegnare priorità alle applicazioni specifiche tramite punto di codice del servizio di differenziazione \(DSCP\) i valori per classificare il traffico di rete e configurare il router per fornire preferenziale modalità di gestione per il traffico di priorità superiore. 

>[!NOTE]
>Per altre informazioni su DSCP, vedere la sezione **definire QoS priorità tramite un Differentiated Services Code Point** nell'argomento [criteri di qualità del servizio (QoS)](qos-policy-top.md).

Oltre ai valori DSCP, i criteri QoS possono specificare una limitazione della velocità. La limitazione delle richieste ha l'effetto di limitare tutto il traffico in uscita che corrispondenze i criteri QoS a una determinata velocità di invio.

### <a name="qos-policy-configuration"></a>Configurazione dei criteri QoS

Con tre separati obiettivi da raggiungere, l'amministratore IT decide di creare tre diversi criteri di QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Criteri QoS per server App LOB

La missione primo\-critiche dell'applicazione per cui il reparto IT crea un criterio QoS è una società\-wide Enterprise resource planning \(ERP\) dell'applicazione. L'applicazione ERP è ospitata in diversi computer su cui sono in esecuzione Windows Server 2016. In servizi di dominio di Active Directory, questi computer sono membri di un'unità organizzativa \(OU\) che è stato creato per line-of-business \(LOB\) server applicazioni. Il client\-lato componente per l'applicazione ERP è installato nel computer che eseguono Windows 10 e Windows 8.1.

Criteri di gruppo, un amministratore IT Seleziona oggetto Criteri di gruppo \(oggetto Criteri di gruppo\) al quale verranno applicati i criteri QoS. Usando la procedura guidata criteri QoS, l'amministratore IT crea un criterio di QoS che ha chiamato "Server LOB criteri" che specifica un valore alto\-priorità valore DSCP pari a 44 per tutte le applicazioni, qualsiasi indirizzo IP, TCP e UDP e numero di porta.

Il criterio QoS viene applicato solo ai server LOB tramite il collegamento oggetto Criteri di gruppo all'unità Organizzativa che contiene solo questi server, tramite la Console Gestione criteri di gruppo \(GPMC\) dello strumento. Questo iniziale del server LOB criterio si applica il valore alto\-priorità DSCP valore ogni volta che il computer invia il traffico di rete. Questo criterio di QoS in un secondo momento può essere modificato \(nello strumento Editor oggetti Criteri di gruppo\) per includere i numeri di porta dell'applicazione ERP, che limita i criteri da applicare solo quando viene utilizzato il numero di porta specificato.

#### <a name="qos-policy-for-the-finance-group"></a>Criteri QoS per il gruppo Finance

Sebbene molti gruppi all'interno della società accedono all'applicazione ERP, gruppo finance dipende da questa applicazione quando si lavora con i clienti e il gruppo richiede in modo coerente ad alte prestazioni dall'app.

Per assicurarsi che il gruppo finance grado di supportare i clienti, i criteri QoS devono classificare il traffico di tali utenti come priorità alta. Tuttavia, i criteri non dovrebbero applicarsi quando i membri del gruppo finance di utilizzano applicazioni diverse dall'applicazione ERP. 

Per questo motivo, il reparto IT definisce un criterio QoS secondo chiamato "Client LOB criteri" nello strumento Editor oggetti Criteri di gruppo che applica un valore DSCP pari a 60 quando il gruppo di utenti finance esegue l'applicazione ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Criteri QoS per un'App di Backup

Un'applicazione di backup separata è in esecuzione in tutti i computer. Per garantire che il traffico dell'applicazione di backup non Usa tutte le risorse di rete disponibile, il reparto IT crea un criterio di backup dei dati. Questo criterio di backup specifica un valore DSCP pari a 1 basato sul nome del file eseguibile per l'app di backup, ovvero **backup.exe**. 

Un terzo oggetto Criteri di gruppo viene creato e distribuito per tutti i computer client nel dominio. Ogni volta che l'applicazione di backup invia dati, viene applicato il valore di DSCP con priorità bassa, anche se ha origine dal computer del reparto finance.
  
>[!NOTE]
>Il traffico di rete senza un criterio QoS Invia con un valore DSCP pari a 0.

### <a name="scenario-policies"></a>Criteri di uno scenario

Nella tabella seguente sono riepilogati i criteri QoS per questo scenario.
  
|Nome criterio|Valore di DSCP|Limitazione della velocità|Applicato a unità organizzative|Descrizione|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nessun criterio]|0|Nessuno|[Nessuna distribuzione]|Migliore trattamento sforzo (impostazione predefinita) per il traffico non classificato.|  
|Dati di backup|1|Nessuno|Tutti i client|Si applica un valore DSCP con priorità bassa per i dati in blocco.|  
|Server LOB|44|Nessuno|Computer unità Organizzativa per i server ERP|Si applica ad alta priorità DSCP per il traffico dei server ERP|  
|Client LOB|60|Nessuno|Gruppo di utenti Finance|Si applica ad alta priorità DSCP per il traffico client ERP|  

>[!NOTE]
>I valori DSCP sono rappresentati in formato decimale.

Con i criteri QoS definiti e applicati tramite criteri di gruppo, il traffico di rete in uscita riceve il valore DSCP criterio specificato. I router quindi forniscono trattamento differenziale basato su questi valori DSCP usando Accodamento messaggi. Per il reparto IT, i router sono configurati con quattro code: con priorità alta, medio-priorità, sforzo e con priorità bassa.

Quando il traffico arriva al router con i valori DSCP "Server LOB criteri" e "Client LOB criteri," i dati vengono inseriti nelle code con priorità alta. Il traffico con un valore DSCP pari a 0 riceve un migliore livello di servizio. I pacchetti con un valore DSCP pari a 1 (dall'applicazione di backup) ricevono un trattamento con priorità bassa.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Prerequisiti per la definizione delle priorità di un'applicazione line-of-business

Per completare questa attività, verificare che siano soddisfatti i requisiti seguenti:

- Tutti i computer coinvolti esegue QoS\-sistemi operativi compatibili.

- Tutti i computer coinvolti sono membri di un Active Directory Domain Services \(Active Directory Domain Services\) dominio in modo che possano essere configurati usando criteri di gruppo.

- Le reti TCP/IP sono impostate con i router configurati per DSCP \(RFC 2474\). Per altre informazioni, vedere [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Siano soddisfatti i requisiti delle credenziali amministrative.

#### <a name="administrative-credentials"></a>Credenziali amministrative

Per completare questa attività, è necessario essere in grado di creare e distribuire oggetti Criteri di gruppo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurazione dell'ambiente di test per definire le priorità di un'applicazione line-of-business

Per configurare l'ambiente di testing, completare le attività seguenti.

- Creare un dominio di Active Directory Domain Services con i client e utenti raggruppati in unità organizzative. Per istruzioni sulla distribuzione di Active Directory Domain Services, vedere la [Guida alla rete Core](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurare i router differenziali coda in base ai valori DSCP. Ad esempio, il valore di DSCP 44 entra in una coda di "tipo Platinum" e tutti gli altri vengono ponderati fair-in coda.

>[!NOTE]
> È possibile visualizzare i valori di DSCP tramite le acquisizioni di rete con strumenti quali monitoraggio di rete. Dopo aver eseguito un'acquisizione di rete, è possibile osservare il campo TOS in dati acquisiti.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Passaggi per la definizione delle priorità di un'applicazione line-of-business

Per stabilire le priorità di un'applicazione line-of-business, completare le attività seguenti:

1. Creare e collegare un oggetto Criteri di gruppo \(oggetto Criteri di gruppo\) con un criterio QoS.

2. Configurare i router e differenziali considerano una line-of-business dell'applicazione (utilizzando Accodamento messaggi) in base ai valori DSCP selezionati. Le procedure di questa attività variano a seconda del tipo di router, che è necessario.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scenario 2: Definire la priorità del traffico di rete per un'applicazione Server HTTP

In Windows Server 2016, QoS basata su criteri include la funzionalità criteri basati su URL. I criteri URL consentono di gestire la larghezza di banda per i server HTTP.

Molte applicazioni aziendali sono sviluppate per e ospitate in Internet Information Services \(IIS\) server web e le app Web sono accessibili dai browser sui computer client.

In questo scenario, si supponga di gestire un set di server IIS che video di formazione su host per tutti i dipendenti dell'organizzazione. L'obiettivo è garantire che il traffico da questi server video non sovraccaricare la rete e assicurarsi che il traffico video si differenzia dal traffico vocale e i dati sulla rete. 

L'attività è simile all'attività nello Scenario 1. Si sarà progettare e configurare le impostazioni di gestione traffico, ad esempio il valore DSCP per il traffico del video e la limitazione di frequenza uguale come si farebbe per le applicazioni line-of-business. Ma quando si specifica il traffico, anziché fornire il nome dell'applicazione, immettere solo l'URL a cui l'applicazione server HTTP risponderà: ad esempio, https://hrweb/training.
  
> [!NOTE]
>È possibile usare i criteri di QoS basata sull'URL per definire la priorità il traffico di rete per i computer che eseguono sistemi operativi Windows che sono stati rilasciati prima di Windows 7 e Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regole di precedenza per i criteri basati su URL

Tutti gli URL seguenti siano validi e possono essere specificato in Criteri di QoS e applicate contemporaneamente a un computer o un utente:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/eBooks

Ma quale di essi riceverà la precedenza? Le regole sono semplici. I criteri basati su URL sono classificati in un ordine di lettura da sinistra a destra. Quindi, dalla priorità più alta alla priorità più bassa, i campi URL sono:
  
[1. Schema URL](#bkmk_QoS_UrlScheme)

[2. Host dell'URL](#bkmk_QoS_UrlHost)

[3. URL di porta](#bkmk_QoS_UrlPort)

[4. Percorso URL](#bkmk_QoS_UrlPath)

Come indicato di seguito sono riportati i dettagli:

####  <a name="bkmk_QoS_UrlScheme"></a> 1. Schema URL

 `https://` ha una priorità maggiore rispetto a `https://`.

####  <a name="bkmk_QoS_UrlHost"></a> 2. Host dell'URL

 Dalla priorità più alta a quello minimo, sono:

1. Hostname

2. Indirizzo IPv6

3. Indirizzo IPv4

4. Carattere jolly

Nel caso di nome host, un nome host con gli elementi più punteggiati (approfonditi) ha una priorità maggiore rispetto a un nome host con un minor numero di elementi punteggiato. Ad esempio, tra i nomi host seguenti:

- video.Internal.training.hr.mycompany.com (profondità = 6)
  
- selfguide.training.mycompany.com (profondità = 4)
  
- set di training (profondità = 1)
  
- libreria (profondità = 1)
  
  **video.Internal.training.hr.mycompany.com** ha la priorità più alta, e **selfguide.training.mycompany.com** ha la priorità più elevata successiva. **Set di training** e **libreria** condividono la stessa priorità più bassa.  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3. URL di porta

Un oggetto specifico o un numero di porta implicita ha una priorità maggiore rispetto a una porta con caratteri jolly.

####  <a name="bkmk_QoS_UrlPath"></a> 4. Percorso URL

Ad esempio un nome host, può essere costituito da un percorso URL di più elementi. Quello con più elementi ha sempre una priorità più alta rispetto a quello con meno risorse. Ad esempio, i percorsi seguenti sono elencati in ordine di priorità:  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /eBooks

4.  /

Se un utente sceglie di includere tutte le sottodirectory e i file seguendo un percorso URL, il percorso URL avrà una priorità più bassa rispetto a un se la scelta non sono stata apportata.

Un utente può anche scegliere di specificare un indirizzo IP di destinazione in un criterio basato su URL. Indirizzo IP di destinazione ha una priorità più bassa rispetto a uno qualsiasi dei quattro campi URL descritti in precedenza.
  
### <a name="quintuple-policy"></a>Criteri quintupla

Viene specificato un criterio quintupla dall'ID del protocollo, indirizzo IP di origine, porta di origine, indirizzo IP di destinazione e porta di destinazione. Un criterio quintupla ha sempre una precedenza maggiore rispetto a qualsiasi criterio basato su URL. 

Se è già applicato un criterio quintupla per un utente, un nuovo criterio basato su URL non causerà conflitti in uno qualsiasi dei client dell'utente, tale computer.

Per l'argomento successivo in questa Guida, vedere [gestire i criteri QoS](qos-policy-manage.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).
