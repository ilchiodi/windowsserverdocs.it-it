---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: Elenco di controllo-configurazione AD FS per l'utilizzo di attestazioni da AD FS 1. x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f91944333da9ce4c1d78bbbf7b3652f1118e1f08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408495"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Elenco di controllo: Configurazione AD FS per inviare attestazioni a una AD FS 1. x Servizio federativo

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Elenco di controllo: Configurazione AD FS per inviare attestazioni a una AD FS 1. x Servizio federativo  
Questo elenco di controllo include le attività necessarie per configurare il Active Directory Federation Services \(AD FS @ no__t-1 Servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un AD FS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configure AD FS inviare attestazioni @ no__t-1Checklist: Configurazione AD FS per inviare attestazioni a una AD FS 1. x Servizio federativo @ no__t-0  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome.|![configure AD FS per inviare](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[la pianificazione delle attestazioni per l'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima di poter ottenere l'interoperabilità con una versione precedente di AD FS, è necessario innanzitutto creare una relying party attendibilità nella AD FS Servizio federativo a AD FS 1. *x* servizio federativo. **Nota:** Non è possibile creare una relazione di trust con un AD FS 1. *x* servizio federativo tramite metadati federativi.<br /><br />Quando si configura il trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nella procedura guidata Aggiungi attendibilità componente per impostare questa relazione di trust per interagire con un AD FS 1. Servizio federativo *x* :<br /><br />1.  Nel **Selezionare un'origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2.  Nel **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3.  Nella pagina **Configura URL** , in **WS @ No__t-2FEDERATION passive URL**, digitare l' **URL dell'endpoint servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.<br />4.  Nella pagina **Configura identificatori** , in **identificatore attendibilità parte**, digitare l' **URI servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.|![configure AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare manualmente un trust della relying party](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel relying party attendibilità creato in precedenza, è necessario creare regole attestazione che accettano le attestazioni in ingresso estratte da un archivio di attributi e pass-through, filtrare o trasformarle in un tipo di attestazione ID nome che può essere riconosciuto e utilizzato da AD FS 1. *x* servizio federativo. **Nota:** Prima di creare questa regola, assicurarsi che nel set di regole attestazioni in cui si sta creando questa regola sia presente una regola che precede il primo, che estrae un protocollo di accesso Lightweight Directory \(LDAP @ no__t-1 attestazione attributo da un archivio di attributi. Questa attestazione verrà utilizzata come input per la regola creata per l'invio di un AD FS 1. *x*\-compatible Claim. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'attestazione compatibile ad FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del AD FS 1. *x* servizio federativo e avere l'amministratore della ad FS 1. *x* servizio federativo configurare una nuova attendibilità del partner account. Specificare anche l'amministratore con l'URI del servizio federativo \(nelle proprietà del servizio federativo\), WS\-Federation Passive URL dell'endpoint \(l'URL dell'endpoint del servizio federativo\), e un token esportato\-file del certificato di firma \(con la chiave pubblica solo\). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N\/A|  
  

