---
title: Caricare un'immagine specializzata di Windows Server 2008/2008 R2 su Azure
description: Si avvicina la fine del supporto per Windows Server 2008 e Windows Server 2008 R2. Scopri come effettuare la rilocazione ad Azure eseguendo l'hosting di Windows Server nel cloud.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 425197d3462762c60a7371fc6ca529ad1b70e7ef
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443376"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>Caricare un'immagine specializzata di Windows Server 2008/2008 R2 su Azure 

![Banner che introduce l'argomento dell'immagine WS08](media/WS08-image-banner-large.png)

Ora è possibile eseguire una macchina virtuale Windows Server 2008/2008 R2 nel cloud con Azure. 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>Prepara l'immagine specializzata Windows Server 2008/2008 R2
Prima di caricare un'immagine, apporta le modifiche seguenti:

- Scarica e installa Windows Server 2008 Service Pack 2 (SP2) se non è già installato sull'immagine.

- Configura le impostazioni Desktop remoto.
  1. Vai a **Pannello di controllo** > **Impostazioni di sistema**.   
  2. Seleziona **Impostazioni di connessione remota** nel menu di sinistra.

     ![Screenshot delle impostazioni di sistema, evidenziando "Impostazioni di connessione remota".](media/1a_remote_settings.png)

  3. Seleziona la scheda di **Connessione remota** nelle Proprietà di sistema.   

     ![Screenshot della scheda di Connessione remota nelle Proprietà di sistema.](media/2c_sysprops.png)

  4. Seleziona Consenti connessioni dai computer che eseguono qualsiasi versione di Desktop remoto (meno sicuro).   
  5. Fai clic su **Applica** e su **OK**.
- Configura le impostazioni di Windows Firewall.   
   1. Nel prompt dei comandi in Modalità amministratore, immetti "**wf.msc**" per Windows Firewall e le impostazioni avanzate di sicurezza.   
   2. Ordina i risultati per **Porte**, seleziona **porta 3389**.   
     ![Regole in ingresso delle impostazioni screenshot di WIndows Firewall.](media/3b_inboundrules.png)   
   3. Abilitare Desktop remoto (TCP-IN) per i profili: **Dominio**, **privati**, e **pubblica** (illustrato in precedenza).

- Salva tutte le impostazioni e arresta l'immagine.   
- Se stai utilizzando Hyper-V, assicurati che l'AVHD figlio sia unito al VHD genitore per modifiche persistenti.

A causa di un attuale bug noto, la password dell'amministratore sull'immagine caricata scadrà entro 24 ore. Segui questi passaggi per risolvere questo problema: 

1. Vai su **Avvia** > **Esegui**
2. Digita **lusrmgr.msc**
3. Seleziona **Utenti** sotto Utenti e Gruppi locali
4. Fai clic con il pulsante destro del mouse **Amministratore** e seleziona **Proprietà**
5. Selezionare **Nessuna scadenza password** e selezionare **OK**
![Screenshot delle proprietà di amministratore.](media/6_adminprops.png)

## <a name="uploading-the-image-vhd"></a>Caricamento del VHD dell'immagine.
È possibile usare lo script sottostante per caricare il VHD. Prima sarà necessario il file di impostazioni di pubblicazione per l'account Azure. Ottieni [le impostazioni dei file Azure](https://azure.microsoft.com/downloads/).

Ecco lo script:

```powershell
Get-AzurePublishSettingsFile 

Login-AzureRmAccount
 
      # Import publishsettings
      Import-AzurePublishSettingsFile -PublishSettingsFile <LocationOfPublishingFile>
      $subscriptionId = 'xxxx-xxxx-xxxx-xxxx-xxxxx'
 
      # Set NodeFlight subscription as default subscription
      Select-AzureRmSubscription -SubscriptionId $subscriptionId
      Set-AzureRmContext -SubscriptionId $subscriptionId
      $rgName = "<resourcegroupname>"
    
      $urlOfUploadedImageVhd = "<BlobUrl>/<NameForVHD>.vhd"
      Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "<FilePath>"  
```
## <a name="deploy-the-image-in-azure"></a>Distribuisci l'immagine in Azure
In questa sezione, verrà distribuito il VHD dell'immagine in Azure. 

> [!IMPORTANT]
> Non utilizzare le immagini degli utenti predefiniti in Azure.

1.  Crea un nuovo [gruppo di risorse](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate). 
2.  Crea un nuovo [blob di archiviazione](https://docs.microsoft.com/rest/api/storageservices/put-blob) all'interno del gruppo di risorse.
3.  Crea un nuovo [contenitore](https://docs.microsoft.com/rest/api/storageservices/create-container) all'interno del blob di archiviazione.
4.  Copia l'URL dell'archiviazione blob dalle proprietà.
5.  Usa lo script sopra fornito per caricare l'immagine nel nuovo blob di archiviazione.
6.  Crea un [disco](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) per il VHD.   
     a. Vai a Dischi, fai clic su **Aggiungi**.  
     b. Immetti un nome per il disco. Seleziona l'abbonamento da utilizzare, imposta la regione e scegli il tipo di account.   
     c. Per il tipo di origine, seleziona archiviazione. Individua la location del VHD del blob creata utilizzando lo script.  
     d. Selezionare il tipo di sistema operativo Windows e le dimensioni (impostazione predefinita: 1023).   
     e. Fare clic su **Crea**.   

7.  Vai a Disco creato, fai clic su **Crea macchina virtuale**.   
     a. Nome della macchina virtuale.   
     b. Seleziona il gruppo esistente che hai creato nel passaggio 5 dove hai caricato il disco.   
     c. Seleziona una dimensione e un piano di SKU per la macchina virtuale.   
     d. Seleziona un'interfaccia di rete nella pagina delle impostazioni. Assicurati che l'interfaccia di rete abbia le seguenti regole specificate:
 
        PORT:3389 Protocol: TCP Action: Allow Priority: 1000 Name: ‘RDP-Rule’.   
     e. Fare clic su **Crea**.




