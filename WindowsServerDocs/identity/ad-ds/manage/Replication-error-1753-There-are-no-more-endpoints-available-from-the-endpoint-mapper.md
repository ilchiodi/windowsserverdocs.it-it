---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2e63d177abd0a6880c1825b821d265c8fa233a22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823164"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint

>Si applica a: Windows Server

Questo articolo descrive i sintomi, le cause e i passaggi di risoluzione per Active Directory operazioni che hanno esito negativo con errore Win32 1753: "non sono disponibili altri endpoint dal mapper degli endpoint".

DCDIAG segnala che il test di connettività, il test delle repliche Active Directory o il test KnowsOfRoleHolders non è riuscito con l'errore 1753: "non sono disponibili altri endpoint dal mapper degli endpoint".

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN. EXE segnala che il tentativo di replica non è riuscito con lo stato 1753.
I comandi REPADMIN che comunemente citano lo stato 1753 includono ma non sono limitati a:

* REPADMIN/REPLSUM.
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN /SYNCALL

L'output di esempio di "REPADMIN/SHOWREPS" che illustra la replica in ingresso da CONTOSO-DC2 a CONTOSO-DC1 ha esito negativo con l'errore "accesso alla replica negato" è riportato di seguito:

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

Il comando **Controlla topologia replica** in Active Directory Sites and Services restituisce "non sono disponibili altri endpoint dal mapper degli endpoint".

Facendo clic con il pulsante destro del mouse sull'oggetto connessione da un controller di dominio di origine e scegliendo **Controlla la topologia di replica** ha esito negativo con "non sono disponibili altri endpoint dal mapper degli endpoint". Di seguito è riportato il messaggio di errore su schermo:

Testo del titolo della finestra di dialogo: controllare la topologia di replica testo del messaggio: si è verificato il seguente errore durante il tentativo di contattare il controller di dominio: non sono disponibili altri endpoint dal mapper di endpoint.

Il comando **Replicate Now** in Active Directory Sites and Services restituisce "non sono disponibili ulteriori endpoint dal mapper degli endpoint".
Facendo clic con il pulsante destro del mouse sull'oggetto connessione da un controller di dominio di origine e scegliendo **Replica ora** si verifica un errore con "non sono disponibili altri endpoint dal mapper degli endpoint".
Di seguito è riportato il messaggio di errore su schermo:

Testo del titolo della finestra di dialogo: Replica ora testo del messaggio di dialogo: si è verificato l'errore seguente durante il tentativo di sincronizzare il contesto dei nomi \<% nome partizione di directory% > dal controller di dominio \<controller di dominio di origine > al controller di dominio \<DC di destinazione >:

Nessun endpoint disponibile nel mapper degli endpoint.
L'operazione non continuerà

Gli eventi NTDS KCC, NTDS General o Microsoft-Windows-ActiveDirectory_DomainService con lo stato-2146893022 vengono registrati nel log dei servizi directory Visualizzatore eventi.

Active Directory eventi che comunemente citano lo stato-2146893022 includono ma non sono limitati a:

| ID evento | Origine evento | Stringa dell'evento|
| --- | --- | --- |
| 1655 | NTDS generale | Active Directory ha tentato di comunicare con il catalogo globale seguente e i tentativi hanno esito negativo. |
| 1925 | NTDS KCC | Non è riuscito il tentativo di stabilire un collegamento di replica per la partizione di directory scrivibile. |
| 1265 | NTDS KCC | Un tentativo del controllo di coerenza informazioni (KCC) per l'aggiunta di un contratto di replica per la partizione di directory e il controller di dominio di origine seguenti non è riuscito. |

## <a name="cause"></a>Causa

Nei passaggi seguenti viene illustrato il flusso di lavoro RPC che inizia con la registrazione dell'applicazione server con il mapping dell'endpoint RPC (EPM) nel passaggio 1 al passaggio dei dati dal client RPC all'applicazione client nel passaggio 7.

### <a name="adds-rpc-workflow"></a>AGGIUNGE un flusso di lavoro RPC

