---
title: wecutil
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 50151a322f1ccde2be927de31b0e5c30732b278e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813952"
---
# <a name="wecutil"></a>wecutil



Consente di creare e gestire sottoscrizioni agli eventi inoltrati da computer remoti. Il computer remoto deve supportare il protocollo WS-Management. Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).


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

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{es \| enum-subscription}|Visualizza i nomi di tutte le sottoscrizioni di eventi remoti esistenti.|
|{gs \| get-subscription} \<Subid > [/ f\<formato >] [/ uni:\<Unicode >]|Visualizza le informazioni di configurazione sottoscrizione remota. \<Subid > è una stringa che identifica in modo univoco una sottoscrizione. \<Subid > è uguale alla stringa specificata nella \<SubscriptionId > tag del file di configurazione XML, che è stato usato per creare la sottoscrizione.|
|{gr \| get-subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|Visualizza lo stato di runtime di una sottoscrizione. \<Subid > è una stringa che identifica in modo univoco una sottoscrizione. \<Subid > è uguale alla stringa specificata nella \<SubscriptionId > tag del file di configurazione XML, che è stato usato per creare la sottoscrizione. \<EventSource > è una stringa che identifica un computer che funge da un'origine di eventi. \<EventSource > deve essere un nome di dominio completo, un nome NetBIOS o un indirizzo IP.|
|{ss \| set-subscription} \<Subid > [/ e: [\<Subenabled >]] [/ sec:\<indirizzo >] [/ ese: [\<Srcenabled >]] [/aes] [/res] [/ annullare la:\<Username >] [/ fino:\< Password >] [/ unità d:\<Desc >] [/ uri:\<Uri >] [/ cm:\<Configmode >] [/ ex:\<Expires >] [/ q:\<Query >] [/ dia:\<sottolinguaggio >] [/ tn:\< TransportName >] [/ tp:\<Transportport >] [/ gestione dei dispositivi:\<Deliverymode >] [/ dmi:\<Deliverymax >] [/ /dmlt:\<Deliverytime >] [/ salve:\<Heartbeat >] [/ cloud Foundry:\< Contenuto >] [/ l:\<delle impostazioni locali >] [/ ree: [\<Readexist >]] [/ lf:\<FileLog >] [/ pn:\<Publishername >] [/ essp:\<Enableport >] [/ hn:\< Nome host >] [/ ct:\<tipo >]</br>oppure</br>{ss \| /c: set-subscription\<Configfile > [/ cun:\<Comusername > /Cup:\<Compassword >]|Modifica la configurazione della sottoscrizione. È possibile specificare l'ID sottoscrizione e le opzioni appropriate per modificare i parametri di sottoscrizione oppure è possibile specificare un file di configurazione XML per modificare i parametri di sottoscrizione.|
|{cs \| Crea sottoscrizione} \<Configfile > [/ cun:\<Username > /Cup:\<Password >]|Crea una sottoscrizione remota. \<ConfigFile > specifica il percorso del file XML che contiene la configurazione della sottoscrizione. Il percorso può essere assoluto o relativo alla directory corrente.|
|{ds \| Elimina-sottoscrizione} \<Subid >|Elimina una sottoscrizione e Annulla la sottoscrizione da tutte le origini eventi che inviano eventi nel registro eventi per la sottoscrizione. Non vengono eliminati tutti gli eventi già ricevuti e registrati. \<Subid > è una stringa che identifica in modo univoco una sottoscrizione. \<Subid > è uguale alla stringa specificata nella \<SubscriptionId > tag del file di configurazione XML, che è stato usato per creare la sottoscrizione.|
|{rs \| ripetizione dei tentativi-subscription} \<Subid > [\<Eventsource >...]|Tenterà di stabilire una connessione e inviare una richiesta di sottoscrizione remoto a una sottoscrizione inattiva. Tenta di riattivare tutte le origini evento o specificare le origini eventi. Origini disabilitate non vengono ripetute. \<Subid > è una stringa che identifica in modo univoco una sottoscrizione. \<Subid > è uguale alla stringa specificata nella \<SubscriptionId > tag del file di configurazione XML, che è stato usato per creare la sottoscrizione. \<EventSource > è una stringa che identifica un computer che funge da un'origine di eventi. \<EventSource > deve essere un nome di dominio completo, un nome NetBIOS o un indirizzo IP.|
|{qc \| quick-config} [/q:[\<Quiet>]]|Configura il servizio Raccolta eventi Windows per garantire una sottoscrizione può essere creata e mantenuta anche dopo il riavvio. Questo include i passaggi seguenti:</br>1.  Attivare il canale ForwardedEvents se è disabilitato.</br>2.  Impostare il servizio Raccolta eventi Windows per ritardare l'avvio.</br>3.  Se non è in esecuzione, avviare il servizio Raccolta eventi Windows.|

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|------|-----------|
|/f:\<formato >|Specifica il formato delle informazioni visualizzate. \<Formato > può essere XML o XML. Se <Format> è XML, viene visualizzato l'output in formato XML. Se \<formato > è XML, l'output viene visualizzato in coppie nome-valore. Il valore predefinito è XML.|
|/c:\<Configfile>|Specifica il percorso del file XML che contiene una configurazione della sottoscrizione. Il percorso può essere assoluto o relativo alla directory corrente. Questa opzione può essere utilizzata solo con il **/cun** e **/cup** Opzioni e si escludono a vicenda con tutte le altre opzioni.|
|/e:[\<Subenabled>]|Abilita o disabilita una sottoscrizione. \<Subenabled > può essere true o false. Il valore predefinito di questa opzione è true.|
|/ESA:\<indirizzo >|Specifica l'indirizzo di un'origine evento. \<Indirizzo > è una stringa che contiene un nome di dominio completo, un nome NetBIOS o un indirizzo IP, che identifica un computer che funge da un'origine di eventi. Questa opzione deve essere utilizzata con il **/ese**, **/aes**, **/res**, o **/un** e **/up** Opzioni.|
|/ese:[\<Srcenabled>]|Abilita o disabilita un'origine evento. \<Srcenabled > può essere true o false. Questa opzione è consentita solo se il **/esa** opzione specificata. Il valore predefinito di questa opzione è true.|
|/AES|Aggiunge l'origine evento specificata dal **/esa** opzione se non è già parte della sottoscrizione. Se l'indirizzo specificato per il **/esa** opzione fa già parte della sottoscrizione, viene restituito un errore. Questa opzione è consentita solo se il **/esa** opzione specificata.|
|/res|Rimuove l'origine evento specificata dal **/esa** opzione se è già parte della sottoscrizione. Se l'indirizzo specificato per il **/esa** opzione non è una parte della sottoscrizione, viene restituito un errore. Questa opzione è consentita solo se **/esa** opzione specificata.|
|/un:\<Username >|Specifica le credenziali utente da utilizzare con l'origine evento specificata tramite il **/esa** (opzione). Questa opzione è consentita solo se il **/esa** opzione specificata.|
|/up:\<Password>|Specifica la password che corrisponde alle credenziali utente. Questa opzione è consentita solo se il **/un** opzione specificata.|
|/d:\<Desc>|Fornisce una descrizione per la sottoscrizione.|
|/uri:\<Uri>|Specifica il tipo di eventi che vengono utilizzate dalla sottoscrizione. \<URI > contiene una stringa URI che viene combinata con l'indirizzo del computer di origine evento per identificare in modo univoco l'origine degli eventi. La stringa URI è utilizzata per tutti gli indirizzi di origine evento nella sottoscrizione.|
|/cm:\<Configmode >|Imposta la modalità di configurazione. \<Configmode > può essere uno delle seguenti stringhe: Normale, personalizzato, MinLatency o MinBandwidth. Le modalità normale, MinLatency e MinBandwidth impostare la modalità di recapito, elementi max di recapito, l'intervallo di heartbeat e tempo di latenza massima di recapito. Il **/dm**, **/dmi**, **/hi** o **/dmlt** le opzioni possono essere solo se la modalità di configurazione è impostato su Custom.|
|/ex:\<Expires>|Imposta l'ora di scadenza della sottoscrizione. \<Scadenza > deve essere definita in formato di data e ora standard XML o ISO8601: aaaa-MM-ggTHH [. SSS] [Z], dove T è il separatore dell'ora e Z indica l'ora UTC.|
|/q:\<Query>|Specifica la stringa di query per la sottoscrizione. Il formato del \<Query > possono essere diverse per diversi valori URI e si applica a tutte le origini nella sottoscrizione.|
|/dia:\<Dialect>|Definisce il sottolinguaggio che usa la stringa di query.|
|/Tn:\<Transportname >|Specifica il nome del trasporto utilizzato per connettersi a un'origine eventi remoto.|
|/TP:\<Transportport >|Imposta il numero di porta utilizzato dal trasporto per la connessione a un'origine eventi remoto.|
|/DM:\<Deliverymode >|Specifica la modalità di recapito. \<DeliveryMode > può essere pull o push. Questa opzione è valida solo se il **/cm** opzione è impostata su Custom.|
|/dmi:\<Deliverymax>|Imposta il numero massimo di elementi per il recapito in batch. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/dmlt:\<Deliverytime >|Imposta la latenza massima nella fornitura di un batch di eventi. \<Deliverytime > è il numero di millisecondi. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/ salve:\<Heartbeat >|Definisce l'intervallo di heartbeat. \<Heartbeat > è il numero di millisecondi. Questa opzione è valida solo se **/cm** è impostato su Custom.|
|/CF:\<content >|Specifica il formato degli eventi che vengono restituiti. \<Contenuto > può essere eventi o informazioni sulla. Quando il valore è informazioni sulla, gli eventi vengono restituiti con le stringhe localizzate (ad esempio, la descrizione dell'evento) associate all'evento. Il valore predefinito è informazioni sulla.|
|/l:\<Locale>|Specifica le impostazioni locali per il recapito delle stringhe localizzate nel formato di informazioni sulla. \<Impostazioni locali > è un identificatore di lingua e paese/area geografica, ad esempio, "EN-us". Questa opzione è valida solo se il **/cf** opzione è impostata su informazioni sulla.|
|/ree:[\<Readexist>]|Identifica gli eventi che vengono distribuiti per la sottoscrizione. \<Readexist > è true o false. Quando il <Readexist> è true, tutti gli eventi esistenti vengono lette da origini di eventi di sottoscrizione. Quando il <Readexist> è false, vengono inviati solo eventi (in entrata) future. Il valore predefinito è true per un **/ree** opzione senza un valore. Se non **/ree** opzione è specificata, il valore predefinito è false.|
|/LF:\<FileLog >|Specifica il registro eventi locale utilizzato per archiviare gli eventi ricevuti dalle origini eventi.|
|/pn:\<Publishername >|Specifica il nome dell'autore. Deve essere un server di pubblicazione che possiede o Importa log specificato dal parametro di **/lf** (opzione).|
|/essp:\<Enableport>|Specifica che il numero di porta deve essere aggiunto al nome dell'entità servizio del servizio remoto. \<Enableport > può essere true o false. Il numero di porta viene aggiunto quando <Enableport> è true. Quando viene aggiunto il numero di porta, alcune operazioni di configurazione potrebbe essere necessario impedire l'accesso alle origini eventi da negata.|
|/HN:\<Hostname >|Specifica il nome DNS del computer locale. Questo nome viene utilizzato dall'origine di eventi remoti per eseguire il push degli eventi e deve essere utilizzato solo per una sottoscrizione push.|
|/ct:\<tipo >|Imposta il tipo di credenziali per l'accesso remoto di origine. \<Tipo > deve essere uno dei seguenti valori: predefinito, negotiate, digest, basic o localmachine. Il valore predefinito è l'impostazione predefinita.|
|/cun:\<Comusername >|Imposta le credenziali utente condivise da utilizzare per le origini evento che non hanno le proprie credenziali utente. Se questa opzione viene specificata con il **/c** UserName e UserPassword le impostazioni delle opzioni per le origini eventi singoli dal file di configurazione vengono ignorate. Se si desidera utilizzare credenziali diverse per un'origine evento specifico, è necessario eseguire l'override di questo valore specificando la **/un** e **/up** Opzioni per un'origine evento specifico nella riga di comando di un altro **ss** comando.|
|/cup:\<Compassword>|Imposta la password dell'utente per le credenziali utente condivise. Quando \<Compassword > è impostato su * (asterisco), la password viene letto dalla console. Questa opzione è valida solo quando il **/cun** opzione specificata.|
|/q:[\<Quiet>]|Specifica se la procedura di configurazione richiede una conferma. \<Quiet > può essere true o false. Se <Quiet> è true, la procedura di configurazione non viene visualizzata una richiesta di conferma. Il valore predefinito di questa opzione è false.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Se si riceve il messaggio, "server RPC non disponibile? Quando si tenta di eseguire wecutil, è necessario avviare il servizio di raccolta eventi Windows (wecsvc). Per avviare wecsvc, a un prompt dei comandi con privilegi elevati, digitare net start wecsvc.

-   Nell'esempio seguente viene illustrato il contenuto di un file di configurazione:  
    ```
    <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
    <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
    <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
    <ConfigurationMode>Normal</ConfigurationMode>
    <Description>Forward Sample Subscription</Description>
    <SubscriptionId>SampleSubscription</SubscriptionId>
    <Query><![CDATA[
    <QueryList>
    <Query Path="Application">
    <Select>*</Select>
    </Query>
    </QueryList>
    ]]></Query>
    <EventSources>
    <EventSource Enabled="true">
    <Address>mySource.myDomain.com</Address>
    <UserName>myUserName</UserName>
    <Password>*</Password>
    </EventSource>
    </EventSources>
    <CredentialsType>Default</CredentialsType>
    <Locale Language="EN-US"></Locale>
    </Subscription>
    ```

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
