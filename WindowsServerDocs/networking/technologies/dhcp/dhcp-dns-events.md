---
title: Eventi di registrazione DHCP per registrazioni di record DNS
description: In questo argomento vengono fornite informazioni sugli eventi di registrazione del server DHCP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5d167666e632aa1a8d92de71feafc9014b66e7ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312600"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventi di registrazione DHCP per le registrazioni DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

I registri eventi del server DHCP forniscono ora informazioni dettagliate sugli errori di registrazione DNS.

>[!NOTE]
>In molti casi, il motivo per cui si verificano errori di registrazione di record DNS da parte dei server DHCP è che una zona DNS di ricerca inversa\-è configurata in modo errato o non configurata.

I nuovi eventi DHCP seguenti consentono di identificare facilmente il momento in cui le registrazioni DNS hanno esito negativo a causa di una zona di ricerca DNS inversa\-o mancante.

|ID|Evento|Valore|
|-----|--------------------|--------------------------------------------------------|
|20317|Estensione DHCPv4. ForwardRecordDNSFailure|La registrazione dei record in diretta per l'indirizzo IPv4% 1 e FQDN %2 non è riuscita con errore %3. È probabile che la zona di ricerca diretta per questo record non esista nel server DNS.|
|20318|Estensione DHCPv4. ForwardRecordDNSTimeout|La registrazione dei record in diretta per l'indirizzo IPv4% 1 e FQDN %2 non è riuscita con errore %3.|
|20319|Estensione DHCPv4. PTRRecordDNSFailure|La registrazione del record PTR per l'indirizzo IPv4% 1 e FQDN %2 non è riuscita con errore %3. Questa operazione è probabilmente dovuta al fatto che la zona di ricerca inversa per questo record non esiste nel server DNS.|
|20320|Estensione DHCPv4. PTRRecordDNSTimeout|La registrazione del record PTR per l'indirizzo IPv4% 1 e FQDN %2 non è riuscita con errore %3.|
|20321|DHCPv6. ForwardRecordDNSFailure|La registrazione del record di avanzamento per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3. È probabile che la zona di ricerca diretta per questo record non esista nel server DNS.|
|20322|DHCPv6. ForwardRecordDNSTimeout|La registrazione del record di avanzamento per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3.|
|20323|DHCPv6. PTRRecordDNSFailure|La registrazione del record PTR per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3. Questa operazione è probabilmente dovuta al fatto che la zona di ricerca inversa per questo record non esiste nel server DNS.|
|20324|DHCPv6. PTRRecordDNSTimeout|La registrazione del record PTR per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3.|
|20325|Estensione DHCPv4. ForwardRecordDNSError|La registrazione del record PTR per l'indirizzo IPv4% 1 e FQDN %2 non è riuscita con errore %3 \(%4\).|
|20326|DHCPv6. ForwardRecordDNSError|La registrazione del record di avanzamento per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3 \(%4\)|
|20327|DHCPv6. PTRRecordDNSError|La registrazione del record PTR per l'indirizzo IPv6% 1 e FQDN %2 non è riuscita con errore %3 \(%4\).|

