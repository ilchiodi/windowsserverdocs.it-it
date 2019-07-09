---
title: Distribuire facilmente Servizi Desktop remoto con Azure Resource Manager e Azure Marketplace
description: Informazioni su come creare una piccola distribuzione di Servizi Desktop remoto in Azure usando modelli di Azure Resource Manager e Azure Marketplace.
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
ms.openlocfilehash: 218e61e5ebe110502ebe139b27607bfeff104fde
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712617"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Distribuire facilmente Servizi Desktop remoto con Azure Resource Manager e Azure Marketplace

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Servizi Desktop remoto (RDS) è la piattaforma ideale per ospitare in modo conveniente le applicazioni e i desktop Windows. Puoi usare un'[offerta di Azure Marketplace](#basic-rds-through-the-azure-marketplace) o un [modello di avvio rapido](#customized-rds-using-quickstart-templates) per creare rapidamente un'istanza di Servizi Desktop remoto in una distribuzione IaaS di Azure. Azure Marketplace crea automaticamente un dominio di test, offrendoti un semplice meccanismo per i test e i modelli di prova. I modelli di avvio rapido ti consentono invece di usare un dominio esistente e rappresentano quindi uno strumento straordinario per creare un ambiente di produzione. Una volta eseguita la configurazione, puoi connetterti alle applicazioni e ai desktop pubblicati da piattaforme e dispositivi diversi, usando le app Desktop remoto Microsoft per Windows, Mac, iOS e Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>Architettura di Servizi Desktop remoto di base tramite Azure Marketplace

