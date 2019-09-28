---
title: Eseguire la migrazione di una farm di server federativo AD FS 2,0
description: Fornisce informazioni sulla migrazione di una farm database interno di AD FS 2,0 Server a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89da3de4bc626e12a1fc34752841f2de1afb5322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408245"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Eseguire la migrazione di una farm AD FS 2,0 WID  
In questo documento vengono fornite informazioni dettagliate sulla migrazione di una farm di database interno di Windows AD FS 2,0 (WID) a Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Eseguire la migrazione di una farm AD FS WID
Per eseguire la migrazione di una farm WID a Windows Server 2012, seguire questa procedura:  
  
1.  Per ogni nodo (Server) della farm WID, rivedere ed eseguire le procedure descritte in [preparare la migrazione di una farm wid](prepare-to-migrate-a-wid-farm.md).  
  
2.  Rimuovere eventuali nodi non primari dal bilanciamento del carico.  
  
3.  Aggiornamento del sistema operativo in questo server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del AD FS in questo server andrà persa e il ruolo del server AD FS 2,0 verrà rimosso. Viene invece installato il ruolo server AD FS Windows Server 2012, ma non è configurato. Per completare la migrazione del server federativo, è necessario creare la configurazione originale del AD FS e ripristinare le impostazioni rimanenti del AD FS.  
  
4. Creare la configurazione di AD FS originale in questo server.  
  
È possibile creare la configurazione di AD FS originale utilizzando la **Configurazione guidata server federativo di ad FS** per aggiungere un server federativo a una farm di database interno di database. Per altre informazioni, vedere [Aggiungere un server federativo a una server farm federativa](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando si raggiunge la pagina **specificare il server federativo primario e un account del servizio** nella **Configurazione guidata del server federativo di ad FS**, immettere il nome del server federativo primario della farm wid e assicurarsi di immettere l'account del servizio. informazioni registrate durante la preparazione per la migrazione del AD FS. Per ulteriori informazioni, vedere [preparazione alla migrazione del server federativo AD FS 2,0](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando si raggiunge la pagina **specifica il nome del servizio federativo** , assicurarsi di selezionare lo stesso certificato SSL registrato nella pagina "prepararsi per la migrazione di una farm wid" in [preparare la migrazione del Server federativo ad FS 2,0](prepare-to-migrate-a-wid-farm.md).  
  
5. Aggiornare le pagine Web di ADFS nel server Se è stato eseguito il backup delle pagine Web personalizzate AD FS durante la preparazione per la migrazione, è necessario usare i dati di backup per sovrascrivere le pagine Web predefinite AD FS create per impostazione predefinita nella directory **%SystemDrive%\Inetpub\Adfs\Ls** in seguito al ad FS configurazione in Windows Server 2012.  
  
6. Aggiungere il server appena aggiornato a Windows Server 2012 al servizio di bilanciamento del carico.  
  
7. Ripetere i passaggi da 1 a 6 per i restanti server secondari nella farm WID.  
  
8. Alzare di livello uno dei server secondari aggiornati convertendolo nel server primario della farm Database interno di Windows. A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Rimuovere il server primario originale della farm Database interno di Windows dal bilanciamento del carico.  
  
10. Abbassare di livello il server primario originale nella farm Database interno di Windows convertendolo nel server secondario tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di AD FS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Eseguire quindi il comando seguente per abbassare di valore il server primario originale come server secondario: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Aggiornamento del sistema operativo nell'ultimo nodo (Server) della farm WID da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del AD FS in questo server andrà persa e il ruolo del server AD FS 2,0 verrà rimosso. Viene invece installato il ruolo server AD FS Windows Server 2012, ma non è configurato. Per completare la migrazione del server federativo, è necessario creare manualmente la configurazione di AD FS originale e ripristinare le impostazioni del AD FS rimanenti.  
  
12. Creare la configurazione di AD FS originale in questo ultimo nodo (Server) nella farm database interno di database di WID.  
  
È possibile creare la configurazione di AD FS originale utilizzando la **Configurazione guidata server federativo di ad FS** per aggiungere un server federativo a una farm di database interno di database. Per altre informazioni, vedere [Aggiungere un server federativo a una server farm federativa](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando si raggiunge la pagina **specifica il server federativo primario e l'account del servizio** nella **Configurazione guidata del server federativo di ad FS**, immettere le informazioni sull'account del servizio registrate durante la preparazione per la migrazione del ad FS. Per ulteriori informazioni, vedere [preparazione alla migrazione del server federativo AD FS 2,0](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando si raggiunge la pagina **specifica il nome del servizio federativo** , assicurarsi di selezionare lo stesso certificato SSL registrato in [preparare la migrazione del Server federativo ad FS 2,0](prepare-to-migrate-a-wid-farm.md).  
  
13. Aggiornare le pagine Web di AD FS in questo ultimo server della farm WID. Se è stato eseguito il backup delle pagine Web personalizzate AD FS durante la preparazione per la migrazione, usare i dati di backup per sovrascrivere le pagine Web predefinite AD FS create per impostazione predefinita nella directory **%SystemDrive%\Inetpub\Adfs\Ls** in seguito al ad FS configurazione in Windows Server 2012.  
  
14. Aggiungere l'ultimo server della farm WID appena aggiornato a Windows Server 2012 al servizio di bilanciamento del carico.  
  
15. Ripristinare le eventuali personalizzazioni di ADFS rimanenti, ad esempio gli archivi di attributi personalizzati.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo di AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)