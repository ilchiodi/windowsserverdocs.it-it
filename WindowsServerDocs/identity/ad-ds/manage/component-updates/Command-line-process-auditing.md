---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Controllo dell'elaborazione della riga di comando
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>Controllo dell'elaborazione della riga di comando

>Si applica a: Windows Server 2016, Windows Server 2012 R2

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
  
-   L'evento di controllo Creazione 4688 ID processo preesistenti ora includerà informazioni di controllo per i processi di riga di comando.  
  
-   Hash SHA1/2 del file eseguibile verrà inoltre registrato nel registro eventi di Applocker  
  
    -   Applicazioni e servizi\microsoft\windows\applocker  
  
-   Si abilita tramite oggetto Criteri di gruppo, ma è disabilitato per impostazione predefinita  
  
    -   "Includi riga di comando di elaborare gli eventi di creazione"  
  
![Il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura SEQ Figure \ * evento 16 ARABO 4688**  
  
Esaminare l'evento ID 4688 aggiornato in REF ref366427278 \h figura 16.  Prima di questa aggiornare nessuna delle informazioni per **riga di comando processo** viene registrato.  A causa di questa registrazione aggiuntiva ora noteremo che è stato avviato il processo wscript.exe non solo, ma che è stato inoltre utilizzato per eseguire uno script VB.  
  
## <a name="configuration"></a>Configurazione  
Per osservare gli effetti di questo aggiornamento, è necessario abilitare due impostazioni dei criteri.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>È necessario che il controllo è attivato per verificare l'evento ID 4688 la creazione del processo di controllo.  
Per abilitare il criterio di creazione del processo di controllo, modificare i criteri di gruppo seguenti:  
  
**Percorso di criteri:** configurazione Computer > Criteri > Impostazioni di Windows > Impostazioni di sicurezza > Configurazione avanzata di controllo > analisi dettagliata  
  
**Nome criterio:** la creazione del processo di controllo  
  
**Supportato in:** Windows 7 e versioni successive  
  
**Descrizione/Guida:**  
  
Questa impostazione di criteri di protezione determina se il sistema operativo genera eventi di controllo quando viene creato un processo (inizio) e il nome del programma o dell'utente che l'ha creata.  
  
Questi eventi di controllo consente di comprendere come viene utilizzato un computer e tenere traccia delle attività dell'utente.  
  
Volume di eventi: bassa a supporto, a seconda di utilizzo del sistema  
  
**Valore predefinito:** non configurato  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Per visualizzare le aggiunte all'evento 4688 ID, è necessario abilitare la nuova impostazione dei criteri: includono una riga di comando di elaborare gli eventi di creazione  
**Tabella SEQ tabella \ \ \ * impostazione di criteri processo riga di comando 19 ARABO**  
  
|Configurazione dei criteri|Dettagli|  
|------------------------|-----------|  
|**Percorso**|Creazione di processi amministrativi Templates\System\Audit|  
|**Impostazione**|**Includi riga di comando di elaborare gli eventi di creazione**|  
|**Impostazione predefinita**|Non configurata (non abilitata)|  
|**Supportato in:**|?|  
|**Descrizione**|Questa impostazione di criterio determina quali informazioni vengono registrate negli eventi di controllo di sicurezza dopo aver creato un nuovo processo.<br /><br />Questa impostazione si applica solo quando il criterio di creazione del processo di controllo è abilitato. Se si abilita questa impostazione di criteri, che le informazioni della riga di comando per ogni processo verranno eseguite in testo normale nel registro eventi di protezione come parte dell'evento di creazione del processo di controllo 4688, "un nuovo processo creato," sulla workstation e server in cui viene applicata questa impostazione di criteri.<br /><br />Se si disabilita o non si configura questa impostazione di criteri, le informazioni del processo della riga di comando non includerà negli eventi di creazione del processo di controllo.<br /><br />Valore predefinito: Non configurato<br /><br />Nota: Quando questa impostazione è abilitata, qualsiasi utente con accesso di leggere che gli eventi di sicurezza sarà in grado di leggere gli argomenti della riga di comando per qualsiasi correttamente creato processo. Argomenti della riga di comando possono contenere informazioni riservate o private, ad esempio password o dati utente.|  
  
![Il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Quando si utilizzano impostazioni avanzate di configurazione di criteri di controllo, è necessario confermare che queste impostazioni non vengano sovrascritte dalle impostazioni di criteri di controllo di base.  Evento 4719 viene registrato quando le impostazioni vengono sovrascritte.  
  
![Il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
La procedura seguente viene illustrato come evitare conflitti, bloccando l'applicazione di eventuali impostazioni di criteri di controllo di base.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Per garantire che le impostazioni di configurazione avanzata dei criteri di controllo non vengano sovrascritte  
![Il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Aprire la console Gestione criteri di gruppo  
  
2.  Fare doppio clic su criterio dominio predefinito e quindi fare clic su Modifica.  
  
3.  Fare doppio clic su configurazione Computer, fare doppio clic su criteri e quindi fare doppio clic su impostazioni di Windows.  
  
4.  Fare doppio clic su impostazioni di sicurezza, fare doppio clic su criteri locali e quindi fare clic su Opzioni di protezione.  
  
5.  Fare doppio clic sul controllo: Imposizione criteri sottocategoria delle impostazioni controllo (Windows Vista o versione successiva) per sostituire le impostazioni di categoria dei criteri di controllo, quindi fare clic su Definisci questa impostazione.  
  
6.  Fare clic su attivato e quindi fare clic su OK.  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
[Creazione del processo di controllo](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guida dettagliata ai criteri di controllo della protezione avanzata](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Domande frequenti](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Procedura: Esplorare controllo dell'elaborazione della riga di comando  
  
1.  Abilitare **creazione del processo di controllo** eventi e verificare la configurazione di criteri di controllo avanzate non viene sovrascritto.  
  
2.  Creare uno script che genera alcuni eventi di interesse ed eseguire lo script.  Osservare gli eventi.  Lo script utilizzato per generare l'evento nella lezione simile al seguente:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Abilitare il controllo dell'elaborazione della riga di comando  
  
4.  Eseguire lo stesso script e osservare gli eventi  
  


