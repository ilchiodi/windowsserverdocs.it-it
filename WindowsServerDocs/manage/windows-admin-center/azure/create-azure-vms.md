---
title: Distribuire macchine virtuali di Azure usando l'interfaccia di amministrazione di Windows
description: Distribuzione di macchine virtuali di Azure con l'interfaccia di amministrazione di Windows. Configurazione di macchine virtuali di Azure come parte degli scenari gestiti dal centro di amministrazione di Windows.
ms.technology: manage
ms.topic: article
author: nedpyle
ms.author: nedpyle
manager: jgerend
ms.date: 01/28/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 551f97d257b7ea3d05cdded73421d2e5c088171e
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830617"
---
# <a name="deploy-azure-virtual-machines-from-within-windows-admin-center"></a>Distribuire macchine virtuali di Azure dall'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center versione 1910 consente di distribuire macchine virtuali di Azure. In questo modo si integra la distribuzione delle VM nei carichi di lavoro gestiti dall'interfaccia di amministrazione di Windows come [servizio migrazione archiviazione](../../../storage/storage-migration-service/overview.md) e [replica di archiviazione](../../../storage/storage-replica/storage-replica-overview.md). Invece di creare nuovi server e macchine virtuali nel portale di Azure prima di distribuire il carico di lavoro e probabilmente mancano i passaggi e la configurazione necessari, l'interfaccia di amministrazione di Windows può distribuire la VM di Azure, configurarne l'archiviazione, aggiungerla al dominio, installare i ruoli e Configurare quindi il sistema distribuito. È anche possibile distribuire nuove macchine virtuali di Azure senza un carico di lavoro dalla pagina connessioni dell'interfaccia di amministrazione di Windows.

L'interfaccia di amministrazione di Windows gestisce anche un'ampia gamma di servizi di Azure. [Altre informazioni sulle opzioni di integrazione di Azure disponibili nell'interfaccia di amministrazione di Windows](../plan/azure-integration-options.md).

## <a name="scenarios"></a>Scenari

La distribuzione di macchine virtuali di Azure della versione 1910 di Windows Admin Center supporta gli scenari seguenti:

- [Servizio migrazione archiviazione](../../../storage/storage-migration-service/overview.md)
- [Replica di archiviazione](../../../storage/storage-replica/storage-replica-overview.md)
- [Nuovo server autonomo (senza ruoli)](index.md#extend-on-premises-capacity-with-azure)

## <a name="requirements"></a>Requisiti

Per creare una nuova VM di Azure dall'interfaccia di amministrazione di Windows, è necessario disporre di:

- Una [sottoscrizione di Azure](https://azure.microsoft.com).
- Un gateway dell'interfaccia di [amministrazione di Windows registrato in Azure](azure-integration.md)
- Un [gruppo di risorse di Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) esistente in cui si hanno le autorizzazioni di creazione.
- Una [rete virtuale](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) e una subnet di Azure esistenti.
- Una soluzione [Azure Express Route](https://azure.microsoft.com/services/expressroute/) o [VPN di Azure](https://azure.microsoft.com/services/vpn-gateway/) legata alla rete virtuale e alla subnet che consente la connettività dalle macchine virtuali di Azure ai client locali, ai controller di dominio, al computer dell'interfaccia di amministrazione di Windows e a tutti i server che richiedono la comunicazione con questa macchina virtuale come parte di una distribuzione del carico di lavoro. Ad esempio, per usare il servizio migrazione archiviazione per eseguire la migrazione dello spazio di archiviazione in una macchina virtuale di Azure, il computer agente di orchestrazione e il computer di origine devono essere in grado di contattare la macchina virtuale di Azure di destinazione a cui si sta eseguendo

## <a name="usage"></a>Usage

Passaggi e procedure guidate per la distribuzione di VM di Azure variano in base allo scenario. Per informazioni dettagliate sullo scenario generale, vedere la documentazione del carico di lavoro.

### <a name="deploying-azure-vms-as-part-of-storage-migration-service"></a>Distribuzione di macchine virtuali di Azure come parte del servizio migrazione archiviazione

Dallo strumento *servizio migrazione archiviazione* all'interno dell'interfaccia di amministrazione di Windows, eseguire un inventario di uno o più server di origine. Quando si è nella fase di *trasferimento dei dati* , selezionare **Crea una nuova macchina virtuale di Azure** nella pagina *specificare una destinazione* e quindi fare clic su **Crea macchina virtuale**. Viene avviato uno strumento di creazione step-by-step che seleziona una macchina virtuale di Azure Windows Server 2012 R2, Windows Server 2016 o Windows Server 2019 come destinazione per la migrazione. Il servizio migrazione archiviazione fornisce le dimensioni di VM consigliate per la corrispondenza con l'origine, ma è possibile eseguirne l'override facendo clic su **Visualizza tutte le dimensioni**. I dati del server di origine vengono usati anche per configurare automaticamente i dischi gestiti e i file System, nonché per aggiungere la nuova macchina virtuale di Azure al dominio Active Directory. Se la macchina virtuale è Windows Server 2019 (consigliata), il centro di amministrazione di Windows installa la funzionalità proxy del servizio migrazione archiviazione. Dopo aver creato la macchina virtuale di Azure, l'interfaccia di amministrazione di Windows torna al flusso di lavoro di trasferimento normale del servizio di migrazione archiviazione.  


### <a name="deploying-azure-vms-as-part-of-storage-replica"></a>Distribuzione di macchine virtuali di Azure come parte della replica di archiviazione

Dallo strumento *replica archiviazione* all'interno dell'interfaccia di amministrazione di Windows, fare clic su **nuovo** nella scheda *partnership* , quindi in *replica con un altro server* fare clic su **Usa una nuova VM di Azure** e quindi su **Avanti**. Specificare le informazioni del server di origine e il nome del gruppo di replica, quindi fare clic su **Avanti**. Viene avviato un processo che seleziona automaticamente una macchina virtuale di Azure Windows Server 2016 o Windows Server 2019 come destinazione per l'origine della migrazione. Il servizio migrazione archiviazione consiglia di usare le dimensioni delle macchine virtuali in base all'origine, ma è possibile eseguire l'override selezionando **Visualizza tutte le dimensioni**. I dati di inventario vengono usati per configurare automaticamente i dischi gestiti e i file System, nonché per aggiungere la nuova VM di Azure al dominio Active Directory. Quando l'interfaccia di amministrazione di Windows crea la macchina virtuale di Azure, specificare un nome per il gruppo di replica e fare clic su **Crea**. L'interfaccia di amministrazione di Windows inizia quindi il normale processo di sincronizzazione iniziale della replica di archiviazione per iniziare a proteggere i dati.


### <a name="deploying-a-new-standalone-azure-vm"></a>Distribuzione di una nuova VM di Azure autonoma

Dalla pagina *Tutte le connessioni* in Windows Admin Center passare ad **Aggiungi** e selezionare **Crea nuovo** in **Azure VM** (Macchina virtuale di Azure). Viene avviato uno strumento di creazione dettagliato che consente di selezionare una macchina virtuale di Azure Windows Server 2012 R2, Windows Server 2016 o Windows Server 2019, scegliere una dimensione, aggiungere dischi gestiti e, facoltativamente, aggiungere il dominio Active Directory.
