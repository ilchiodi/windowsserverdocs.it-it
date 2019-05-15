---
title: What ' s Server Core?
description: Scopri l'opzione di installazione Server Core in Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885602"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Che cos'è l'opzione di installazione Server Core in Windows Server?

> Si applica a: Windows Server (canale semestrale) e Windows Server 2016

L'opzione Server Core è un'opzione di installazione minima che è disponibile quando si distribuisce l'edizione Standard o Datacenter di Windows Server. Base del server include la maggior parte ma non tutti i ruoli del server. Server Core ha un footprint del disco e pertanto una superficie di attacco più piccola a causa di una codebase più piccola. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Server (Core) rispetto a Server con esperienza Desktop 
Quando si installa Windows Server, si installa solo i ruoli del server che si sceglie: ciò consente di ridurre il footprint complessivo per Windows Server. Tuttavia, il Server con opzione di installazione di esperienza Desktop installa ancora molti servizi e altri componenti che spesso non sono necessari per un determinato scenario di utilizzo. 

Questo è dove entra in gioco la base del Server: installazione Server Core consente di eliminare tutti i servizi e altre funzionalità che non sono essenziali per il supporto di alcuni comunemente usati i ruoli del server. Ad esempio, un server Hyper-V non è necessario un'interfaccia utente grafica (GUI), poiché è possibile gestire praticamente tutti gli aspetti di Hyper-V dalla riga di comando tramite Windows PowerShell o in modalità remota tramite la gestione di Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La differenza di Server Core - funzionalità di base senza gli increspature
Al termine dell'installazione Server Core in un sistema e di accesso per la prima volta, viene visualizzato per un po' di stupore. La differenza principale tra il Server con opzione di installazione di esperienza Desktop e Server Core è che Server Core non include i pacchetti di shell GUI seguenti:

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

In altre parole, è presente **desktop non** in Server Core, per impostazione predefinita. Pur mantenendo le funzionalità necessarie per supportare le applicazioni aziendali tradizionali e i carichi di lavoro in base al ruolo, Server Core non è un'interfaccia desktop tradizionale. Invece, Server Core è progettato per essere gestiti in remoto tramite la riga di comando, PowerShell o un strumento GUI (ad esempio [RSAT](../../remote/remote-server-administration-tools.md) oppure [Windows Admin Center](../../manage/windows-admin-center/overview.md)).

Oltre a alcuna interfaccia utente, Server Core è differente dal Server con esperienza Desktop nei modi seguenti:

- Base del server non dispone di tutti gli strumenti di accessibilità
- Nessuna configurazione guidata (out-of-finestra-experience) per la configurazione di Server Core
- Nessun supporto audio

Nella tabella seguente indica quali applicazioni sono disponibili *localmente* in Server Core e Server con esperienza Desktop. **Important**: Nella maggior parte dei casi, le applicazioni elencate come "non disponibile" di seguito possono essere eseguiti in modalità remota da un computer client Windows e consente di gestire l'installazione Server Core.

> [!NOTE]
> Questo elenco è concepito per riferimento rapido: non costituisce un elenco completo.


| Applicazione                     | Server Core     | Server con Esperienza desktop |
|------------------------------------|-----------------|--------------------------------|
| Prompt dei comandi                     | disponibile       | disponibile                      |
| Windows PowerShell o Microsoft .NET | disponibile       | disponibile                      |
| Perfmon.exe                        | non disponibile  | disponibile                      |
| Windbg (GUI)                         | supportata       | supportata                      |
| Resmon.exe                         | non disponibile   | disponibile                      |
| Regedit                            | disponibile       | disponibile                      |
| Fsutil.exe                         | disponibile       | disponibile                      |
| Disksnapshot.exe                   | non disponibile   | disponibile                      |
| Diskpart.exe                       | disponibile       | disponibile                      |
| Diskmgmt.msc                       | non disponibile   | disponibile                      |
| Devmgmt.msc                        | non disponibile   | disponibile                      |
| Server Manager                     | non disponibile  | disponibile                      |
| Mmc.exe                            | non disponibile   | disponibile                      |
| Eventvwr                           | non disponibile  | disponibile                      |
| Wevtutil (query di eventi)           | disponibile       | disponibile                      |
| Services.msc                       | non disponibile   | disponibile                      |
| Pannello di controllo                      | non disponibile   | disponibile                      |
| Windows Update (GUI)                 | non disponibile | disponibile                      |
| Esplora risorse                   | non disponibile   | disponibile                      |
| Barra delle applicazioni                            | non disponibile   | disponibile                      |
| Notifiche della barra delle applicazioni              | non disponibile   | disponibile                      |
| Taskmgr                            | disponibile       | disponibile                      |
| Internet Explorer o Microsoft Edge          | non disponibile   | disponibile                      |
| Sistema della Guida incorporato               | non disponibile   | disponibile                      |
| Windows 10 Shell                   | non disponibile   | disponibile                      |
| Windows Media Player               | non disponibile   | disponibile                      |
| PowerShell                         | disponibile       | disponibile                      |
| PowerShell ISE                     | non disponibile   | disponibile                      |
| IME di PowerShell                     | disponibile       | disponibile                      |
| Mstsc.exe                          | non disponibile   | disponibile                      |
| Servizi Desktop remoto            | disponibile       | disponibile                      |
| Console di gestione di Hyper-V                    | non disponibile  | disponibile                      |


Per altre informazioni su cosa *viene* inclusa in Server Core, vedere [ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core](server-core-roles-and-services.md). E per informazioni su cosa *non è* inclusa in Server Core, vedere [ruoli, servizi ruolo e funzionalità non incluse in Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introduzione all'uso di Server Core
Usare le informazioni seguenti per installare, configurare e gestire l'opzione di installazione Server Core di Windows Server.

Installazione Server Core: 
- [I ruoli, servizi ruolo e funzionalità incluse in Server Core](server-core-roles-and-services.md)
- [I ruoli, servizi ruolo e funzionalità non in Server Core](server-core-removed-roles.md)
- [Installare l'opzione di installazione Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurazione di base del Server con lo strumento SConfig](../../get-started/sconfig-on-ws2016.md)

Uso di Server Core:
- [Attività di amministrazione Server Core di base tramite Windows PowerShell o la riga di comando](server-core-administer.md)
- [Gestire Server Core](server-core-manage.md)
- [L'applicazione di patch Server Core](server-core-servicing.md)
- [Configurare i file di dump di memoria](server-core-memory-dump.md)