---
title: Installare Windows Admin Center
description: Come installare Windows Admin Center in un PC Windows o un server.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 496a67cfb93bf9b42b202f6a61211a085a9253b6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296693"
---
# Installare Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## Determinare il tipo di installazione

Esamina le [Opzioni di installazione](..\plan\installation-options.md) che include i [sistemi operativi supportati](..\plan\installation-options.md#supported-operating-systems-installation). Per installare Windows Admin Center in una macchina virtuale in Azure, vedi [Distribuisci Windows Admin Center in Azure](../azure/deploy-wac-in-azure.md).

## Installare su Windows10

Quando si installa Windows Admin Center in Windows 10, utilizza la porta 6516 per impostazione predefinita, ma è possibile specificare una porta diversa. Puoi anche creare un collegamento sul desktop e consentire a Windows Admin Center di gestire il TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Quando si avvia Windows Admin Center dal menu **Start** , viene aperto nel browser predefinito.

Quando si avvia Windows Admin Center per la prima volta, vedrai un'icona nell'area di notifica del desktop. Fai clic con il pulsante destro su questa icona e scegli **Apri** per aprire lo strumento nel browser predefinito oppure scegli **Esci** per chiudere il processo in background.

## Effettuare l'installazione su Windows Server con l'esperienza desktop

In Windows Server Windows Admin Center è installato come un servizio di rete. Devi specificare la porta su cui è in ascolto il servizio e richiede un certificato per il protocollo HTTPS. Un certificato autofirmato può essere creato dal programma di installazione a scopo di test oppure puoi specificare l'identificazione personale di un certificato già installato nel computer. Se utilizzi il certificato generato, corrisponderà al nome DNS del server. Se si utilizza il proprio certificato, assicurati che il nome indicato nel certificato corrisponda al nome di computer (caratteri jolly non sono supportati i certificati). Potrai anche scegliere di consentire a Windows Admin Center di gestire TrustedHosts.

> [!NOTE]
> La modifica di TrustedHosts è necessaria in un ambiente di gruppo di lavoro o quando si usano credenziali di amministratore locale in un dominio. Se scegli di ignorare questa impostazione, dovrai [configurare manualmente TrustedHosts](../support/troubleshooting.md#configure-trustedhosts).

Una volta completata l'installazione, Apri un browser da un computer remoto e passa all'URL presentati nell'ultimo passaggio del programma di installazione.

> [!WARNING]
> I certificati generati automaticamente scadono 60 giorni dopo l'installazione.

## Installare nel Server Core

Se disponi di un'installazione dei componenti di base per il server di Windows Server, puoi installare Windows Admin Center dal prompt dei comandi (in esecuzione come amministratore). Specifica una porta e un certificato SSL utilizzando rispettivamente gli argomenti `SME_PORT` e `SSL_CERTIFICATE_OPTION`. Se prevedi di utilizzare un certificato esistente, utilizza `SME_THUMBPRINT` per specificare la relativa identificazione personale.

> [!WARNING]
> L'installazione di Windows Admin Center verrà riavviato il servizio WinRM che interromperà tutte le sessioni remote PowerShells. È consigliabile che installare da un Cmd locale o PowerShell. Se stai installando con una soluzione di automazione che verrebbe ripartita mediante il servizio WinRM riavvio, puoi aggiungere il parametro ```RESTART_WINRM=0``` per l'installazione deve essere riavviato argomenti, ma WinRM per Windows Admin Center alla funzione.

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

## Aggiornamento di Windows Admin Center

Puoi aggiornare le versioni non in anteprima di Windows Admin Center tramite Microsoft Update o installare manualmente. 

Le impostazioni vengono mantenute durante l'aggiornamento a una nuova versione di Windows Admin Center. Abbiamo non supporta ufficialmente versioni di Insider Preview l'aggiornamento di Windows Admin Center: pensiamo che è preferibile eseguire un'installazione pulita, ma abbiamo non bloccarlo.