1. L'app Server registra gli endpoint con l'agente mapping endpoint RPC (EPM)
1. Il client esegue una chiamata RPC (per conto di un utente, un sistema operativo o un'operazione avviata dall'applicazione)
1. RPC lato client contatta i computer di destinazione EPM e chiede all'endpoint di completare la chiamata client
1. EPM del computer server risponde con un endpoint
1. Il lato client RPC contatta l'app Server
1. L'app Server esegue la chiamata, restituisce il risultato alla RPC client
1. RPC sul lato client restituisce il risultato all'app client

L'errore 1753 viene generato da un errore tra i passaggi #3 e #4. In particolare, l'errore 1753 indica che il client RPC (controller di dominio di destinazione) è stato in grado di contattare il server RPC (controller di dominio di origine) sulla porta 135, ma la versione EPM sul server RPC (DC di origine) non è stata in grado di individuare l'applicazione RPC di interesse e restituito l'errore sul lato server 1753. La presenza dell'errore 1753 indica che il client RPC (DC di destinazione) ha ricevuto la risposta di errore sul lato server dal server RPC (DC Replication source controller) in rete.

Le cause principali specifiche per l'errore 1753 includono:

* L'app Server non è mai stata avviata, ad esempio il passaggio #1 nel diagramma "ulteriori informazioni" non è mai stato tentato.
* L'app Server è stata avviata ma si è verificato un errore durante l'inizializzazione che ha impedito la registrazione con il mapper di endpoint RPC (ad esempio, il passaggio #1 nel diagramma "altre informazioni" precedente è stato tentato ma non è riuscito).
* L'app Server è stata avviata ma successivamente è stata arrestata. (ad esempio, il passaggio #1 nel diagramma "altre informazioni" precedente è stato completato correttamente, ma è stato annullato in un secondo momento perché il server è morto).
* L'app Server ha annullato la registrazione manuale degli endpoint (simile a 3 ma intenzionale. Probabilmente, ma incluso per completezza.
* Il client RPC (controller di dominio di destinazione) ha contattato un server RPC diverso da quello previsto a causa di un errore di mapping tra nome e IP nel file DNS, WINS o host/LMHOSTS.

L'errore 1753 non è causato da:

* Mancanza di connettività di rete tra il client RPC (DC di destinazione) e il server RPC (controller di dominio di origine) sulla porta 135
* Mancanza di connettività di rete tra il server RPC (controller di dominio di origine) utilizzando la porta 135 e il client RPC (DC di destinazione) sulla porta temporanea.
* Mancata corrispondenza della password o dell'impossibilità da parte del controller di dominio di origine di decrittografare un pacchetto Kerberos crittografato

## <a name="resolutions"></a>Soluzioni

Verificare che il servizio che registra il servizio con il mapper di endpoint sia stato avviato

* Per i controller di dominio Windows 2000 e Windows Server 2003: assicurarsi che il controller di dominio di origine venga avviato in modalità normale.
* Per Windows Server 2008 o Windows Server 2008 R2: dalla console del controller di dominio di origine, avviare Gestione servizi (Services. msc) e verificare che il servizio Active Directory Domain Services sia in esecuzione.

Verificare che il client RPC (controller di dominio di destinazione) sia connesso al server RPC previsto (DC di origine)

Tutti i controller di dominio in una foresta Active Directory comune registrano un record CNAME del controller di dominio nel _msdcs. \<dominio radice della foresta > zona DNS indipendentemente dal dominio in cui risiedono all'interno della foresta. Il record CNAME DC deriva dall'attributo objectGUID dell'oggetto Impostazioni NTDS per ogni controller di dominio.

Quando si eseguono operazioni basate sulla replica, un controller di dominio di destinazione esegue una query DNS per il record CNAME del controller di dominio di origine Il record CNAME contiene il nome del computer completo del controller di dominio di origine che viene usato per derivare l'indirizzo IP del controller di dominio di origine tramite la ricerca della cache del client DNS, la ricerca di file host/LMHOSTS, l'host A/AAAA record in DNS o WINS.

Gli oggetti Impostazioni NTDS non aggiornati e i mapping da nome a indirizzo IP errato nei file DNS, WINS, host e LMHOSTS possono causare la connessione del client RPC (DC di destinazione) al server RPC errato (DC di origine). Inoltre, il mapping da nome a indirizzo IP non valido può causare la connessione del client RPC (DC di destinazione) a un computer che non dispone neanche dell'applicazione server RPC di interesse (in questo caso il ruolo Active Directory). (Esempio: un record host non aggiornato per DC2 contiene l'indirizzo IP di DC3 o un computer membro).

