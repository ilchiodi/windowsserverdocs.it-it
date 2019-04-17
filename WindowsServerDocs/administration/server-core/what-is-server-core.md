---
title: Che cos'è Server Core?
description: Informazioni sull'opzione di installazione Server Core in Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718535"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Che cos'è l'opzione di installazione Server Core in Windows Server?

> Si applica a: Windows Server (Channel semi-annuale) e Windows Server 2016

L'opzione Server Core è un'opzione di installazione minimi che è disponibile quando si distribuisce l'edizione Standard o Datacenter di Windows Server. Server Core include la maggior parte dei ma non tutti i ruoli del server. Server Core ha un footprint del disco e pertanto una superficie di attacco dimensioni inferiore a causa di una base di codice più piccola. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Server (Core) e Server con esperienza Desktop 
Quando si installa Windows Server, è installare solo i ruoli del server che si sceglie: ciò consente di ridurre il footprint globale per Windows Server. Tuttavia, il Server con l'opzione di installazione di esperienza Desktop ancora consente di installare molti servizi e altri componenti che spesso non sono necessarie per uno scenario di utilizzo particolare. 

È in Server Core ha luogo: installazione Server Core Elimina tutti i servizi e altre caratteristiche che non sono essenziali per il supporto di determinati comunemente utilizzano ruoli del server. Ad esempio, un server Hyper-V non ha necessità di un'interfaccia utente grafica (GUI), dal momento che è possibile gestire virtualmente tutti gli aspetti di Hyper-V dalla riga di comando utilizzando Windows PowerShell o in remoto tramite la tecnologia Hyper-V Manager. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La differenza Server Core - funzionalità di base senza gli increspature
Al termine dell'installazione Server Core in un sistema e di accesso per la prima volta, è possibile aprire per un certo una sorpresa. La differenza principale tra il Server con l'opzione di installazione di esperienza Desktop e Server Core è che Server Core non include i seguenti pacchetti shell GUI:

- Microsoft-Windows-Shell-pacchetto del Server
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

In altre parole, non esiste **alcun desktop** in Server Core, per impostazione predefinita. Mantenendo le funzionalità necessarie per supportare le tradizionali applicazioni aziendali e dei carichi di lavoro basate sui ruoli Server Core non dispone di un'interfaccia desktop tradizionale. In realtà, Server Core è progettato per essere gestiti in remoto tramite la riga di comando, PowerShell o uno strumento GUI (ad esempio, o [Windows interfaccia di amministrazione](../../manage/windows-admin-center/overview.md) [remota del](../../remote/remote-server-administration-tools.md) ).

Oltre a interfaccia utente non Server Core è differente dal Server con esperienza Desktop nei modi seguenti:

- Server Core non dispone di alcun accesso facilitato
- Nessun configurazione guidata (out-of-casella-experience) per la configurazione di Server Core
- Nessun supporto audio

Nella tabella seguente sono disponibili *in locale* nel Server Core vs Server con esperienza Desktop delle applicazioni. **Importante**: nella maggior parte dei casi, le applicazioni che vengono elencate come "non disponibile" seguito possono essere eseguiti in remoto da un computer client Windows e vengono utilizzate per gestire l'installazione Server Core.

> [!NOTE]
> In questo elenco è destinato Guida di riferimento rapido - non è progettato per essere un elenco completo.


| Applicazione                     | Server Core     | Server con Esperienza desktop |
|------------------------------------|-----------------|--------------------------------|
| Al prompt dei comandi                     | disponibile       | disponibile                      |
| Windows PowerShell / Microsoft .NET | disponibile       | disponibile                      |
| Perfmon.exe                        | non disponibile  | disponibile                      |
| WinDbg (GUI)                         | supportato       | supportato                      |
| Resmon.exe                         | non disponibile   | disponibile                      |
| Regedit                            | disponibile       | disponibile                      |
| Fsutil.exe                         | disponibile       | disponibile                      |
| Disksnapshot.exe                   | non disponibile   | disponibile                      |
| Diskpart.exe                       | disponibile       | disponibile                      |
| Diskmgmt                       | non disponibile   | disponibile                      |
| Devmgmt                        | non disponibile   | disponibile                      |
| Server Manager                     | non disponibile  | disponibile                      |
| Mmc.exe                            | non disponibile   | disponibile                      |
| Eventvwr                           | non disponibile  | disponibile                      |
| Wevtutil (query evento)           | disponibile       | disponibile                      |
| Services. msc                       | non disponibile   | disponibile                      |
| Pannello di controllo                      | non disponibile   | disponibile                      |
| Aggiornamento di Windows (GUI)                 | non disponibile | disponibile                      |
| In Esplora risorse                   | non disponibile   | disponibile                      |
| Barra delle applicazioni                            | non disponibile   | disponibile                      |
| Notifiche sulla barra delle applicazioni              | non disponibile   | disponibile                      |
| Taskmgr                            | disponibile       | disponibile                      |
| Internet Explorer o Edge          | non disponibile   | disponibile                      |
| Sistema di Guida incorporato               | non disponibile   | disponibile                      |
| Shell di Windows 10                   | non disponibile   | disponibile                      |
| Windows Media Player               | non disponibile   | disponibile                      |
| PowerShell                         | disponibile       | disponibile                      |
| PowerShell ISE                     | non disponibile   | disponibile                      |
| IME PowerShell                     | disponibile       | disponibile                      |
| Mstsc.exe                          | non disponibile   | disponibile                      |
| Servizi Desktop remoto            | disponibile       | disponibile                      |
| Console di gestione di Hyper-V                    | non disponibile  | disponibile                      |


Per ulteriori informazioni sulla quale *è* incluso nel Server Core, vedere [ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core](server-core-roles-and-services.md). E per sapere quali *non è* incluso nel Server Core, vedere [ruoli, servizi ruolo e funzionalità non incluse nel Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introduzione all'utilizzo di Server Core
Utilizzare le informazioni seguenti per installare, configurare e gestire l'opzione di installazione Server Core di Windows Server.

Installazione Server Core: 
- [Ruoli, servizi ruolo e funzionalità incluse nel Server Core](server-core-roles-and-services.md)
- [Ruoli, servizi ruolo e funzionalità non in Server Core](server-core-removed-roles.md)
- [Installare l'opzione di installazione Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurare Server Core con lo strumento SConfig](../../get-started/sconfig-on-ws2016.md)

Utilizzo di Server Core:
- [Attività di amministrazione Server Core di base utilizzando Windows PowerShell o la riga di comando](server-core-administer.md)
- [Gestire i Server Core](server-core-manage.md)
- [Applicazione delle patch Server Core](server-core-servicing.md)
- [Configurare i file dump di memoria](server-core-memory-dump.md)