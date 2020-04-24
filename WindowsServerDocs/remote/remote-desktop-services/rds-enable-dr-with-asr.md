---
title: Abilitare il ripristino di emergenza di Servizi Desktop remoto con Azure Site Recovery
description: Informazioni su come abilitare il ripristino di emergenza di Servizi Desktop remoto con Azure Site Recovery.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0c7af18be4aa767009f1dd0b82f145ffe6874768
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861404"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Abilitare il ripristino di emergenza di Servizi Desktop remoto con Azure Site Recovery

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Per essere certo che la distribuzione di Servizi Desktop remoto sia configurata in modo adeguato per il ripristino di emergenza, devi proteggere tutti i componenti che costituiscono la distribuzione di Servizi Desktop remoto:

- Active Directory
- Livello di SQL Server
- Componenti di Servizi Desktop remoto
- Componenti di rete

## <a name="configure-active-directory-and-dns-replication"></a>Configurare la replica di Active Directory e DNS

È necessario Active Directory nel sito di ripristino di emergenza per garantire il funzionamento della distribuzione di Servizi Desktop remoto. Sono disponibili due opzioni in base alla complessità della distribuzione di Servizi Desktop remoto:

- Opzione 1: nel caso di un numero ridotto di applicazioni e di un singolo controller di dominio per l'intero sito locale, se intendi eseguire il failover dell'intero sito usa la replica di Azure Site Recovery per replicare il controller di dominio nel sito secondario (applicabile per scenari da sito a sito e da sito ad Azure).
- Opzione 2: nel caso di un numero elevato di applicazioni e di una foresta di Active Directory, se intendi eseguire il failover di poche applicazioni per volta, configura un controller di dominio aggiuntivo nel sito di ripristino di emergenza, che può essere un sito secondario o un sito di Azure.

Vedi [Proteggere Active Directory e DNS con Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) per i dettagli su come rendere disponibile un controller di dominio nel sito di ripristino di emergenza. Nella parte restante di questo documento si presuppone che tu abbia seguito i passaggi sopra indicati e abbia reso disponibile il controller di dominio.

## <a name="set-up-sql-server-replication"></a>Configurare la replica di SQL Server

Vedi [Proteggere SQL Server tramite il ripristino di emergenza di SQL Server e Azure Site Recovery](/azure/site-recovery/site-recovery-sql) per la procedura da seguire per configurare la replica di SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Abilitare la protezione per i componenti dell'applicazione Servizi Desktop remoto

A seconda del tipo di distribuzione di Servizi Desktop remoto, puoi abilitare la protezione per macchine virtuali di componenti diversi in Azure Site Recovery, come indicato nella tabella riportata di seguito. Configura gli elementi di Azure Site Recovery pertinenti a seconda che le macchine virtuali siano distribuite in Hyper-V o VMWare.


|               Tipo di distribuzione                |                                                                                                     Passaggi di protezione                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Desktop virtuale personale (non gestito)     | 1. Assicurati che tutti gli host di virtualizzazione siano pronti con il ruolo Host di virtualizzazione Desktop remoto installato.    </br>2. Gestore connessione.  </br>3. Desktop personali. </br>4. Modello di macchina virtuale Oro. </br>5. Server Accesso Web, licenze e gateway |
| Desktop virtuale in pool (gestito, senza dischi dei profili utente) |                    1. Tutti gli host di virtualizzazione pronti con il ruolo Host di virtualizzazione Desktop remoto installato.  </br>2. Gestore connessione.  </br>3. Modello di macchina virtuale Oro. </br>4. Server Accesso Web, licenze e gateway.                    |
|   Sessioni desktop e RemoteApp (senza dischi dei profili utente)   |                                                          1. Host sessione.  </br>2. Gestore connessione. </br>3. Server Accesso Web, licenze e gateway.                                                           |

