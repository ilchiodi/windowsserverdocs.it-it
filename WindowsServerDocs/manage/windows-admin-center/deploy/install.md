---
title: Installare Windows Admin Center
description: Come installare Windows Admin Center in un PC Windows o un server in modo da consentire a più utenti di accedere a Windows Admin Center tramite un Web browser.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: cab128a3da9fa58c598cebcdf188058631c33977
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950003"
---
# <a name="install-windows-admin-center"></a>Installare Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Questo argomento illustra come installare Windows Admin Center in un PC Windows o un server in modo da consentire a più utenti di accedere a Windows Admin Center tramite un Web browser.

> [!Tip]
> Se non hai esperienza con Windows Admin Center,
> [scopri di più su questa app](../overview.md) o [scaricala ora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinare il tipo di installazione

Esamina le [opzioni di installazione](../plan/installation-options.md), inclusi i [sistemi operativi supportati](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems). Per installare Windows Admin Center in una macchina virtuale di Azure, vedi [Distribuire Windows Admin Center in Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Installare in Windows 10

Quando esegui l'installazione in Windows 10, Windows Admin Center usa la porta 6516 per impostazione predefinita, ma hai la possibilità di specificare una porta diversa. Puoi anche creare un collegamento sul desktop e consentire a Windows Admin Center di gestire TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, devi [configurare TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

All'avvio dal menu **Start**, Windows Admin Center viene aperto nel browser predefinito.

Quando avvii Windows Admin Center per la prima volta, vedrai un'icona nell'area di notifica del desktop. Fai clic con il pulsante destro del mouse su questa icona e scegli **Apri** per aprire lo strumento nel browser predefinito oppure **Esci** per chiudere il processo in background.

## <a name="install-on-windows-server-with-desktop-experience"></a>Eseguire l'installazione in Windows Server con esperienza desktop

In Windows Server, Windows Admin Center viene installato come servizio di rete. Devi specificare la porta su cui il servizio rimane in ascolto ed è necessario un certificato per HTTPS. Il programma di installazione può creare un certificato autofirmato per il test oppure puoi specificare l'identificazione personale di un certificato già installato nel computer. Se usi il certificato generato, il nome che contiene corrisponderà al nome DNS del server. Se usi un certificato personale, assicurati che il nome specificato nel certificato corrisponda al nome del computer (certificati con caratteri jolly non sono supportati). Hai anche la possibilità di consentire a Windows Admin Center di gestire TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, devi [configurare TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Al termine dell'installazione, apri un browser da un computer remoto e passa all'URL visualizzato nell'ultimo passaggio del programma di installazione.

> [!WARNING]
> I certificati generati automaticamente scadono 60 giorni dopo l'installazione.

## <a name="install-on-server-core"></a>Eseguire l'installazione in Server Core

Se hai un'installazione di Windows Server di tipo Server Core, puoi installare Windows Admin Center dal prompt dei comandi eseguendo il sistema come amministratore. Specifica una porta e un certificato SSL usando rispettivamente gli argomenti `SME_PORT` e `SSL_CERTIFICATE_OPTION`. Se prevedi di usare un certificato esistente, usa `SME_THUMBPRINT` per specificare la relativa identificazione personale.

> [!WARNING]
> L'installazione di Windows Admin Center riavvia il servizio WinRM, che determina l'interruzione di tutte le sessioni remote di PowerShell. Si consiglia di eseguire l'installazione da un Cmd locale o da PowerShell. Se l'installazione viene eseguita con una soluzione di automazione che verrebbe interrotta dal riavvio del servizio WinRM, puoi aggiungere il parametro ```RESTART_WINRM=0``` agli argomenti di installazione. Tuttavia, per il corretto funzionamento di Windows Admin Center, WinRM deve essere riavviato.

Esegui il comando seguente per installare Windows Admin Center e generare automaticamente un certificato autofirmato:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Esegui il comando seguente per installare Windows Admin Center con un certificato esistente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> Non richiamare `msiexec` da PowerShell usando la notazione del percorso relativo con punto e barra (ad esempio, `.\<WindowsAdminCenterInstallerName>.msi`). Tale notazione non è supportata e l'installazione non viene eseguita. Rimuovi il prefisso `.\` o specifica il percorso completo del file MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Aggiornamento a una nuova versione di Windows Admin Center

Puoi aggiornare le versioni non di anteprima di Windows Admin Center tramite Microsoft Update oppure con l'installazione manuale.

Quando si esegue l'aggiornamento a una nuova versione di Windows Admin Center, le impostazioni vengono mantenute. Ufficialmente non supportiamo l'aggiornamento delle versioni Insider Preview di Windows Admin Center, perché riteniamo che sia preferibile eseguire un'installazione pulita, ma la procedura non è bloccata.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Aggiornamento del certificato usato da Windows Admin Center

Quando Windows Admin Center è distribuito come servizio, devi fornire un certificato per HTTPS. Per aggiornare questo certificato in un secondo momento, esegui nuovamente il programma di installazione e scegli ```change```.
