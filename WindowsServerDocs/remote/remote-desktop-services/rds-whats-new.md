---
title: Novità di Servizi Desktop remoto
description: Fornisce le descrizioni delle nuove funzionalità di Servizi Desktop remoto in Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63711778"
---
# <a name="whats-new-in-remote-desktop-services"></a>Novità di Servizi Desktop remoto

Servizi Desktop remoto (RDS) basato su Windows Server 2016 è una piattaforma di virtualizzazione abilitare una vasta gamma di scenari di clienti. I miglioramenti apportati alla soluzione Servizi Desktop remoto racchiudono sia il lavoro svolto dal team di Desktop remoto che quello degli altri partner tecnologici Microsoft. Le tecnologie e gli scenari seguenti sono nuovi o migliorati in Windows Server 2016.

Segui inoltre la nostra sessione di Ignite 2016: [Sfruttare i miglioramenti di Servizi Desktop remoto in Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). In questo video il team del prodotto esamina tutte le funzionalità nuove e migliorate in Servizi Desktop remoto, incluso il supporto per vGPU. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilità delle app: Windows Server 2016 e Windows 10
Windows Server 2016 si basa sugli stessi fondamenti di Windows 10 e non solo ha lo stesso aspetto che ti aspetti da un desktop, ma può anche eseguire molte delle stesse applicazioni. La combinazione di Windows Server 2016 con le funzionalità di grafica (illustrate sotto) offre un ambiente che aiuta tutti gli utenti a lavorare in modo produttivo. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Database SQL di Azure: il nuovo database per un ambiente a disponibilità elevata
Gestore connessione Desktop remoto consente di archiviare tutte le informazioni di distribuzione (ad esempio, stati della connessione e mapping utente/host) in un database SQL condiviso, ad esempio un database SQL di Azure. Non dovrai più distribuire manualmente Gruppi di disponibilità Always On di SQL Server, ma sarà sufficiente usare la stringa di connessione al database SQL di Azure per iniziare a lavorare in un ambiente a disponibilità elevata.

Altre informazioni: [Usare il database SQL di Azure per l'ambiente a disponibilità elevata di Gestore connessione Desktop remoto](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Grafica: risposta alle esigenze di grafica in vari scenari
Grazie all'assegnazione discreta di dispositivi Hyper-V, puoi ora mappare le GPU in un computer host direttamente a una macchina virtuale per l'utilizzo da parte delle applicazioni che richiedono la GPU. Sono anche stati apportati miglioramenti alla vGPU RemoteFX, incluso il supporto per OpenGL 4.4, OpenCL 1.1, risoluzione 4K e macchine virtuali Windows Server.

Altre informazioni: [Assegnazione di dispositivi discreti](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Gestore connessione Desktop remoto: gestione delle connessioni migliorata durante i picchi di accessi
Con la gestione migliorata delle connessioni, Gestore connessione Desktop remoto è ora in grado di gestire oltre 10.000 richieste di accesso simultanee, come accade durante i picchi di accessi. I miglioramenti apportati a Gestore connessione Desktop remoto semplificano anche la manutenzione della distribuzione, grazie alla possibilità di aggiungere server più rapidamente nell'ambiente.

Altre informazioni: [Prestazioni migliorate di Gestore connessione Desktop remoto](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10: nuove funzionalità integrate nel protocollo
RDP 10 usa ora il codec H.264/AVC 444, per un'ottimizzazione di video e testo. Con questa versione è supportato anche l'uso remoto della penna. Grazie a queste funzionalità, le sessioni remote sono ancora più simili a una sessione locale.  

Altre informazioni: [Miglioramenti di AVC/H.264 in RDP 10 in Windows 10 e Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Desktop di sessioni personali: disponibilità di singoli desktop per qualsiasi utente finale
I desktop di sessioni personali rappresentano un nuovo metodo di hosting del desktop personale nel cloud. I privilegi amministrativi e gli host sessione dedicati rimuovono la complessità dell'hosting di ambienti in cui gli utenti vogliono gestire il desktop come se fosse il loro personale.

Altre informazioni: [Desktop di sessioni personali](rds-personal-session-desktops.md)
