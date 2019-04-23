---
title: Prodotti ed edizioni di Windows Server 2016
description: Illustra le differenze tra le edizioni Standard e Datacenter
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2c26d6d0c4c4465b5f9073dbcac951fc0adce1d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882052"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Confronto tra le edizioni Standard e Datacenter di Windows Server 2016

> Si applica a: Windows Server 2016
  
## <a name="locks-and-limits"></a>Blocchi e limiti
|Blocchi e limiti|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Numero massimo di utenti|Basato su licenze CAL|Basato su licenze CAL|
|Numero massimo di connessioni SMB|16777216|16777216|
|Numero massimo di connessioni RRAS|Nessun limite|Nessun limite|
|Numero massimo di connessioni IAS|2147483647|2147483647|
|Numero massimo di connessioni RDS|65535|65535|
|Numero massimo di socket a 64 bit|64|64|
|Numero massimo di core|Nessun limite|Nessun limite|
|RAM massima|24 TB|24 TB|
|Utilizzabile come guest di virtualizzazione|Sì; due macchine virtuali, più un host Hyper-V per licenza|Sì; macchine virtuali illimitate, più un host Hyper-V per licenza|
|Il server può essere aggiunto a un dominio|sì|sì|
|Protezione della rete perimetrale/firewall|no|no|
|DirectAccess|sì|sì|
|Codec DLNA e flussi multimediali web|Sì, se installato come Server con Esperienza desktop|Sì, se installato come Server con Esperienza desktop|

## <a name="server-roles"></a>Ruoli server
|Ruoli Windows Server disponibili|Servizi ruolo|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Servizi certificati Active Directory| |Yes|Yes|
|Servizi di dominio di Active Directory| |Yes|Yes|
|Active Directory Federation Services| |Yes|Yes|
|AD Lightweight Directory Services| |Yes|Yes|
|AD Rights Management Services| |Yes|Yes|
|Attestazione dell'integrità dei dispositivi| |Yes|Yes|
|Server DHCP| |Yes|Yes|
|Server DNS| |Yes|Yes|
|Server fax| |Yes|Yes|
|Servizi file e archiviazione|File Server|Yes|Yes|
|Servizi file e archiviazione|BranchCache per file di rete|Yes|Yes|
|Servizi file e archiviazione|Deduplicazione dati|Yes|Yes|
|Servizi file e archiviazione|Spazi dei nomi DFS|Yes|Yes|
|Servizi file e archiviazione|Replica DFS|Yes|Yes|
|Servizi file e archiviazione|Gestione risorse file server|Yes|Yes|
|Servizi file e archiviazione|Servizio agente File Server VSS|Yes|Yes|
|Servizi file e archiviazione|Server di destinazione iSCSI|Yes|Yes|
|Servizi file e archiviazione|Provider di archiviazione destinazioni iSCSI|Yes|Yes|
|Servizi file e archiviazione|Server per NFS|Yes|Yes|
|Servizi file e archiviazione|Cartelle di lavoro|Yes|Yes|
|Servizi file e archiviazione|Servizi di archiviazione|Yes|Yes|
|Servizio Sorveglianza host| |Yes|Yes|
|Hyper-V| |Yes|Sì; incluse macchine virtuali schermate|
|MultiPoint Services| |Yes|Yes|
|Controller di rete| |No|Yes|
|Servizi di accesso e criteri di rete| |Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Servizi di stampa e digitalizzazione| |Yes|Yes|
|Accesso remoto| |Yes|Yes|
|Servizi Desktop remoto| |Yes|Yes|
|Servizi di attivazione contratti multilicenza| |Yes|Yes|
|Servizi Web (IIS)| |Yes|Yes|
|Servizi di distribuzione Windows| |Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Esperienza Windows Server Essentials| |Yes|Yes|
|Windows Server Update Services| |Yes|Yes|

## <a name="features"></a>Funzionalità

