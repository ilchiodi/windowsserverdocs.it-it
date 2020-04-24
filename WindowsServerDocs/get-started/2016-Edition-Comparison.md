---
title: Prodotti ed edizioni di Windows Server 2016
description: Illustra le differenze tra le edizioni Standard e Datacenter di Windows Server
ms.prod: windows-server
ms.date: 10/04/2019
ms.technology: server-general
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: ce4c35f0b65d0461e9dc2e23404d2637aecff415
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80827104"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Confronto tra le edizioni Standard e Datacenter di Windows Server 2016

> Si applica a: Windows Server 2016
  
## <a name="locks-and-limits"></a>Blocchi e limiti

| Blocchi e limiti | Windows Server 2016 Standard | Windows Server 2016 Datacenter |
| ------------------- |---------- | --------------------------- |  
| Numero massimo di utenti | Basato su licenze CAL   | Basato su licenze CAL     |
| Numero massimo di connessioni SMB | 16.777.216      | 16.777.216          |
| Numero massimo di connessioni RRAS| Nessun limite       | Nessun limite         |
| Numero massimo di connessioni IAS | 2\.147.483.647   | 2\.147.483.647        |
| Numero massimo di connessioni RDS | 65535           | 65535             |
| Numero massimo di socket a 64 bit | 64     | 64                |
| Numero massimo di core | Nessun limite       | Nessun limite      |
| RAM massima             | 24 TB           | 24 TB             |
| Utilizzabile come guest di virtualizzazione | Sì; due macchine virtuali, più un host Hyper-V per licenza | Sì; <strong>macchine virtuali illimitate</strong>, più un host Hyper-V per licenza |
| Il server può essere aggiunto a un dominio | sì            | sì                |
| Protezione della rete perimetrale/firewall | no     | no                 |
| DirectAccess            | sì             | sì                |
| Codec DLNA e flussi multimediali web | Sì, se installato come Server con Esperienza desktop | Sì, se installato come Server con Esperienza desktop |

## <a name="server-roles"></a>Ruoli server

| Ruoli Windows Server disponibili     | Servizi ruolo | Windows Server 2016 Standard | Windows Server 2016 Datacenter |  
| -------------------                | ----------    | ----------                   | ---------------------------    |  
| Servizi certificati Active Directory|              | Sì                          | Sì                            |
| Servizi di dominio di Active Directory    |               | Sì                          | Sì                            |
| Active Directory Federation Services|               | Sì                          | Sì                            |
| AD Lightweight Directory Services| |Sì|Sì|
| AD Rights Management Services| |Sì|Sì|
| Attestazione dell'integrità dei dispositivi| |Sì|Sì|
| Server DHCP| |Sì|Sì|
| Server DNS| |Sì|Sì|
| Server fax| |Sì|Sì|
| Servizi file e archiviazione|File Server|Sì|Sì|
| Servizi file e archiviazione|BranchCache per file di rete|Sì|Sì|
| Servizi file e archiviazione|Deduplicazione dati|Sì|Sì|
| Servizi file e archiviazione|Spazi dei nomi DFS|Sì|Sì|
| Servizi file e archiviazione|Replica DFS|Sì|Sì|
| Servizi file e archiviazione|Gestione risorse file server|Sì|Sì|
| Servizi file e archiviazione|Servizio agente File Server VSS|Sì|Sì|
| Servizi file e archiviazione|Server di destinazione iSCSI|Sì|Sì|
| Servizi file e archiviazione|Provider di archiviazione destinazioni iSCSI|Sì|Sì|
| Servizi file e archiviazione|Server per NFS|Sì|Sì|
| Servizi file e archiviazione|Cartelle di lavoro|Sì|Sì|
| Servizi file e archiviazione|Servizi di archiviazione|Sì|Sì|
| Servizio Sorveglianza host| |Sì|Sì|
| Hyper-V| |Sì|Sì; <strong>incluse macchine virtuali schermate</strong>|
| MultiPoint Services| |Sì|Sì|
| Controller di rete| |No| <strong>Sì</strong> |
| Servizi di accesso e criteri di rete| |Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
| Servizi di stampa e digitalizzazione| |Sì|Sì|
| Accesso remoto| |Sì|Sì|
| Servizi Desktop remoto| |Sì|Sì|
| Servizi di attivazione contratti multilicenza| |Sì|Sì|
| Servizi Web (IIS)| |Sì|Sì|
| Servizi di distribuzione Windows| |Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
| Esperienza Windows Server Essentials| |Sì|Sì|
| Windows Server Update Services| |Sì|Sì|

## <a name="features"></a>Caratteristiche

