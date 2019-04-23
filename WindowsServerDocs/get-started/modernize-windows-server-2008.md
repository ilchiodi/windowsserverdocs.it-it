---
title: Eseguire l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2
description: Si avvicina la fine del supporto per Windows Server 2008 e Windows Server 2008 R2. Scopri come eseguire l'aggiornamento locale o il rehosting di Azure.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 4127eab613abb429a200f513a11b944e05da0f76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851342"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Eseguire l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2

Il supporto esteso per Windows Server 2008 e Windows Server 2008 R2 terminerà il 14 gennaio 2020. Esistono due percorsi di modernizzazione: Aggiornamento di un'istanza locale o la migrazione con rehosting in Azure. **Se rehosting in Azure, è possibile eseguire la migrazione di Server immagini esistenti senza oneri.**

![Diagrammi di flusso che descrive i percorsi di aggiornamento da Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Aggiornamento locale
Qualora si vogliano mantenere i server in locale e Windows Server 2008 o Windows Server 2008 R2 siano in esecuzione, sarà necessario [eseguire l'aggiornamento a Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) prima di poter [eseguire l'aggiornamento a Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Quando esegui l'aggiornamento, hai comunque la possibilità di eseguire la migrazione ad Azure eseguendo il rehosting.

Consulta [Aggiornamento da Windows Server 2008 R2 o Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008) per ulteriori informazioni sulle opzioni di aggiornamento locale.

Se Windows Server 2003 è in esecuzione, sarà necessario [eseguire l'aggiornamento a Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Consulta [Versioni di valutazione e opzioni di aggiornamento per Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) per ulteriori informazioni sulle opzioni di aggiornamento locale.


## <a name="migrate-to-azure"></a>Esegui la migrazione ad Azure
È possibile eseguire la migrazione dei server locali Windows Server 2008 e Windows Server 2008 R2 ad Azure, dove è possibile continuare l'esecuzione sulle macchine virtuali. In Azure resterai conforme, sarai più sicuro e aggiungerai l'innovazione cloud al tuo lavoro. I vantaggi della migrazione ad Azure includono:

- Aggiornamenti di sicurezza in Azure.
- Ottieni gratuitamente altri tre anni di importanti aggiornamenti di sicurezza critici di Windows Server 2008 R2 o 2008. 
- Aggiornamenti gratuiti in Azure.
- Adottare altri servizi cloud non appena sei pronto.
- Migrando SQL Server ad Azure Managed Instances o a macchine virtuali, otterrai gratuitamente altri tre anni di importanti aggiornamenti di sicurezza critici di Windows Server 2008 R2 o 2008. 
- Utilizzare le esistenti licenze SQL Server e Windows Server per risparmi cloud unici per Azure.

<a href="uploading-specialized-WS08-image-to-azure.md"><img src="media/WS08-image-banner-small.png"></a>

Per iniziare la migrazione, consulta [Caricamento di un'immagine specializzata Windows Server 2008/2008 R2 su Azure](uploading-specialized-WS08-image-to-azure.md).

Per aiutarti a comprendere come analizzare le risorse IT esistenti, valutare ciò che hai e identificare i benefici del trasferimento a servizi e applicazioni specifici nel cloud o del mantenimento dei carichi di lavoro in locale e dell'aggiornamento all'ultima versione di Windows Server, consulta [Guida alla migrazione per Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).


## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Aggiorna SQL Server 2008/2008 R2 in parallelo con i server Windows

![Logo di SQL Server](media/sqlr2.jpg)

Se SQL Server 2008/2008 R2 è in esecuzione, è possibile eseguire l'aggiornamento a SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) or [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Risorse aggiuntive
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)