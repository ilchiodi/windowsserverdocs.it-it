---
title: wecutil
description: Windows Commands Topic for wecutil, che consente di creare e gestire le sottoscrizioni di eventi che vengono trasmessi da computer remoti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 2bb397ace7cc99c8b8d6bbed3598346ff2d0801c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829438"
---
# <a name="wecutil"></a>wecutil



Consente di creare e gestire le sottoscrizioni agli eventi che vengono trasmessi da computer remoti. Il computer remoto deve supportare il protocollo WS-Management. Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).


## <a name="syntax"></a>Sintassi

```
wecutil  [{es | enum-subscription}] 
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] 
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] 
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] 
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] 
[{ds | delete-subscription} <Subid>] 
[{rs | retry-subscription} <Subid> [<Eventsource>…]] 
[{qc | quick-config} [/q:[<Quiet>]]].
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{es \| enum-Subscription}|Visualizza i nomi di tutte le sottoscrizioni di eventi remoti esistenti.|
|{GS \| Get-Subscription} \<subid > [/f:\<Format >] [/UNI:\<Unicode >]|Visualizza le informazioni di configurazione sottoscrizione remota. \<subid > è una stringa che identifica in modo univoco una sottoscrizione. \<subid > corrisponde alla stringa specificata nel tag \<SubscriptionId > del file di configurazione XML, utilizzato per creare la sottoscrizione.|
|{gr \| Get-subscriptionruntimestatus} \<subid > [\<EventSource >...]|Visualizza lo stato di runtime di una sottoscrizione. \<subid > è una stringa che identifica in modo univoco una sottoscrizione. \<subid > corrisponde alla stringa specificata nel tag \<SubscriptionId > del file di configurazione XML, utilizzato per creare la sottoscrizione. \<EventSource > è una stringa che identifica un computer che funge da origine di eventi. \<EventSource > deve essere un nome di dominio completo, un nome NetBIOS o un indirizzo IP.|
|{SS \| set-Subscription} \<subid > [/e: [\<subenabled >]] [/ESA:\<address >] [/ESE: [\<Srcenabled >]] [/AES] [/RES] [/un:\<username >] [/up:\<password >] [/d:\<desc >] [/URI:\<Uri >] [/cm:\<Configmode >] [/ex:\<scade >] [/q:\<query >] [/dia:\<dialetto >] [/TN:\<transportaname >] [/TP:\<Transportport >] [/DM:\<DeliveryMode >] [/DMI:\<Deliverymax >] [/DMLT:\<deliverytime >] [/Hi:\<heartbeat >] [/CF:\<contenuto >] [/l :\<impostazioni locali >] [/ree: [\<Readexist >]] [/LF:\<logfile >] [/PN:\<PublisherName >] [/essp:\<Enableport >] [/HN:\<nome host >] [/CT:\<tipo >]</br>oppure</br>{SS \| set-Subscription/c:\<ConfigFile > [/CUN:\<comusername >/Cup:\<compassword >]|Modifica la configurazione della sottoscrizione. È possibile specificare l'ID sottoscrizione e le opzioni appropriate per modificare i parametri di sottoscrizione oppure è possibile specificare un file di configurazione XML per modificare i parametri di sottoscrizione.|
|{CS \| create-Subscription} \<ConfigFile > [/CUN:\<username >/Cup:\<password >]|Crea una sottoscrizione remota. \<ConfigFile > specifica il percorso del file XML che contiene la configurazione della sottoscrizione. Il percorso può essere assoluto o relativo alla directory corrente.|
|{DS \| delete-subscription} \<subid >|Elimina una sottoscrizione e Annulla la sottoscrizione da tutte le origini eventi che inviano eventi nel registro eventi per la sottoscrizione. Non vengono eliminati tutti gli eventi già ricevuti e registrati. \<subid > è una stringa che identifica in modo univoco una sottoscrizione. \<subid > corrisponde alla stringa specificata nel tag \<SubscriptionId > del file di configurazione XML, utilizzato per creare la sottoscrizione.|
|{RS \| Retry-Subscription} \<subid > [\<EventSource >...]|Tenterà di stabilire una connessione e inviare una richiesta di sottoscrizione remoto a una sottoscrizione inattiva. Tenta di riattivare tutte le origini evento o specificare le origini eventi. Origini disabilitate non vengono ripetute. \<subid > è una stringa che identifica in modo univoco una sottoscrizione. \<subid > corrisponde alla stringa specificata nel tag \<SubscriptionId > del file di configurazione XML, utilizzato per creare la sottoscrizione. \<EventSource > è una stringa che identifica un computer che funge da origine di eventi. \<EventSource > deve essere un nome di dominio completo, un nome NetBIOS o un indirizzo IP.|
|{QC \| Quick-config} [/q: [\<> non interattiva]]|Configura il servizio Raccolta eventi Windows per garantire una sottoscrizione può essere creata e mantenuta anche dopo il riavvio. Questo include i passaggi seguenti:</br>1. abilitare il canale ForwardedEvents se è disabilitato.</br>2. impostare il servizio raccolta eventi Windows per ritardare l'avvio.</br>3. avviare il servizio raccolta eventi Windows se non è in esecuzione.|

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|------|-----------|
|/f: formato\<>|Specifica il formato delle informazioni visualizzate. il formato \<> può essere XML o conciso. Se <Format> è XML, viene visualizzato l'output in formato XML. Se \<Format > è conciso, l'output viene visualizzato in coppie nome-valore. Il valore predefinito è XML.|
|/c:\<ConfigFile >|Specifica il percorso del file XML che contiene una configurazione della sottoscrizione. Il percorso può essere assoluto o relativo alla directory corrente. Questa opzione può essere utilizzata solo con il **/cun** e **/cup** Opzioni e si escludono a vicenda con tutte le altre opzioni.|
|/e: [\<> abilitata]|Abilita o disabilita una sottoscrizione. \<> non abilitato può essere true o false. Il valore predefinito di questa opzione è true.|
|/ESA: Indirizzo\<>|Specifica l'indirizzo di un'origine evento. \<indirizzo > è una stringa che contiene un nome di dominio completo, un nome NetBIOS o un indirizzo IP, che identifica un computer che funge da origine di eventi. Questa opzione deve essere utilizzata con il **/ese**, **/aes**, **/res**, o **/un** e **/up** Opzioni.|
|/ESE: [\<Srcenabled >]|Abilita o disabilita un'origine evento. \<Srcenabled > può essere true o false. Questa opzione è consentita solo se il **/esa** opzione specificata. Il valore predefinito di questa opzione è true.|
|/AES|Aggiunge l'origine evento specificata dal **/esa** opzione se non è già parte della sottoscrizione. Se l'indirizzo specificato per il **/esa** opzione fa già parte della sottoscrizione, viene restituito un errore. Questa opzione è consentita solo se il **/esa** opzione specificata.|
|/res|Rimuove l'origine evento specificata dal **/esa** opzione se è già parte della sottoscrizione. Se l'indirizzo specificato per il **/esa** opzione non è una parte della sottoscrizione, viene restituito un errore. Questa opzione è consentita solo se **/esa** opzione specificata.|
|/un:\<nome utente >|Specifica le credenziali utente da utilizzare con l'origine evento specificata tramite il **/esa** (opzione). Questa opzione è consentita solo se il **/esa** opzione specificata.|
|/up:\<> password|Specifica la password che corrisponde alle credenziali utente. Questa opzione è consentita solo se il **/un** opzione specificata.|
|/d:\<desc >|Fornisce una descrizione per la sottoscrizione.|
|/URI: URI\<>|Specifica il tipo di eventi che vengono utilizzate dalla sottoscrizione. \<Uri > contiene una stringa URI combinata con l'indirizzo del computer di origine evento per identificare in modo univoco l'origine degli eventi. La stringa URI è utilizzata per tutti gli indirizzi di origine evento nella sottoscrizione.|
|/cm:\<Configmode >|Imposta la modalità di configurazione. \<Configmode > può essere una delle seguenti stringhe: Normal, Custom, MinLatency o MinBandwidth. Le modalità normale, MinLatency e MinBandwidth impostare la modalità di recapito, elementi max di recapito, l'intervallo di heartbeat e tempo di latenza massima di recapito. Il **/dm**, **/dmi**, **/hi** o **/dmlt** le opzioni possono essere solo se la modalità di configurazione è impostato su Custom.|
|/ex:\<scade >|Imposta l'ora di scadenza della sottoscrizione. \<scade > deve essere definito nel formato di data e ora standard XML o ISO8601: AAAA-MM-GGThh: mm: SS [. sss] [Z], dove T è il separatore dell'ora e Z indica l'ora UTC.|
|/q:\<query >|Specifica la stringa di query per la sottoscrizione. Il formato di \<query > può essere diverso per i diversi valori URI e si applica a tutte le origini nella sottoscrizione.|
|/dia:\<dialetto >|Definisce il dialetto utilizzato dalla stringa di query.|
|/TN:\<transportaname >|Specifica il nome del trasporto utilizzato per connettersi a un'origine eventi remoto.|
|/TP:\<Transportport >|Imposta il numero di porta utilizzato dal trasporto per la connessione a un'origine eventi remoto.|
|/DM:\<DeliveryMode >|Specifica la modalità di recapito. \<DeliveryMode > può essere pull o push. Questa opzione è valida solo se il **/cm** opzione è impostata su Custom.|
|/DMI:\<Deliverymax >|Imposta il numero massimo di elementi per il recapito in batch. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/DMLT:\<deliverytime >|Imposta la latenza massima nella fornitura di un batch di eventi. \<> deliverytime è il numero di millisecondi. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/Hi: > heartbeat\<|Definisce l'intervallo di heartbeat. \<> heartbeat è il numero di millisecondi. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/CF: > contenuto\<|Specifica il formato degli eventi che vengono restituiti. \<> di contenuto possono essere eventi o informazioni sulla. Quando il valore è informazioni sulla, gli eventi vengono restituiti con le stringhe localizzate (ad esempio, la descrizione dell'evento) associate all'evento. Il valore predefinito è informazioni sulla.|
|/l: impostazioni locali\<>|Specifica le impostazioni locali per il recapito delle stringhe localizzate nel formato di informazioni sulla. \<impostazioni locali > è un identificatore di lingua e paese/area geografica, ad esempio EN-US. Questa opzione è valida solo se il **/cf** opzione è impostata su informazioni sulla.|
|/Ree: [\<Readexist >]|Identifica gli eventi che vengono recapitati per la sottoscrizione. \<Readexist > può essere true o false. Quando il <Readexist> è true, tutti gli eventi esistenti vengono lette da origini di eventi di sottoscrizione. Quando il <Readexist> è false, vengono inviati solo eventi (in entrata) future. Il valore predefinito è true per un **/ree** opzione senza un valore. Se non **/ree** opzione è specificata, il valore predefinito è false.|
|/LF: > logfile\<|Specifica il registro eventi locale utilizzato per archiviare gli eventi ricevuti dalle origini eventi.|
|/PN:\<PublisherName >|Specifica il nome dell'autore. Deve essere un server di pubblicazione che possiede o Importa log specificato dal parametro di **/lf** (opzione).|
|/essp:\<Enableport >|Specifica che il numero di porta deve essere aggiunto al nome dell'entità servizio del servizio remoto. \<Enableport > può essere true o false. Il numero di porta viene aggiunto quando <Enableport> è true. Quando viene aggiunto il numero di porta, alcune operazioni di configurazione potrebbe essere necessario impedire l'accesso alle origini eventi da negata.|
|/HN:\<hostname >|Specifica il nome DNS del computer locale. Questo nome viene utilizzato dall'origine di eventi remoti per eseguire il push degli eventi e deve essere utilizzato solo per una sottoscrizione push.|
|/CT: tipo di\<>|Imposta il tipo di credenziali per l'accesso remoto di origine. il tipo di \<> deve essere uno dei valori seguenti: default, Negotiate, digest, Basic o LocalMachine. Il valore predefinito è l'impostazione predefinita.|
|/cun:\<comusername >|Imposta le credenziali utente condivise da utilizzare per le origini evento che non hanno le proprie credenziali utente. Se questa opzione viene specificata con il **/c** UserName e UserPassword le impostazioni delle opzioni per le origini eventi singoli dal file di configurazione vengono ignorate. Se si desidera utilizzare credenziali diverse per un'origine evento specifico, è necessario eseguire l'override di questo valore specificando la **/un** e **/up** Opzioni per un'origine evento specifico nella riga di comando di un altro **ss** comando.|
|/Cup:\<> compassword|Imposta la password dell'utente per le credenziali utente condivise. Quando \<> compassword è impostato su * (asterisco), la password viene letta dalla console. Questa opzione è valida solo quando il **/cun** opzione specificata.|
|/q: [\<> non interattiva]|Specifica se la procedura di configurazione richiede la conferma. \<> non interattiva può essere true o false. Se <Quiet> è true, la procedura di configurazione non richiede la conferma. Il valore predefinito di questa opzione è false.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Se viene visualizzato il messaggio "il server RPC non è disponibile? Quando si tenta di eseguire wecutil, è necessario avviare il servizio raccolta eventi Windows (wecsvc). Per avviare wecsvc, a un prompt dei comandi con privilegi elevati, digitare net start wecsvc.

- Nell'esempio seguente viene illustrato il contenuto di un file di configurazione:  
  ```
  <Subscription xmlns=https://schemas.microsoft.com/2006/03/windows/events/subscription>
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path=Application>
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled=true>
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language=EN-US></Locale>
  </Subscription>
  ```

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Informazioni di configurazione per una sottoscrizione denominata sub1 di output:
```
wecutil gs sub1
```
Output di esempio:
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
Visualizzare lo stato di runtime di una sottoscrizione denominata sub1:
```
wecutil gr sub1
```
Aggiornare la configurazione della sottoscrizione denominata sub1 da un nuovo file XML denominato WsSelRg2.xml:
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Aggiornare la configurazione della sottoscrizione denominata sub2 con più parametri:
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Creare una sottoscrizione per inoltrare gli eventi da un registro eventi applicazioni di Windows Vista di un computer remoto in mySource.myDomain.com nel registro ForwardedEvents (vedere la sezione Osservazioni per un esempio di un file di configurazione):
```
wecutil cs subscription.xml
```
Eliminare una sottoscrizione denominata sub1:
```
wecutil ds sub1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
