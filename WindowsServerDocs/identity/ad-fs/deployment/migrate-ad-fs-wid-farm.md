---
title: Eseguire la migrazione di una farm database interno di Windows di AD FS 2.0 federation server
description: Fornisce informazioni sulla migrazione di una farm database interno di Windows di AD FS 2.0 server a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Eseguire la migrazione di una farm database interno di Windows di AD FS 2.0  
Questo documento vengono fornite informazioni dettagliate sulla migrazione di un annuncio farm ADFS 2.0 Windows Database interno di Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Eseguire la migrazione di una farm database interno di Windows di AD FS
Per eseguire la migrazione di una farm database interno di Windows a Windows Server 2012, eseguire la procedura seguente:  
  
1.  Per ogni nodo (server) della farm database interno di Windows, rivedere ed eseguire le procedure descritte in [preparare la migrazione di una farm WID](prepare-to-migrate-a-wid-farm.md).  
  
2.  Rimuovere eventuali nodi non primari dal bilanciamento del carico.  
  
3.  Eseguire l'aggiornamento del sistema operativo server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione di ADFS nel server andrà persa e il ruolo di AD FS 2.0 server viene rimosso. Suo posto verrà installato il ruolo server ADFS di Windows Server 2012, ma non è configurato. È necessario creare la configurazione originale di ADFS e ripristinare le impostazioni di ADFS rimanenti per completare la migrazione del server federativo.  
  
4.  Creare la configurazione originale di ADFS nel server.  
  
È possibile creare la configurazione originale di ADFS tramite il **configurazione guidata Server federativo di AD FS** per aggiungere un server federativo a una farm database interno di Windows. Per ulteriori informazioni, vedere [aggiunge un Server federativo a una Server Farm federativa](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando si raggiunge il **specificare il Server federativo primario e un Account del servizio** pagina il **configurazione guidata Server federativo di AD FS**, immettere il nome del server federativo primario della farm database interno di Windows e assicurarsi di immettere le informazioni sull'account del servizio registrate durante la preparazione per la migrazione di ADFS. Per ulteriori informazioni, vedere [prepararsi alla migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando si raggiunge il **specificare il nome del servizio federativo** pagina, assicurarsi di selezionare lo stesso certificato SSL registrato nel "preparare la migrazione di una farm database interno di Windows" in [prepararsi alla migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-a-wid-farm.md).  
  
5.  Aggiornare le pagine Web di ADFS nel server. Se è stato eseguito il backup delle pagine di ADFS durante la preparazione per la migrazione, è necessario utilizzare i dati di backup per sovrascrivere le pagine Web di ADFS che sono state create per impostazione predefinita nel **%systemdrive%\inetpub\adfs\ls** directory in seguito alla configurazione di ADFS in Windows Server 2012.  
  
6.  Aggiungere il server appena aggiornato a Windows Server 2012 per il bilanciamento del carico.  
  
7.  Ripetere i passaggi da 1 a 6 per i restanti server secondari nella farm database interno di Windows.  
  
8.  Alzare di livello uno dei server secondari aggiornati convertendolo nel server primario della farm database interno di Windows. A tale scopo, aprire Windows PowerShell ed eseguire il comando seguente: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Rimuovere il server primario originale della farm database interno di Windows dal bilanciamento del carico.  
  
10. Abbassare di livello il server primario originale nella farm database interno di Windows in un server secondario tramite Windows PowerShell. Aprire Windows PowerShell ed eseguire il comando seguente per aggiungere i cmdlet di ADFS alla sessione di Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Quindi eseguire il comando seguente per abbassare di livello il server primario originale da un server secondario: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Eseguire l'aggiornamento del sistema operativo nell'ultimo nodo (server) della farm database interno di Windows da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Il risultato di aggiornamento del sistema operativo, la configurazione di ADFS nel server andrà persa e il ruolo di AD FS 2.0 server viene rimosso. Suo posto verrà installato il ruolo server ADFS di Windows Server 2012, ma non è configurato. Manualmente, è necessario creare la configurazione originale di ADFS e ripristinare le impostazioni di ADFS rimanenti per completare la migrazione del server federativo.  
  
12. Creare la configurazione originale di ADFS nell'ultimo nodo (server) della farm database interno di Windows.  
  
È possibile creare la configurazione originale di ADFS tramite il **configurazione guidata Server federativo di AD FS** per aggiungere un server federativo a una farm database interno di Windows. Per ulteriori informazioni, vedere [aggiunge un Server federativo a una Server Farm federativa](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando si raggiunge il **specificare il server federativo primario e un Account del servizio** pagina il **configurazione guidata Server federativo di AD FS**, immettere le informazioni sull'account del servizio registrate durante la preparazione per la migrazione di ADFS. Per ulteriori informazioni, vedere [prepararsi alla migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando si raggiunge il **specificare il nome del servizio federativo** pagina, assicurarsi di selezionare lo stesso certificato SSL registrato nel [prepararsi alla migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-a-wid-farm.md).  
  
13. Aggiornare le pagine Web di ADFS in questo ultimo server della farm database interno di Windows. Se è stato eseguito il backup delle pagine di ADFS durante la preparazione per la migrazione, è possibile utilizzare i dati di backup per sovrascrivere le pagine Web di ADFS che sono state create per impostazione predefinita nel **%systemdrive%\inetpub\adfs\ls** directory in seguito alla configurazione di ADFS in Windows Server 2012.  
  
14. Aggiungere il server della farm database interno di Windows appena aggiornato a Windows Server 2012 per il bilanciamento del carico.  
  
15. Ripristinare eventuali personalizzazioni di ADFS rimanenti, ad esempio gli archivi attributi personalizzati.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)