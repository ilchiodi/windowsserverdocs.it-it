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
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Elenco di controllo: configurazione di ADFS per l'invio di attestazioni a un agente Web in grado di riconoscere attestazioni AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Elenco di controllo: configurazione AD FS per inviare attestazioni a una AD FS 1. x attestazioni\-agente Web in grado di riconoscere  
Questo elenco di controllo include le attività necessarie per configurare il Active Directory Federation Services \(AD FS\) Servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'applicazione ospitata da un server Web che esegue il AD FS 1. attestazioni *x*\-agente Web in grado di riconoscere.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configurare AD FS per inviare attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione ad FS per l'invio di attestazioni a un ad FS 1. x attestazioni\-agente Web in grado di riconoscere**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome.|![configurare AD FS per l'invio](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[della pianificazione delle attestazioni per l'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Se non è già stato fatto, usare il collegamento a destra per creare prima un trust di relying party tra il Servizio federativo di AD FS in Windows Server 2012 e il AD FS 1. *x* servizio federativo.|[Elenco di controllo: configurazione AD FS per l'invio di attestazioni a una AD FS 1. x Servizio federativo](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima di poter ottenere l'interoperabilità con un'applicazione ospitata da AD FS 1. *x* attestazioni\-agente Web in grado di riconoscere, è necessario innanzitutto creare una relying party attendibilità nel ad FS servizio federativo in Windows Server 2012 al ad FS 1. *x* attestazioni\-agente Web compatibile con. **Nota:** La creazione di questa relazione di trust nell'Servizio federativo di AD FS equivale all'aggiunta di una nuova **applicazione** al ad FS 1. x servizio federativo \(servizio federativo **criteri di attendibilità\\organizzazione\\applicazione**\\.\) Questo trust della relying party è necessario perché ADFS non è disponibile un equivalente **applicazione** nodo in un proprio snap\-in. Tuttavia, comunque necessario un canale sicuro per l'applicazione.<br /><br />Quando si configura il trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nella procedura guidata Aggiungi attendibilità componente per impostare questa relazione di trust per interagire con un AD FS 1. attestazioni *x*\-agente Web in grado di riconoscere:<br /><br />1. nella pagina **Seleziona origine dati** selezionare **immettere manualmente i dati sulla relazione di trust relying party**.<br />2. nella pagina **Scegli profilo** selezionare **ad FS profilo 1,0 e 1,1**.<br />3. nella pagina **Configura URL** , in **WS\-Federation Passive URL**, digitare l' **URL dell'applicazione** come definito nella ad FS 1. *x* servizio federativo del partner.<br />4. nella pagina **Configura identificatori** , in **identificatore attendibilità parte**, digitare l' **URL dell'applicazione** come definito nella ad FS 1. *x* attestazioni\-agente Web in grado di riconoscere|![configurare AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare manualmente un trust della relying party](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del server Web che esegue il AD FS 1. *x* attestazioni\-agente Web in grado di riconoscere e fare in modo che l'amministratore modifichi il file Web. config associato a attestazioni\-applicazione in grado di riconoscere \(nel sito Web predefinito in Internet Information Services \(IIS\)\) per puntare l'agente Web alla ad FS servizio federativo.<br /><br />Ad esempio, sostituire *myresourcefederationserver* nel tag `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del file Web. config con un nome di server federativo ADFS valido.<br /><br />Questa operazione è necessaria per l'applicazione e per le attestazioni AD FS 1. x\-agente Web in grado di utilizzare le attestazioni inviate dal AD FS Servizio federativo in Windows Server 2012.|N\/A|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel relying party attendibilità creato in precedenza, è necessario creare regole attestazione che accettano le attestazioni in ingresso estratte da un archivio di attributi e pass-through, filtrare o trasformarle in un tipo di attestazione ID nome che può essere riconosciuto e utilizzato da AD FS 1. attestazioni *x*\-agente Web in grado di riconoscere. **Nota:** prima di creare questa regola, assicurarsi che il set di regole di attestazione in cui si sta creando questa regola è che la precede una regola che consente di estrarre innanzitutto un Lightweight Directory Access Protocol \(LDAP\) attestazione attributo da un archivio di attributi. Questa attestazione verrà utilizzata come input per la regola creata per l'invio di un AD FS 1. attestazione *x*\-compatibile. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare AD FS per inviare attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'attestazione compatibile ad FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

