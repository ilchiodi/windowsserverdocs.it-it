---
title: Eseguire la migrazione di una farm SQL Server federativa AD FS 2,0
description: Vengono fornite informazioni sulla migrazione di una farm SQL di AD FS 2,0 Server a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3c43d26868f39896ec8632397dc0fce1dfe2dd2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408288"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Eseguire la migrazione di una farm AD FS 2,0 WID  
In questo documento vengono fornite informazioni dettagliate sulla migrazione di una farm SQL AD FS 2,0 a Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrare una farm SQL Server  
 Per eseguire la migrazione di una farm SQL Server a Windows Server 2012, attenersi alla procedura seguente:  
  
1.  Per ogni server della farm SQL Server, rivedere ed eseguire le procedure descritte in [eseguire la migrazione di una SQL Server farm](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Rimuovere eventuali server della farm SQL Server dal bilanciamento del carico.  
  
3.  Aggiornare il sistema operativo del server in SQL Server farm da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del AD FS in questo server andrà persa e il ruolo del server AD FS 2,0 verrà rimosso. Viene invece installato il ruolo server AD FS Windows Server 2012, ma non è configurato. Per completare la migrazione del server federativo, è necessario creare manualmente la configurazione di AD FS originale e ripristinare le impostazioni del AD FS rimanenti.  
  
4. Creare la configurazione di AD FS originale in questo server nella farm di SQL Server utilizzando AD FS cmdlet di Windows PowerShell per aggiungere un server a una farm esistente.  
  
> [!IMPORTANT]
>  È necessario utilizzare Windows PowerShell per creare la configurazione originale del AD FS se si utilizza SQL Server per archiviare il database di configurazione AD FS.  

  - Aprire Windows PowerShell ed eseguire il comando seguente: `$fscredential = Get-Credential`.  
  - Immettere il nome e la password dell'account del servizio registrato durante la preparazione della farm SQL Server per la migrazione.  
  - Eseguire il comando seguente: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, dove `Data Source` è il valore dell'origine dati nel valore stringa della connessione all'archivio criteri nel file seguente: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5. Aggiungere il server appena aggiornato a Windows Server 2012 al servizio di bilanciamento del carico.  
  
6. Ripetere i passaggi da 2 a 6 per i nodi rimanenti della farm SQL Server.  
  
7. Quando tutti i server della farm SQL Server vengono aggiornati a Windows Server 2012, ripristinare le eventuali personalizzazioni AD FS rimanenti, ad esempio gli archivi di attributi personalizzati.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)



