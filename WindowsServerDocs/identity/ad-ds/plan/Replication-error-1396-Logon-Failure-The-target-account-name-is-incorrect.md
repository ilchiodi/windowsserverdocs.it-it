---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: Errore di replica 1396 Errore di accesso. Il nome dell'account di destinazione non è corretto
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ef3da06dd348b804f538d37cafbfabdd8bf45beb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844502"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Errore di replica 1396 Errore di accesso. Il nome dell'account di destinazione non è corretto

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Questo articolo descrive i sintomi, cause e come risolvere la replica di Active Directory ha esito negativo con errore Win32 1396: "Errore durante l'accesso: Il nome di account di destinazione non è corretto." </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Sintomi</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Cause</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Soluzioni</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Sintomi</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Report DCDIAG che il test di repliche di Active Directory non è riuscita con errore 1396: Errore di accesso: Il nome di account di destinazione non è corretto."</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. Report EXE che l'ultimo tentativo di replica non è riuscita con stato 1396.</para><para>REPADMIN comandi che in genere riportare lo stato 1396 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM.</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Esempio di output da "REPADMIN /SHOWREPS" che descrivono la replica in ingresso da CONTOSO-DC2 per CONTOSO-DC1 ha esito negativo con il "errore di accesso: Il nome di account di destinazione non è corretto." di seguito è riportato l'errore::</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>Il <ui>Replicate now</ui> comando in Active Directory Sites and Services restituisce "errore di accesso: Il nome di account di destinazione non è corretto."</para><para>Facendo clic sull'oggetto connessione da un controller di dominio di origine e scegliendo <ui>Replicate now</ui> ha esito negativo con "errore di accesso: Il nome di account di destinazione non è corretto." Sullo schermo viene visualizzato il messaggio di errore seguente:</para><para>Testo del titolo della finestra:</para><para>Replicare ora</para><para>Testo del messaggio finestra di dialogo: </para><para>Si è verificato l'errore seguente durante il tentativo di sincronizzare il contesto di denominazione &lt;percorso DNS della partizione&gt; dal controller di dominio &lt;controller di dominio di origine&gt; al controller di dominio &lt;destinazione controller di dominio&gt;: Errore di accesso: Il nome di account di destinazione non è corretto. Questa operazione non prosegue. </para></listItem><listItem><para>Gli eventi di KCC NTDS, NTDS generale o Microsoft-Windows-ActiveDirectory DomainService con lo stato 1396 vengono registrati nel Registro di servizi di Directory nel Visualizzatore eventi.</para><para>Eventi Active Directory che in genere riportare lo stato 1396 includono ma non sono limitati a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID evento</para></TD><TD><para>Origine evento</para></TD><TD><para>Stringa dell'evento</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Il dominio servizi di installazione guidata Active Directory (Dcpromo) non è riuscito a stabilire una connessione con il seguente controller di dominio.</para></TD></tr><tr><TD><para>1645</para><para>Questo evento vengono indicati il nome SPN in tre parti.</para></TD><TD><para>Replica NTDS</para></TD><TD><para>Impossibile eseguire una chiamata di procedura remota (RPC) autenticata verso un altro controller di dominio. Il nome principale del servizio (SPN) per il controller di dominio di destinazione non è registrato nel controller di dominio Centro distribuzione chiavi (KDC) che risolve il nome SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Servizi di dominio Active Directory ha tentato di comunicare con il catalogo globale seguente e i tentativi hanno esito negativo.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Il controllo di coerenza che si trova una connessione di replica per il servizio directory locale di sola lettura e ha tentato di aggiornare in remoto nell'istanza del servizio di directory seguente. L'operazione non riuscita. Verrà ripetuto.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Non è riuscito il tentativo di stabilire un collegamento di replica per la partizione di directory scrivibile.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Il tentativo di stabilire un collegamento di replica per una partizione di directory di sola lettura con i seguenti parametri non è riusciti.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> Il server non è possibile registrare il nome nel DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO ha esito negativo con un errore sullo schermo</para><para>Testo del titolo della finestra:</para><para>Installazione di Active Directory non è riuscita</para><para>Testo del messaggio finestra di dialogo:</para><para>Operazione non riuscita. Motivo: Il servizio Directory non è stato possibile creare l'oggetto server per CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = Site, CN = Sites, CN = Configuration, DC = contoso, DC = com nel server ReplicationSourceDC.contoso.com. </para><para>Verificare le credenziali di rete specificate hanno diritti di accesso sufficienti per aggiungere una replica. </para><para>
"Errore durante l'accesso: Il nome di account di destinazione non è corretto. "</para><para>In questo caso, l'ID evento 1645 e 1168 1125 vengono registrati nel server in cui viene alzata di livello.</para></listItem><listItem><para>Eseguire il mapping di un'unità utilizzando <embeddedLabel>net usare</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>In questo caso, il server di può inoltre la registrazione 333 ID evento nel registro eventi di sistema e usare una quantità elevata di memoria virtuale per un'applicazione, ad esempio SQL Server.</para></listItem><listItem><para>Il tempo di controller di dominio non è corretto.</para></listItem><listItem><para>Il KDC non verrà avviato in un RODC dopo un ripristino dell'account krbtgt per il controller di dominio, che è stata eliminata. Ad esempio, dopo un ripristino, viene visualizzato errore 1396. </para><para>
ID evento 1645 viene registrato nel RODC. </para><para>
Inoltre, Dcdiag segnala un errore che non è possibile aggiornare l'account krbtgt del RODC. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Cause</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>Il nome SPN non esiste nel catalogo globale-cercati dal KDC per conto del client tenta di eseguire l'autenticazione tramite Kerberos.</para>
          <para>Nel contesto di replica di Active Directory, il client Kerberos è la destinazione controller di dominio, il KDC esegue la ricerca del nome SPN è probabile che la destinazione controller di dominio stesso ma potrebbe essere un controller di dominio remoto.</para>
        </listItem>
        <listItem>
          <para>L'utente o account del servizio deve contenere il nome dell'entità servizio che vengono cercato non esiste nel catalogo globale-cercati dal KDC per conto di controller di dominio tentando di eseguire la replica di destinazione.</para>
          <para>Nel contesto di replica di Active Directory, l'account computer del controller di dominio di origine non esiste nel catalogo globale-eseguire la ricerca dal controller di dominio per conto della replica in ingresso dalle prestazioni controller di dominio di destinazione.</para>
        </listItem>
        <listItem>
          <para>Il controller di dominio di destinazione non dispone di un segreto LSA per il dominio controller di dominio di origine.</para>
        </listItem>
        <listItem>
          <para>Il nome SPN che vengono cercati presente in un account computer diverso da quello di controller di dominio di origine.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Soluzioni</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Controllare il registro eventi del servizio Directory sul controller di dominio di destinazione per l'evento NTDS replica 1645 e tenere presente quanto segue:</para>
          <para>Il nome della destinazione controller di dominio</para>
          <para>Il nome dell'entità servizio che viene ricercata (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;guid per l'oggetto impostazioni NTDS i controller di dominio di origine dell'oggetto&gt;/&lt;dominio di destinazione&gt;.&lt; TLD&gt;@&lt;dominio di destinazione&gt;.&lt; dominio di primo livello&gt;</para>
          <para>Il centro distribuzione CHIAVI utilizzato dalla destinazione dei controller di dominio</para>
        </listItem>
        <listItem>
          <para>Dalla console del centro KDC identificati nel passaggio 1, digitare: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Eseguire il test di localizzazione NLTEST subito dopo un tentativo di replica non riuscita con l'errore 1396 sul controller di dominio di destinazione. </para>
          <para>Questo dovrebbe identificare tale catalogo globale che il KDC esegue ricerche di SPN. </para>
          <para>Il Garbage Collector cercato dal KDC potrebbe anche essere acquisite nell'evento Microsoft-Windows-ActiveDirectory DomainService 1655.</para>
        </listItem>
        <listItem>
          <para>Cercare il nome dell'entità servizio individuata nel passaggio 1 nel catalogo globale-individuato nel passaggio 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>O</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Verificare che sia presente l'oggetto host per il nome SPN.</para>
          <para>Verificare il percorso del nome distinto per l'oggetto di host ad esempio se l'oggetto è CNF / conflitto danneggiato o si trova nel contenitore lost and found.</para>
          <para>Verificare che l'origine i controller di dominio Active Directory replica SPN viene registrato solo per l'account computer del controller di dominio di origine.</para>
          <para>Se il nome dell'entità servizio di replica è manca, determinare se il controller di dominio di origine ha registrato il proprio nome SPN con se stesso e se il nome SPN non è presente sul catalogo globale utilizzato dal KDC a causa della latenza di replica semplice o un errore di replica.</para>
        </listItem>
        <listItem>
          <para>Controllare l'integrità del canale sicuro e attendibile di integrità.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Risoluzione dei problemi di operazioni di Active Directory con 1396 errore: Errore di accesso: Il nome di account di destinazione non è corretto.</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


