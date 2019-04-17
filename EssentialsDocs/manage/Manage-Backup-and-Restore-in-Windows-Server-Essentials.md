---
title: Gestire Backup e ripristino in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gestire Backup e ripristino in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials offre modi affidabili per eseguire backup regolari del server e backup dei computer di rete. In caso di perdita di dati, è possibile ripristinare i dati da un backup riuscito sul server senza ripristinare l'intero computer. Se necessario, è possibile eseguire un ripristino completo del sistema al server o nel computer client nella rete. La tabella seguente descrive le diverse opzioni di backup disponibili insieme ai relativi vantaggi.  
  
|Funzionalità di backup|Descrizione|Vantaggi|  
|--------------------|-----------------|----------------|  
|Backup del server|Esegue il backup del server che esegue Windows Server Essentials. Viene eseguito il backup dei dati in un'unità USB esterna.<br /><br /> Per ulteriori informazioni, vedere [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) e [ripristinare o riparare il server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -Eseguire il ripristino completo del sistema del server.|  
|Backup dei Computer client|Esegue il backup dei computer client in rete. I dati viene eseguito il backup del server che esegue Windows Server Essentials.<br /><br /> Per ulteriori informazioni, vedere [gestire Backup Client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [ripristinare un intero sistema dal backup di un computer client esistente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -Eseguire il ripristino completo del sistema del computer client.|  
| Backup di Microsoft Azure|Esegue un backup in linea di file o cartelle sul server. Quando si utilizza Azure Backup per eseguire il backup dei dati del server, le informazioni vengono crittografate con una passphrase prima di essere caricate in un Data Center sicuro su Internet.<br /><br /> Per ulteriori informazioni, vedere [gestire il Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -Con i backup incrementali, solo le modifiche ai file vengono trasferite nel cloud.<br /><br /> -Backup vengono archiviati in Microsoft Azure e sono fuori sede, riducendo la necessità di proteggere e supporto di backup in loco.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
