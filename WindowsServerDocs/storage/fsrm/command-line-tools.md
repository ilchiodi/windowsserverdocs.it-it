---
title: 'Strumenti da riga di comando Gestione risorse file server:'
description: Questo articolo descrive gli strumenti da riga di comando di Windows Server 2016
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7d36a30ca65ae88afdc11e516968c6f9900a5ae6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-command-line-tools"></a>Strumenti da riga di comando Gestione risorse file server:

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Gestione risorse file server viene installa i cmdlet di PowerShell [FileServerResourceManager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager) e i seguenti strumenti da riga di comando:

-   **Dirquota.exe**. Utilizzare per creare e gestire le quote, le quote automatiche e i modelli quota.
-   **Filescrn.exe**. Utilizzare per creare e gestire screening dei file, modelli screening dei file, eccezioni screening dei file e gruppi di file.
-   **Storrept.exe**. Utilizzare per configurare i parametri dei rapporti e generare rapporti di archiviazione su richiesta. Inoltre, utilizzare per creare attività rapporto, pianificabili utilizzando **schtasks.exe**.

È possibile utilizzare questi strumenti per gestire risorse di archiviazione in un computer locale o un computer remoto. Per ulteriori informazioni su questi strumenti da riga di comando, vedere i seguenti riferimenti:

-   **Dirquota**: <http://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**: <http://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**: <http://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> Per vedere la sintassi del comando e i parametri disponibili per un comando, eseguire il comando con il parametro <strong>/?</strong> .


## <a name="remote-management-using-the-command-line-tools"></a>Gestione remota tramite gli strumenti da riga di comando

Ogni strumento offre diverse opzioni per eseguire azioni simili a quelle disponibili nello snap-in di MMC di Gestione risorse file server. Per fare in modo che un comando esegua un'azione su un computer remoto anziché sul computer locale , utilizzare il parametro **/remote**:*ComputerName*.

Ad esempio,**Dirquota.exe** include un parametro **template export** per scrivere le impostazioni dei modelli quota in un file XML e un parametro **template import** per importare le impostazioni dei modelli dal file XML. L'aggiunta del parametro **/remote**:*ComputerName* al comando **Dirquota.exe template import** comando importerà i modelli dal file XML nel computer locale al computer remoto.

> [!Note]
> Quando si esegue gli strumenti da riga di comando con il parametro **/remote**:<em>ComputerName</em> per eseguire un'esportazione (o importazione) di modelli in un computer remoto, i modelli vengono scritti in (o copiati da) un file XML nel computer locale.

<br />

## <a name="additional-considerations"></a>Considerazioni aggiuntive 

Per gestire le risorse remote con gli strumenti da riga di comando:

-   È necessario effettuare l'accesso con un account di dominio che sia membro del gruppo **Amministratori** sul computer locale e sul computer remoto.
-   È necessario eseguire gli strumenti da riga di comando da una finestra del prompt dei comandi con privilegi elevati. Per aprire una finestra del prompt dei comandi con privilegi elevati, fare clic sul pulsante **Start**, scegliere **Tutti i programmi**, **Accessori**, fare clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliere **Esegui come amministratore**.
-   Sul computer remoto deve essere in esecuzione Windows Server e deve essere installato Gestione risorse file server.
-   L'eccezione **Gestione remota Gestione risorse file server** sul computer remoto deve essere abilitata. Abilitare questa eccezione tramite Windows Firewall nel Pannello di controllo.


## <a name="see-also"></a>Vedere anche

-   [Gestione delle risorse di archiviazione remota](managing-remote-storage-resources.md)