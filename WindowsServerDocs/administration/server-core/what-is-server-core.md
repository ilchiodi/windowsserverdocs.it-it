---
title: Informazioni su Server Core
description: Informazioni sull'opzione di installazione dei componenti di base del server in Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 269be253367ba2bc692a5903e7d519a40f487d8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383344"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Qual è l'opzione di installazione dei componenti di base del server in Windows Server?

> Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

L'opzione componenti di base del server è un'opzione di installazione minima disponibile quando si distribuisce l'edizione standard o Datacenter di Windows Server. Server Core include la maggior parte, ma non tutti i ruoli del server. Server Core ha un footprint di disco inferiore e pertanto una superficie di attacco più piccola a causa di una codebase di dimensioni inferiori. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Server (Core) vs server con esperienza desktop 
Quando si installa Windows Server, si installano solo i ruoli del server scelti. Ciò consente di ridurre il footprint complessivo per Windows Server. Tuttavia, l'opzione di installazione server con esperienza Desktop installa ancora molti servizi e altri componenti spesso non necessari per uno scenario di utilizzo specifico. 

Ecco dove entra in gioco Server Core: l'installazione dei componenti di base del server Elimina tutti i servizi e altre funzionalità che non sono essenziali per il supporto di determinati ruoli server usati comunemente. Ad esempio, un server Hyper-V non necessita di un'interfaccia utente grafica (GUI), perché è possibile gestire virtualmente tutti gli aspetti di Hyper-V dalla riga di comando usando Windows PowerShell o in modalità remota tramite la console di gestione di Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>Differenza tra i componenti di base del server e le funzionalità principali senza fronzoli
Quando si completa l'installazione di Server Core in un sistema e si accede per la prima volta, ci si trova in una sorpresa. La differenza principale tra l'opzione di installazione server con esperienza desktop e Server Core è che Server Core non include i pacchetti della Shell GUI seguenti:

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-GUI-Mgmt-Package
- Microsoft-Windows-Server-GUI-strumenti di amministrazione
- Microsoft-Windows-Cortana-PAL-desktop-Package

In altre parole, non è disponibile **alcun desktop** in Server Core, da progettazione. Mantenendo le funzionalità necessarie per supportare le applicazioni aziendali tradizionali e i carichi di lavoro basati sui ruoli, Server Core non dispone di un'interfaccia desktop tradizionale. Al contrario, Server Core è progettato per essere gestito in modalità remota tramite la riga di comando, PowerShell o uno strumento GUI (ad esempio, amministrazione [remota](../../remote/remote-server-administration-tools.md) o [Windows](../../manage/windows-admin-center/overview.md)).

Oltre a nessuna interfaccia utente, Server Core differisce anche dal server con esperienza desktop nei modi seguenti:

- Server Core non dispone di strumenti di accessibilità
- Nessuna OOBE (impostazione predefinita) per la configurazione dei componenti di base del server
- Nessun supporto audio

La tabella seguente mostra quali applicazioni sono disponibili *localmente* in Server Core vs server con esperienza desktop. **Important**: Nella maggior parte dei casi, le applicazioni elencate come "non disponibili" di seguito possono essere eseguite in modalità remota da un computer client Windows e utilizzate per gestire l'installazione dei componenti di base del server.

> [!NOTE]
> Questo elenco è destinato a un riferimento rapido, non è destinato a essere un elenco completo.


| Applicazione                     | Server Core     | Server con Esperienza desktop |
|------------------------------------|-----------------|--------------------------------|
| Prompt dei comandi                     | disponibile       | disponibile                      |
| Windows PowerShell/Microsoft .NET | disponibile       | disponibile                      |
| Perfmon. exe                        | non disponibile  | disponibile                      |
| WinDbg (GUI)                         | supportata       | supportata                      |
| Resmon. exe                         | non disponibile   | disponibile                      |
| Regedit                            | disponibile       | disponibile                      |
| Fsutil. exe                         | disponibile       | disponibile                      |
| Disksnapshot. exe                   | non disponibile   | disponibile                      |
| Diskpart.exe                       | disponibile       | disponibile                      |
| Diskmgmt. msc                       | non disponibile   | disponibile                      |
| Devmgmt. msc                        | non disponibile   | disponibile                      |
| Server Manager                     | non disponibile  | disponibile                      |
| MMC. exe                            | non disponibile   | disponibile                      |
| Eventvwr                           | non disponibile  | disponibile                      |
| Wevtutil (query di eventi)           | disponibile       | disponibile                      |
| Services. msc                       | non disponibile   | disponibile                      |
| Pannello di controllo                      | non disponibile   | disponibile                      |
| Windows Update (GUI)                 | non disponibile | disponibile                      |
| Esplora risorse                   | non disponibile   | disponibile                      |
| Barra delle applicazioni                            | non disponibile   | disponibile                      |
| Notifiche della barra delle applicazioni              | non disponibile   | disponibile                      |
| Taskmgr                            | disponibile       | disponibile                      |
| Internet Explorer o Microsoft Edge          | non disponibile   | disponibile                      |
| Sistema della Guida incorporato               | non disponibile   | disponibile                      |
| Shell di Windows 10                   | non disponibile   | disponibile                      |
| Windows Media Player               | non disponibile   | disponibile                      |
| PowerShell                         | disponibile       | disponibile                      |
| PowerShell ISE                     | non disponibile   | disponibile                      |
| IME per PowerShell                     | disponibile       | disponibile                      |
| Mstsc. exe                          | non disponibile   | disponibile                      |
| Servizi Desktop remoto            | disponibile       | disponibile                      |
| Console di gestione di Hyper-V                    | non disponibile  | disponibile                      |


Per ulteriori informazioni su ciò che *è* incluso in Server Core, vedere [ruoli, servizi ruolo e funzionalità inclusi in Windows Server-Server Core](server-core-roles-and-services.md). Per informazioni sugli elementi *non* inclusi in Server Core, vedere [ruoli, servizi ruolo e funzionalità non inclusi in Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introduzione all'uso di Server Core
Usare le informazioni seguenti per installare, configurare e gestire l'opzione di installazione dei componenti di base del server di Windows Server.

Installazione Server Core: 
- [Ruoli, servizi ruolo e funzionalità inclusi in Server Core](server-core-roles-and-services.md)
- [Ruoli, servizi ruolo e funzionalità non in Server Core](server-core-removed-roles.md)
- [Installare l'opzione di installazione dei componenti di base del server](../../get-started/getting-started-with-server-core.md)
- [Configurare Server Core con lo strumento SConfig](../../get-started/sconfig-on-ws2016.md)

Uso di Server Core:
- [Attività di amministrazione di base del server core usando Windows PowerShell o la riga di comando](server-core-administer.md)
- [Gestire Server Core](server-core-manage.md)
- [Applicazione di patch a Server Core](server-core-servicing.md)
- [Configurare i file dump della memoria](server-core-memory-dump.md)