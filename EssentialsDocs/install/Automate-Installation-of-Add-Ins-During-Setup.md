---
title: Automazione dell’installazione di componenti aggiuntivi durante l’installazione
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 579ed4e6e780c261ca582e943cebf2fc18b5ef62
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310117"
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automazione dell’installazione di componenti aggiuntivi durante l’installazione

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="automate-installing-add-ins-during-setup"></a><a name="BKMK_AddIns"></a>Automatizzare l'installazione di componenti aggiuntivi durante l'installazione  
 Per installare i componenti aggiuntivi durante l’installazione, utilizzare il metodo PostIC.cmd descritto nella sezione [Creazione del file PostIC.cmd per l’esecuzione delle operazioni successive alla configurazione iniziale](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) di questo documento.  
  
 Aggiungere la seguente voce al proprio PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 Ora il componente aggiuntivo supporta le operazioni di preinstallazione e disinstallazione personalizzata.  
  
 L'operazione di preinstallazione viene eseguita prima di installare tutti i file **.msi** specificati in addin.xml. Quando si lavora in modalità interattiva, viene visualizzata la finestra di avanzamento che però non mostra alcun progresso. Il pulsante di annullamento è disabilitato durante la fase di preinstallazione. Per implementare l'operazione di preinstallazione, aggiungere il seguente contenuto nel file addin.xml (direttamente in Package):  
  
> [!NOTE]
>  Lo schema xml deve essere esattamente come il seguente:  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>  
<Executable>exefile</Executable>  
<NormalArgs>args-for-interactive-mode</NormalArgs>  
<SilentArgs>args-for-silent-mode</SilentArgs>  
<IgnoreExitCode>true</IgnoreExitCode>  
  </Preinstall>  
  <UninstallConfirm>...</UninstallConfirm>      
</Package>  
<¦>  
<¦>  
```  
  
 **exefile** è il file eseguibile nel pacchetto del componente aggiuntivo che consente di eseguire l'operazione di preinstallazione e deve quindi essere specificato. **NormalArgs** specifica gli argomenti da inviare a exefile nella riga di comando quando si utilizza la modalità interattiva. In questa modalità, exefile può visualizzare alcune finestre di dialogo per l'interazione utente. **SilentArgs** specifica gli argomenti da inviare a exefile nella riga di comando quando si utilizza la modalità non interattiva (-q viene specificato quando si richiama installaddin.exe). In questa modalità, exefile non visualizzerà alcuna finestra. Se si imposta **IgnoreExitCode** su true, l'operazione di preinstallazione viene sempre considerata riuscita, altrimenti il codice di uscita 0 indica che l'operazione è stata completata, 1 che è stata annullata e altri valori che non è riuscita. I tag **NormalArgs**, **SilentArgs** e **IgnoreExitCode** sono facoltativi.  
  
 È possibile utilizzare l'operazione di disinstallazione personalizzata per:  
  
- Sostituire la finestra di dialogo di conferma predefinita.  
  
- Inserire i dati nelle finestre di dialogo personalizzare prima della disinstallazione.  
  
- Eseguire alcune attività prima della disinstallazione.  
  
  Per implementare l'operazione di disinstallazione, aggiungere il seguente contenuto nel file addin.xml (direttamente in Package):  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>¦</Preinstall>  
<UninstallConfirm>  
<Executable>full-path-to-exefile</Executable>  
<Arguments>command-line-arguments</Arguments>  
</UninstallConfirm>  
</Package>  
```  
  
 **full-path-to-exefile** indica l'exefile già installato sul sistema. **Arguments** è facoltativo e specifica gli argomenti della riga di comando per l'exefile. L'exefile viene richiamato prima che venga visualizzata la finestra di conferma della disinstallazione.  
  
 In questa fase, l'exefile può fare quanto segue:  
  
- Visualizzare alcune finestre di dialogo per consentire l'interazione utente.  
  
- Eseguire alcune attività in background.  
  
  Dal codice di uscita del file exe dipende l'andamento del processo di disinstallazione:  
  
- 0: il processo di disinstallazione prosegue senza inserire i dati nella finestra di conferma predefinita, come già confermato dall'utente. Questo approccio può essere utilizzato per ignorare la finestra di conferma predefinita;  
  
- 1: il processo di disinstallazione viene annullato e visualizza un messaggio per informare l'utente. Nulla cambia;  
  
- Altro: il processo di disinstallazione prosegue con la finestra di conferma predefinita, come se l'operazione di disinstallazione personalizzata non fosse presente.  
  
  Qualsiasi errore che si verifica mentre viene richiamato exefile provoca lo stesso risultato che si ottiene quando exefile restituisce un codice diverso da 0 o 1.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)