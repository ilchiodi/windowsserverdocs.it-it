---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Non sono di errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e7412f5edc6c206888551fdc250883b5c0ced3e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Non sono di errore di replica 1753 Nessun endpoint disponibile nel mapping degli endpoint

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>In questo argomento vengono illustrati i sintomi, cause e come risolvere Active Directory replica errore 8524 l'operazione DSA è in grado di procedere a causa di un errore di ricerca DNS.</para>
    <list class="bullet">
      <listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Sintomi</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">causa</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">risoluzioni</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">informazioni</link></para></listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomi</title>
    <content>
      <para>questo articolo descrive i sintomi, cause e i passaggi di risoluzione per operazioni di Active Directory non riuscite con errore Win32 1753: "Non esistono Nessun endpoint disponibile nel mapping degli endpoint." </para>
      <list class="ordered">
        <listItem>
          <para>Report DCDIAG che il test della connettività, i test di repliche di Active Directory o KnowsOfRoleHolders test non è riuscita con errore 1753: "Non esistono Nessun endpoint disponibile nel mapping degli endpoint." </para>
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
<listItem><para>REPADMIN. Fornisce un report EXE che tentano di replica non è riuscito con stato 1753. </para><para>Comandi REPADMIN che in genere riportare lo stato 1753 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /replsum.</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>Il comando REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Esempio di output da "REPADMIN /SHOWREPS" che rappresenta la replica in ingresso da CONTOSO-DC2 per CONTOSO-DC1 ha generato l'errore con l'errore "accesso di replica è stato negato" è illustrata di seguito:</para><code>Default-First-Site-NameCONTOSO-DC1
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

</code></listItem><listItem><para>il <ui>controllo topologia di replica</ui> comando in Active Directory Sites and Services restituisce "Non esistono Nessun endpoint disponibile nel mapping degli endpoint." </para><para>Pulsante destro del mouse sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>controllo topologia di replica</ui> ha esito negativo con "Non esistono Nessun endpoint disponibile nel mapping degli endpoint." Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>testo del titolo di finestra di dialogo: controllo topologia di replica</para><para>testo del messaggio finestra di dialogo: </para><para>si è verificato l'errore seguente durante il tentativo di contattare il controller di dominio: non esistono Nessun endpoint disponibile nel mapping degli endpoint. </para></listItem><listItem><para>Il <ui>Replicate now</ui> comando in Active Directory Sites and Services restituisce "non esistono Nessun endpoint disponibile nel mapping degli endpoint." </para><para>Pulsante destro del mouse sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>Replicate now</ui> ha esito negativo con "Non esistono Nessun endpoint disponibile nel mapping degli endpoint." Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>testo del titolo di finestra di dialogo: Replicate Now</para><para>testo del messaggio finestra di dialogo: si è verificato l'errore seguente durante il tentativo di sincronizzare il contesto dei nomi &lt;nome partizione di directory di %&gt; dal Controller di dominio &lt;controller di dominio di origine&gt; a Controller di dominio &lt;controller di dominio di destinazione&gt;:</para><para>

