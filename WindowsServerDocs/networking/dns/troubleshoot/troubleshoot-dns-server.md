---
title: Risoluzione dei problemi relativi ai server DNS
description: Questo articolo illustra come risolvere i problemi relativi a DNS dal lato server.
manager: dcscontentpm
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 23e51adafa5ab6da0a9317a1b0fad88bd3901073
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265803"
---
# <a name="troubleshooting-dns-servers"></a>Risoluzione dei problemi relativi ai server DNS

Questo articolo illustra come risolvere i problemi relativi ai server DNS.

## <a name="check-ip-configuration"></a>Controllare la configurazione IP

1. Eseguire `ipconfig /all` al prompt dei comandi e verificare l'indirizzo IP, subnet mask e il gateway predefinito.

2. Controllare se il server DNS è autorevole per il nome che viene cercato. In tal caso, vedere [Verifica dei problemi relativi ai dati autorevoli](#checking-for-problems-with-authoritative-data).

3. Eseguire il comando seguente:

   ```cmd
   nslookup <name> <IP address of the DNS server>
   ```
   Ad esempio: 
   ```cmd
   nslookup app1 10.0.0.1
   ```
   Se viene visualizzato un errore o una risposta di timeout, vedere [Verifica dei problemi di ricorsione](#checking-for-recursion-problems).

4. Svuotare la cache del resolver. A tale scopo, eseguire il comando seguente in una finestra del prompt dei comandi di amministrazione:

   ```cmd
   dnscmd /clearcache
   ```
   In alternativa, in una finestra di PowerShell amministrativa eseguire il cmdlet seguente:
   ```powershell
   Clear-DnsServerCache
   ```

5. Ripetere il passaggio 3. 

## <a name="check-dns-server-problems"></a>Controllare i problemi del server DNS

### <a name="event-log"></a>Registro eventi

Controllare i log seguenti per verificare se sono presenti errori registrati:

- Applicazione

- Sistema

- Server DNS

### <a name="test-by-using-nslookup-query"></a>Testare usando la query nslookup

Eseguire il comando seguente e verificare se il server DNS è raggiungibile dai computer client.

```cmd
nslookup <client name> <server IP address>
```

- Se il resolver restituisce l'indirizzo IP del client, il server non presenta alcun problema.

- Se il resolver restituisce una risposta "errore del server" o "Query rifiutata", la zona è probabilmente sospesa o il server è probabilmente sottoposta a overload. È possibile sapere se è sospeso controllando la scheda generale delle proprietà della zona nella console DNS.

Se il resolver restituisce una risposta "richiesta al server scaduta" o "nessuna risposta dal server", probabilmente il servizio DNS non è in esecuzione. Provare a riavviare il servizio server DNS immettendo quanto segue al prompt dei comandi nel server:

```cmd
net start DNS
```

Se il problema si verifica quando il servizio è in esecuzione, il server potrebbe non essere in ascolto sull'indirizzo IP usato nella query nslookup. Nella scheda **interfacce** della pagina delle proprietà del server nella console DNS gli amministratori possono limitare l'ascolto dei soli indirizzi selezionati da parte di un server DNS. Se il server DNS è stato configurato per limitare il servizio a un elenco specifico di indirizzi IP configurati, è possibile che l'indirizzo IP usato per contattare il server DNS non sia presente nell'elenco. È possibile provare con un indirizzo IP diverso nell'elenco o aggiungere l'indirizzo IP all'elenco.

In rari casi, è possibile che il server DNS disponga di una configurazione avanzata per la sicurezza o il firewall. Se il server si trova in un'altra rete raggiungibile solo tramite un host intermedio, ad esempio un router di filtro pacchetti o un server proxy, il server DNS può utilizzare una porta non standard per l'ascolto e la ricezione delle richieste client. Per impostazione predefinita, nslookup Invia le query ai server DNS sulla porta UDP 53. Pertanto, se il server DNS utilizza qualsiasi altra porta, le query NSLOOKUP avranno esito negativo. Se si ritiene che questo potrebbe essere il problema, controllare se un filtro intermedio viene intenzionalmente usato per bloccare il traffico sulle porte DNS note. In caso contrario, provare a modificare i filtri pacchetti o le regole di porta nel firewall per consentire il traffico sulla porta UDP/TCP 53.

## <a name="checking-for-problems-with-authoritative-data"></a>Verifica della presenza di problemi con dati autorevoli

Controllare se il server che restituisce la risposta non corretta è un server primario per la zona (il server primario standard per la zona o un server che usa l'integrazione Active Directory per caricare la zona) o un server che ospita una copia secondaria della zona. 

### <a name="if-the-server-is-a-primary-server"></a>Se il server è un server primario

Il problema potrebbe essere causato da un errore dell'utente quando gli utenti immettono dati nell'area. In alternativa, potrebbe essere causato da un problema che influisca Active Directory replica o aggiornamento dinamico.

### <a name="if-the-server-is-hosting-a-secondary-copy-of-the-zone"></a>Se il server ospita una copia secondaria della zona

1. Esaminare la zona nel server master (il server da cui il server esegue il pull dei trasferimenti di zona).

   > [!NOTE]
   >Per determinare quale server è il server master, è possibile esaminare le proprietà della zona secondaria nella console DNS.

   Se il nome non è corretto nel server master, andare al passaggio 4.

2. Se il nome è corretto nel server master, verificare se il numero di serie del server master è minore o uguale al numero di serie nel server secondario. In tal caso, modificare il server master o il server secondario in modo che il numero di serie nel server master sia maggiore del numero di serie sul server secondario. 
  
3. Nel server secondario forzare un trasferimento di zona all'interno della console DNS oppure eseguendo il comando seguente:
  
   ```cmd
   dnscmd /zonerefresh <zone name>
   ```
  
   Se, ad esempio, la zona è corp.contoso.com, immettere: `dnscmd /zonerefresh corp.contoso.com`.
  
4. Esaminare di nuovo il server secondario per verificare se la zona è stata trasferita correttamente. In caso contrario, è probabile che si verifichi un problema di trasferimento di zona. Per altre informazioni, vedere [problemi di trasferimento di zona](#zone-transfer-problems).

5. Se la zona è stata trasferita correttamente, controllare se i dati sono ora corretti. In caso contrario, i dati non sono corretti nella zona primaria. Il problema potrebbe essere causato da un errore dell'utente quando gli utenti immettono dati nell'area. In alternativa, potrebbe essere causato da un problema che influisca Active Directory replica o aggiornamento dinamico.

## <a name="checking-for-recursion-problems"></a>Verifica dei problemi di ricorsione

Per il corretto funzionamento della ricorsione, tutti i server DNS utilizzati nel percorso di una query ricorsiva devono essere in grado di rispondere e trasmettere i dati corretti. In caso contrario, una query ricorsiva può avere esito negativo per uno dei motivi seguenti:

- Si verifica il timeout della query prima che possa essere completata.

- Un server usato durante la query non risponde.

- Un server utilizzato durante la query fornisce dati non corretti.

Avviare la risoluzione dei problemi nel server usato nella query originale. Verificare che il server inoltri le query a un altro server esaminando la scheda server d' **inoltri** nelle proprietà del server nella console DNS. Se è selezionata la casella di controllo Abilita server d' **avanzamento** e sono elencati uno o più server, il server esegue l'inoltri delle query.

Se il server esegue l'invio di query a un altro server, verificare la presenza di problemi che interessano il server a cui il server esegue l'esecuzione delle query. Per verificare la presenza di problemi, vedere [controllare i problemi del server DNS](#check-dns-server-problems). Quando tale sezione indica di eseguire un'attività sul client, eseguirla sul server.

Se il server è integro e in grado di eseguire le query, ripetere questo passaggio ed esaminare il server a cui questo server invia le query.

Se il server non invia query a un altro server, verificare se il server è in grado di eseguire query su un server radice. A tale scopo, esegui il comando seguente:

```cmd
nslookup
server <IP address of server being examined>
set q=NS
 ```

- Se il resolver restituisce l'indirizzo IP di un server radice, probabilmente si disporrà di una delega interruppe tra il server radice e il nome o l'indirizzo IP che si sta tentando di risolvere. Per determinare la posizione in cui si dispone di una delega interruppe, attenersi alla procedura [test a Broken Delegation](#test-a-broken-delegation) .

- Se il resolver restituisce una risposta "timeout della richiesta al server", controllare se gli hint radice puntano ai server radice funzionanti. A tale scopo, utilizzare [per visualizzare la procedura dei parametri radice corrente](#to-view-the-current-root-hints) . Se i parametri radice fanno riferimento ai server radice funzionanti, potrebbe essersi verificato un problema di rete oppure il server potrebbe usare una configurazione avanzata del firewall che impedisce al resolver di eseguire query sul server, come descritto nella sezione [controllare i problemi del server DNS](#check-dns-server-problems) . È anche possibile che il valore predefinito di timeout ricorsivo sia troppo breve.

### <a name="test-a-broken-delegation"></a>Testare una delega interruppe

Avviare i test nella procedura seguente eseguendo una query su un server radice valido. Il test esegue un processo di esecuzione di query su tutti i server DNS dalla radice al server che si sta testando per una delega interrotta.

1. Al prompt dei comandi nel server che si sta testando, immettere quanto segue:

   ```cmd
   nslookup
   server <server IP address>
   set norecursion
   set querytype= <resource record type>
   <FQDN>
   ```
   > [!NOTE]
   >Il tipo di record di risorse è il tipo di record di risorsa per cui si sta eseguendo una query nella query originale e FQDN è il nome di dominio completo per cui si sta eseguendo una query (terminato da un punto).
 
2. Se la risposta include un elenco di record di risorse "NS" e "A" per i server delegati, ripetere il passaggio 1 per ogni server e usare l'indirizzo IP dei record di risorse "A" come indirizzo IP del server.

   - Se la risposta non contiene un record di risorse "NS", si disporrà di una delega interruppe.
   
   - Se la risposta contiene record di risorse "NS", ma nessun record di risorse "A", immettere la **ricorsione set**ed eseguire una query singolarmente per i record di risorse "a" dei server elencati nei record "NS". Se non si trova almeno un indirizzo IP valido di un record di risorse "A" per ogni record di risorse NS in una zona, si disporrà di una delega interruppe.

3. Se si dispone di una delega interruppe, correggerla aggiungendo o aggiornando un record di risorse "A" nella zona padre utilizzando un indirizzo IP valido per un server DNS corretto per la zona delegata.

### <a name="to-view-the-current-root-hints"></a>Per visualizzare i parametri radice correnti

1. Avviare la console DNS.

2. Aggiungere o connettersi al server DNS che non ha superato una query ricorsiva.

3. Fare clic con il pulsante destro del mouse sul server e scegliere **Proprietà**.

4. Fare clic su parametri radice.

Verificare la connettività di base ai server radice.

- Se i parametri radice sembrano essere configurati correttamente, verificare che il server DNS usato in una risoluzione dei nomi non riuscita possa effettuare il ping dei server radice in base all'indirizzo IP.

- Se i server radice non rispondono al ping in base all'indirizzo IP, è possibile che gli indirizzi IP dei server radice siano stati modificati. Tuttavia, non è raro vedere una riconfigurazione dei server radice.

## <a name="zone-transfer-problems"></a>Problemi di trasferimento di zona

Eseguire i controlli seguenti:

- Controllare Visualizzatore eventi sia per il server DNS primario sia per quello secondario.

- Controllare il server master per verificare se sta rifiutando di inviare il trasferimento per la sicurezza. 

- Controllare la scheda **trasferimenti di zona** delle proprietà della zona nella console DNS. Se il server limita i trasferimenti di zona a un elenco di server, ad esempio quelli elencati nella scheda **Server dei nomi** delle proprietà della zona, verificare che il server secondario sia presente nell'elenco. Verificare che il server sia configurato per l'invio di trasferimenti di zona.

- Verificare la presenza di problemi nel server master attenendosi alla procedura descritta nella sezione [verificare i problemi del server DNS](#check-dns-server-problems) . Quando viene richiesto di eseguire un'attività sul client, eseguire l'attività sul server secondario.

- Controllare se nel server secondario è in esecuzione un'altra implementazione del server DNS, ad esempio BIND. In caso contrario, il problema potrebbe avere una delle cause seguenti:

  - Il server master Windows può essere configurato per l'invio di trasferimenti di zona veloci, ma il server secondario di terze parti potrebbe non supportare i trasferimenti di zona rapida. In tal caso, disabilitare i trasferimenti di zona veloce sul server master dall'interno della console DNS selezionando la casella di controllo **Abilita binding secondari** nella scheda **Avanzate** delle proprietà del server.

  - Se una zona di ricerca diretta in Windows Server contiene un tipo di record, ad esempio un record SRV, che il server secondario non supporta, il server secondario potrebbe avere problemi di pull della zona.

Controllare se nel server master è in esecuzione un'altra implementazione del server DNS, ad esempio BIND. In tal caso, è possibile che la zona nel server master includa record di risorse incompatibili non riconosciuti da Windows.
 
Se nel server master o secondario è in esecuzione un'altra implementazione del server DNS, controllare entrambi i server per assicurarsi che supportino le stesse funzionalità. È possibile controllare Windows Server nella console DNS nella scheda **Avanzate** della pagina delle proprietà per il server. Oltre alla casella Abilita binding secondari, questa pagina include l'elenco a discesa **controllo nomi** . In questo modo è possibile selezionare l'applicazione della conformità RFC rigorosa per i caratteri nei nomi DNS.

