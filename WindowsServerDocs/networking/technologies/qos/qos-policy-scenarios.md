---
title: Scenari di criterio QoS
description: Questo argomento fornisce scenari dei criteri di qualità del servizio (QoS), che illustra come usare criteri di gruppo per definire la priorità il traffico di rete di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>Scenari di criterio QoS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per esaminare scenari ipotetici che illustrano come, quando e perché usare criteri QoS.

I due scenari in questo argomento sono:

1. Stabilire le priorità del traffico di rete per un'applicazione Line-of-Business
2. Stabilire le priorità del traffico di rete per un'applicazione Server HTTP

>[!NOTE]
>Alcune sezioni di questo argomento contengono procedure generali che utili per eseguire le azioni descritte. Per ulteriori istruzioni sulla gestione dei criteri di QoS, vedere [gestire criteri di QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scenario 1: Definire le priorità del traffico di rete per un'applicazione Line-of-Business

In questo scenario, un reparto IT ha diversi obiettivi che possono essere eseguite tramite criteri di QoS:

- Fornire prestazioni di rete migliori per applicazioni critiche mission\.
- Fornire i migliori prestazioni di rete per un set di chiavi degli utenti durante l'utilizzo di un'applicazione specifica.
- Assicurarsi che i dati company\ a livello di applicazione di Backup non limitare le prestazioni di rete utilizzando una quantità eccessiva larghezza di banda in una sola volta.

Il reparto IT decide di configurare i criteri QoS per stabilire le priorità di specifiche applicazioni utilizzando il punto di codice servizio differenziazione \(DSCP\) valori per classificare il traffico di rete e per configurare i router per fornire trattamento preferenziale per il traffico di priorità superiore. 

>[!NOTE]
>Per ulteriori informazioni su DSCP, vedere la sezione **definire QoS priorità tramite un Differentiated Services Code Point** nell'argomento [criteri Quality of Service (QoS)](qos-policy-top.md).

Oltre ai valori DSCP, i criteri QoS possono specificare una velocità. Limitazione l'effetto della limitazione di tutto il traffico in uscita che corrisponde a una specifica i criteri di QoS frequenza di trasmissione.

### <a name="qos-policy-configuration"></a>Configurazione dei criteri QoS

Con tre obiettivi separati per portare a termine, l'amministratore IT decide di creare tre diversi criteri QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Criteri QoS per il server di App LOB

La prima applicazione mission\ critici per cui il reparto IT crea un criterio QoS è una risorsa di Enterprise company\ livello pianificazione \(ERP\) applicazione. L'applicazione ERP è ospitato in diversi computer che sono tutti in esecuzione Windows Server 2016. In servizi di dominio Active Directory, questi computer sono membri di un'unità organizzativa \(OU\) creato per i server di applicazioni line-of-business \(LOB\). Il componente lato connessione tra client per l'applicazione ERP viene installato nei computer che eseguono Windows 10 e Windows 8.1.

In Criteri di gruppo, un amministratore IT seleziona \(GPO\) l'oggetto Criteri di gruppo su cui verrà applicato il criterio QoS. Utilizzando la procedura guidata criteri di QoS, l'amministratore IT crea un criterio QoS denominato "Server LOB criteri" che specifica un valore di priorità alto DSCP pari a 44 pixel per tutte le applicazioni, qualsiasi indirizzo IP, TCP e UDP e numero di porta.

Il criterio QoS viene applicato solo ai server LOB collegando l'oggetto Criteri di gruppo all'unità Organizzativa che contiene solo questi server, tramite lo strumento \(GPMC\) Console Gestione criteri di gruppo. Questo server iniziale LOB criterio si applica il valore DSCP priorità elevata ogni volta che il computer invia il traffico di rete. Questo criterio QoS in un secondo momento è possibile modificare \ (in tool\ l'Editor oggetto Criteri di gruppo) per includere i numeri di porta dell'applicazione ERP, che limita i criteri per applicare solo quando viene usato il numero di porta specificato.

#### <a name="qos-policy-for-the-finance-group"></a>Criteri QoS per il gruppo Finance

Mentre molti gruppi all'interno della società accedere all'applicazione ERP, al gruppo finance dipende l'applicazione quando si gestiscono i clienti e il gruppo richiede in modo coerente ad alte prestazioni dall'app.

Per assicurarsi che al gruppo finance può supportare i clienti, il criterio QoS debba classificare il traffico di tali utenti una priorità alta. Tuttavia, i criteri non applicare quando i membri del gruppo finance utilizzano applicazioni diverse dall'applicazione ERP. 

Per questo motivo, il reparto IT definisce un secondo criterio di QoS chiamato "Client LOB criteri" nello strumento Editor oggetti Criteri di gruppo che applica un valore DSCP pari a 60, quando il gruppo di utenti Finanza esegue l'applicazione ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Criteri QoS per un'App di Backup

Un'applicazione di backup separata è in esecuzione in tutti i computer. Per garantire che il traffico dell'applicazione di backup non Usa tutte le risorse di rete disponibile, il reparto IT crea un criterio di dati di backup. Criteri di backup specificano un valore DSCP pari a 1 in base al nome di eseguibile per il backup app, ovvero **backup.exe **. 

Un terzo oggetto Criteri di gruppo viene creato e distribuito per tutti i computer client nel dominio. Ogni volta che l'applicazione di backup invia dati, viene applicato il valore DSCP priorità bassa, anche se ha origine da computer del reparto finanziario.
  
>[!NOTE]
>Il traffico di rete senza un criterio QoS Invia con un valore DSCP pari a 0.

### <a name="scenario-policies"></a>Scenario criteri

La tabella seguente riepiloga i criteri QoS per questo scenario.
  
|Policy name|Valore DSCP|Velocità in uscita|Applicato a unità organizzative|Descrizione|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Nessun criterio]|0|Nessuno|[Non distribuzione]|Migliore trattamento sforzo (impostazione predefinita) per il traffico non classificato.|  
|Dati di backup|1|Nessuno|Tutti i client|Si applica un valore DSCP priorità bassa per questi dati di massa.|  
|Server Line|44|Nessuno|Computer unità Organizzativa per i server ERP|Si applica ad alta priorità DSCP per il traffico tra server ERP|  
|Client LOB|60|Nessuno|Gruppo utenti Finanza|Si applica ad alta priorità DSCP per il traffico client ERP|  

>[!NOTE]
>I valori DSCP sono rappresentati in formato decimale.

Con criteri di QoS definiti e applicati tramite criteri di gruppo, il traffico di rete in uscita riceve il valore DSCP criterio specificato. I router quindi forniscono trattamento differenziale basate su questi valori DSCP tramite Accodamento messaggi. Il reparto IT, i router sono configurati con le code di quattro: priorità alta, priorità medio, massimo sforzo e priorità bassa.

Quando il traffico arriva al router con i valori DSCP "LOB Server dei criteri" e "Client LOB criteri," i dati vengono inseriti nelle code ad alta priorità. Il traffico con un valore DSCP pari a 0 riceve un livello massimo sforzo di servizio. I pacchetti con un valore DSCP pari a 1 (dall'applicazione di backup) ricevono un trattamento priorità bassa.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Prerequisiti per l'assegnazione di priorità di un'applicazione line-of-business

Per completare questa attività, verificare che siano soddisfatti i requisiti seguenti:

- I computer coinvolti sono in esecuzione sistemi operativi compatibili con QoS\.

- I computer coinvolti sono membri di un dominio di Active Directory Domain Services \(AD DS\) in modo che è possibile configurarle tramite criteri di gruppo.

- Reti TCP/IP sono configurate con router configurato per \(RFC 2474\) DSCP. Per ulteriori informazioni, vedere [RFC 2474](http://www.ietf.org/rfc/rfc2474.txt).

- Requisiti di credenziali amministrative.

#### <a name="administrative-credentials"></a>Credenziali amministrative

Per completare questa attività, è necessario essere in grado di creare e distribuire oggetti Criteri di gruppo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurazione dell'ambiente di test per definire la priorità di un'applicazione line-of-business

Per configurare l'ambiente di testing, completare le attività seguenti.

- Creare un dominio di Active Directory con i client e gli utenti raggruppati in unità organizzative. Per istruzioni sulla distribuzione di Active Directory, vedere il [Guida alla rete Core ](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurare i router differenziali coda in base ai valori DSCP. Ad esempio, il valore DSCP 44 immette una coda "Platinum" e tutti gli altri sono ponderato-equo-in coda.

>[!NOTE]
> È possibile visualizzare i valori DSCP con acquisizioni di rete con strumenti come Network Monitor. Dopo aver eseguito un'acquisizione di rete, è possibile osservare il campo TOS nei dati acquisiti.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Passaggi per definire la priorità di un'applicazione line-of-business

Per stabilire le priorità di un'applicazione line-of-business, completare le attività seguenti:

1. Creare e collegare un oggetto Criteri di gruppo \(GPO\) con un criterio QoS.

2. Configurare i router per considerare differenziali un line-of-business dell'applicazione (tramite Accodamento messaggi) in base ai valori DSCP selezionati. Le procedure di questa attività variano a seconda del tipo di router che hai.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scenario 2: Stabilire le priorità del traffico di rete per un'applicazione Server HTTP

In Windows Server 2016, QoS basata su criteri include la funzionalità criteri basati su URL. URL criteri consentono di gestire la larghezza di banda per i server HTTP.

Molte applicazioni aziendali vengono sviluppate per ospitate nei server web Internet Information Services \(IIS\) e le app Web sono accessibili dal browser nei computer client.

In questo scenario, si supponga di gestire un set di server IIS che video di formazione su host per i dipendenti tutti dell'organizzazione. L'obiettivo consiste nel verificare che il traffico da questi server video non sovraccaricare la rete e assicurarsi che il traffico video si distingue dal traffico voce e i dati sulla rete. 

L'attività è simile all'attività nello Scenario 1. Si verrà progettare e configurare le impostazioni di gestione del traffico, ad esempio il valore DSCP per il traffico, video e la limitazione di velocità lo stesso come si farebbe per le applicazioni line-of-business. Ma quando si specifica il traffico, invece di fornire il nome dell'applicazione, immettere solo l'URL a cui l'applicazione server HTTP risponderà: ad esempio,https://hrweb/training.
  
> [!NOTE]
>È possibile utilizzare criteri di QoS basata su URL per stabilire le priorità del traffico di rete per i computer che eseguono sistemi operativi Windows che sono stati rilasciati prima di Windows 7 e Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Regole di precedenza per i criteri basati su URL

Tutti gli URL seguenti sono validi e possono essere specificato in Criteri di QoS e applicate simultaneamente a un computer o un utente:

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

Ma quale riceverà la priorità? Le regole sono semplici. Criteri basati su URL sono classificati in un ordine di lettura da sinistra a destra. Pertanto, dalla priorità più alta alla priorità più bassa, i campi URL sono:
  
[1. Schema URL](#bkmk_QoS_UrlScheme)

[2. Host URL](#bkmk_QoS_UrlHost)

[3. Porta URL](#bkmk_QoS_UrlPort)

[4. Percorso URL](#bkmk_QoS_UrlPath)

Come indicato di seguito sono riportati i dettagli:

####  <a name="bkmk_QoS_UrlScheme"></a>1. Schema URL

 `https://` Ha una priorità maggiore rispetto a `http://`.

####  <a name="bkmk_QoS_UrlHost"></a>2. host URL

 Dalla priorità più alta al valore più basso, sono:

1. Nome host

2. Indirizzo IPv6

3. Indirizzo IPv4

4. Con caratteri jolly

Nel caso di nome host, un nome host con più punti elementi (più approfondito) ha una priorità maggiore rispetto a un nome host con meno elementi puntati. Ad esempio, tra i nomi host seguenti:

- video.internal.training.hr.mycompany.com (profondità = 6)
  
- selfguide.training.mycompany.com (profondità = 4)
  
- Formazione (depth = 1)
  
- Libreria (depth = 1)
  
 **video.internal.training.hr.mycompany.com** ha la priorità più alta e **selfguide.training.mycompany.com** ha la priorità più alta Avanti. **Formazione** e **libreria** condividono la stessa priorità più bassa.  
  
####  <a name="bkmk_QoS_UrlPort"></a>3. porta URL

Un numero di porta implicita o di uno specifico ha una priorità maggiore rispetto a una porta con caratteri jolly.

####  <a name="bkmk_QoS_UrlPath"></a>4. percorso URL

Ad esempio un nome host, un percorso URL può essere costituito più elementi. Quella con più elementi sempre ha una priorità maggiore rispetto a quello con minore. Ad esempio, i percorsi seguenti sono elencati in ordine di priorità:  

1.  /eBooks/Tech/Windows/Networking/QoS

2.  / eBook/tech/windows /

3.  /ebooks

4.  /

Se un utente sceglie di includere tutte le sottodirectory e i file dopo un percorso di URL, questo percorso URL avrà una priorità più bassa rispetto a un se la scelta non sono stata apportata.

Un utente può anche scegliere di specificare un indirizzo IP di destinazione in un criterio basato su URL. Indirizzo IP di destinazione ha una priorità inferiore rispetto a qualsiasi dei quattro campi URL descritti in precedenza.
  
### <a name="quintuple-policy"></a>Criteri quintupla

ID protocollo, indirizzo IP di origine, porta di origine, indirizzo IP di destinazione e porta di destinazione viene specificato un criterio quintupla. Un criterio quintupla ha sempre la precedenza rispetto a qualsiasi criterio basato su URL. 

Se è già applicato un criterio quintupla per un utente, un nuovo criterio basato su URL non causa conflitti in uno qualsiasi dei client di tale utente computer.

Per l'argomento successivo in questa Guida, vedere [gestire criteri di QoS](qos-policy-manage.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).
