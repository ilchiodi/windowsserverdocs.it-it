---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Elenco di controllo: configurazione di ADFS per utilizzare attestazioni AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887032"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un servizio federativo di AD FS 1.x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni per AD FS 1.x Federation Service  
Questo elenco di controllo include le attività necessarie per la configurazione di Active Directory Federation Services \(ADFS\) servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'istanza di ADFS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configurare ADFS per l'invio di attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Configurazione di AD FS per l'invio di attestazioni per AD FS 1.x Federation Service**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Piano per l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di AD FS e altre che informazioni sull'ID nome del tipo di attestazione.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione dell'interoperabilità con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Per poter ottenere l'interoperabilità con una versione precedente di ADFS, è innanzitutto necessario creare un trust della relying party nel servizio federativo AD FS per AD FS 1. *x* servizio federativo. **Nota:** È possibile creare una relazione di trust con ADFS 1. *x* servizio federativo utilizzando i metadati della federazione.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario effettuare le operazioni seguenti nell'Aggiunta guidata attendibilità componente per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* servizio federativo:<br /><br />1.  Nel **Selezionare un'origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2.  Nel **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3.  Nel **Configura URL** nella pagina **WS\-federazione passiva URL**, digitare il **URL dell'endpoint del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.<br />4.  Nel **Configura identificatori** nella pagina **Relying parte identificatore dell'attendibilità**, digitare il **URI del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una Relying Party Trust manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel trust della relying party creato in precedenza, è necessario creare attestazione regole che verranno attestazioni in ingresso che sono stati estratti da un archivio attributi e pass-through, filtrare o li trasforma in un ID Nome attestazione di tipo che può essere riconosciuto e utilizzato da Active Directory ADFS 1. *x* servizio federativo. **Nota:** Prima di creare questa regola, assicurarsi che il set di regole di attestazione in cui si sta creando questa regola ha una regola che precede che consente di estrarre innanzitutto un Lightweight Directory Access Protocol \(LDAP\) attestazione attributo da un archivio di attributi. Questa attestazione verrà usata come input per la regola creati per l'invio di un'istanza di ADFS 1. *x*\-attestazione compatibile. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore di AD FS 1. *x* servizio federativo e che l'amministratore di AD FS 1. *x* servizio federativo impostare una nuova attendibilità del partner account. Specificare anche l'amministratore con l'URI del servizio federativo \(nelle proprietà del servizio federativo\), WS\-Federation Passive URL dell'endpoint \(l'URL dell'endpoint del servizio federativo\), e un token esportato\-file del certificato di firma \(con la chiave pubblica solo\). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N\/A|  
  