Verificare che il objectGUID per il controller di dominio di origine presente nella copia del controller di dominio di destinazione di Active Directory corrisponda al objectGUID del controller di dominio di origine archiviato nella copia di Active Directory del controller di dominio di origine. Se è presente una discrepanza, usare repadmin/showobjmeta nell'oggetto Impostazioni NTDS per vedere quale corrisponde all'ultima promozione del controller di dominio di origine (Suggerimento: confrontare timbri data per l'oggetto Impostazioni NTDS creare data da/showobjmeta rispetto alla data dell'ultima promozione nel file Dcpromo. log del controller di dominio di origine. Potrebbe essere necessario utilizzare la data dell'Ultima modifica/creazione di DCPROMO. File di LOG. Se i GUID oggetto non sono identici, è probabile che il controller di dominio di destinazione disponga di un oggetto Impostazioni NTDS non aggiornato per il controller di dominio di origine il cui record CNAME faccia riferimento a un record host con un nome non valido al mapping IP.

Nel controller di dominio di destinazione eseguire IPCONFIG per individuare i server DNS utilizzati dal controller di dominio di destinazione per la risoluzione dei nomi:

```
c:>ipconfig /all
```

Nel controller di dominio di destinazione eseguire NSLOOKUP sul record DC CNAME completo del controller di dominio di origine:

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

Verificare che l'indirizzo IP restituito da NSLOOKUP "appartenga" al nome host/identità di sicurezza del controller di dominio di origine:

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

Accedere alla console del controller di dominio di origine, eseguire "IPCONFIG" dal prompt dei comandi e verificare che il controller di dominio di origine sia proprietario dell'indirizzo IP restituito dal comando NSLOOKUP precedente

Verificare l'host non aggiornato/duplicato per i mapping IP in DNS

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

Se nei record host sono presenti indirizzi IP non validi, verificare se lo scavenging DNS è abilitato e configurato correttamente.

Se i test sopra o una traccia di rete non visualizzano una query nome che restituisce un indirizzo IP non valido, prendere in considerazione le voci non aggiornate nei file HOST, i file LMHOSTS e i server WINS. Si noti che i server DNS possono anche essere configurati per eseguire la risoluzione dei nomi di fallback WINS.

* Verificare che l'applicazione server (Active Directory et al) sia stata registrata con l'agente di mapping degli endpoint sul server RPC (DC di origine)
* Active Directory utilizza una combinazione di porte note e registrate in modo dinamico. Questa tabella elenca le porte e i protocolli noti utilizzati dai controller di dominio Active Directory.

| Applicazione server RPC | Porta | TCP | UDP |
| --- | --- | --- | --- |
| Server DNS | 53 | X | X |
| Kerberos | 88 | X | X |
| Server LDAP | 389 | X | X |
| Microsoft-DS | 445 | X | X |
| SSL LDAP | 636 | X | X |
| Server di catalogo globale | 3268 | X |   |
| Server di catalogo globale | 3269 | X |   |

Le porte note non sono registrate con il mapper di endpoint.

Active Directory e altre applicazioni registrano anche servizi che ricevono porte assegnate in modo dinamico nell'intervallo di porte temporanee RPC. Tali applicazioni server RPC sono porte TCP assegnate in modo dinamico tra 1024 e 5000 in Windows 2000 e Windows Server 2003 computer e porte tra l'intervallo 49152 e 65535 nei computer Windows Server 2008 e Windows Server 2008 R2. La porta RPC utilizzata dalla replica può essere hardcoded nel registro di sistema tramite i passaggi descritti nell' [articolo 224196 della Knowledge](https://support.microsoft.com/kb/224196)base. Active Directory continua a eseguire la registrazione con l'EPM quando è configurato per l'uso di una porta hardcoded.

Verificare che l'applicazione server RPC di interesse sia stata registrata con il mapper di endpoint RPC nel server RPC (il controller di dominio di origine nel caso di replica di Active Directory).

Esistono diversi modi per eseguire questa attività, ma uno consiste nell'installare ed eseguire PORTQRY da un prompt dei comandi con privilegi amministrativi nella console del controller di dominio di origine usando la sintassi:

```
portquery -n <source DC> -e 135 > file.txt
```

Nell'output di PortQry prendere nota dei numeri di porta registrati dinamicamente da "MS NT Directory DRS Interface" (UUID = 351...) per il protocollo ncacn_ip_tcp. Il frammento di codice seguente mostra l'output di esempio di portquery da un controller di dominio Windows Server 2008 R2:

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

Altri possibili modi per risolvere l'errore:

* Verificare che il controller di dominio di origine venga avviato in modalità normale e che il ruolo del sistema operativo e del controller di dominio nel controller di dominio di origine sia stato avviato completamente.
* Verificare che il servizio Dominio di Active Directory sia in esecuzione. Se il servizio è attualmente arrestato o non è stato configurato con i valori di avvio predefiniti, reimpostare i valori di avvio predefiniti, riavviare il controller di dominio modificato, quindi ripetere l'operazione.
* Verificare che il valore di avvio e lo stato del servizio per il servizio RPC e il localizzatore RPC siano corretti per la versione del sistema operativo del client RPC (controller di dominio di destinazione) e del server RPC (DC di origine). Se il servizio è attualmente arrestato o non è stato configurato con i valori di avvio predefiniti, reimpostare i valori di avvio predefiniti, riavviare il controller di dominio modificato, quindi ripetere l'operazione.
   * Assicurarsi inoltre che il contesto del servizio corrisponda alle impostazioni predefinite elencate nella tabella seguente.

      | Service | Stato predefinito (tipo di avvio) in Windows Server 2003 e versioni successive | Stato predefinito (tipo di avvio) in Windows Server 2000 |
      | --- | --- | --- |
      | Chiamata a procedura remota (RPC) | Avviata (automatica) | Avviata (automatica) |
      | Localizzatore RPC (Remote Procedure Call) | Null o arrestato (manuale) | Avviata (automatica) |

* Verificare che le dimensioni dell'intervallo di porte dinamiche non siano state vincolate. Di seguito è riportata la sintassi NETSH per Windows Server 2008 e Windows Server 2008 R2 per enumerare l'intervallo di porte RPC:

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* Verificare che le definizioni di porta hardcoded definite in KB 224196 rientrino nell'intervallo di porte dinamiche per la versione del sistema operativo dei controller di dominio di origine. Esaminare l' [articolo 224196 della Knowledge](https://support.microsoft.com/kb/224196) base e verificare che la porta hardcoded rientri nell'intervallo di porte temporanee per la versione del sistema operativo del controller di dominio di origine.

* Verificare che la chiave protocolli esista in HKLM\Software\Microsoft\Rpc e che contenga i 5 valori predefiniti seguenti:

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>Altre informazioni

Esempio di un nome non valido al mapping IP che causa l'errore RPC 1753 e-2146893022: il nome dell'entità di destinazione non è corretto

Il dominio contoso.com è costituito da DC1 e DC2 con indirizzi IP x. x. 1.1 e x. x. 1.2. I record "A"/"AAAA" dell'host per DC2 sono registrati correttamente in tutti i server DNS configurati per DC1. Inoltre, il file HOSTs in DC1 contiene una voce mapping DC2s nome host completo all'indirizzo IP x. x. 1.2. Successivamente, l'indirizzo IP di DC2 passa da X. X. 1.2 a X. 1.3 e un nuovo computer membro viene aggiunto al dominio con indirizzo IP x. x. 1.2. I tentativi di replica di Active Directory attivati dal comando **Replicate Now** nello snap-in Active Directory siti e servizi non riescono con l'errore 1753, come illustrato nella traccia seguente:

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

Al frame **10**, il controller di dominio di destinazione esegue una query sul mapper di endpoint del controller di dominio di origine sulla porta 135 per la classe del servizio di replica Active Directory UUID E351...

Nel frame **11**, il controller di dominio di origine, in questo caso un computer membro che non ospita ancora il ruolo controller di dominio e pertanto non ha registrato E351... L'UUID per il servizio di replica con l'EPM locale risponde con un errore simbolico EP_S_NOT_REGISTERED che esegue il mapping all'errore decimale 1753, l'errore esadecimale 0x6d9 e l'errore descrittivo "non sono disponibili altri endpoint dal mapper di endpoint".

Successivamente, il computer membro con indirizzo IP x. x. 1.2 viene promosso come replica "MayberryDC" nel dominio contoso.com. Anche in questo caso, il comando **Replica ora** viene usato per attivare la replica, ma questa volta ha esito negativo con l'errore sullo schermo "il nome dell'entità di destinazione non è corretto". Il computer di cui è assegnata la scheda di rete l'indirizzo IP x. x. 1.2 è un controller di dominio, è attualmente avviato in modalità normale e ha registrato il E351... UUID del servizio di replica con l'EPM locale ma non è il proprietario del nome o dell'identità di sicurezza di DC2 e non è in grado di decrittografare la richiesta Kerberos da DC1, quindi la richiesta ha esito negativo con l'errore "il nome dell'entità di destinazione non è corretto". L'errore è mappato all'errore decimale-2146893022/0x80090322 errore esadecimale.

I mapping da host a IP non validi potrebbero essere causati da voci obsolete nei file host/LMHOSTS, da host a/AAAA registrazioni in DNS o WINS.

Riepilogo: questo esempio non è riuscito perché un mapping da host a IP non valido (nel file HOST in questo caso) ha causato la risoluzione del controller di dominio di destinazione in un controller di dominio di origine in cui non è stato eseguito il servizio Active Directory Domain Services (o anche installato), quindi il nome SPN della replica non è stato ancora registrato e il controller di dominio di origine ha restituito l'errore 1753 Nel secondo caso, un mapping da host a IP non valido (di nuovo nel file HOST) ha causato la connessione del controller di dominio di destinazione a un controller di dominio che ha registrato E351... nome SPN della replica ma tale origine aveva un nome host e un'identità di sicurezza diversi rispetto al controller di dominio di origine previsto, quindi i tentativi non sono riusciti con errore-2146893022: nome dell'entità di destinazione non corretto.

## <a name="related-topics"></a>Argomenti correlati

* [Risoluzione dei problemi di Active Directory operazioni che hanno esito negativo con errore 1753: non sono disponibili altri endpoint dal mapper degli endpoint.](https://support.microsoft.com/kb/2089874)
* [Articolo della Knowledge base 839880 risoluzione degli errori di mapping degli endpoint RPC mediante gli strumenti di supporto di Windows Server 2003 dal CD del prodotto](https://support.microsoft.com/kb/839880)
* [Articolo della Knowledge 832017 Panoramica dei servizi e requisiti per le porte di rete per Windows Server System](https://support.microsoft.com/kb/832017/)
* [Articolo della Knowledge base 224196 limitazione Active Directory il traffico di replica e il traffico RPC client a una porta specifica](https://support.microsoft.com/kb/224196/)
* [Articolo della Knowledge 154596 sulla configurazione dell'allocazione delle porte dinamiche RPC per l'uso con i firewall](https://support.microsoft.com/kb/154596)
* [Funzionamento di RPC](https://msdn.microsoft.com/library/aa373935(VS.85).aspx)
* [Preparazione del server per una connessione](https://msdn.microsoft.com/library/aa373938(VS.85).aspx)
* [Come il client stabilisce una connessione](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [Registrazione dell'interfaccia](https://msdn.microsoft.com/library/aa375357(VS.85).aspx)
* [Rendere disponibile il server in rete](https://msdn.microsoft.com/library/aa373974(VS.85).aspx)
* [Registrazione degli endpoint](https://msdn.microsoft.com/library/aa375255(VS.85).aspx)
* [Ascolto delle chiamate client](https://msdn.microsoft.com/library/aa373966(VS.85).aspx)
* [Come il client stabilisce una connessione](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [Limitazione del traffico di replica Active Directory e del traffico RPC del client a una porta specifica](https://support.microsoft.com/kb/224196)
* [SPN per un controller di dominio di destinazione in servizi di dominio Active Directory](https://msdn.microsoft.com/library/dd207688(PROT.13).aspx)
