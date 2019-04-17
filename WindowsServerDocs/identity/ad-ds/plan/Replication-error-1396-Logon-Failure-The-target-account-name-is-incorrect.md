---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: "Errore di replica 1396 errore di accesso il nome di account di destinazione non è corretto"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 84799de26e1260f914d9b959357d5eed6fef62f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Errore di replica 1396 errore di accesso il nome di account di destinazione non è corretto

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Questo articolo descrive i sintomi, cause e come risolvere la replica di Active Directory non riesce con errore Win32 1396: "errore di accesso: il nome di account di destinazione è errato." </para><list class="bullet"><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Sintomi</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">causa</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">risoluzioni</link></para></listItem></list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomi</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>report DCDIAG che il test di repliche di Active Directory non è riuscita con errore 1396: errore di accesso: il nome di account di destinazione è errato. " </para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. Report EXE che l'ultimo tentativo di replica non è riuscita con stato 1396. </para><para>Comandi REPADMIN che in genere riportare lo stato 1396 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /replsum.</para></listItem><listItem><para>/REHOST REPADMIN</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>Il comando REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Esempio di output da "REPADMIN /SHOWREPS" che rappresenta la replica in ingresso da CONTOSO-DC2 per CONTOSO-DC1 non riesce e il "errore di accesso: il nome di account di destinazione è errato." di seguito è riportato l'errore::</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para>il <ui>Replicate now</ui> comando in Active Directory Sites and Services restituisce "errore di accesso: il nome di account di destinazione è errato." </para><para>Pulsante destro del mouse sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>Replicate now</ui> ha esito negativo con "errore di accesso: il nome di account di destinazione è errato." Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>testo del titolo di finestra di dialogo:</para><para>Replicate Now</para><para>testo del messaggio finestra di dialogo: </para><para>si è verificato l'errore seguente durante il tentativo di sincronizzare il contesto dei nomi &lt;percorso DNS partizione&gt; dal controller di dominio &lt;controller di dominio di origine&gt; a controller di dominio &lt;controller di dominio di destinazione&gt;: errore di accesso: il nome di account di destinazione è errato. Questa operazione verrà interrotta. </para></listItem><listItem><para>KCC NTDS, NTDS generale o Microsoft-Windows-ActiveDirectory DomainService con lo stato 1396 vengono registrati nel Registro di servizi di Directory nel Visualizzatore eventi. </para><para>Gli eventi di active Directory che in genere riportare lo stato 1396 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID evento</para></TD><TD><para>Origine evento</para></TD><TD><para>Stringa dell'evento</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory DomainService</para></TD><TD><para>Il dominio servizi di installazione guidata Active Directory (Dcpromo) non è riuscito a stabilire una connessione con il seguente controller di dominio.</para></TD></tr><tr><TD><para>1645</para><para>questo evento sono elencati il nome SPN in tre parti.</para></TD><TD><para>Replica NTDS</para></TD><TD><para>Active Directory non eseguiva una chiamata di procedura remota autenticato (RPC) a un altro controller di dominio perché il nome dell'entità servizio (SPN) per il controller di dominio di destinazione non è registrato nel controller di dominio Centro distribuzione chiavi (KDC) che risolve il nome SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory DomainService</para></TD><TD><para>Servizi di dominio Active Directory ha tentato di comunicare con il catalogo globale seguente e i tentativi hanno esito negativo.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory DomainService</para></TD><TD><para>Controllo di coerenza trova una connessione di replica per il servizio directory locale di sola lettura e ha tentato di aggiornare in remoto sull'istanza del servizio directory seguente. L'operazione non riuscita. Verrà ripetuta.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Non è riuscito il tentativo di stabilire un collegamento di replica per la partizione di directory scrivibile.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Il tentativo di stabilire un collegamento di replica per una partizione di directory di sola lettura con i parametri seguenti non è riusciti.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>SERVIZIO ACCESSO RETE</para></TD><TD><para> Il server non può registrare il proprio nome in DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO ha esito negativo con errore sullo schermo</para><para>testo del titolo di finestra di dialogo:</para><para>installazione non riuscita di Active Directory</para><para>testo del messaggio finestra di dialogo:</para><para>l'operazione non riuscita perché: il servizio Directory non è riuscito a creare l'oggetto server per CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = Site, CN = Sites, CN = Configuration, DC = contoso, DC = com nel server ReplicationSourceDC.contoso.com. </para><para>, Assicurati che le credenziali di rete fornite dispongono di accesso sufficienti per aggiungere una replica.</para><para> "Accesso non riuscito: il nome di account di destinazione è errato." </para><para>In questo caso, l'ID evento 1645 e 1168 1125 vengono registrati nel server in cui viene innalzato di livello. </para></listItem><listItem><para>Mappare un'unità utilizzando <embeddedLabel>net uso</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>In questo caso, il server in grado anche registrazione 333 ID evento nel registro eventi di sistema e usare una quantità elevata di memoria virtuale per un'applicazione, ad esempio SQL Server. </para></listItem><listItem><para>Tempo il controller di dominio non è corretta. </para></listItem><listItem><para>Il KDC non verrà avviata nel RODC dopo un ripristino dell'account krbtgt per il controller di dominio, è stato eliminato. Ad esempio, dopo il ripristino, viene visualizzato l'errore 1396. </para><para>1645 ID evento viene registrato nel RODC. </para><para>Dcdiag segnala inoltre un errore che non sarà possibile aggiornare l'account krbtgt.</para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>causa</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>il SPN non esiste nel catalogo globale ricerca per conto del client tenta di eseguire l'autenticazione Kerberos usando KDC.</para>
          <para>nel contesto della replica di Active Directory, il client Kerberos è il controller di dominio, il KDC esegue la ricerca del nome SPN di destinazione è probabile che la destinazione di controller di dominio stesso ma potrebbe essere un controller di dominio remoto.</para>
        </listItem>
        <listItem>
          <para>l'account utente o un servizio che dovrebbe contenere il nome dell'entità viene cercato di servizio non esiste nel catalogo globale ricerca per conto di destinazione controller di dominio tenta di replicare il KDC.</para>
          <para>nel contesto della replica di Active Directory, l'account computer del controller di dominio di origine non esiste nel catalogo globale effettuata la ricerca dal controller di dominio per conto di replica in ingresso prestazioni controller di dominio di destinazione.</para>
        </listItem>
        <listItem>
          <para>il controller di dominio di destinazione non dispone di un segreto LSA per il dominio del controller di dominio di origine.</para>
        </listItem>
        <listItem>
          <para>esiste il SPN viene cercato in un account computer diverso da quello di controller di dominio di origine.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Risoluzioni</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>controllare il registro eventi del servizio Directory sul controller di dominio di destinazione per l'evento NTDS replica 1645 e tenere presente quanto segue:</para>
          <para>il nome del controller di dominio di destinazione</para>
          <para>il SPN viene cercato (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;oggetto guid dell'oggetto impostazioni NTDS i controller di dominio di origine&gt;/&lt;dominio di destinazione&gt;.&lt; TLD&gt;@&lt;dominio di destinazione&gt;. &lt;tld&gt;</para>
          <para>il KDC utilizzato dal controller di dominio di destinazione</para>
        </listItem>
        <listItem>
          <para>dalla console del KDC identificato nel passaggio 1, digitare: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>eseguire il test del localizzatore NLTEST immediatamente dopo un tentativo di replica ha esito negativo con l'errore 1396 sul controller di dominio di destinazione. </para>
          <para>Questo deve identificare tale catalogo globale che il KDC esegue ricerche di SPN. </para>
          <para>Nell'evento Microsoft-Windows-ActiveDirectory DomainService 1655 potrebbe essere acquisita anche il catalogo globale viene eseguita la ricerca dal KDC. </para>
        </listItem>
        <listItem>
          <para>Cercare il nome SPN individuato nel passaggio 1 sul catalogo globale individuato nel passaggio 2. </para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>o</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>verificare che l'oggetto host per il nome SPN disponibile. </para>
          <para>Verificare il percorso DN per l'oggetto di host compreso se l'oggetto è CNF / conflitto alterati o si trova nel contenitore lost and found. </para>
          <para>Verificare che l'origine i controller di dominio Active Directory replica SPN sia registrato solo sull'account computer del controller di dominio di origine. </para>
          <para>Se la replica SPN è manca, determinare se il controller di dominio di origine è stato registrato il proprio nome SPN con se stesso e se il nome SPN manca sul catalogo globale utilizzato dal KDC a causa di latenza di replica semplice o un errore di replica. </para>
        </listItem>
        <listItem>
          <para>Controlla l'integrità del canale sicuro e attendibile integrità.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>operazioni di risoluzione dei problemi di Active Directory non riuscite con errore 1396: errore di accesso: il nome di account di destinazione è errato.</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


