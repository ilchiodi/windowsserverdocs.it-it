---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: Elenco di controllo-configurazione AD FS per l'utilizzo di attestazioni da AD FS 1. x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408482"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni ADFS 1. x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Elenco di controllo: configurazione di AD FS per l'utilizzo di attestazioni da AD FS 1. x  
Questo elenco di controllo include le attività necessarie per configurare il Active Directory Federation Services \(AD FS\) Servizio federativo in Windows Server 2012 per utilizzare le attestazioni inviate da un AD FS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![utilizzo delle attestazioni da AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione ad FS per l'utilizzo di attestazioni da ad FS 1. x**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome.|![utilizzo delle attestazioni da AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione per l'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Prima di poter interagire con una versione precedente di AD FS, è necessario creare prima di tutto un trust del provider di attestazioni nel Servizio federativo AD FS. **Nota:** Non è possibile creare una relazione di trust con un AD FS 1. *x* servizio federativo tramite metadati federativi.<br /><br />Quando si configura il trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nella procedura guidata Aggiungi attendibilità provider di attestazioni per configurare l'attendibilità per l'interoperabilità con un AD FS 1. Servizio federativo *x* :<br /><br />1. nella pagina **Seleziona origine dati** selezionare **immettere manualmente i dati sulla relazione di trust relying party**.<br />2. nella pagina **Scegli profilo** selezionare **ad FS profilo 1,0 e 1,1**.<br />3. nella pagina **Configura URL** , in **WS\-Federation Passive URL**, digitare l' **URL dell'endpoint servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.<br />4. nella pagina **Configura identificatori** , in **identificatore attendibilità provider di attestazioni**, digitare l' **URI servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.|![utilizzare attestazioni da AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare manualmente un trust del provider di attestazioni](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Nel trust del provider di attestazioni creato in precedenza, è necessario creare una regola attestazioni che consentirà di ottenere le attestazioni in ingresso dal AD FS 1. x Servizio federativo e pass-through, filtrare o trasformarle in un tipo di attestazione ID nome.<br /><br />Quando il tipo di attestazione ID nome è stato passato mediante, filtrati o trasformati, può essere utilizzato come input per un'altra regola o le regole in modo che può essere riconosciuto e utilizzato dal servizio federativo ADFS in Windows Server 2012.|![utilizzare attestazioni da AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'attestazione compatibile ad FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Contattare l'amministratore del AD FS 1. *x* servizio federativo e avere l'amministratore della ad FS 1. *x* servizio federativo configurare una nuova attendibilità del partner risorse. Specificare anche l'amministratore con l'URI del servizio federativo \(nelle proprietà del servizio federativo\), l'URL dell'endpoint del servizio federativo e un token esportato\-file del certificato di firma \(con la chiave pubblica solo\). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N\/A|  
  

