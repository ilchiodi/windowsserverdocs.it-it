---
title: Gestire desktop virtuali
description: Informazioni su come gestire i desktop virtuali (VDI) in MultiPoint Services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 45bb3e98779bc27913c7e675a9c9db7e575d9d72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389593"
---
# <a name="manage-virtual-desktops"></a>Gestire desktop virtuali
Single computer VDI consente di configurare ogni stazione MultiPoint Services *locale* per connettersi a un sistema operativo guest Windows 10 Enterprise in esecuzione in una macchina virtuale Hyper-V (VM) nello stesso computer Servizi multipoint della stazione. È possibile personalizzare le stazioni di desktop virtuali con applicazioni che non possono essere installate su una versione di Windows Server.  
  
## <a name="enable-the-virtual-desktop-feature"></a>Abilitare la funzionalità desktop virtuale  
  
1.  Aprire Gestione MultiPoint, quindi fare clic sulla scheda **Desktop virtuali**.  
  
2.  In **VDI Tasks** (Attività VID), selezionare **Create virtual desktop** (Crea desktop virtuale), quindi scegliere il file ISO di Windows 10 Enterprise o un disco rigido virtuale.  
  
Il sistema viene riavviato e questa operazione può richiedere diversi minuti.  
  
## <a name="create-a-virtual-desktop-template"></a>Creare un modello di desktop virtuale  
  
1.  Aprire Gestione MultiPoint, quindi fare clic sulla scheda **Desktop virtuali**.  
  
2.  In **VDI Tasks** (Attività VDI), selezionare **Create virtual desktop** (Crea desktop virtuale), quindi scegliere il file ISO di Windows 10 Enterprise o un disco rigido virtuale.  
  
    Se si usa il DVD, il programma trova automaticamente il file WIM di Windows 10 Enterprise. In caso contrario, fare clic su **Browse** (Sfoglia), quindi passare al file ISO di Windows 10 Enterprise.  
  
    Se si vuole, è possibile modificare il prefisso. Il prefisso predefinito è il nome del computer host.  
  
    > [!NOTE]  
    > Il prefisso viene usato per assegnare un nome al modello e alle stazioni di desktop virtuali. Il nome del modello sarà prefisso \- t. I nomi delle stazioni di desktop virtuali saranno prefisso \-*n*, dove *n* è l'ID della stazione.  
  
4.  Immettere un nome e una password per l'account amministratore locale che dovrà essere usato per accedere a tutti i desktop virtuali creati dal modello e fare clic su **OK**.  
  
    La creazione del modello richiede diversi minuti.  
      
    A questo punto è possibile imparare a personalizzare il modello di desktop virtuale.  
      
    > [!NOTE]  
    > Se il server MultiPoint è appartenente a un dominio, nella finestra di dialogo viene popolato un altro campo che consente di specificare se le macchine virtuali create con il modello devono essere aggiunte a un dominio.   
  
## <a name="import-a-virtual-desktop-template"></a>Importare un modello di desktop virtuale  
Nel caso in cui sia stato creato un modello di desktop virtuale in un altro server MultiPoint, è possibile importare tale modello seguendo questa procedura.  

1.  Aprire Gestione MultiPoint, quindi fare clic sulla scheda **Desktop virtuali**.  
  
2.  In VDI Tasks (Attività VDI) fare clic su **Import virtual desktop template** (Importa modello di desktop virtuale).  
  
3.  Scegliere il modello e definire il percorso e il prefisso per il modello importato.  
  
## <a name="customize-the-virtual-desktop-template"></a>Personalizzare il modello di desktop virtuale  
Dopo aver creato il modello di desktop virtuale, è possibile personalizzarlo con le applicazioni, gli aggiornamenti software e configurare le impostazioni di sistema.   

1. Aprire Gestione MultiPoint, quindi fare clic sulla scheda **Desktop virtuali**.  
2. Scegliere il modello di desktop virtuale, quindi fare clic su **Customize virtual desktop template** (Personalizza modello di desktop virtuale).  
Il modello si aprirà in una finestra separata in cui saranno disponibili altre istruzioni e i passaggi più importanti per personalizzare il modello virtuale. Leggere attentamente queste istruzioni.  
  
## <a name="create-virtual-desktop-stations"></a>Creazione di stazioni desktop virtuali  
  
1.  Aprire Gestione MultiPoint in modalità stazione, quindi fare clic sulla scheda **Desktop virtuali**.  
  
    > [!NOTE]  
    > Se il sistema MultiPoint Services non è in modalità stazione, è necessario riavviarlo per completare la procedura.  
  
2.  Selezionare il modello di desktop virtuale nel riquadro di sinistra @ no__t-0hand. Il modello è denominato <prefix –t>.  
  
3.  In Template Tasks (Attività modello) fare clic su **Create virtual desktop stations** (Crea stazioni di desktop virtuali), quindi selezionare **OK**.  
  
    La creazione della stazione di desktop virtuale richiede diversi minuti.  
  
    > [!NOTE]  
    > Se una delle stazioni locali è attualmente connessa a una sessione @ no__t-0based Virtual Desktop, è necessario disconnettersi da tali stazioni per potersi connettere a una delle stazioni di desktop virtuali appena create.  
  
### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>Convalidare le stazioni desktop virtuali personalizzate appena create  
  
È possibile convalidare i desktop della stazione virtuale personalizzata accedendo a una o più stazioni di desktop virtuali usando un account amministratore locale o un account di dominio, quindi verificare che i desktop virtuali @ no__t-0based della nuova VM funzionino correttamente.  
  
## <a name="disable-virtual-desktops"></a>Disabilitare i desktop virtuali  
  
La funzionalità Hyper-V sarà disattivata se i desktop virtuali vengono disabilitati. Tutti gli utenti saranno disconnessi e il sistema sarà riavviato. Tutte le stazioni virtuali saranno assegnate a sessioni locali MultiPoint dopo il riavvio del sistema.  

1. Aprire Gestione MultiPoint in modalità stazione, quindi fare clic sulla scheda **Desktop virtuali**.  
  
2. In VDI Tasks (Attività VDI) fare clic su **Disable virtual desktops** (Disabilità desktop virtuali). 