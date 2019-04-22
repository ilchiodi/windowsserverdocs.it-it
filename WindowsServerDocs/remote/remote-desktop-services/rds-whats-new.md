---
title: Quali sono le novità in Servizi Desktop remoto
description: Contiene le descrizioni delle nuove funzionalità di servizi desktop remoto in Windows Server 2016.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816312"
---
# <a name="whats-new-in-remote-desktop-services"></a>Quali sono le novità in Servizi Desktop remoto

Servizi Desktop remoto (RDS) basato su Windows Server 2016 è una piattaforma di virtualizzazione abilitare una vasta gamma di scenari di clienti. Miglioramenti della soluzione complessiva di servizi desktop remoto incorpora il lavoro svolto da team di Desktop remoto e altri partner di tecnologia Microsoft. Gli scenari e le tecnologie seguenti sono nuove o migliorate in Windows Server 2016.

Assicurarsi inoltre di estrarre la sessione da Ignite 2016: [Sfrutta i miglioramenti di RDS in Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). In questo video, il team del prodotto esamina tutte le funzionalità nuove e migliorate in Servizi Desktop remoto, incluso il supporto GPU virtualizzata. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilità delle applicazioni - Windows Server 2016 e Windows 10
Basato sulle stesse basi di Windows 10, Windows Server 2016 non solo ha lo stesso aspetto e comportamento previsto da un desktop, ma può anche eseguire molte delle stesse applicazioni. Associazione di Windows Server 2016 con le funzionalità di grafica (sotto) offre un ambiente per tutti gli utenti di essere produttivi. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Database SQL di Azure - nuovo database per l'ambiente a disponibilità elevata
Gestore connessione desktop remoto è in grado di archiviare tutte le informazioni di distribuzione (ad esempio, stati di connessione e i mapping utente/host) in un database SQL condiviso, ad esempio un database SQL di Azure. Scartare il manuale di distribuzione di SQL Server gruppo di disponibilità AlwaysOn, recuperare la stringa di connessione al database SQL di Azure e iniziare a usare l'ambiente a disponibilità elevata.

Altre informazioni: [Usare database SQL di Azure per l'ambiente di disponibilità elevata di Gestore connessione Desktop remoto](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Grafica - possibile soddisfare varie esigenze grafiche in vari scenari
Grazie a discreti dispositivo assegnazione Hyper-V, è ora possibile mappare le GPU in un computer host direttamente a una macchina virtuale deve essere utilizzato da applicazioni che richiedono GPU. Sono anche stati apportati miglioramenti nella GPU virtualizzata RemoteFX, incluso il supporto per 4.4 OpenGL, OpenCL 1.1, risoluzione KB, 4 e macchine virtuali Windows Server.

Altre informazioni: [Assegnazione dispositivo discreti](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Gestore connessione desktop remoto - connessione migliorata gestito durante il numero elevato di accesso
Con la gestione connessione migliorata, Gestore connessione desktop remoto è ora in grado di gestire più di 10.000 richieste di accesso simultanei, a volte usate durante il "numero elevato di accesso". Il Gestore connessione desktop remoto migliorato consente anche di manutenzione della distribuzione più semplice con la possibilità di più aggiungere rapidamente server nuovamente nell'ambiente di.

Altre informazioni: [Miglioramento delle prestazioni di Gestore connessione Desktop remoto](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - nuove funzionalità incorporate nel protocollo
RDP 10 ora Usa il codec video H.264/AVC 444, ottimizzare in modo appropriato tra testo e video. Con questa versione, .NET remoting penna è anche supportato. Con queste funzionalità, le sessioni remote avviare sentirsi ancora più come una sessione locale.  

Altre informazioni: [Miglioramenti di AVC/H.264 10 RDP in Windows 10 e Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Desktop di sessioni personali - fornire singoli desktop per qualsiasi utente finale
Desktop personali sessione è un nuovo modo per avere il proprio desktop personali ospitati automaticamente nel cloud. Privilegi amministrativi e rimuove gli host sessione dedicata la complessità degli ambienti in cui gli utenti vogliono gestire il desktop, ad esempio, di hosting è i propri.

Altre informazioni: [Desktop di sessioni personali](rds-personal-session-desktops.md)
