---
title: Eseguire la migrazione di una farm SQL di AD FS 2.0 federation server
description: Vengono fornite informazioni sulla migrazione di una farm ADFS a SQL server 2.0 per Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Eseguire la migrazione di una farm database interno di Windows di AD FS 2.0  
Questo documento fornisce informazioni dettagliate sulla migrazione di una farm AD FS 2.0 SQL a Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Eseguire la migrazione di una farm SQL Server  
 Per eseguire la migrazione di una farm SQL Server a Windows Server 2012, eseguire la procedura seguente:  
  
1.  Per ogni server della farm SQL Server, rivedere ed eseguire le procedure descritte in [migrare una farm SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Rimuovere eventuali server della farm SQL Server dal bilanciamento del carico.  
  
3.  Aggiornare il sistema operativo in questo server della farm SQL Server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione di ADFS nel server andrà persa e il ruolo di AD FS 2.0 server viene rimosso. Suo posto verrà installato il ruolo server ADFS di Windows Server 2012, ma non è configurato. Manualmente, è necessario creare la configurazione originale di ADFS e ripristinare le impostazioni di ADFS rimanenti per completare la migrazione del server federativo.  
  
4.  Creare la configurazione originale di ADFS in questo server della farm SQL Server utilizzando i cmdlet di ADFS di Windows PowerShell per aggiungere un server a una farm esistente.  
  
> [!IMPORTANT]
>  È necessario utilizzare Windows PowerShell per creare la configurazione originale di ADFS, se si utilizza SQL Server per archiviare il database di configurazione di ADFS.  

  - Aprire Windows PowerShell ed eseguire il comando seguente: `$fscredential = Get-Credential`.  
  - Immettere il nome e la password dell'account del servizio registrate durante la preparazione della farm SQL Server per la migrazione.  
  - Eseguire il comando seguente: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, dove `Data Source` è il valore dell'origine dati nel valore stringa connessione archivio criteri nel file seguente: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Aggiungere il server appena aggiornato a Windows Server 2012 per il bilanciamento del carico.  
  
6.  Ripetere i passaggi da 2 a 6 per i restanti nodi nella farm SQL Server.  
  
7.  Quando tutti i server della farm SQL Server vengono aggiornati a Windows Server 2012, è possibile ripristinare eventuali personalizzazioni di ADFS rimanenti, ad esempio gli archivi attributi personalizzati.  

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)



