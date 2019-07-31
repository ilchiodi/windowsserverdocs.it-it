---
title: Installare Windows Admin Center
description: Come installare l'interfaccia di amministrazione di Windows in un computer Windows o in un server in modo che più utenti possano accedere all'interfaccia di amministrazione di Windows tramite un Web browser.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e67102d1fa8b35d90e97df64cb8bd2991b205ad5
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658882"
---
# <a name="install-windows-admin-center"></a>Installare Windows Admin Center

> Si applica a Windows Admin Center, Windows Admin Center Preview

Questo argomento descrive come installare l'interfaccia di amministrazione di Windows in un computer Windows o in un server in modo che più utenti possano accedere all'interfaccia di amministrazione di Windows tramite un Web browser.

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinare il tipo di installazione

Esaminare le [Opzioni di installazione](../plan/installation-options.md) che includono i [sistemi operativi supportati](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems). Per installare l'interfaccia di amministrazione di Windows in una macchina virtuale in Azure, vedere Distribuire l'interfaccia [di amministrazione di Windows in Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Installare su Windows 10

Quando si installa Windows Admin Center in Windows 10, utilizza la porta 6516 per impostazione predefinita, ma è possibile specificare una porta diversa. Puoi anche creare un collegamento sul desktop e consentire a Windows Admin Center di gestire il TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Quando si avvia Windows Admin Center dal menu **Start** , viene aperto nel browser predefinito.

Quando si avvia Windows Admin Center per la prima volta, vedrai un'icona nell'area di notifica del desktop. Fai doppio clic su questa icona e scegli **Apri** per aprire lo strumento nel browser predefinito oppure scegli **Esci** per chiudere il processo in background.

## <a name="install-on-windows-server-with-desktop-experience"></a>Effettuare l'installazione su Windows Server con l'esperienza desktop

In Windows Server Windows Admin Center è installato come un servizio di rete. Devi specificare la porta su cui è in ascolto il servizio e richiede un certificato per il protocollo HTTPS. Un certificato autofirmato può essere creato dal programma di installazione a scopo di test oppure puoi specificare l'identificazione personale di un certificato già installato nel computer. Se utilizzi il certificato generato, corrisponderà al nome DNS del server. Se si usa un certificato personalizzato, assicurarsi che il nome specificato nel certificato corrisponda al nome del computer (i certificati con caratteri jolly non sono supportati). Si ha anche la possibilità di consentire a centro di amministrazione di Windows di gestire la TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Al termine dell'installazione, aprire un browser da un computer remoto e passare all'URL visualizzato nell'ultimo passaggio del programma di installazione.

> [!WARNING]
> I certificati generati automaticamente scadono 60 giorni dopo l'installazione.

## <a name="install-on-server-core"></a>Installare nel Server Core

Se disponi di un'installazione dei componenti di base per il server di Windows Server, puoi installare Windows Admin Center dal prompt dei comandi (in esecuzione come amministratore). Specifica una porta e un certificato SSL utilizzando rispettivamente gli argomenti `SME_PORT` e `SSL_CERTIFICATE_OPTION`. Se prevedi di utilizzare un certificato esistente, utilizza `SME_THUMBPRINT` per specificare la relativa identificazione personale.

> [!WARNING]
> L'installazione dell'interfaccia di amministrazione di Windows riavvierà il servizio WinRM, che eseguirà il server per tutte le sessioni remote di PowerShell. Si consiglia di installare da un cmd locale o da PowerShell. Se si sta installando con una soluzione di automazione che verrebbe interruppe dal riavvio del servizio gestione remota Windows, è possibile ```RESTART_WINRM=0``` aggiungere il parametro agli argomenti di installazione, ma WinRM deve essere riavviato per il funzionamento del centro di amministrazione di Windows.

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

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Aggiornamento a una nuova versione dell'interfaccia di amministrazione di Windows

Puoi aggiornare le versioni non di anteprima di Windows Admin Center tramite Microsoft Update oppure con l'installazione manuale.

Le impostazioni vengono mantenute quando si esegue l'aggiornamento a una nuova versione dell'interfaccia di amministrazione di Windows. Microsoft non supporta ufficialmente l'aggiornamento delle versioni di anteprima dell'interfaccia di amministrazione di Windows, ma è preferibile eseguire un'installazione pulita, ma non bloccarlo.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Aggiornamento del certificato utilizzato dall'interfaccia di amministrazione di Windows

Quando l'interfaccia di amministrazione di Windows è distribuita come servizio, è necessario fornire un certificato per HTTPS. Per aggiornare il certificato in un secondo momento, eseguire di nuovo il programma di installazione ```change```e scegliere.
