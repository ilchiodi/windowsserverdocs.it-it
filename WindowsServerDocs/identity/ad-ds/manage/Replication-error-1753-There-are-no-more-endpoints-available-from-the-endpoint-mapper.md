---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e7d2b47c9c14af22cdcf29fb388779e7639e38cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442772"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>In questo argomento vengono illustrati i sintomi, cause e come risolvere l'operazione di Active Directory replica errore 8524 DSA il è in grado di procedere a causa di un errore di ricerca DNS.</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Sintomi</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">Cause</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">Soluzioni</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">Altre informazioni</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomi</title>
    <content>
      <para>Questo articolo descrive i sintomi, cause e la risoluzione dei passaggi per le operazioni di Active Directory che non soddisfano 1753 s chybou Win32: &quot;Non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot;</para>
      <list class="ordered">
        <listItem>
          <para>Report DCDIAG che il test della connettività, test di repliche di Active Directory o KnowsOfRoleHolders test non è riuscita con errore 1753: &quot;Non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot;</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN. File EXE segnala che il tentativo di replica non è riuscita con stato 1753.</para><para>REPADMIN comandi che in genere riportare lo stato 1753 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM.</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Esempio di output dal &quot;REPADMIN /SHOWREPS&quot; che descrivono la replica in ingresso da CONTOSO-DC2 per CONTOSO-DC1 ha esito negativo con il &quot;è stato negato l'accesso alla replica&quot; errore è illustrato di seguito:</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para>Il <ui>topologia di replica controllare</ui> comando in Active Directory Sites and Services restituisce &quot;non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot;</para><para>Facendo clic sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>topologia di replica controllare</ui> ha esito negativo con &quot;non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot; Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>Testo del titolo della finestra: Controllare la topologia di replica</para><para>Testo del messaggio finestra di dialogo: </para><para>Si è verificato l'errore seguente durante il tentativo di contattare il controller di dominio: Non esistono Nessun endpoint disponibile nel mapping degli endpoint.</para></listItem><listItem><para>Il <ui>Replicate now</ui> comando in Active Directory Sites and Services restituisce &quot;non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot;</para><para>Facendo clic sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>Replicate now</ui> ha esito negativo con &quot;non esistono Nessun endpoint disponibile nel mapping degli endpoint.&quot; Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>Testo del titolo della finestra: Replicare ora</para><para>Testo del messaggio finestra di dialogo: Si è verificato l'errore seguente durante il tentativo di sincronizzare il contesto di denominazione &lt;% nome directory partition&gt; dal Controller di dominio &lt;controller di dominio di origine&gt; al Controller di dominio &lt;controller di dominio di destinazione&gt;:</para><para>

