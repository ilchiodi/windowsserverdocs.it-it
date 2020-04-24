---
title: Eseguire l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2
description: Si avvicina la fine del supporto per Windows Server 2008 e Windows Server 2008 R2. Scopri come eseguire l'aggiornamento locale o il rehosting di Azure.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 3d2c55430a78eaabfe55b764275c6e61fa80368a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826214"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Eseguire l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2

Il supporto esteso per Windows Server 2008 e Windows Server 2008 R2 terminerà il 14 gennaio 2020. Esistono due percorsi di modernizzazione: aggiornamento di un'istanza locale o migrazione con rehosting in Azure. **Se esegui il rehosting in Azure, puoi eseguire la migrazione delle immagini server esistenti gratuitamente.**

![Diagramma di flusso che descrive i percorsi di aggiornamento da Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Eseguire l'aggiornamento locale
Se vuoi mantenere i server in locale ed è in esecuzione Windows Server 2008 o Windows Server 2008 R2, dovrai [eseguire l'aggiornamento a Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) prima dell'[aggiornamento a Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Quando esegui l'aggiornamento, hai comunque la possibilità di eseguire la migrazione ad Azure tramite il rehosting.

Vedi [Aggiornamento da Windows Server 2008 R2 o Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008) per altre informazioni sulle opzioni di aggiornamento locale.

Se è in esecuzione Windows Server 2003, dovrai [eseguire l'aggiornamento a Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Vedi [Percorsi di aggiornamento per Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) per altre informazioni sulle opzioni di aggiornamento locale.


## <a name="migrate-to-azure"></a>Eseguire la migrazione ad Azure
Puoi eseguire la migrazione dei server Windows Server 2008 e Windows Server 2008 R2 locali ad Azure, dove puoi continuare a eseguirli su macchine virtuali. Con Azure potrai avere la garanzia della conformità, usufruire di maggiore sicurezza e aggiungere l'innovazione del cloud alla tua attività. I vantaggi della migrazione ad Azure includono:

- Aggiornamenti di sicurezza in Azure.
- Ottieni gratuitamente altri tre anni di importanti aggiornamenti di sicurezza critici di Windows Server 2008 R2 o 2008. 
- Aggiornamenti gratuiti in Azure.
- Adottare altri servizi cloud non appena sei pronto.
- Eseguendo la migrazione di SQL Server a Istanze gestite di Azure Managed o a macchine virtuali, otterrai gratuitamente altri tre anni di importanti aggiornamenti di sicurezza critici di Windows Server 2008 R2 o 2008. 
- Usare le licenze di SQL Server e Windows Server per usufruire di risparmi cloud esclusivi per Azure.

[![Avvia la migrazione ad Azure con un'immagine specializzata](./media/WS08-image-banner-small.png)](uploading-specialized-WS08-image-to-azure.md)

Per iniziare la migrazione, vedi [Caricare un'immagine specializzata di Windows Server 2008/2008 R2 su Azure](uploading-specialized-WS08-image-to-azure.md).

Per comprendere come analizzare le risorse IT esistenti, valutare le risorse a tua disposizione e identificare i benefici del trasferimento a servizi e applicazioni specifici nel cloud o del mantenimento dei carichi di lavoro in locale e dell'aggiornamento all'ultima versione di Windows Server, consulta [Guida alla migrazione per Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).

## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Aggiornare SQL Server 2008/2008 R2 in parallelo con i server Windows

![Logo di SQL Server](media/sqlr2.jpg)

Se è in esecuzione SQL Server 2008/2008 R2, puoi eseguire l'aggiornamento a SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) o [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Risorse aggiuntive
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)