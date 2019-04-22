---
title: DHCP eventi di registrazione per le registrazioni dei Record DNS
description: Questo argomento vengono fornite informazioni sui server DHCP la registrazione eventi in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816902"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DHCP eventi di registrazione per le registrazioni DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Registri eventi del server DHCP forniscono ora informazioni dettagliate sugli errori di registrazione DNS.

>[!NOTE]
>In molti casi, il motivo per gli errori di registrazione dei record DNS dai server DHCP è che un DNS inverso\-zona di ricerca è configurata in modo non corretto o non è configurata.

I nuovi eventi DHCP seguenti consentono di identificare facilmente quando le registrazioni DNS non riuscite a causa di una configurazione errata e mancante inversa DNS\-zona di ricerca.

|ID|Evento|Value|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|Registrazione dei record in avanti per %2 FQDN e indirizzo IPv4 %1 non è riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca diretta per questo record non esiste nel server DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|Registrazione dei record in avanti per %2 FQDN e indirizzo IPv4 %1 non è riuscita con errore %3.|
|20319|DHCPv4.PTRRecordDNSFailure|Registrazione dei record PTR per %2 FQDN e indirizzo IPv4 %1 non è riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca inversa per il record non esiste nel server DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|Registrazione dei record PTR per %2 FQDN e indirizzo IPv4 %1 non è riuscita con errore %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|Registrazione dei record in avanti per l'indirizzo IPv6 %1 e FQDN %2 non è riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca diretta per questo record non esiste nel server DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|Registrazione dei record in avanti per l'indirizzo IPv6 %1 e FQDN %2 non è riuscita con errore %3.|
|20323|DHCPv6.PTRRecordDNSFailure|Registrazione dei record PTR per IPv6 indirizzo %1 e %2 FQDN non è riuscita con errore %3. Si tratta probabilmente perché la zona di ricerca inversa per il record non esiste nel server DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|Registrazione dei record PTR per IPv6 indirizzo %1 e %2 FQDN non è riuscita con errore %3.|
|20325|DHCPv4.ForwardRecordDNSError|Registrazione dei record PTR per %2 FQDN e indirizzo IPv4 %1 non è riuscita con errore %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|Registrazione dei record in avanti per l'indirizzo IPv6 %1 e FQDN %2 non è riuscita con errore %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|Registrazione dei record PTR per IPv6 indirizzo %1 e %2 FQDN non è riuscita con errore %3 \(%4\).|

