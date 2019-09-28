---
title: Caricare un'immagine specializzata di Windows Server 2008/2008 R2 su Azure
description: Si avvicina la fine del supporto per Windows Server 2008 e Windows Server 2008 R2. Scopri come effettuare la rilocazione ad Azure eseguendo l'hosting di Windows Server nel cloud.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 19e4abf1573b8d3ee99b8e8828c1674f24d27695
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391499"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>Caricare un'immagine specializzata di Windows Server 2008/2008 R2 su Azure 

![Banner che introduce l'argomento dell'immagine WS08](media/WS08-image-banner-large.png)

È ora possibile eseguire una macchina virtuale Windows Server 2008/2008 R2 nel cloud con Azure. 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>Preparare l'immagine specializzata Windows Server 2008/2008 R2
Prima di caricare un'immagine, apporta le modifiche seguenti:

- Scarica e installa Windows Server 2008 Service Pack 2 (SP2) se non è già installato sull'immagine.

- Configura le impostazioni Desktop remoto (RDP).
  1. Vai a **Pannello di controllo** > **Impostazioni di sistema**.   
  2. Seleziona **Impostazioni di connessione remota** nel menu di sinistra.

     ![Screenshot delle impostazioni di sistema, evidenziando "Impostazioni di connessione remota".](media/1a_remote_settings.png)

  3. Seleziona la scheda **Connessione remota** nelle Proprietà di sistema.   

     ![Screenshot della scheda di connessione remota nelle Proprietà di sistema.](media/2c_sysprops.png)

  4. Seleziona Consenti connessioni dai computer che eseguono qualsiasi versione di Desktop remoto (meno sicuro).   
  5. Fai clic su **Applica** e su **OK**.
- Configura le impostazioni di Windows Firewall.   
   1. Nel prompt dei comandi in Modalità amministratore, immetti "**wf.msc**" per Windows Firewall e le impostazioni avanzate di sicurezza.   
   2. Ordina i risultati per **Porte** e seleziona **porta 3389**.   
     ![Screenshot delle regole in entrata delle impostazioni Windows Firewall.](media/3b_inboundrules.png)   
   3. Abilita Desktop remoto (TCP-IN) per i profili: **Dominio**, **Privato** e **Pubblico** (mostrati sopra).

- Salva tutte le impostazioni e arresta l'immagine.   
- Se stai usando Hyper-V, assicurati che l'AVHD figlio sia unito al VHD genitore per modifiche persistenti.

A causa di un attuale bug noto, la password dell'amministratore sull'immagine caricata scadrà entro 24 ore. Segui questi passaggi per risolvere il problema: 

1. Vai su **Start** > **Esegui**
2. Digita **lusrmgr.msc**
3. Seleziona **Utenti** sotto Utenti e Gruppi locali
4. Fai clic con il pulsante destro del mouse su **Amministratore** e seleziona **Proprietà**
5. Seleziona **nessuna scadenza password** e seleziona **OK**
![Screenshot delle proprietà dell'amministratore.](media/6_adminprops.png)

## <a name="uploading-the-image-vhd"></a>Caricamento del VHD dell'immagine.
È possibile usare lo script sottostante per caricare il VHD. Per poter eseguire questa operazione sarà necessario il file di impostazioni di pubblicazione per l'account Azure. Ottieni le [impostazioni dei file Azure](https://azure.microsoft.com/downloads/).

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
In questa sezione verrà distribuito il VHD dell'immagine in Azure. 

> [!IMPORTANT]
> Non usare le immagini utente predefinite in Azure.

1.  Crea un nuovo [gruppo di risorse](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate). 
2.  Crea un nuovo [blob di archiviazione](https://docs.microsoft.com/rest/api/storageservices/put-blob) all'interno del gruppo di risorse.
3.  Crea un [contenitore](https://docs.microsoft.com/rest/api/storageservices/create-container) all'interno del BLOB di archiviazione.
4.  Copia l'URL dell'archiviazione BLOB dalle proprietà.
5.  Usa lo script sopra riportato per caricare l'immagine nel nuovo BLOB di archiviazione.
6.  Crea un [disco](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) per il VHD.   
     a. Vai a Dischi e fai clic su **Aggiungi**.  
     b. Immetti un nome per il disco. Seleziona l'abbonamento da usare, imposta la regione e scegli il tipo di account.   
     c. Per il tipo di origine, seleziona "archiviazione". Individua il percorso del VHD del BLOB creato con lo script.  
     d. Seleziona il tipo del sistema operativo Windows e la dimensione (predefinita: 1023).   
     e. Fare clic su **Crea**.   

7.  Vai a Disco creato e fai clic su **Crea macchina virtuale**.   
     a. Assegnare un nome alla macchina virtuale.   
     b. Seleziona il gruppo esistente creato nel passaggio 5 durante la creazione del disco.   
     c. Seleziona una dimensione e un piano di SKU per la macchina virtuale.   
     d. Seleziona un'interfaccia di rete nella pagina delle impostazioni. Assicurati che l'interfaccia di rete abbia le seguenti regole specificate:
 
        PORT:3389 Protocol: TCP Action: Allow Priority: 1000 Name: ‘RDP-Rule'.   
     e. Fare clic su **Crea**.