Non esistono Nessun endpoint disponibile nel mapping degli endpoint. </para><para>L'operazione verrà interrotta</para></listItem><listItem><para>KCC NTDS, NTDS generale o Microsoft-Windows-ActiveDirectory DomainService con lo stato-2146893022 vengono registrati nel Registro di servizi di Directory nel Visualizzatore eventi. </para><para>Gli eventi di active Directory che in genere riportare lo stato-2146893022 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID evento</para></TD><TD><para>Origine evento</para></TD><TD><para>Stringa dell'evento</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS generale</para></TD><TD><para>Active Directory ha tentato di comunicare con il catalogo globale seguente e i tentativi hanno esito negativo.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Non è riuscito il tentativo di stabilire un collegamento di replica per la partizione di directory scrivibile.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Non è riuscito per il controllo di coerenza informazioni (KCC) tenta di aggiungere un accordo di replica per le seguenti directory partizione e origine controller di dominio.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Causa</title>
    <content>
      <para>il diagramma seguente mostra il flusso di lavoro RPC a partire dalla registrazione dell'applicazione server con RPC Endpoint Mapper (EPM) nel passaggio 1 per la trasmissione dei dati dal client RPC per l'applicazione client nel passaggio 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>passaggi da 1 a 7 mappa per le operazioni seguenti:</para>
      <list class="ordered">
        <listItem>
          <para>Server app registra i propri endpoint con il mapping degli Endpoint RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>Client effettua una chiamata RPC (per conto di un utente, del sistema operativo o applicazione avviata operazione) </para>
        </listItem>
        <listItem>
          <para>Client lato RPC contatti, computer di destinazione EPM e richiedere all'endpoint completare la chiamata di client </para>
        </listItem>
        <listItem>
          <para>EPM del computer Server risponde con un endpoint </para>
        </listItem>
        <listItem>
          <para>lato Client RPC contatta l'app server </para>
        </listItem>
        <listItem>
          <para>Server app verrà eseguita la chiamata, restituisce il risultato al client RPC </para>
        </listItem>
        <listItem>
          <para>lato Client RPC passa il risultato all'App client</para>
        </listItem>
      </list>
      <para>1753 errore viene generato un errore tra i passaggi #3 e 4 #. In particolare, errore 1753 significa che il client RPC (controller di dominio di destinazione) è stato in grado di contattare il Server RPC (controller di dominio di origine) sulla porta 135 ma il EPM sul Server RPC (controller di dominio di origine) non è riuscito a individuare l'applicazione RPC di interesse e ha restituito l'errore sul lato server 1753. La presenza dell'errore 1753 indica che il client RPC (controller di dominio di destinazione) ha ricevuto la risposta di errore sul lato server dal Server RPC (origine della replica controller di dominio di Active Directory) in rete. </para>
      <para>Cause specifiche per l'errore 1753 includono: </para>
      <list class="ordered">
        <listItem>
          <para>l'app server mai stato avviato (ad esempio, passaggio 1 nel diagramma "ulteriori informazioni" sopra è stata mai tentata). </para>
        </listItem>
        <listItem>
          <para>Avviato l'app server ma si è verificato un errore durante l'inizializzazione che ha impedito la registrazione con l'agente mapping Endpoint RPC (ad esempio, il passaggio #1 nel diagramma precedente "ulteriori informazioni" non riuscita). </para>
        </listItem>
        <listItem>
          <para>L'app server avviato ma successivamente arresto. (ad esempio, il passaggio #1 nel diagramma precedente "ulteriori informazioni" è stata completata correttamente, ma è stata annullata in seguito perché il server di arresto). </para>
        </listItem>
        <listItem>
          <para>L'app server annullata la registrazione manualmente gli endpoint (simili a 3 ma intenzionale. Ma probabilmente non inclusa per completezza.) </para>
        </listItem>
        <listItem>
          <para>Client RPC (controller di dominio di destinazione) contattare un server RPC diverso da quello desiderato a causa di un nome all'errore di mapping IP in DNS, WINS o host/Lmhosts file. </para>
        </listItem>
      </list>
      <para>1753 errore non è causato da: </para>
      <list class="bullet">
        <listItem>
          <para>una mancanza di connettività di rete tra il client RPC (controller di dominio di destinazione) e il Server RPC (controller di dominio di origine) sulla porta 135</para>
        </listItem>
        <listItem>
          <para>una mancanza di connettività di rete tra il server RPC (controller di dominio di origine) tramite la porta 135 e il client RPC (controller di dominio di destinazione) sulla porta temporanea. </para>
        </listItem>
        <listItem>
          <para>Una mancata corrispondenza di password o l'impossibilità per il controller di dominio di origine per decrittografare un pacchetto crittografato Kerberos</para>
        </listItem>
      </list>
      <para></para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Risoluzioni</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>verificare che il servizio registrazione il servizio con il mapping degli endpoint è stato avviato</embeddedLabel>
          </para>
          <para>per Windows 2000 e i controller di dominio di Windows Server 2003: assicurarsi che il controller di dominio di origine è stato avviato in modalità normale. </para>
          <para>Per Windows Server 2008 o Windows Server 2008 R2: dalla console del controller di dominio di origine, avviare servizi Manager (services.msc) e verificare che il <embeddedLabel>servizi di dominio Active Directory</embeddedLabel> servizio è in esecuzione. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Verificare tale client (controller di dominio di destinazione) connesso al server RPC (controller di dominio di origine)</embeddedLabel>
          </para>
          <para>tutti i controller di dominio in un registro comune di insieme di strutture Active Directory un controller di dominio CNAME record nella zona msdcs. &lt;dominio radice della foresta&gt; zona DNS indipendentemente dal dominio cui si trovano all'interno della foresta. Il record CNAME di controller di dominio è derivato dal <embeddedLabel>objectGUID</embeddedLabel> attributo dell'oggetto impostazioni NTDS per ogni controller di dominio. </para>
          <para>Durante le operazioni basate su replica, un controller di dominio di destinazione esegue una query DNS per il record CNAME i controller di dominio di origine. Il record CNAME contiene il nome del computer completo controller di dominio di origine che viene usato per derivare l'indirizzo IP di controller di dominio di origine tramite la ricerca nella cache client DNS, ospitare / LMHost file ricerca, l'host A o AAAA record in DNS o WINS. </para>
          <para>Oggetti Impostazioni NTDS non aggiornati e i mapping nome non valido - to-IP nei file di DNS, WINS, Host e LMHOST possono causare il client RPC (controller di dominio di destinazione) per connettersi a un Server RPC non valido (controller di dominio di origine). Inoltre, il mapping nome non valido - to-IP può provocare il client RPC (controller di dominio di destinazione) per connettersi a un computer che non dispone anche l'applicazione Server RPC di interesse (il ruolo di Active Directory in questo caso) installato. (Esempio: un record host non aggiornati per DC2 contiene l'indirizzo IP di un computer membro o DC3). </para>
          <para>Verifica objectGUID per il controller di dominio esistente nella destinazione di copia di controller di dominio di Active Directory di origine corrispondente l'objectGUID controller di dominio di origine archiviata nell'origine copia i controller di dominio di Active Directory. Se è presente una discrepanza, utilizzare repadmin /showobjmeta sull'oggetto impostazioni ntds per vedere quale corrisponde all'ultima innalzamento di livello del controller di dominio di origine (Suggerimento: confrontare gli indicatori di data per l'oggetto impostazioni NTDS creare data da /showobjmeta contro l'ultima data di innalzamento di livello nel file dcpromo.log controller di dominio di origine. Potrebbe essere necessario usare l'ultima modifica / creare data la DCPROMO.LOG stesso). Se l'oggetto GUID non è identico, la destinazione di controller di dominio che dispone di un oggetto impostazioni NTDS non aggiornato per il controller di dominio di origine cui record CNAME fa riferimento a un record host con un nome per il mapping di IP non valido. </para>
          <para>Nel controller di dominio di destinazione, eseguire IPCONFIG//ALL per determinare quale server DNS utilizza il controller di dominio di destinazione per la risoluzione dei nomi:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>sul controller di dominio di destinazione, eseguire il record CNAME di controller di dominio completo di controller di dominio di origine NSLOOKUP:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>verificare che l'indirizzo IP restituito da NSLOOKUP "proprietario" il nome host / identità di sicurezza del controller di dominio di origine:</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>o</para>
          <para>registro la console del dominio di origine., eseguire "IPCONFIG" dal prompt di CMD e verificare che il controller di dominio di origine appartiene l'indirizzo IP restituito dal comando NSLOOKUP sopra</para>
          <para>cercare host aggiornato o duplicate per il mapping IP in DNS</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>se gli indirizzi IP non validi è presente nel record di host, verificare se lo scavenging DNS è abilitato e configurato correttamente. </para><para>Se i test precedenti o una traccia di rete non mostra una query di nome che restituisca un indirizzo IP non valido, prendere in considerazione delle voci obsolete nel file HOST, i file LMHOSTS e i server WINS. Si noti che i server DNS può anche essere configurati per eseguire la risoluzione dei nomi di fallback di WINS. </para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Verificare che l'applicazione server (Active Directory e al) è stato registrato con il mapping degli endpoint sul server RPC (controller di dominio di origine)</embeddedLabel>
          </para>
          <para>Active Directory utilizza una combinazione di porte registrate in modo dinamico e ben note. Questa tabella elenca note porte e protocolli utilizzati dai controller di dominio Active Directory.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Applicazione Server RPC</para>
                </TD>
                <TD>
                  <para>Porta</para>
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
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
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
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
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
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Directory di Microsoft</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
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
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
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
                  <para>X</para>
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
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>Well-known ports are NOT registered with the endpoint mapper. </para>
          <para>Active Directory and other applications also register services that receive dynamically assigned ports in the RPC ephemeral port range. Such RPC server applications are dynamically assigned TCP ports between 1024 and 5000 on Windows 2000 and Windows Server 2003 computers and ports between 49152 and 65535 range on Windows Server 2008 and Windows Server 2008 R2 computers. The RPC port used by replication can be hard-coded in the registry using the steps documented in <externalLink><linkText>KB article 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>. Active Directory continues to register with the EPM when configured to use a hard coded port. </para>
          <para>Verify that the RPC Server application of interest has registered itself with the RPC endpoint mapper on the RPC Server (the source DC in the case of AD replication). </para>
          <para>There are a number of ways to accomplish this task but one is to install and run PORTQRY from an admin privileged CMD prompt on the console of the source DC using the syntax: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>In the portqry output, note the port numbers dynamically registered by the "MS NT Directory DRS Interface" (UUID = 351...) for the <embeddedLabel>ncacn_ip_tcp protocol</embeddedLabel>. The snippet below shows sample portquery output from a Windows Server 2008 R2 DC and the UUID / protocol pair specifically used by Active Directory highlighted in <embeddedLabel>bold</embeddedLabel>: </para>
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
          <para>Other possible ways to resolve this error:</para>
          <list class="ordered">
            <listItem>
              <para>Verify that the source DC is booted in normal mode and that the OS and DC role on the source DC have fully started.</para>
            </listItem>
            <listItem>
              <para>Verify that the Active Directory Domain Service is running. If the service is currently stopped or was not configured with default startup values, reset the default startup values, reboot the modified DC then retry the operation.</para>
            </listItem>
            <listItem>
              <para>Verify that the startup value and service status for RPC service and RPC Locator is correct for OS version of the RPC Client (destination DC) and RPC Server (source DC). If the service is currently stopped or was not configured with default startup values, reset the default startup values, reboot the modified DC then retry the operation.</para>
              <para>In addition, ensure that the service context matches default settings listed in the following table.</para>
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
                      <para>Avvio (automatico)</para>
                    </TD>
                    <TD>
                      <para>Avvio (automatico)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localizzatore chiamata di procedura remota</para>
                    </TD>
                    <TD>
                      <para>Null o arrestato (manuale)</para>
                    </TD>
                    <TD>
                      <para>Avvio (automatico)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Verify that the size of the dynamic port range has not been constrained. The Windows Server 2008 and Windows Server 2008 R2 NETSH syntax to enumerate the RPC port range is shown below:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>Verify that hard coded port definitions defined in KB 224196 fall within the dynamic port range for source DCs OS version.</para>
              <para>Review <externalLink><linkText>KB article 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink> and ensure that the hard coded port falls within the ephemeral port range for the source DC's operating system version.</para>
            </listItem>
            <listItem>
              <para>Verify that the ClientProtocols key exists under HKLM\Software\Microsoft\Rpc and contains the following 5 default values:</para>
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
        <embeddedLabel>esempio di un nome non valido per IP mapping che causano errore RPC 1753 e -2146893022: il nome dell'entità di destinazione è errato</embeddedLabel>
      </para>
      <para>dominio contoso.com è costituito da DC1 e DC2 con IP indirizzi x.x.1.1 e x.x.1.2. L'host "A" / record "AAAA" per DC2 siano registrati correttamente in tutti i server DNS configurato per DC1. Inoltre, il file HOSTS nel DC1 contiene una voce mapping nome host completo DC2s x.x.1.2 indirizzo IP. Modifiche all'indirizzo IP del DC2 da X.X.1.2 X.X.1.3 e un computer membro di nuovo in un secondo momento, viene aggiunto al dominio con x.x.1.2 indirizzo IP. Replica di Active Directory tentativi attivate dal <ui>Replicate now</ui> comando nello snap-in Active Directory Sites and Services non riesce con errore 1753 come illustrato nella traccia di seguito:</para>
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
      <para>fotogramma <embeddedLabel>10</embeddedLabel>, il controller di dominio di destinazione esegue una query il mapping di punto finale di controller di dominio di origine tramite la porta 135 per la classe di servizio di replica di Active Directory E351 UUID... </para>
      <para>Nel frame <embeddedLabel>11</embeddedLabel>, l'origine controller di dominio, in questo caso un computer membro che non è ancora ospita il ruolo di controller di dominio e pertanto non è stato registrato il E351... UUID per il servizio di replica con il relativo EPM locale risponde con errore simbolico EP_S_NOT_REGISTERED mappato all'errore decimale 1753, errore esadecimale 0x6d9 e descrittivo errore "non sono presenti più endpoint disponibile nel mapping degli endpoint". </para>
      <para>In un secondo momento, il computer membro con x.x.1.2 indirizzo IP può essere promosso come una replica "MayberryDC" nel dominio contoso.com. Nuovamente, il <ui>Replicate now</ui> comando viene utilizzato per avviare la replica, ma questa volta ha esito negativo con sullo schermo errore "il nome dell'entità di destinazione è errato." Il computer il cui scheda di rete viene assegnata l'IP indirizzo x.x.1.2 <placeholder>è</placeholder> un controller di dominio attualmente è avviato in modalità normale e ha registrato il E351... il servizio di replica UUID con il relativo EPM locale, ma non possiede l'identità di sicurezza o di nome di DC2 e non puoi decrittografare la richiesta di Kerberos da DC1, in modo che la richiesta ora ha esito negativo con errore "il nome dell'entità di destinazione è errato." L'errore associato a un errore decimale -2146893022 / esadecimale errore 0x80090322. </para>
      <para>Tali mapping to-IP host non valido - potrebbe essere causata da delle voci obsolete nell'host o file lmhost, ospitare un / registrazioni AAAA in DNS o WINS. </para>
      <para>Riepilogo: in questo esempio non è riuscita causa di un mapping to-IP host non valido-, nel file HOST in questo caso, il controller di dominio di destinazione risolvere una "origine" controller di dominio di servizi di dominio Active Directory non è in esecuzione del servizio (o anche installato a tale scopo) quindi la replica SPN non è stato ancora registrato e il controller di dominio ha restituito l'errore 1753 di origine. Nel secondo caso, un mapping to-IP host non valido-(nuovamente nel file HOST) ha causato il controller di dominio per connettersi a un controller di dominio che ha registrato il E351... replica SPN di destinazione, ma tale origine era un'identità di sicurezza e il nome host diversa da quello di controller di dominio di origine previsto in modo che i tentativi non riuscita con errore -2146893022: il nome dell'entità di destinazione è errato.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Troubleshooting Active Directory operations that fail with error 1753: There are no more endpoints available from the endpoint mapper.</linkText>
      <linkUri>https://support.microsoft.com/kb/2089874</linkUri>
    </externalLink>
<externalLink><linkText>KB article 839880 Troubleshooting RPC Endpoint Mapper errors using the Windows Server 2003 Support Tools from the product CD</linkText><linkUri>https://support.microsoft.com/kb/839880</linkUri></externalLink>
<externalLink><linkText>KB article 832017 Service overview and network port requirements for the Windows Server system</linkText><linkUri>https://support.microsoft.com/kb/832017/</linkUri></externalLink>
<externalLink><linkText>KB article 224196 Restricting Active Directory replication traffic and client RPC traffic to a specific port</linkText><linkUri>https://support.microsoft.com/kb/224196/</linkUri></externalLink>
<externalLink><linkText>KB article 154596 How to configure RPC dynamic port allocation to work with firewalls</linkText><linkUri>https://support.microsoft.com/kb/154596</linkUri></externalLink><externalLink><linkText>How RPC Works</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>How the Server Prepares for a Connection</linkText><linkUri>https://msdn.microsoft.com/library/aa373938(VS.85).aspx</linkUri></externalLink>
<externalLink><linkText>How the Client Establishes a Connection</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Registering the Interface</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Making the Server Available on the Network</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Registering Endpoints</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Listening for Client Calls</linkText><linkUri>https://msdn.microsoft.com/library/aa373966(VS.85).aspx</linkUri></externalLink><externalLink><linkText>How the Client Establishes a Connection</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Restricting Active Directory replication traffic and client RPC traffic to a specific port</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>SPN for a Target DC in AD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


