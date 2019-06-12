---
title: Abilitare il ripristino di emergenza di servizi desktop remoto con Azure Site Recovery
description: Informazioni su come abilitare il ripristino di emergenza di servizi desktop remoto con Azure Site Recovery.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7aa25602c71e5d114be7ae59c5e3ce168844d700
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446554"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Abilitare il ripristino di emergenza di servizi desktop remoto con Azure Site Recovery

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Per assicurarsi che la distribuzione di servizi desktop remoto sia configurata in modo adeguato per il ripristino di emergenza, è necessario proteggere tutti i componenti che costituiscono la distribuzione di servizi desktop remoto:

- Active Directory
- Livello di SQL Server
- Componenti Servizi Desktop remoto
- Componenti di rete

## <a name="configure-active-directory-and-dns-replication"></a>Configurare la replica di Active Directory e DNS

È necessario Active Directory nel sito di ripristino di emergenza per la distribuzione di servizi desktop remoto per il funzionamento. Sono disponibili due opzioni di base alla complessità della distribuzione di servizi desktop remoto:

- Opzione 1: se si dispone di un numero ridotto di applicazioni e un singolo controller di dominio per l'intero sito locale e si eseguirà il failover dell'intero sito, usare la replica di Azure Site Recovery per replicare il controller di dominio nel sito secondario (true per entrambi scenari Site-to-site e site-to-Azure).
- Opzione 2: se si dispone di un numero elevato di applicazioni e si esegue una foresta di Active Directory e si imposterà un failover di poche applicazioni alla volta, impostare un controller di dominio aggiuntivo nel sito di ripristino di emergenza (in un sito secondario o in Azure).

Visualizzare [proteggere Active Directory e DNS con Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) per informazioni dettagliate su come rendere disponibile un controller di dominio nel sito di ripristino di emergenza. Per il resto di questo materiale sussidiario, si presuppone che sono stati eseguiti questi passaggi e che il controller di dominio disponibili.

## <a name="set-up-sql-server-replication"></a>Configurare la replica di SQL Server

Visualizzare [proteggere SQL Server tramite ripristino di emergenza di SQL Server e Azure Site Recovery](/azure/site-recovery/site-recovery-sql) per i passaggi per configurare la replica di SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Abilitare la protezione per i componenti dell'applicazione di servizi desktop remoto

A seconda del tipo di distribuzione di servizi desktop remoto è possibile abilitare la protezione per le macchine virtuali componente diverso (come indicato nella tabella riportata di seguito) in Azure Site Recovery. Configurare gli elementi di Azure Site Recovery pertinenti in base che le macchine virtuali vengano distribuite in Hyper-V o VMWare.


|               Tipo di distribuzione                |                                                                                                     Passaggi di protezione                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Desktop virtuale personale (non gestito)     | 1. Assicurarsi che tutti gli host di virtualizzazione siano pronti con il ruolo RDVH installato.    </br>2. Gestore di connessione.  </br>3. Desktop personali. </br>4. Gold modello di macchina virtuale. </br>5. Web Access, server licenze e server Gateway |
| Desktop virtuali in pool (gestito con nessun UPD) |                    1. Tutti gli host di virtualizzazione sono pronti con il ruolo RDVH installato.  </br>2. Gestore di connessione.  </br>3. Gold modello di macchina virtuale. </br>4. Web Access, server licenze e server Gateway.                    |
|   Su macchine virtuali e le sessioni Desktop (nessun UPD)   |                                                          1. Host della sessione.  </br>2. Gestore di connessione. </br>3. Web Access, server licenze e server Gateway.                                                           |

