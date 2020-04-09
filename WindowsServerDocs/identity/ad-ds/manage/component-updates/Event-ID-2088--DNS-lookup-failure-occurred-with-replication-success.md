---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 'ID evento 2088: si è verificato un errore di ricerca DNS con esito positivo della replica'
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f84fd7be45995e9e0b318b42c8b4152af244a9da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823054"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID evento 2088: errore di ricerca DNS con esito positivo della replica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

La configurazione DNS non valida può influire su altre operazioni essenziali sui computer membri, i controller di dominio o i server applicazioni in questa foresta Active Directory Domain Services, inclusa l'autenticazione di accesso o l'accesso alle risorse di rete. 

È necessario risolvere immediatamente questo errore di configurazione DNS, in modo che il controller di dominio possa risolvere l'indirizzo IP del controller di dominio di origine utilizzando DNS. 

Nome server alternativo: DC1 con nome host DNS non riuscito: 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com 

Nota: per impostazione predefinita, vengono visualizzati solo fino a 10 errori DNS per un determinato periodo di 12 ore, anche se si verificano più di 10 errori.  Per registrare tutti i singoli eventi di errore, impostare il seguente valore del registro di sistema di diagnostica su 1: 

Percorso del registro di sistema: client RPC HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS 

Azione dell'utente: 

1) Se il controller di dominio di origine non è più funzionante o il sistema operativo è stato reinstallato con un nome computer diverso o un GUID oggetto NTDSDSA, rimuovere i metadati del controller di dominio di origine con Ntdsutil. exe, seguendo la procedura descritta nell'articolo 216498 di MSKB. 

2) Verificare che il controller di dominio di origine sia in esecuzione Active Directory e che sia accessibile in rete digitando "NET View \\&lt;nome del controller di dominio di origine&gt;" o "ping &lt;origine DC name&gt;". 

3) Verificare che il controller di dominio di origine usi un server DNS valido per i servizi DNS e che il record host del controller di dominio di origine e il record CNAME siano registrati correttamente, usando la versione avanzata di DCDIAG di DNS. EXE disponibile in <https://www.microsoft.com/dns> 

Dcdiag/test: DNS 

4) Verificare che il controller di dominio di destinazione usi un server DNS valido per i servizi DNS, eseguendo la versione migliorata di DCDIAG di DNS. Comando EXE nella console del controller di dominio di destinazione, come indicato di seguito: 

Dcdiag/test: DNS 

5) Per ulteriori analisi degli errori di errore DNS, vedere KB 824449: <https://support.microsoft.com/?kbid=824449> 

Valore di errore dati aggiuntivi: 11004 il nome richiesto è valido, ma non sono stati trovati dati del tipo richiesto</code> </introduction>
  <section>
    <title>Diagnosi</title>
    <content>
      <para>La mancata risoluzione del nome del controller di dominio di origine tramite il record di risorse alias (CNAME) in DNS può essere dovuta a errori di configurazione o ritardi DNS nella propagazione dei dati DNS.</para>
    </content>
  </section>
  <section>
    <content>di 
    <title>risoluzione</title> 
      <para>Procedere con il test DNS come descritto in &quot;<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID evento 2087: errore di ricerca DNS ha causato l'esito negativo della replica</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