|Funzionalità Windows Server installabili con Server Manager (o PowerShell)|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|Yes|Yes|
|.NET framework 4.6|Yes|Yes|
|Servizio trasferimento intelligente in background (BITS)|Yes|Yes|
|Crittografia unità BitLocker|Yes|Yes|
|Sblocco di rete via BitLocker|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|BranchCache|Yes|Yes|
|Client per NFS|Yes|Yes|
|Contenitori|Sì (contenitori Windows illimitati; fino a 2 contenitori Hyper-V)|Sì (tutti tipi di contenitore illimitati)|
|Data Center Bridging|Yes|Yes|
|Esecuzione diretta|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Archiviazione avanzata|Yes|Yes|
|Clustering di failover|Yes|Yes|
|Gestione Criteri di gruppo|Yes|Yes|
|Supporto Sorveglianza host Hyper-V|No|Yes|
|QoS (Quality of Service) I/O|Yes|Yes|
|HWC (Hostable Web Core) di IIS|Yes|Yes|
|Client di stampa Internet|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Server di Gestione indirizzi IP|Yes|Yes|
|Servizio server iSNS|Yes|Yes|
|Monitoraggio porta LPR|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Estensione IIS OData gestione|Yes|Yes|
|Media Foundation|Yes|Yes|
|Accodamento messaggi|Yes|Yes|
|Multipath I/O|Yes|Yes|
|MultiPoint Connector|Yes|Yes|
|Bilanciamento carico di rete|Yes|Yes|
|Protocollo PNRP (Peer Name Resolution Protocol)|Yes|Yes|
|Servizio audio/video Windows di qualità|Yes|Yes|
|Connection Manager Administration Kit RAS|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Assistenza remota|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Compressione differenziale remota|Yes|Yes|
|Strumenti di amministrazione remota del server|Yes|Yes|
|Proxy RPC su HTTP|Yes|Yes|
|Raccolta eventi di configurazione e avvio|Yes|Yes|
|Servizi TCP/IP semplificati|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Supporto condivisione di file SMB 1.0/CIFS|Installato|Installato|
|Limite larghezza di banda SMB|Yes|Yes|
|Server SMTP|Yes|Yes|
|Servizio SNMP|Yes|Yes|
|Bilanciamento del carico software|Yes|Yes|
|Replica archiviazione|No|Yes|
|Client Telnet|Yes|Yes|
|Client TFTP|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Strumenti di schermatura VM per la gestione dell'infrastruttura|Yes|Yes|
|Redirector WebDAV|Yes|Yes|
|Windows Biometric Framework|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Funzionalità di Windows Defender|Installato|Installato|
|Windows Identity Foundation 3.5|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Database interno di Windows|Yes|Yes|
|Windows PowerShell|Installato|Installato|
|Servizio Attivazione dei processi Windows|Yes|Yes|
|Servizio Windows Search|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Windows Server Backup|Yes|Yes|
|Strumenti di migrazione per Windows Server|Yes|Yes|
|Gestione archiviazione basata su standard Windows|Yes|Yes|
|Windows TIFF IFilter|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Estensione IIS di Gestione remota Windows|Yes|Yes|
|Server WINS|Yes|Yes|
|Servizio LAN Wireless|Yes|Yes|
|Supporto di WoW64|Installato|Installato|
|XPS Viewer|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|

|Funzionalità disponibili in generale|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Best Practices Analyzer|Yes|Yes|
|Direct Access|Yes|Yes|
|Memoria dinamica (in virtualizzazione)|Yes|Yes|
|Aggiunta/Sostituzione RAM a caldo|Yes|Yes|
|Microsoft Management Console|Yes|Yes|
|Interfaccia server minima|Yes|Yes|
|Bilanciamento carico di rete|Yes|Yes|
|Windows PowerShell|Yes|Yes|
|Opzione di installazione dei componenti di base del server|Yes|Yes|
|Opzione di installazione Nano Server|Yes|Yes|
|Server Manager|Yes|Yes|
|SMB diretto e SMB su RDMA|Yes|Yes|
|Rete definita tramite software|No|Yes|
|Servizio Gestione dell'archiviazione|Yes|Yes|
|Spazi di archiviazione|Yes|Yes|
|Spazi di archiviazione diretta|No|Yes|
|Servizi di attivazione contratti multilicenza|Yes|Yes|
|Integrazione VSS (Servizio Copia Shadow del volume)|Yes|Yes|
|Windows Server Update Services|Yes|Yes|
|Gestione risorse di sistema Windows|Yes|Yes|
|Registrazione licenze del server|Yes|Yes|
|Attivazione ereditata|Come guest se ospitato in Datacenter|Può essere host o guest|
|Cartelle di lavoro|Yes|Yes|

