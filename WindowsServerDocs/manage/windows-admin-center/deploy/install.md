---
title: Installare Windows Admin Center
description: Come installare Windows Admin Center in un PC Windows o in un server in modo che più utenti possono accedere a Windows Admin Center tramite un web browser.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a9eb7944cd35dfa68e3c36cdc6c016f483a9f1e1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811960"
---
# <a name="install-windows-admin-center"></a>Installare Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center anteprima

In questo argomento viene descritto come installare Windows Admin Center in un PC Windows o in un server in modo che più utenti possono accedere a Windows Admin Center tramite un web browser.

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinare il tipo di installazione

Esaminare i [le opzioni di installazione](../plan/installation-options.md) che include il [sistemi operativi supportati](../plan/installation-options.md#supported-operating-systems-installation). Per installare Windows Admin Center in una macchina virtuale in Azure, vedere [Deploy Windows Admin Center in Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Installare su Windows 10

Quando si installa Windows Admin Center in Windows 10, utilizza la porta 6516 per impostazione predefinita, ma è possibile specificare una porta diversa. Puoi anche creare un collegamento sul desktop e consentire a Windows Admin Center di gestire il TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Quando si avvia Windows Admin Center dal menu **Start** , viene aperto nel browser predefinito.

Quando si avvia Windows Admin Center per la prima volta, vedrai un'icona nell'area di notifica del desktop. Fai doppio clic su questa icona e scegli **Apri** per aprire lo strumento nel browser predefinito oppure scegli **Esci** per chiudere il processo in background.

## <a name="install-on-windows-server-with-desktop-experience"></a>Effettuare l'installazione su Windows Server con l'esperienza desktop

In Windows Server Windows Admin Center è installato come un servizio di rete. Devi specificare la porta su cui è in ascolto il servizio e richiede un certificato per il protocollo HTTPS. Un certificato autofirmato può essere creato dal programma di installazione a scopo di test oppure puoi specificare l'identificazione personale di un certificato già installato nel computer. Se utilizzi il certificato generato, corrisponderà al nome DNS del server. Se si usa il proprio certificato, assicurarsi che il nome specificato nel certificato corrisponde al nome del computer (carattere jolly non sono supportati i certificati). Si viene fornita anche la possibilità di gestire Windows Admin Center il TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Una volta completata l'installazione, aprire un browser da un computer remoto e passare all'URL presentato nel passaggio precedente del programma di installazione.

> [!WARNING]
> I certificati generati automaticamente scadono 60 giorni dopo l'installazione.

## <a name="install-on-server-core"></a>Installare nel Server Core

Se disponi di un'installazione dei componenti di base per il server di Windows Server, puoi installare Windows Admin Center dal prompt dei comandi (in esecuzione come amministratore). Specifica una porta e un certificato SSL utilizzando rispettivamente gli argomenti `SME_PORT` e `SSL_CERTIFICATE_OPTION`. Se prevedi di utilizzare un certificato esistente, utilizza `SME_THUMBPRINT` per specificare la relativa identificazione personale.

> [!WARNING]
> Installazione di Windows Admin Center riavvierà il servizio Gestione remota Windows, che consente di dividere tutte le sessioni remote di PowerShell riportato. È consigliabile installare da un Cmd locale o da PowerShell. Se si installa una soluzione di automazione che viene interrotta dal riavvio del servizio di gestione remota Windows, è possibile aggiungere il parametro ```RESTART_WINRM=0``` per installare gli argomenti, ma WinRM è necessario riavviare Windows Admin Center alla funzione.

Esegui il comando seguente per installare Windows Admin Center e generare automaticamente un certificato autofirmato:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Esegui il comando seguente per installare Windows Admin Center con un certificato esistente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> Non richiamare `msiexec` da PowerShell utilizzando una notazione in percorso relativo punto-barra (ad esempio, `.\<WindowsAdminCenterInstallerName>.msi`). Tale notazione non è supportata, quindi l'installazione avrà esito negativo. Rimuovi il prefisso `.\` o specifica il percorso completo per il file MSI.

## <a name="updating-windows-admin-center"></a>L'aggiornamento Windows Admin Center

È possibile aggiornare le versioni non di anteprima di Windows Admin Center tramite Microsoft Update oppure installare manualmente. 

Le impostazioni vengono mantenute durante l'aggiornamento a una nuova versione di Windows Admin Center. Non è ufficialmente supportato l'aggiornamento Insider Preview le versioni di Windows Admin Center: Riteniamo che si consiglia di eseguire un'installazione pulita, ma è non bloccarlo.