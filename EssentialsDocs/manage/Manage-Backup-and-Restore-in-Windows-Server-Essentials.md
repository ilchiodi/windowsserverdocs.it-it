---
title: Gestire backup e ripristino in Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828522"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gestire backup e ripristino in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials offre modi affidabili per eseguire backup regolari del server e backup dei computer di rete. In caso di perdita dei dati, sarà possibile ripristinare i dati da un backup riuscito sul server, senza ripristinare l'intero computer. Se necessario, è possibile eseguire un ripristino completo del sistema sul server o nei computer client in rete. Nella tabella seguente sono illustrate le diverse opzioni di backup disponibili, insieme ai relativi vantaggi.  
  
|Funzionalità di backup|Descrizione|Vantaggi|  
|--------------------|-----------------|----------------|  
|Backup del server|Esegue il backup del server che esegue Windows Server Essentials. Il backup dei dati è eseguito in un'unità USB esterna.<br /><br /> Per altre informazioni, vedere [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) e [ripristinare o riparare il server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -È possibile eseguire il ripristino completo del sistema del server.|  
|Backup computer client|Esegue il backup dei computer client in rete. Il backup dei dati è eseguito nel server che esegue Windows Server Essentials.<br /><br /> Per altre informazioni, vedere [gestire Backup Client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [ripristinare un intero sistema dal backup di un computer client esistenti](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -È possibile eseguire il ripristino completo del sistema del computer client.|  
| Microsoft Azure Backup|Esegue un backup online dei file o delle cartelle disponibili nel server. Quando si usa Backup di Azure per eseguire il backup dei dati del server, le informazioni vengono crittografate usando la passphrase prima di essere caricato in un Data Center sicuro su Internet.<br /><br /> Per altre informazioni, vedere [gestire il Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -Con i backup incrementali, solo le modifiche ai file vengono trasferite nel cloud.<br /><br /> -Backup vengono archiviati in Microsoft Azure e sono fuori sede, riducendo la necessità di proteggere i supporti di backup.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
