---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: Elenco di controllo-configurazione AD FS per l'utilizzo di attestazioni da AD FS 1. x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d522dddf99d71d8921db511af6f00488e424736e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854124"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Elenco di controllo: configurazione di ADFS per l'invio di attestazioni a un servizio federativo di AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Elenco di controllo: configurazione AD FS per l'invio di attestazioni a una AD FS 1. x Servizio federativo  
Questo elenco di controllo include le attività necessarie per configurare il Active Directory Federation Services \(AD FS\) Servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un AD FS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configurare AD FS per inviare attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione ad FS per l'invio di attestazioni a un ad FS 1. x servizio federativo**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome.|![configurare AD FS per l'invio](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[della pianificazione delle attestazioni per l'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima di poter ottenere l'interoperabilità con una versione precedente di AD FS, è necessario innanzitutto creare una relying party attendibilità nella AD FS Servizio federativo a AD FS 1. *x* servizio federativo. **Nota:** Non è possibile creare una relazione di trust con un AD FS 1. *x* servizio federativo tramite metadati federativi.<p>Quando si configura il trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nella procedura guidata Aggiungi attendibilità componente per impostare questa relazione di trust per interagire con un AD FS 1. Servizio federativo *x* :<p>1. nella pagina **Seleziona origine dati** selezionare **immettere manualmente i dati sulla relazione di trust relying party**.<br />2. nella pagina **Scegli profilo** selezionare **ad FS profilo 1,0 e 1,1**.<br />3. nella pagina **Configura URL** , in **WS\-Federation Passive URL**, digitare l' **URL dell'endpoint servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.<br />4. nella pagina **Configura identificatori** , in **identificatore attendibilità parte**, digitare l' **URI servizio federativo** come definito nella ad FS 1. *x* servizio federativo del partner.|![configurare AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare manualmente un trust della relying party](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel relying party attendibilità creato in precedenza, è necessario creare regole attestazione che accettano attestazioni in ingresso estratte da un archivio di attributi e pass-through, filtrare o trasformarle in un tipo di attestazione ID nome che può essere riconosciuto e utilizzato da AD FS 1. *x* servizio federativo. **Nota:** prima di creare questa regola, assicurarsi che il set di regole di attestazione in cui si sta creando questa regola è che la precede una regola che consente di estrarre innanzitutto un Lightweight Directory Access Protocol \(LDAP\) attestazione attributo da un archivio di attributi. Questa attestazione verrà utilizzata come input per la regola creata per l'invio di un AD FS 1. attestazione *x*\-compatibile. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'attestazione compatibile ad FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del AD FS 1. *x* servizio federativo e avere l'amministratore della ad FS 1. *x* servizio federativo configurare una nuova attendibilità del partner account. Specificare anche l'amministratore con l'URI del servizio federativo \(nelle proprietà del servizio federativo\), WS\-Federation Passive URL dell'endpoint \(l'URL dell'endpoint del servizio federativo\), e un token esportato\-file del certificato di firma \(con la chiave pubblica solo\). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N\/A|  
  

