---
title: Eventi di registrazione DHCP per le registrazioni dei Record DNS
description: Questo argomento fornisce informazioni sul server DHCP la registrazione eventi in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a5099cd5e1ef1d4687baa8c20411c96ea8f519
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventi di registrazione DHCP per le registrazioni DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Registri eventi del server DHCP forniscono ora informazioni dettagliate sugli errori di registrazione DNS.

>[!NOTE]
>In molti casi, la causa degli errori di registrazione dei record DNS dal server DHCP è che una zona di ricerca Reverse\ DNS è configurata in modo errato o non è configurata.

I seguenti nuovi eventi DHCP consentono di identificare facilmente quando vengono esito negativo a causa di una configurazione errata o mancante zona DNS di ricerca di Reverse\ le registrazioni DNS.

|ID|Evento|Valore|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|Registrazione dei record diretta per l'indirizzo IPv4 %1 e FQDN %2 non riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca diretta per questo record non esiste nel server DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|Registrazione dei record diretta per l'indirizzo IPv4 %1 e FQDN %2 non riuscita con errore %3.|
|20319|DHCPv4.PTRRecordDNSFailure|Registrazione dei record PTR per l'indirizzo IPv4 %1 e FQDN %2 non riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca inversa per il record non esiste nel server DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|Registrazione dei record PTR per l'indirizzo IPv4 %1 e FQDN %2 non riuscita con errore %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|Registrazione dei record diretta per l'indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca diretta per questo record non esiste nel server DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|Registrazione dei record diretta per l'indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3.|
|20323|DHCPv6.PTRRecordDNSFailure|Registrazione dei record PTR per l'indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca inversa per il record non esiste nel server DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|Registrazione dei record PTR per l'indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3.|
|20325|DHCPv4.ForwardRecordDNSError|Registrazione dei record PTR per l'indirizzo IPv4 %1 e FQDN %2 non riuscita con errore %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|Inoltra registrazione dei record di indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|Registrazione dei record PTR per l'indirizzo IPv6 %1 e FQDN %2 non riuscita con errore %3 \(%4\).|

