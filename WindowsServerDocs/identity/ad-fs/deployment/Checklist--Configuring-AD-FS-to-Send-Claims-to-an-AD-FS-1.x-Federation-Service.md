---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: Elenco di controllo - configurazione di ADFS per utilizzare attestazioni ADFS 1. x
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Elenco di controllo: Configurazione di ADFS per l'invio di attestazioni a un servizio federativo di AD FS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Elenco di controllo: Configurazione di ADFS per l'invio di attestazioni ADFS 1. x servizio federativo  
Questo elenco di controllo include le attività necessarie per la configurazione del servizio federativo di \(AD FS\) Active Directory Federation Services in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'istanza di ADFS 1. *x* servizio federativo.  
  
> [!NOTE]  
> Completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a una procedura, tornare a questo argomento dopo aver completato i passaggi in questa procedura in modo che è possibile procedere con le attività rimanenti nell'elenco di controllo.  
  
![configurare ADFS per l'invio di attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione di ADFS per l'invio di attestazioni ADFS 1. x servizio federativo**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e ulteriori che informazioni sull'ID nome del tipo di attestazione.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione per l'interoperabilità con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima di poter ottenere l'interoperabilità con una versione precedente di ADFS, è innanzitutto necessario creare un trust della relying party nel servizio federativo ADFS per AD FS 1. *x* servizio federativo. **Nota:** è possibile creare una relazione di trust con ADFS 1. *x* servizio federativo tramite i metadati federativi.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nell'Aggiunta guidata attendibilità componente per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* servizio federativo:<br /><br />1. scegliere il **Seleziona origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2. scegliere il **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3. nel **Configura URL** nella pagina **URL protocollo passivo WS-Federation**, tipo di **URL dell'endpoint del servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.<br />4. nel **Configura identificatori** nella pagina **Relying parte identificatore dell'attendibilità**, tipo di **URI servizio federativo** come definito in AD FS 1. *x* servizio federativo del partner.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una Relying Party Trust manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel trust della relying party creato in precedenza, è necessario creare regole che verranno attestazioni in ingresso che sono stati estratti da un archivio di attributi e pass-through, filtrare oppure li trasforma in un ID nome tipo che può essere riconosciuto e utilizzato da Active Directory di attestazione attestazione ADFS 1. *x* servizio federativo. **Nota:** prima di creare questa regola, assicurarsi che il set di regole attestazione in cui si sta creando questa regola è che la precede una regola che consente di estrarre innanzitutto un'attestazione attributo Lightweight Directory Access Protocol \(LDAP\) da un archivio attributi. L'attestazione verrà utilizzata come input per la regola creata per l'invio di un'istanza di ADFS 1. *x*\-compatible attestazione. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per l'invio di un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore di AD FS 1. *x* servizio federativo e che l'amministratore di AD FS 1. *x* servizio federativo impostare una nuova attendibilità del partner account. Inoltre, specificare l'amministratore con l'URI del servizio federativo \ (in properties\ servizio federativo), l'URL dell'endpoint WS-Federation passivo \ (il servizio federativo endpoint URL\), e un esportato token\ per la firma di file di certificato \ (con only\ chiave pubblica). L'amministratore dovrà questi elementi per impostare la relazione di trust.|N \ / A|  
  

