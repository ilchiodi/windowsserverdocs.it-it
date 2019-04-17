---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "ID evento 2088 - si è verificato un errore di ricerca DNS con esito positivo della replica"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID evento 2088: Si è verificato un errore di ricerca DNS con esito positivo della replica

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

Configurazione DNS non valido può riguardare altre operazioni fondamentali sul computer membri, i controller di dominio o server applicazioni della foresta Servizi di dominio Active Directory, tra cui l'autenticazione di accesso o l'accesso alle risorse di rete. 

È necessario risolvere immediatamente l'errore di configurazione di DNS in modo che il controller di dominio può risolvere l'indirizzo IP del controller di dominio di origine tramite DNS. 

Nome server alternativo: nome host DNS di DC1 esito negativo: 4a8717eb-8e58-456 c-995a-c92e4add7e8e._msdcs. contoso.com 

Nota: Per impostazione predefinita, solo fino a 10 errori DNS vengono visualizzati per un periodo di 12 ore specificato, anche se si verificano errori più di 10.  Per accedere a tutti gli eventi singoli di errore, impostare la diagnostica seguente valore del Registro di sistema su 1: 

Percorso del Registro di sistema: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

Azione utente: 

1) Se il controller di dominio di origine è più non funziona o del sistema operativo è stato reinstallato con un nome computer diverso o NTDSDSA oggetto GUID, rimuovere i metadati del controller di dominio di origine con ntdsutil.exe, utilizzando la procedura descritta nell'articolo MSKB 216498. 

2) Verificare che il controller di dominio di origine è in esecuzione Active Directory e sia accessibile in rete digitando "net view \ \&lt;nome controller di dominio di origine&gt;" o "ping &lt;nome controller di dominio di origine&gt;". 

3) Verify that the source domain controller is using a valid DNS server for DNS services, and that the source domain controller's host record and CNAME record are correctly registered, using the DNS Enhanced version of DCDIAG.EXE available on https://www.microsoft.com/dns 

Dcdiag//test: dns 

4) Verificare che il controller di dominio di destinazione utilizzi un server DNS valido per i servizi DNS, eseguendo la versione migliorata DNS di DCDIAG.EXE comando nella console del controller di dominio di destinazione, come segue: 

Dcdiag//test: dns 

5) For further analysis of DNS error failures see KB 824449: https://support.microsoft.com/?kbid=824449 

Ulteriori dati valore di errore: 11004 il nome richiesto è valido, ma è stato trovato alcun dato del tipo richiesto</code>
  </introduction>
  <section>
    <title>Diagnosi</title>
    <content>
      <para>impossibilità di risolvere il nome del controller di dominio di origine utilizzando il record di risorse alias (CNAME) in DNS può essere dovuto a errori di configurazione DNS o i ritardi nella propagazione dati DNS.</para>
    </content>
  </section>
  <section>
    <title>Risoluzione</title>
    <content>
      <para>procedere con il test del DNS, come descritto in "<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID evento 2087: errore di ricerca DNS causato replica avrà esito negativo</link>."</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


