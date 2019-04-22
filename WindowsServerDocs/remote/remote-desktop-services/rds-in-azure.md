---
title: Distribuire facilmente Servizi Desktop remoto con ARM e Azure Marketplace
description: Informazioni su come creare una piccola distribuzione di servizi desktop remoto in Azure usando modelli ARM e Azure Marketplace.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823082"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Distribuire facilmente Servizi Desktop remoto con ARM e Azure Marketplace

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2

Servizi Desktop remoto (RDS) è la piattaforma ideale per ospitare in modo conveniente le applicazioni e desktop Windows. È possibile usare un [offerta del Marketplace di Azure](#basic-rds-through-the-azure-marketplace) o una [modello di avvio rapido](#Customized-RDS-using-Quickstart-templates) per creare rapidamente un Servizi Desktop remoto nella distribuzione IaaS di Azure. Azure marketplace consente di creare un dominio di prova per l'utente, rendendo un meccanismo semplice e facile per i test e di prova. Modelli di avvio rapido, d'altra parte, consentono di usare un dominio esistente, che lo rendono uno strumento straordinario per creare un ambiente di produzione. Una volta configurata, è possibile connettersi per i desktop pubblicate e le applicazioni da diversi dispositivi e piattaforme, usare le app Microsoft Remote Desktop per Windows, Mac, iOS e Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>RDS di base tramite il Marketplace di Azure

Creazione della distribuzione tramite Azure Marketplace è il modo più rapido per iniziare subito e in esecuzione. Quando tutto ciò che viene completato, l'ambiente avrà un aspetto, ad esempio la [architettura di servizi desktop remoto base](desktop-hosting-logical-architecture.md#basic-deployment). L'offerta crea tutti i componenti Servizi Desktop remoto che è necessario, è sufficiente è fornire alcune informazioni. 

È necessario fornire le informazioni seguenti quando si distribuisce l'offerta del Marketplace:
- Nome utente e amministratore password. Si tratta di un nuovo utente che gestirà la distribuzione.
- Nome DNS e nome di dominio Active Directory. Queste sono le nuove risorse create. Assicurarsi che i nomi sono significativi.
- Dimensioni della macchina virtuale. È possibile scegliere le dimensioni delle macchine virtuali da usare per gli endpoint di host sessione Desktop remoto. È possibile modificare le dimensioni anche manualmente dopo la distribuzione iniziale per aiutarti a ottimizzare le macchine virtuali per i carichi di lavoro e i costi.

Usare questi passaggi per creare la distribuzione di servizi desktop remoto di piccole da Azure Marketplace: 

1. Avviare la distribuzione di servizi desktop remoto di Azure Marketplace:
   1. Accedi il [portale di Azure](https://portal.azure.com).
   2. Fare clic su **New** aggiungere la distribuzione.
   3. Digitare "Servizi Desktop remoto" nel campo di ricerca e premere INVIO.
   4. Fare clic su **Servizi Desktop remoto (RDS) - Basic - Devtest**, quindi fare clic su **crea**.
   5. Seguire i passaggi nel portale per creare e distribuire servizi desktop remoto. Si aggiungeranno i dettagli di configurazione della chiave, come le informazioni elencate in precedenza. 
2. Connettersi alla distribuzione. Al termine della distribuzione, controllare la sezione di output dei passaggi finali di completare e connettersi alla distribuzione.
   1. Scaricare ed eseguire [questo script di PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) nel dispositivo di test per installare i certificati necessari per connettersi alla distribuzione di servizi desktop remoto. 
   
      Questo passaggio è necessario solo durante la fase di test. Quando si distribuisce Servizi Desktop remoto in Azure nell'ambiente di produzione, assicurarsi di seguire le procedure consigliate, ad esempio l'acquisto e l'uso di un certificato SSL pubblicamente attendibile nei server web.

   2. Quando richiesto, accedere all'account Azure. Selezionare la sottoscrizione di Azure, un gruppo di risorse e un indirizzo IP pubblico creato per questa nuova distribuzione.
   3. Al termine dello script, la pagina Web desktop remoto avvia nel browser predefinito. È possibile controllare la pagina Web desktop remoto confrontando l'URL della pagina per l'indirizzo DNS specificato durante la distribuzione. 
   
      Accedere con le credenziali dell'amministratore creato durante la distribuzione per il desktop predefiniti pubblicati automaticamente. È anche possibile inviare agli utenti nel sito Web desktop remoto per desktop e applicazioni di test.

      > [!TIP]
      > Dimenticherà l'utente amministratore o nome di dominio? È possibile passare al nuovo gruppo di risorse nel portale, fare clic su **distribuzioni**e quindi visualizzare i parametri immessi.

Ora che si dispone di una distribuzione di servizi desktop remoto, è possibile [aggiungere e gestire gli utenti](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>Servizi Desktop remoto personalizzato usando i modelli di avvio rapido

È possibile usare i modelli Azure Resource Manager per distribuire servizi desktop remoto in Azure. Ciò è particolarmente utile se si desidera che una distribuzione di servizi desktop remoto di base ma includono componenti esistenti (ad esempio, Active Directory) che si desidera utilizzare. A differenza di offerta nel Marketplace, è possibile apportare ulteriori personalizzazioni, ad esempio l'uso di un'istanza di AD esistente in una rete virtuale, usando un'immagine del sistema operativo personalizzata per le macchine virtuali host sessione Desktop remoto e dei livelli sulla disponibilità elevata per i componenti Servizi Desktop remoto. Dopo aver aggiunto sulla disponibilità elevata di ogni componente, l'ambiente avrà un aspetto simile il [architettura di servizi desktop remoto a altamente disponibile](desktop-hosting-logical-architecture.md#highly-available-deployment).

Usare questi passaggi per creare la distribuzione di servizi desktop remoto di piccole con un modello di servizi desktop remoto di Azure: 

1. Selezionare il modello di avvio rapido di Azure:
   1. Andare alla [RDS Azure Quickstart Templates](https://aka.ms/rdautomation) sito.
   2. Scegliere il modello che corrisponde a ciò che si sta tentando di eseguire operazioni. Assicurarsi che siano soddisfatti i prerequisiti per il modello specifico. (Ad esempio, se vuoi usare un'immagine personalizzata per le macchine virtuali, assicurarsi che tale immagine già stato caricato in un account di archiviazione di Azure.)
   3. Fare clic su **Distribuisci in Azure**.
   4. È necessario fornire alcuni dettagli (ad esempio nome utente amministratore, il nome di dominio Active Directory) nel portale di Azure. Ciò varia a seconda del modello che scelto.
   5. Fare clic su **acquisto**.
2. Connettersi alla distribuzione. 
   1. Scaricare ed eseguire [questo script di PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) nel dispositivo di test per installare i certificati necessari per connettersi alla distribuzione di servizi desktop remoto. 
   
      Questo passaggio è necessario solo durante la fase di test. Quando si distribuisce Servizi Desktop remoto in Azure nell'ambiente di produzione, assicurarsi di seguire le procedure consigliate, ad esempio l'acquisto e l'uso di un certificato SSL pubblicamente attendibile nei server web.

   2. Quando richiesto, accedere all'account Azure. Selezionare la sottoscrizione di Azure, un gruppo di risorse e un indirizzo IP pubblico creato per questa nuova distribuzione.
   3. Al termine dello script, la pagina Web desktop remoto avvia nel browser predefinito. È possibile controllare la pagina Web desktop remoto confrontando l'URL della pagina per l'indirizzo DNS specificato durante la distribuzione. 
   
      Accedere con le credenziali dell'amministratore creato durante la distribuzione per il desktop predefiniti pubblicati automaticamente. È anche possibile inviare agli utenti nel sito Web desktop remoto per desktop e applicazioni di test.

      > [!TIP]
      > Dimenticherà l'utente amministratore o nome di dominio? È possibile passare al nuovo gruppo di risorse nel portale, fare clic su **distribuzioni**e quindi visualizzare i parametri immessi.

Ora che si dispone di una distribuzione di servizi desktop remoto, è possibile [aggiungere e gestire gli utenti](rds-user-management.md).
