---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 874984b469303c0f8a40a676632c144ee6079f44
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192435"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un agente di AD FS 1.x Claims-Aware Web

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni alle attestazioni di AD FS 1.x\-agente Web compatibile con  
Questo elenco di controllo include le attività necessarie per la configurazione di Active Directory Federation Services \(ADFS\) servizio federativo in Windows Server 2012 per inviare attestazioni che possono essere riconosciute da un'applicazione ospitata da un Server Web che esegue AD FS 1. *x* attestazioni\-agente Web compatibile con.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configurare ADFS per l'invio di attestazioni](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Configurazione di AD FS per l'invio di attestazioni alle attestazioni di AD FS 1.x\-agente Web compatibile con**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Piano per l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di AD FS e altre che informazioni sull'ID nome del tipo di attestazione.|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione dell'interoperabilità con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Se non è già stato fatto, usare il collegamento a destra prima di tutto creare un trust della relying party tra il servizio federativo ADFS in Windows Server 2012 e AD FS 1. *x* servizio federativo.|[Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un servizio federativo di AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|In precedenza è possibile ottenere l'interoperabilità con un'applicazione ospitata da AD FS 1. *x* attestazioni\-agente Web compatibile con, è necessario innanzitutto creare un trust della relying party nel servizio federativo AD FS in Windows Server 2012 per AD FS 1. *x* attestazioni\-agente Web compatibile con. **Nota:** Creazione di questa relazione di trust nel servizio federativo AD FS è l'equivalente di aggiunta di una nuova **Application** per AD FS 1. x servizio federativo \( **servizio federativo\\dei criteri di attendibilità\\ La mia organizzazione\\Application**\). Questo trust della relying party è necessario perché ADFS non è disponibile un equivalente **applicazione** nodo in un proprio snap\-in. Tuttavia, comunque necessario un canale sicuro per l'applicazione.<br /><br />Quando si configura la relazione di trust utilizzando la procedura nel collegamento a destra, è necessario effettuare le operazioni seguenti nell'Aggiunta guidata attendibilità componente per impostare questa relazione di trust per interagire con un'istanza di ADFS 1. *x* attestazioni\-agente Web compatibile con:<br /><br />1.  Nel **Selezionare un'origine dati** selezionare **immettere dati sulla relying party trust manualmente**.<br />2.  Nel **Scegli profilo** selezionare **profilo ADFS 1.0 e 1.1**.<br />3.  Nel **Configura URL** nella pagina **WS\-federazione passiva URL**, digitare il **URL dell'applicazione** come definito in AD FS 1. *x* servizio federativo del partner.<br />4.  Nel **Configura identificatori** nella pagina **Relying parte identificatore dell'attendibilità**, digitare il **URL dell'applicazione** come definito in AD FS 1. *x* attestazioni\-agente Web compatibile con|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una Relying Party Trust manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Contattare l'amministratore del server Web che esegue AD FS 1. *x* attestazioni\-compatibile con Web agente e hanno l'amministratore di modificare il file Web. config associato le attestazioni\-applicazione compatibile con \(sotto il sito Web predefinito in Internet Information I servizi \(IIS\) \) punta l'agente Web al servizio federativo di ADFS.<br /><br />Ad esempio, sostituire *myresourcefederationserver* nel tag `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del file Web. config con un nome di server federativo ADFS valido.<br /><br />Questa operazione è necessaria per l'applicazione e AD FS 1. x attestazioni\-agente Web compatibile con la possibilità di utilizzare le attestazioni che vengono inviate dal servizio federativo AD FS in Windows Server 2012.|N\/A|  
|![configurare ADFS per l'invio di attestazioni](media/icon_checkboxo.gif)|Nel trust della relying party creato in precedenza, è necessario creare regole attestazioni che verranno attestazioni in ingresso che sono stati estratti da un archivio attributi e pass-through, filtrare o li trasforma in un tipo di attestazione ID nome che può essere riconosciuto e utilizzato per il AD FS 1. *x* attestazioni\-agente Web compatibile con. **Nota:** Prima di creare questa regola, assicurarsi che il set di regole di attestazione in cui si sta creando questa regola ha una regola che precede che consente di estrarre innanzitutto un Lightweight Directory Access Protocol \(LDAP\) attestazione attributo da un archivio di attributi. Questa attestazione verrà usata come input per la regola creati per l'invio di un'istanza di ADFS 1. *x*\-attestazione compatibile. Per ulteriori informazioni su come creare una regola per estrarre un attributo LDAP, vedere [creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurare ADFS per l'invio di attestazioni](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creare una regola per inviare un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

