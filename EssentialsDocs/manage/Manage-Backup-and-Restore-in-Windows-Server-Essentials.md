---
title: Gestire backup e ripristino in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37435b8b51bbc6ba4deb09a50f2c85045ecf0536
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311364"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gestire backup e ripristino in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials offre modi affidabili per eseguire backup regolari del server e backup dei computer di rete. In caso di perdita dei dati, sarà possibile ripristinare i dati da un backup riuscito sul server, senza ripristinare l'intero computer. Se necessario, è possibile eseguire un ripristino completo del sistema sul server o nei computer client in rete. Nella tabella seguente sono illustrate le diverse opzioni di backup disponibili, insieme ai relativi vantaggi.  
  
|Funzionalità di backup|Descrizione|Vantaggi|  
|--------------------|-----------------|----------------|  
|Backup del server|Esegue il backup del server che esegue Windows Server Essentials. Il backup dei dati è eseguito in un'unità USB esterna.<br /><br /> Per ulteriori informazioni, vedere [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) and [restore or Repair Your Server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle nel server.<br /><br /> -È in grado di eseguire il ripristino completo del sistema del server.|  
|Backup computer client|Esegue il backup dei computer client in rete. Il backup dei dati è eseguito nel server che esegue Windows Server Essentials.<br /><br /> Per ulteriori informazioni, vedere [gestire il backup dei client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [ripristinare un sistema completo da un backup del computer client esistente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -È possibile eseguire il ripristino completo del sistema del computer client.|  
| Backup di Microsoft Azure|Esegue un backup online dei file o delle cartelle disponibili nel server. Quando si usa backup di Azure per eseguire il backup dei dati del server, le informazioni vengono crittografate usando la passphrase prima di essere caricate in un data center sicuro su Internet.<br /><br /> Per ulteriori informazioni, vedere [Manage online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-È possibile ripristinare file e cartelle dal server.<br /><br /> -Con i backup incrementali, solo le modifiche ai file vengono trasferite nel cloud.<br /><br /> -I backup vengono archiviati in Microsoft Azure e sono fuori sede, riducendo la necessità di proteggere e proteggere i supporti di backup nel sito.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
