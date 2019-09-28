---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 689bd33bc95c2b142dfbe6d0448a604b2971979e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359985"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un agente Web in grado di riconoscere attestazioni AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione AD FS per inviare attestazioni a una AD FS 1. x attestazioni @ no__t-0aware Web Agent  
Questo elenco di controllo include le attività necessarie per configurare il Active Directory Federation Services \(AD FS @ no__t-1 Servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'applicazione ospitata da un server Web esecuzione del AD FS 1. *x* Claims @ no__t-3aware Web Agent.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configure AD FS inviare attestazioni @ no__t-1Checklist: Configurazione AD FS per inviare attestazioni a una AD FS 1. x attestazioni @ no__t-0aware Web Agent @ no__t-1  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome.|![configure AD FS per inviare](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[la pianificazione delle attestazioni per l'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Se non è già stato fatto, usare il collegamento a destra per creare prima un trust di relying party tra il Servizio federativo di AD FS in Windows Server 2012 e il AD FS 1. *x* servizio federativo.|[Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un servizio federativo di AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima di poter ottenere l'interoperabilità con un'applicazione ospitata da AD FS 1. *x* Claims @ no__t-1aware Web Agent, è necessario innanzitutto creare una relying party trust nell'ad FS servizio federativo in Windows Server 2012 al ad FS 1. *x* attestazioni\-agente Web compatibile con. **Nota:** La creazione di questa relazione di trust nell'Servizio federativo AD FS equivale all'aggiunta di una nuova **applicazione** al ad FS 1. x servizio federativo \(**servizio federativo @ No__t-3Trust Policy @ No__t-4My Organization @ no__t-5Application**\). Questo trust della relying party è necessario perché ADFS non è disponibile un equivalente **applicazione** nodo in un proprio snap\-in. Tuttavia, comunque necessario un canale sicuro per l'applicazione.<br /><br />Quando si configura il trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nella procedura guidata Aggiungi attendibilità componente per impostare questa relazione di trust per interagire con un AD FS 1. *x* Claims @ no__t-1aware Web Agent:<br /><br />1.  Nel **Selezionare un'origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2.  Nel **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3.  Nella pagina **Configura URL** , in **WS @ No__t-2FEDERATION passive URL**, digitare l' **URL dell'applicazione** come definito nella ad FS 1. *x* servizio federativo del partner.<br />4.  Nella pagina **Configura identificatori** , in **identificatore attendibilità parte**, digitare l' **URL dell'applicazione** come definito nella ad FS 1. *x* Claims @ no__t-4aware Web Agent|![configure AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare manualmente un trust della relying party](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del server Web che esegue il AD FS 1. *x* Claims @ no__t-1aware Web Agent e fare in modo che l'amministratore modifichi il file Web. config associato all'applicazione Claims @ no__t-2aware \(Under il sito Web predefinito Internet Information Services \(IIS @ no__t-5 @ no__t-6 a puntare l'agente Web al Servizio federativo AD FS.<br /><br />Ad esempio, sostituire *myresourcefederationserver* nel tag `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del file Web. config con un nome di server federativo ADFS valido.<br /><br />Questa operazione è necessaria per l'applicazione e AD FS 1. x Claims @ no__t-0aware Web Agent in modo che sia in grado di utilizzare le attestazioni inviate al AD FS Servizio federativo in Windows Server 2012.|N\/A|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel relying party attendibilità creato in precedenza, è necessario creare regole attestazione che accettano le attestazioni in ingresso estratte da un archivio di attributi e pass-through, filtrare o trasformarle in un tipo di attestazione ID nome che può essere riconosciuto e utilizzato dal AD FS 1. *x* Claims @ no__t-1aware Web Agent. **Nota:** Prima di creare questa regola, assicurarsi che nel set di regole attestazioni in cui si sta creando questa regola sia presente una regola che precede il primo, che estrae un protocollo di accesso Lightweight Directory \(LDAP @ no__t-1 attestazione attributo da un archivio di attributi. Questa attestazione verrà utilizzata come input per la regola creata per l'invio di un AD FS 1. *x*\-compatible Claim. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'attestazione compatibile ad FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