Non esistono Nessun endpoint disponibile nel mapping degli endpoint.</para><para>Non sarà possibile continuare l'operazione</para></listItem><listItem><para>Gli eventi di KCC NTDS, NTDS generale o Microsoft-Windows-ActiveDirectory DomainService con lo stato-2146893022 vengono registrati nel Registro di servizi di Directory nel Visualizzatore eventi.</para><para>Eventi Active Directory che in genere riportare lo stato-2146893022 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID evento</para></TD><TD><para>Origine evento</para></TD><TD><para>Stringa dell'evento</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS generale</para></TD><TD><para>Active Directory ha tentato di comunicare con il catalogo globale seguente e i tentativi hanno esito negativo.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Non è riuscito il tentativo di stabilire un collegamento di replica per la partizione di directory scrivibile.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Un tentativo dal controllo di coerenza informazioni (KCC) per aggiungere un contratto di replica per le seguenti directory origine e partizione controller di dominio non riuscito.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Cause</title>
    <content>
      <para>Il diagramma seguente illustra il flusso di lavoro RPC inizia con la registrazione dell'applicazione server con RPC Endpoint Mapper (EPM) nel passaggio 1 per la trasmissione dei dati dal client RPC per l'applicazione client nel passaggio 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>Passaggi da 1 a 7 mappa per le operazioni seguenti:</para>
      <list class="ordered">
        <listItem>
          <para>App server registra i propri endpoint con il RPC Endpoint Mapper (EPM) </para>
        </listItem>
        <listItem>
          <para>Il client effettua una chiamata RPC (per conto di un utente, sistema operativo o applicazione iniziato l'operazione) </para>
        </listItem>
        <listItem>
          <para>Sul lato client RPC contatta i computer di destinazione EPM e chiedere l'endpoint completare la chiamata del client </para>
        </listItem>
        <listItem>
          <para>Computer server&#39;s EPM risponde con un endpoint </para>
        </listItem>
        <listItem>
          <para>Sul lato client RPC contatta l'app server </para>
        </listItem>
        <listItem>
          <para>App server viene eseguita la chiamata, restituisce il risultato al client RPC </para>
        </listItem>
        <listItem>
          <para>Sul lato client RPC passa il risultato all'App client</para>
        </listItem>
      </list>
      <para>Errore 1753 viene generato da un errore tra i passaggi da 3 # e #4. In particolare, errore 1753 significa che il client RPC (controller di dominio di destinazione) è stato in grado di contattare il Server RPC (controller di dominio di origine) sulla porta 135 ma EPM sul Server RPC (controller di dominio di origine) non è riuscito a individuare l'applicazione di RPC di interesse e ha restituito l'errore sul lato server 1753. La presenza dell'errore 1753 indica che il client RPC (controller di dominio di destinazione) ha ricevuto la risposta di errore sul lato server da Server RPC (origine replica controller di dominio di Active Directory) in rete. </para>
      <para>Le cause radice specifico per l'errore 1753 includono: </para>
      <list class="ordered">
        <listItem>
          <para>L'app server mai avviata (vale a dire passaggio n. 1 nel &quot;ulteriori informazioni&quot; diagramma che si trova sopra è stata mai tentata).</para>
        </listItem>
        <listItem>
          <para>Avvio dell'app server, ma si è verificato un errore durante l'inizializzazione che ne ha impedito la registrazione con l'agente mapping Endpoint RPC (vale a dire passaggio n. 1 nel &quot;ulteriori informazioni&quot; diagramma precedente non è riuscita).</para>
        </listItem>
        <listItem>
          <para>L'app server avviata ma successivamente è stata interrotta. (ad esempio & passaggio 1 nel &quot;ulteriori informazioni&quot; diagramma precedente è stata completata correttamente, ma è stata annullata in un secondo momento perché il server morte).</para>
        </listItem>
        <listItem>
          <para>L'app server annullata manualmente gli endpoint (simili a 3 ma intenzionale. Ma probabilmente non incluso per motivi di completezza.)</para>
        </listItem>
        <listItem>
          <para>Il client RPC (controller di dominio di destinazione) contattare un server RPC diverso da quello previsto a causa di un nome all'errore di mapping IP nel DNS, WINS o file Lmhosts/host.</para>
        </listItem>
      </list>
      <para>Errore 1753 non è causato da: </para>
      <list class="bullet">
        <listItem>
          <para>Una mancanza di connettività di rete tra il client RPC (controller di dominio di destinazione) e il Server RPC (controller di dominio di origine) sulla porta 135</para>
        </listItem>
        <listItem>
          <para>Una mancanza di connettività di rete tra il server RPC (controller di dominio di origine) usando la porta 135 e il client RPC (controller di dominio di destinazione) tramite le porte temporanee. </para>
        </listItem>
        <listItem>
          <para>Mancata corrispondenza di una password o l'impossibilità per il controller di dominio di origine per decrittografare un pacchetto crittografato Kerberos </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Soluzioni</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Verificare che il servizio registrazione il servizio con l'endpoint mapper sia stato avviato</embeddedLabel>
          </para>
          <para>Per Windows 2000 e i controller di dominio di Windows Server 2003: assicurarsi che il controller di dominio di origine è stato avviato in modalità normale. </para>
          <para>
Per Windows Server 2008 o Windows Server 2008 R2: dalla console di controller di dominio di origine, avviare Gestione servizi (Services. msc) e verificare che il <embeddedLabel>Active Directory Domain Services</embeddedLabel> servizio è in esecuzione. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Verificare che per client RPC (controller di dominio di destinazione) connessi al server RPC (controller di dominio di origine)</embeddedLabel>
          </para>
          <para>Tutti i controller di dominio in una foresta di Active Directory comune registrare un controller di dominio record CNAME nella zona msdcs. &lt;dominio radice della foresta&gt; zona DNS indipendentemente dal dominio cui si trovano all'interno della foresta. Il record CNAME di controller di dominio è derivato dal <embeddedLabel>objectGUID</embeddedLabel> attributo dell'oggetto impostazioni NTDS per ogni controller di dominio. </para>
          <para>Quando si eseguono operazioni basate su replica, un controller di dominio di destinazione esegue una query DNS per il record CNAME i controller di dominio di origine. Il record CNAME contiene il nome completo del computer del controller di dominio utilizzato per derivare l'indirizzo IP di controller di dominio di origine tramite la ricerca nella cache client DNS, ospitare / LMHost file ricerca, A host / record AAAA in DNS o WINS. </para>
          <para>Non aggiornati gli oggetti Impostazioni NTDS e mapping nome-per-IP non valido nei file di DNS, WINS, Host e LMHOST può causare il client RPC (controller di dominio di destinazione) per connettersi a un Server RPC non valido (controller di dominio di origine). Inoltre, il mapping nome-per-IP non valido potrebbe essere il client RPC (controller di dominio di destinazione) per connettersi a un computer che non dispone inoltre dell'applicazione Server RPC di interesse (il ruolo di Active Directory in questo caso) installato. (Esempio: un record host non aggiornato per DC2 contiene l'indirizzo IP del DC3 o in un computer membro). </para>
          <para>Verificare che l'attributo objectGUID per il controller di dominio di origine che esiste nella destinazione copia i controller di dominio di Active Directory corrisponda l'objectGUID di controller di dominio di origine archiviato nell'origine copia i controller di dominio di Active Directory. Se è presente una discrepanza, utilizzare repadmin. exe /showobjmeta sull'oggetto impostazioni ntds per visualizzare quello che corrisponde all'ultimo promozione del controller di dominio di origine (hint: confrontare i timbri data per l'oggetto impostazioni NTDS data di creazione da. exe /showobjmeta contro l'ultima data di innalzamento di livello di file di log Dcpromo. log i controller di dominio di origine. Potrebbe essere necessario usare l'ultima modifica / creazione data di DCPROMO. File di registro stesso). Se l'oggetto GUID non è identico, la destinazione controller di dominio che include un oggetto impostazioni NTDS non aggiornato per il controller di dominio di origine cui il record CNAME si riferisce a un record host con un nome non valido per il mapping IP. </para>
          <para>Nel controller di dominio di destinazione, eseguire IPCONFIG/ALL per determinare i server DNS che il controller di dominio utilizza per la risoluzione dei nomi di destinazione:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>Sul controller di dominio di destinazione, eseguire NSLOOKUP il record CNAME di controller di dominio completo di controller di dominio di origine:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>Verificare che l'indirizzo IP restituito dal NSLOOKUP &quot;possiede&quot; il nome host / identità di sicurezza del controller di dominio di origine:</para>
          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>oppure</para>
          <para>Accedere alla console del controller di dominio, eseguire l'origine &quot;IPCONFIG&quot; da CMD prompt e verificare che il controller di dominio di origine appartiene l'indirizzo IP restituito dal comando NSLOOKUP precedente</para>
          <para>Controllo per i mapping IP nel DNS dell'host non aggiornati / duplicati</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>In presenza di indirizzi IP non validi nei record di host, provare a utilizzare se lo scavenging DNS è abilitato e configurato correttamente. </para><para>Se i test precedenti o a una rete esegue la traccia&#39;visualizzare una query di nome che restituisca un indirizzo IP non valido, prendere in considerazione le voci non aggiornate nel file HOST, i file LMHOSTS e server WINS. Si noti che i server DNS può anche essere configurati per eseguire la risoluzione dei nomi fallback di WINS.</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Verificare che l'applicazione server (Active Directory et al) sia registrato con il mapping degli endpoint nel server RPC (controller di dominio di origine)</embeddedLabel>
          </para>
          <para>Active Directory viene usata una combinazione di porte note e registrate in modo dinamico. Questa tabella elenca i ben note porte e protocolli usati dai controller di dominio Active Directory.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Applicazione Server RPC</para>
                </TD>
                <TD>
                  <para>Port</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>Server DNS</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Server LDAP</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP SSL</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Server di catalogo globale</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Server di catalogo globale</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>x</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>Le porte non sono registrate con il mapper di endpoint. </para>
          <para>Active Directory e altre applicazioni registrare anche i servizi assegnate dinamicamente le porte di ricevano nell'intervallo di porte temporanee di RPC. Tali applicazioni server RPC vengono assegnate dinamicamente le porte TCP compreso tra 1024 e 5000 nei computer Windows 2000 e Windows Server 2003 e le porte tra 49152 e 65535 intervallo nei computer Windows Server 2008 e Windows Server 2008 R2. La porta RPC utilizzata dalla replica può essere impostate come hardcoded nel Registro di sistema usando i passaggi illustrati nella <externalLink> <linkText>articolo della Knowledge Base 224196</linkText> <linkUri> <a href="https://support.microsoft.com/kb/224196" data-raw-source="https://support.microsoft.com/kb/224196"> https://support.microsoft.com/kb/224196 </a> </linkUri> </externalLink>. Active Directory continua a registrare con Gestione endpoint quando configurato per usare una porta hardcoded. </para>
          <para>Verificare che l'applicazione Server RPC di interesse è stato registrato con l'agente mapping endpoint RPC sul Server RPC (il controller di dominio origine nel caso di replica di Active Directory). </para>
          <para>Esistono diversi modi per eseguire questa operazione, ma una è per installare ed eseguire PORTQRY da un prompt dei comandi con privilegiata di amministratore nella console dell'origine di controller di dominio usando la sintassi: </para>
          <code>c:&amp;gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>Nell'output di portqry, tenere presente i numeri di porta registrati in modo dinamico per il &quot;MS NT Directory DRS Interface&quot; (UUID = 351...) per il <embeddedLabel>ncacn_ip_tcp protocollo</embeddedLabel>. Il frammento di codice seguente Mostra output di esempio portquery da Windows Server 2008 R2 controller di dominio e l'UUID / coppia di protocollo usato in particolare da Active Directory evidenziato nel <embeddedLabel>grassetto</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>Altri modi possibili per risolvere questo errore:</para>
          <list class="ordered">
            <listItem>
              <para>Verificare che il controller di dominio di origine viene avviato in modalità normale e che il ruolo del sistema operativo e controller di dominio nel controller di dominio di origine siano stati avviati completamente.</para>
            </listItem>
            <listItem>
              <para>Verificare che il servizio di dominio Active Directory sia in esecuzione. Se il servizio è inattivo o non è stato configurato con i valori di avvio predefiniti, reimpostare i valori di avvio predefinite, riavviare il controller di dominio modificato, quindi ripetere l'operazione.</para>
            </listItem>
            <listItem>
              <para>Verificare che lo stato di valore e il servizio di avvio per il servizio RPC e RPC Locator sia corretto per la versione del sistema operativo del Client RPC (controller di dominio di destinazione) e Server RPC (controller di dominio di origine). Se il servizio è inattivo o non è stato configurato con i valori di avvio predefiniti, reimpostare i valori di avvio predefinite, riavviare il controller di dominio modificato, quindi ripetere l'operazione.</para>
              <para>Inoltre, assicurarsi che il contesto del servizio corrisponde a impostazioni predefinite elencate nella tabella seguente.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Servizio</para>
                    </TD>
                    <TD>
                      <para>Stato predefinito (tipo di avvio) in Windows Server 2003 e versioni successive </para>
                    </TD>
                    <TD>
                      <para>Stato predefinito (tipo di avvio) in Windows Server 2000</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>Chiamata di procedura remota</para>
                    </TD>
                    <TD>
                      <para>Avviato (automatica)</para>
                    </TD>
                    <TD>
                      <para>Avviato (automatica)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localizzatore di chiamata di procedura remota</para>
                    </TD>
                    <TD>
                      <para>Null o l'arresto (manuale)</para>
                    </TD>
                    <TD>
                      <para>Avviato (automatica)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Verificare che le dimensioni dell'intervallo di porte dinamiche non sono stata vincolata. La sintassi di Windows Server 2008 e Windows Server 2008 R2 NETSH per enumerare l'intervallo di porte RPC è illustrata di seguito:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>Verificare che le definizioni di porta hardcoded definite nella Knowledge Base 224196 rientrano nell'intervallo di porte dinamiche per la versione del sistema operativo i controller di dominio di origine.</para>
              <para>Revisione <externalLink> <linkText>articolo della Knowledge Base 224196</linkText> <linkUri> <a href="https://support.microsoft.com/kb/224196" data-raw-source="https://support.microsoft.com/kb/224196"> https://support.microsoft.com/kb/224196 </a> </linkUri> </externalLink> e assicurarsi che la porta hardcoded sia compreso nell'intervallo di porte temporanee per il controller di dominio di origine&#39;versione del sistema operativo s.</para>
            </listItem>
            <listItem>
              <para>Verificare che la chiave di protocolli client in HKLM\Software\Microsoft\Rpc esiste e contiene i valori 5 predefiniti seguente:</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>Altre informazioni</title>
    <content>
      <para>
        <embeddedLabel>Esempio di un nome non valido per IP mapping che provocano errori RPC 1753 VS -2146893022: il nome dell'entità di destinazione non è corretto</embeddedLabel>
      </para>
      <para>Il dominio contoso.com costituito DC1 e DC2 con x.x.1.1 gli indirizzi IP e x.x.1.2. L'host &quot;un'&quot; / &quot;AAAA&quot; record per DC2 sono stati registrati correttamente in tutti i server DNS configurato per DC1. Inoltre, il file HOSTS nel DC1 contiene una voce di mapping nome host completo DC2s a x.x.1.2 indirizzo IP. In un secondo momento, DC2&#39;indirizzo IP passa da X.X.1.2 a X.X.1.3 e un nuovo computer membro viene aggiunto al dominio con x.x.1.2 indirizzo IP. Replica di Active Directory tenta attivato dal <ui>Replicate now</ui> comando Active Directory Sites and Services snap-in non riesce con errore 1753 come illustrato nella traccia riportato di seguito:</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>Fotogramma <embeddedLabel>10</embeddedLabel>, il controller di dominio di destinazione esegue una query il mapper di endpoint di origine i controller di dominio sulla porta 135 per la classe di servizio di replica di Active Directory UUID E351... </para>
      <para>Nel frame <embeddedLabel>11</embeddedLabel>, controller di dominio, l'origine in questo caso, un computer membro che non ospita ancora il ruolo di controller di dominio e pertanto non è stato registrato il E351... UUID per il servizio di replica con relativo EPM locale risponde con errore simbolico EP_S_NOT_REGISTERED che esegue il mapping all'errore decimale 1753, hex errore 0x6d9 ed errore descrittivo &quot;non esistono Nessun endpoint disponibile nel mapping degli endpoint&quot; .</para>
      <para>In un secondo momento, il computer membro con x.x.1.2 indirizzo IP viene alzato di livello come replica &quot;MayberryDC&quot; nel dominio contoso.com. Anche in questo caso, il <ui>Replicate now</ui> comando viene utilizzato per attivare la replica, ma questa volta ha esito negativo con sullo schermo errore &quot;il nome dell'entità di destinazione non è corretto.&quot; Computer con scheda di rete è assegnato l'indirizzo IP indirizzo x.x.1.2 <placeholder>è</placeholder> un controller di dominio è attualmente avviato in modalità normale e ha registrato il E351... il servizio di replica UUID con relativo EPM locale ma l'identità di sicurezza o nome di DC2 non è il proprietario e non può decrittografare la richiesta Kerberos da DC1 in modo che la richiesta avrà esito negativo con errore &quot;il nome dell'entità di destinazione non è corretto.&quot; L'errore viene eseguito il mapping all'errore decimale -2146893022 / errore 0x80090322 hex. </para>
      <para>Tali mapping host IP non valido potrebbe essere causato da voci non aggiornate nell'host / file lmhost, host A / registrazioni AAAA in DNS o WINS. </para>
      <para>Riepilogo In questo esempio non è riuscita perché un mapping di host IP non valido (nel file di HOST in questo caso) ha causato la destinazione controller di dominio di risolvere in un &quot;origine&quot; controller di dominio che non è in esecuzione il servizio Active Directory Domain Services (o anche installati per a tal fine) in modo che il nome SPN non è stata ancora registrata la replica e il controller di dominio di origine ha restituito l'errore 1753. Nel secondo caso, un mapping di host IP non valido (nuovamente nel file HOST) ha causato il controller di dominio per connettersi a un controller di dominio che ha registrato il E351 di destinazione... replica SPN, ma tale origine ha un'identità del nome host e sicurezza diversi rispetto all'origine desiderata controller di dominio in modo che i tentativi non è riuscita con errore -2146893022: Il nome dell'entità di destinazione non è corretto.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Risoluzione dei problemi di operazioni di Active Directory non riuscite con errore 1753: Non esistono Nessun endpoint disponibile nel mapping degli endpoint. </linkText> 
      <linkUri> <a href="https://support.microsoft.com/kb/2089874" data-raw-source="https://support.microsoft.com/kb/2089874"> https://support.microsoft.com/kb/2089874 </a> </linkUri> 
    </externalLink> 
