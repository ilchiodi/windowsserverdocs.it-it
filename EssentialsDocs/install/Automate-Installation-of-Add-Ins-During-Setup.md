---
title: Automazione dell'installazione di componenti aggiuntivi durante l'installazione
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automazione dell'installazione di componenti aggiuntivi durante l'installazione

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_AddIns"></a>Automatizzare l'installazione di componenti aggiuntivi durante l'installazione  
 Per installare i componenti aggiuntivi durante l'installazione, utilizzare il metodo PostIC.cmd descritto nel [creare il PostIC.cmd File per esegue attività di configurazione iniziale di Post](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) sezione di questo documento.  
  
 Aggiungere la seguente voce al proprio PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 Il componente aggiuntivo supporta ora i passaggi di pre-installazione e disinstallazione personalizzata.  
  
 L'operazione di preinstallazione viene eseguita prima di installare tutti **msi** file specificati in addin.xml. Quando esegue in modalità interattiva, la finestra di dialogo di avanzamento sarà visualizzata ma senza modificare lo stato di avanzamento. Il pulsante di annullamento è disabilitato durante la fase di pre-installazione. Per implementare l'operazione di preinstallazione, aggiungere il seguente contenuto nel file addin.xml (direttamente in Package):  
  
> [!NOTE]
>  Lo schema xml deve essere esattamente come quello riportato di seguito:  
  
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
  
 Wherein **exefile** il file eseguibile nel pacchetto di componente aggiuntivo per eseguire l'operazione di preinstallazione e deve essere specificato. **NormalArgs** specifica gli argomenti da passare a exefile nella riga di comando quando interattivo modalità viene utilizzato. In questa modalità, exefile può visualizzare alcune finestre di dialogo per l'interazione dell'utente. **SilentArgs** specifica gli argomenti da inviare a exefile in modalità riga di comando quando viene utilizzato (-q viene specificato quando si richiama installaddin.exe). L'EXEFile non visualizzerà tutte le finestre in questa modalità. Se **IgnoreExitCode** è specificato con true, l'operazione di preinstallazione viene sempre considerata riuscita, in caso contrario, il codice di uscita 0 indica l'esito positivo, 1 è stata annullata e altri valori indicano un errore. Tag **NormalArgs**, **SilentArgs**, e **IgnoreExitCode** sono facoltativi.  
  
 Operazione di disinstallazione personalizzata può essere utilizzato per le seguenti:  
  
-   Sostituire la finestra di dialogo di conferma predefinita.  
  
-   Popolare le finestre di dialogo personalizzate prima della disinstallazione.  
  
-   Eseguire alcune attività prima della disinstallazione.  
  
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
  
 Wherein **full-path-to-exefile** indica l'exefile già installato nel sistema. **Argomenti** è facoltativo e specifica gli argomenti della riga di comando per l'exefile. L'exefile viene richiamato prima di disinstallazione conferma visualizzata la finestra di.  
  
 L'exefile può fare quanto segue in questa fase:  
  
-   Verrà visualizzata in alcune finestre di dialogo per l'interazione dell'utente.  
  
-   Eseguire alcune attività in background.  
  
 Il codice di uscita del file exe determina come il processo di disinstallazione si sposta in avanti:  
  
-   0: il processo di disinstallazione prosegue senza popolare la finestra di dialogo di conferma predefinita, come già confermato dall'utente. (questo approccio può essere utilizzato per sostituire la finestra di dialogo di conferma predefinita);  
  
-   1: il processo di disinstallazione viene annullato e infine verrà visualizzato un messaggio all'utente. Nulla cambia;  
  
-   Altro: il processo di disinstallazione prosegue con finestra di dialogo di conferma predefinita, come l'operazione di disinstallazione personalizzata non è presente.  
  
 Qualsiasi errore mentre viene richiamato exefile provoca lo stesso risultato quando exefile restituisce un codice diverso da 0 o 1.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)