|Funzionalità Windows Server installabili con Server Manager (o PowerShell)|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|Sì|Sì|
|.NET framework 4.6|Sì|Sì|
|Servizio trasferimento intelligente in background (BITS)|Sì|Sì|
|Crittografia unità BitLocker|Sì|Sì|
|Sblocco di rete via BitLocker|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|BranchCache|Sì|Sì|
|Client per NFS|Sì|Sì|
|Contenitori|Sì (contenitori Windows illimitati; fino a 2 contenitori Hyper-V)|Sì (tutti i tipi di contenitore senza limiti)|
|Data Center Bridging|Sì|Sì|
|Esecuzione diretta|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Archiviazione avanzata|Sì|Sì|
|Clustering di failover|Sì|Sì|
|Gestione Criteri di gruppo|Sì|Sì|
|Supporto Sorveglianza host Hyper-V|No|<strong>Sì</strong> |
|QoS (Quality of Service) I/O|Sì|Sì|
|HWC (Hostable Web Core) di IIS|Sì|Sì|
|Client di stampa Internet|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Server di Gestione indirizzi IP|Sì|Sì|
|Servizio server iSNS|Sì|Sì|
|Monitoraggio porta LPR|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Estensione IIS OData gestione|Sì|Sì|
|Media Foundation|Sì|Sì|
|Accodamento messaggi|Sì|Sì|
|Multipath I/O|Sì|Sì|
|MultiPoint Connector|Sì|Sì|
|Bilanciamento carico di rete|Sì|Sì|
|Protocollo PNRP (Peer Name Resolution Protocol)|Sì|Sì|
|Servizio audio/video Windows di qualità|Sì|Sì|
|Connection Manager Administration Kit RAS|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Assistenza remota|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Compressione differenziale remota|Sì|Sì|
|Strumenti di amministrazione remota del server|Sì|Sì|
|Proxy RPC su HTTP|Sì|Sì|
|Raccolta eventi di configurazione e avvio|Sì|Sì|
|Servizi TCP/IP semplificati|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Supporto condivisione di file SMB 1.0/CIFS|Installato|Installato|
|Limite larghezza di banda SMB|Sì|Sì|
|Server SMTP|Sì|Sì|
|Servizio SNMP|Sì|Sì|
|Bilanciamento del carico software|No| <strong>Sì</strong> |
|Replica archiviazione|No|Sì|
|Client Telnet|Sì|Sì|
|Client TFTP|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Strumenti di schermatura VM per la gestione dell'infrastruttura|Sì|Sì|
|Redirector WebDAV|Sì|Sì|
|Windows Biometric Framework|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Funzionalità di Windows Defender|Installato|Installato|
|Windows Identity Foundation 3.5|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Database interno di Windows|Sì|Sì|
|Windows PowerShell|Installato|Installato|
|Servizio Attivazione dei processi Windows|Sì|Sì|
|Servizio Windows Search|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Windows Server Backup|Sì|Sì|
|Strumenti di migrazione per Windows Server|Sì|Sì|
|Gestione archiviazione basata su standard Windows|Sì|Sì|
|Windows TIFF IFilter|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|
|Estensione IIS di Gestione remota Windows|Sì|Sì|
|Server WINS|Sì|Sì|
|Servizio LAN Wireless|Sì|Sì|
|Supporto di WoW64|Installato|Installato|
|XPS Viewer|Sì, quando installato come Server con Esperienza desktop|Sì, quando installato come Server con Esperienza desktop|

|Funzionalità disponibili in generale|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Best Practices Analyzer|Sì|Sì|
|Direct Access|Sì|Sì|
|Memoria dinamica (in virtualizzazione)|Sì|Sì|
|Aggiunta/Sostituzione RAM a caldo|Sì|Sì|
|Microsoft Management Console|Sì|Sì|
|Interfaccia server minima|Sì|Sì|
|Bilanciamento carico di rete|Sì|Sì|
|Windows PowerShell|Sì|Sì|
|Opzione di installazione dei componenti di base del server|Sì|Sì|
|Opzione di installazione Nano Server|Sì|Sì|
|Server Manager|Sì|Sì|
|SMB diretto e SMB su RDMA|Sì|Sì|
| Rete software-defined | No | <strong>Sì</strong> |
|Replica archiviazione | No | <strong>Sì</strong> |
|Spazi di archiviazione|Sì|Sì|
|Spazi di archiviazione diretta|No| <strong>Sì</strong> |
|Servizi di attivazione contratti multilicenza|Sì|Sì|
|Integrazione VSS (Servizio Copia Shadow del volume)|Sì|Sì|
|Windows Server Update Services|Sì|Sì|
|Gestione risorse di sistema Windows|Sì|Sì|
|Registrazione licenze del server|Sì|Sì|
|Attivazione ereditata|Come guest se ospitato in Datacenter| <strong>Può essere host o guest</strong> |
|Cartelle di lavoro|Sì|Sì|

