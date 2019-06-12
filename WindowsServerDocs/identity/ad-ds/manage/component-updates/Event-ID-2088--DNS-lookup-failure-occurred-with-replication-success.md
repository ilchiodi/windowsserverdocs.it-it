---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 'ID evento 2088: si è verificato un errore di ricerca DNS con esito positivo della replica'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0c5e838290a8ebf33f0f7891dc10f8b00e5bcba
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442657"
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

Configurazione DNS non valido può interessare altre operazioni essenziali sul computer membro, i controller di dominio o server applicazioni nella foresta Active Directory Domain Services, tra cui l'autenticazione di accesso o l'accesso alle risorse di rete. 

È necessario risolvere immediatamente l'errore di configurazione di DNS in modo che il controller di dominio può risolvere l'indirizzo IP del controller di dominio con DNS. 

Nome server alternativo: Nome host DNS di DC1 non superati: 4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

NOTA: Per impostazione predefinita, gli errori DNS solo fino a 10 vengono visualizzati per un periodo di 12 ore specificato, anche se si verificano più di 10 errori.  Per registrare tutti gli eventi di errore di singoli, impostare i dati di diagnostica seguente valore del Registro di sistema su 1: 

Percorso del Registro di sistema: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 RPC DS Client 

Azione dell'utente: 

1) Se il controller di dominio di origine non viene più funzioni o il suo sistema operativo è stato reinstallato con un nome computer diverso o NTDSDSA oggetto GUID, rimuovere i metadati del controller di dominio di origine con ntdsutil.exe, usando la procedura descritta nell'articolo MSKB 216498. 

2) Verificare che il controller di dominio di origine è in esecuzione Active Directory e sia accessibile in rete, digitare "net view \\ &lt;nome del controller di dominio&gt;" o "ping &lt;nome del controller di dominio&gt;". 

3) Verificare che il controller di dominio di origine Usa un server DNS valido per i servizi DNS e che il controller di dominio di origine record host CNAME record e sono registrati correttamente, utilizzando la versione migliorata di DNS di DCDIAG. Disponibile nel file EXE <https://www.microsoft.com/dns> 

Dcdiag /test: DNS 

4) Verificare che il controller di dominio di destinazione utilizzi un server DNS valido per i servizi DNS, eseguendo la versione migliorata di DNS di DCDIAG. Comando EXE sulla console del controller di dominio di destinazione, come indicato di seguito: 

Dcdiag /test: DNS 

5) Vedere 824449 KB per un'ulteriore analisi degli errori di errore DNS: <https://support.microsoft.com/?kbid=824449> 

Ulteriori dati Valore di errore: 11004. il nome richiesto è valido, ma nessun dato del tipo richiesto è stato trovato</code> </introduction>
  <section>
    <title>Diagnosi</title>
    <content>
      <para>Impossibilità di risolvere il nome del controller di dominio di origine tramite il record di risorse alias (CNAME) nel sistema DNS può essere a causa di errori di configurazione di DNS o i ritardi nella propagazione dei dati DNS.</para>
    </content>
  </section>
  <section>
    <title>Risoluzione</title>
    <content>
      <para>Procedere con il test del DNS, come descritto in &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID evento 2087: Errore di ricerca DNS causato replica avrà esito negativo</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