La creazione della distribuzione tramite Azure Marketplace è il modo più rapido per diventare subito operativi. Una volta completate le operazioni, l'ambiente sarà simile all'[architettura di Servizi Desktop remoto di base](desktop-hosting-logical-architecture.md#basic-deployment). L'offerta crea tutti i componenti di Servizi Desktop remoto necessari e dovrai solo fornire alcune informazioni. 

Se esegui la distribuzione dell'offerta del Marketplace, dovrai fornire le informazioni seguenti:
- Nome utente e password dell'amministratore. Si tratta di un nuovo utente che gestirà la distribuzione.
- Nome DNS e nome di dominio di Active Directory. Queste sono le NUOVE risorse create. Assicurati di usare nomi significativi.
- Dimensioni delle macchine virtuali. Puoi scegliere le dimensioni delle macchine virtuali da usare per gli endpoint di Host sessione Desktop remoto. Puoi anche modificare le dimensioni manualmente dopo la distribuzione iniziale per ottimizzare le macchine virtuali per i carichi di lavoro e i costi.

Segui questa procedura per creare una distribuzione di Servizi Desktop remoto di piccole dimensioni da Azure Marketplace: 

1. Avvia la distribuzione di Servizi Desktop remoto tramite Azure Marketplace:
   1. Accedi al [portale di Azure](https://portal.azure.com).
   2. Fai clic su **Nuovo** per aggiungere la distribuzione.
   3. Digita "RDS" nel campo di ricerca e premi INVIO.
   4. Fai clic su **Remote Desktop Services (RDS) - Basic - Dev/Test** e quindi su **Crea**.
   5. Segui i passaggi nel portale per creare e distribuire Servizi Desktop remoto. Aggiungerai i dettagli di configurazione principali, come le informazioni elencate in precedenza. 
2. Stabilisci la connessione alla distribuzione. Al termine della distribuzione, controlla la sezione relativa agli output per i passaggi finali per completare la distribuzione e stabilire una connessione.
   1. Scarica ed esegui [questo script di PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) nel dispositivo di test per installare i certificati necessari per la connessione alla distribuzione di Servizi Desktop remoto. 
   
      Questo passaggio è necessario solo durante la fase di test. Quando distribuisci Servizi Desktop remoto in Azure nell'ambiente di produzione, assicurati di seguire le procedure consigliate, ad esempio l'acquisto e l'uso di un certificato SSL pubblicamente attendibile nei server Web.

   2. Quando viene richiesto, accedi all'account Azure. Seleziona la sottoscrizione di Azure, il gruppo di risorse e l'indirizzo IP pubblico creato per questa nuova distribuzione.
   3. Al completamento dello script, la pagina Web Desktop remoto viene avviata nel browser predefinito. Puoi controllare la pagina Web Desktop remoto confrontando l'URL della pagina con l'indirizzo DNS specificato durante la distribuzione. 
   
      Accedi con le credenziali di amministratore create durante la distribuzione e il desktop predefinito verrà pubblicato automaticamente. Puoi anche invitare gli utenti a visitare il sito Web Desktop remoto per testare i loro desktop e applicazioni.

      > [!TIP]
      > Hai dimenticato il nome di dominio o l'utente amministratore? Torna al nuovo Gruppo di risorse nel portale, fai clic su **Distribuzioni** e visualizza i parametri immessi.

Ora che hai una distribuzione di Servizi Desktop remoto, puoi [aggiungere e gestire gli utenti](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>Architettura di Servizi Desktop remoto personalizzata tramite modelli di avvio rapido

Puoi usare i modelli di Azure Resource Manager per distribuire Servizi Desktop remoto in Azure. Ciò è particolarmente utile se vuoi una distribuzione di Servizi Desktop remoto di base ma hai componenti esistenti (ad esempio, Active Directory) che vuoi usare. A differenza dell'offerta del Marketplace, puoi apportare ulteriori personalizzazioni, ad esempio usare un'istanza di Active Directory esistente in una rete virtuale, usare un'immagine del sistema operativo personalizzata per le macchine virtuali di Host sessione Desktop remoto e usare livelli di disponibilità elevata per i componenti di Servizi Desktop remoto. Dopo aver aggiunto la disponibilità elevata a ogni componente, l'ambiente sarà simile all'[architettura di Servizi Desktop remoto a disponibilità elevata](desktop-hosting-logical-architecture.md#highly-available-deployment).

Segui questa procedura per creare una distribuzione di Servizi Desktop remoto di piccole dimensioni con un modello di Servizi Desktop remoto di Azure: 

1. Scegli il modello di avvio rapido di Azure:
   1. Vai al sito [Modelli di avvio rapido di Azure per Servizi Desktop remoto](https://aka.ms/rdautomation).
   2. Scegli il modello adatto per le tue esigenze. Assicurati che siano soddisfatti i prerequisiti per il modello specifico. Ad esempio, se vuoi usare un'immagine personalizzata per le macchine virtuali, assicurati che tale immagine sia già stata caricata in un account di archiviazione di Azure.
   3. Fai clic su **Distribuisci in Azure**.
   4. Dovrai fornire alcuni dettagli (ad esempio nome utente amministratore, nome di dominio di Active Directory) nel portale di Azure. Le informazioni necessarie variano in base al modello scelto.
   5. Fai clic su **Acquista**.
2. Stabilisci la connessione alla distribuzione. 
   1. Scarica ed esegui [questo script di PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) nel dispositivo di test per installare i certificati necessari per la connessione alla distribuzione di Servizi Desktop remoto. 
   
      Questo passaggio è necessario solo durante la fase di test. Quando distribuisci Servizi Desktop remoto in Azure nell'ambiente di produzione, assicurati di seguire le procedure consigliate, ad esempio l'acquisto e l'uso di un certificato SSL pubblicamente attendibile nei server Web.

   2. Quando viene richiesto, accedi all'account Azure. Seleziona la sottoscrizione di Azure, il gruppo di risorse e l'indirizzo IP pubblico creato per questa nuova distribuzione.
   3. Al completamento dello script, la pagina Web Desktop remoto viene avviata nel browser predefinito. Puoi controllare la pagina Web Desktop remoto confrontando l'URL della pagina con l'indirizzo DNS specificato durante la distribuzione. 
   
      Accedi con le credenziali di amministratore create durante la distribuzione e il desktop predefinito verrà pubblicato automaticamente. Puoi anche invitare gli utenti a visitare il sito Web Desktop remoto per testare i loro desktop e applicazioni.

      > [!TIP]
      > Hai dimenticato il nome di dominio o l'utente amministratore? Torna al nuovo Gruppo di risorse nel portale, fai clic su **Distribuzioni** e visualizza i parametri immessi.

Ora che hai una distribuzione di Servizi Desktop remoto, puoi [aggiungere e gestire gli utenti](rds-user-management.md).
