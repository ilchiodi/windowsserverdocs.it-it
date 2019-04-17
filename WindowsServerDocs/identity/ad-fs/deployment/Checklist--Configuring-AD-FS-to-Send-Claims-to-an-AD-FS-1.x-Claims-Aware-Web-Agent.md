---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Distribuzione di server federativi
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione di ADFS per l'invio di attestazioni a un agente di riconoscere attestazioni AD FS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione di ADFS per l'invio di attestazioni a un agente Web compatibile con claims\ di AD FS 1. x  
Questo elenco di controllo include le attività necessarie per la configurazione del servizio federativo di \(AD FS\) Active Directory Federation Services in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'applicazione ospitata da un server Web con AD FS 1. *x* agente Web compatibile con claims\.  
  
> [!NOTE]  
> Completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a una procedura, tornare a questo argomento dopo aver completato i passaggi in questa procedura in modo che è possibile procedere con le attività rimanenti nell'elenco di controllo.  
  
![configurare ADFS per l'invio di attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: configurazione di ADFS per l'invio di attestazioni a un agente Web compatibile con claims\ di AD FS 1. x**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Pianificare l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e ulteriori che informazioni sull'ID nome del tipo di attestazione.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione per l'interoperabilità con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Se non è già stato fatto, utilizzare il collegamento a destra per creare un trust della relying party tra il servizio federativo ADFS in Windows Server 2012 e AD FS 1. *x* servizio federativo.|[Elenco di controllo: Configurazione di ADFS per l'invio di attestazioni a un servizio federativo di AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Prima poter ottenere l'interoperabilità con un'applicazione ospitata da AD FS 1. *x* agente Web compatibile con claims\, è necessario innanzitutto creare un trust della relying party nel servizio federativo AD FS in Windows Server 2012 per AD FS 1. *x* agente Web compatibile con claims\. **Nota:** la creazione di questa relazione di trust nel servizio federativo AD FS è l'equivalente di aggiunta di una nuova **applicazione** per AD FS 1. x servizio federativo \ (**federativo Service\\Trust Policy\\My Organization\\Application**\). Questo trust della relying party è necessario perché ADFS non è disponibile un equivalente **applicazione** nodo un proprio snap-in. Tuttavia, comunque necessario un canale sicuro per l'applicazione.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario eseguire le operazioni seguenti nell'Aggiunta guidata attendibilità componente per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* agente Web compatibile con claims\:<br /><br />1. scegliere il **Seleziona origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2. scegliere il **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3. nel **Configura URL** nella pagina **URL protocollo passivo WS-Federation**, tipo di **URL dell'applicazione** come definito in AD FS 1. *x* servizio federativo del partner.<br />4. scegliere il **Configura identificatori** nella pagina **Relying parte identificatore dell'attendibilità**, tipo di **URL dell'applicazione** come definito in AD FS 1. *x* agente Web compatibile con claims\|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una Relying Party Trust manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del server Web con AD FS 1. *x* claims\ compatibile con Web agente e hanno l'amministratore di modificare il file Web. config associato con l'applicazione in grado di riconoscere claims\ \ (sotto il sito Web predefinito in Internet Information Services \(IIS\)\) punta l'agente Web al servizio federativo di ADFS.<br /><br />Ad esempio, sostituire *myresourcefederationserver* nel tag `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del file Web. config con un nome di server federativo ADFS valido.<br /><br />Ciò è necessario per l'applicazione e l'agente Web compatibile con claims\ di AD FS 1. x essere in grado di utilizzare le attestazioni che vengono inviate dal servizio federativo ADFS in Windows Server 2012.|N \ / A|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel trust della relying party creato in precedenza, è necessario creare regole che verranno attestazioni in ingresso che sono stati estratti da un archivio di attributi e pass-through, filtrare oppure li trasforma in un ID nome tipo che può essere riconosciuto e utilizzato da Active Directory di attestazione attestazione ADFS 1. *x* agente Web compatibile con claims\. **Nota:** prima di creare questa regola, assicurarsi che il set di regole attestazione in cui si sta creando questa regola è che la precede una regola che consente di estrarre innanzitutto un'attestazione attributo Lightweight Directory Access Protocol \(LDAP\) da un archivio attributi. L'attestazione verrà utilizzata come input per la regola creata per l'invio di un'istanza di ADFS 1. *x*\-compatible attestazione. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per l'invio di un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

