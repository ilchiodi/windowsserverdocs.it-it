---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: Elenco di controllo - configurazione di ADFS per utilizzare attestazioni ADFS 1. x
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni ADFS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni ADFS 1. x  
Questo elenco di controllo include le attività necessarie per la configurazione del servizio federativo di \(AD FS\) Active Directory Federation Services in Windows Server 2012 per utilizzare le attestazioni inviate da un'istanza di ADFS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a una procedura, tornare a questo argomento dopo aver completato i passaggi in questa procedura in modo che è possibile procedere con le attività rimanenti nell'elenco di controllo.  
  
![utilizzare attestazioni ADFS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione di ADFS per utilizzare attestazioni ADFS 1. x**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![utilizzare attestazioni ADFS](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e apprendere che informazioni sull'ID nome del tipo di attestazione.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione per l'interoperabilità con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![utilizzare attestazioni ADFS](media/icon_checkboxo.gif)|Prima è possibile interagire con una versione precedente di ADFS, è innanzitutto necessario creare un trust del provider di attestazioni nel servizio federativo AD FS. **Nota:** è possibile creare una relazione di trust con ADFS 1. *x* servizio federativo tramite i metadati federativi.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti in attestazioni Provider attendibilità Installazione guidata per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* servizio federativo:<br /><br />1. scegliere il **Seleziona origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2. scegliere il **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3. nel **Configura URL** nella pagina **URL protocollo passivo WS-Federation**, tipo di **URL dell'endpoint del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.<br />4. nel **Configura identificatori** nella pagina **identificatore trust del provider di attestazioni**, tipo di **URI servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare un'attestazioni Provider attendibilità manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![utilizzare attestazioni ADFS](media/icon_checkboxo.gif)|Nell'attendibilità del provider di attestazioni creato in precedenza, è necessario creare una regola attestazione che verranno attestazioni in ingresso da AD FS 1. x servizio federativo e pass-through, filtrare oppure li trasforma in un tipo di attestazione ID nome.<br /><br />Quando il tipo di attestazione ID nome è stato passato mediante, filtrati o trasformati, può essere utilizzato come input per un'altra regola o le regole in modo che può essere riconosciuto e utilizzato dal servizio federativo AD FS in Windows Server 2012.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per l'invio di un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![utilizzare attestazioni ADFS](media/icon_checkboxo.gif)|Contattare l'amministratore di AD FS 1. *x* servizio federativo e che l'amministratore di AD FS 1. *x* servizio federativo impostare una nuova attendibilità del partner risorse. Inoltre, specificare l'amministratore con l'URI del servizio federativo \ (in properties\ servizio federativo), l'URL dell'endpoint del servizio federativo e un esportato token\ per la firma di file di certificato \ (con only\ chiave pubblica). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N \ / A|  
  