<externalLink> <linkText>KB articolo 839880 errori di risoluzione dei problemi RPC Endpoint Mapper utilizzando il supporto per Windows Server 2003 Gli strumenti dal CD del prodotto</linkText><linkUri><a href="https://support.microsoft.com/kb/839880" data-raw-source="https://support.microsoft.com/kb/839880">https://support.microsoft.com/kb/839880</a></linkUri></externalLink>
<externalLink><linkText>KB articolo 832017 panoramica e rete porta del servizio requisiti per il sistema di Windows Server</linkText><linkUri><a href="https://support.microsoft.com/kb/832017/" data-raw-source="https://support.microsoft.com/kb/832017/">https://support.microsoft.com/kb/832017/</a></linkUri></externalLink>
<externalLink><linkText>KB articolo 224196 limitazione Active Il traffico di replica e il client RPC del traffico a una porta specifica</linkText><linkUri><a href="https://support.microsoft.com/kb/224196/" data-raw-source="https://support.microsoft.com/kb/224196/">https://support.microsoft.com/kb/224196/</a></linkUri></externalLink>
<externalLink><linkText>articolo della Knowledge Base 154596 jak nakonfigurovat assegnazione delle porte dinamiche RPC per lavorare con i firewall</linkText><linkUri><a href="https://support.microsoft.com/kb/154596" data-raw-source="https://support.microsoft.com/kb/154596">https://support.microsoft.com/kb/154596</a></linkUri></externalLink><externalLink><linkText>come RPC Funziona</linkText><linkUri><a href="https://msdn.microsoft.com/library/aa373935(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373935(VS.85).aspx">https://msdn.microsoft.com/library/aa373935(VS.85).aspx</a></linkUri></externalLink><externalLink><linkText>come consente di preparare il Server per una connessione</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373938(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373938(VS.85).aspx"> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </a> </linkUri> </externalLink> 
<externalLink> <linkText>Come il Client stabilisce una connessione</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373937(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373937(VS.85).aspx"> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>La registrazione dell'interfaccia</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa375357(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa375357(VS.85).aspx"> https://msdn.microsoft.com/library/aa375357(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Rende disponibile il Server in rete</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373974(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373974(VS.85).aspx"> https://msdn.microsoft.com/library/aa373974(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Registrazione degli endpoint</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa375255(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa375255(VS.85).aspx"> https://msdn.microsoft.com/library/aa375255(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Per ascoltare le chiamate Client</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373966(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373966(VS.85).aspx"> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Come il Client stabilisce una connessione</linkText><linkUri><a href="https://msdn.microsoft.com/library/aa373937(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373937(VS.85).aspx">https://msdn.microsoft.com/library/aa373937(VS.85).aspx</a></linkUri></externalLink><span class=" class=""></span class="></linkText><linkUri><a href="https://msdn.microsoft.com/library/dd207688(PROT.13).aspx" data-raw-source="https://msdn.microsoft.com/library/dd207688(PROT.13).aspx">https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</a></linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


