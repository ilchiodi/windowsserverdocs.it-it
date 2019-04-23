---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Elenco di controllo: configurazione di ADFS per utilizzare attestazioni AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828292"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni AD FS 1.x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni AD FS 1.x  
Questo elenco di controllo include le attività necessarie per la configurazione di Active Directory Federation Services \(ADFS\) servizio federativo in Windows Server 2012 per utilizzare le attestazioni inviate da un'istanza di ADFS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![utilizzare attestazioni ADFS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Configurazione di ADFS per utilizzare attestazioni AD FS 1.x**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Piano per l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e apprendere che informazioni sull'ID nome del tipo di attestazione.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione dell'interoperabilità con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Prima è possibile interagire con una versione precedente di ADFS, è innanzitutto necessario creare un trust del provider di attestazioni nel servizio federativo AD FS. **Nota:** È possibile creare una relazione di trust con ADFS 1. *x* servizio federativo utilizzando i metadati della federazione.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario effettuare le operazioni seguenti in attestazioni Provider attendibilità Installazione guidata per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* servizio federativo:<br /><br />1.  Nel **Selezionare un'origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2.  Nel **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3.  Nel **Configura URL** nella pagina **WS\-federazione passiva URL**, digitare il **URL dell'endpoint del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.<br />4.  Nel **Configura identificatori** nella pagina **identificatore trust del provider di attestazioni**, digitare il **URI del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare un'attestazioni Provider attendibilità manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Nell'attendibilità del provider di attestazioni creato in precedenza, è necessario creare una regola di attestazione che avranno le attestazioni in ingresso da AD FS 1.x Federation Service e pass-through, filtrare oppure li trasforma in un tipo di attestazione ID nome.<br /><br />Quando il tipo di attestazione ID nome è stato passato mediante, filtrati o trasformati, può essere utilizzato come input per un'altra regola o le regole in modo che può essere riconosciuto e utilizzato dal servizio federativo ADFS in Windows Server 2012.|![utilizzare attestazioni ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Utilizzare le attestazioni di ADFS](media/icon_checkboxo.gif)|Contattare l'amministratore di AD FS 1. *x* servizio federativo e che l'amministratore di AD FS 1. *x* servizio federativo impostare una nuova attendibilità del partner risorse. Specificare anche l'amministratore con l'URI del servizio federativo \(nelle proprietà del servizio federativo\), l'URL dell'endpoint del servizio federativo e un token esportato\-file del certificato di firma \(con la chiave pubblica solo\). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N\/A|  
  

