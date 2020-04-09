---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Controllo dell'elaborazione della riga di comando
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dc6cba306a36589d8b585b23ecb43e7d16b7d201
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823074"
---
# <a name="command-line-process-auditing"></a>Controllo dell'elaborazione della riga di comando

>Si applica a: Windows Server 2016, Windows Server 2012 R2

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
  
-   L'evento di controllo Creazione 4688 ID processo preesistenti ora includerà informazioni di controllo per i processi di riga di comando.  
  
-   Hash SHA1/2 del file eseguibile verrà inoltre registrato nel registro eventi di Applocker  
  
    -   Applicazioni e servizi\microsoft\windows\applocker  
  
-   Si abilita tramite oggetto Criteri di gruppo, ma è disabilitato per impostazione predefinita  
  
    -   "Includi riga di comando di elaborare gli eventi di creazione"  
  
![il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura SEQ figura \\\* arabo 16 evento 4688**  
  
Esaminare l'evento ID 4688 aggiornato in REF _Ref366427278 \h figura 16.  Prima di questa aggiornare nessuna delle informazioni per **riga di comando processo** viene registrato.  A causa di questa registrazione aggiuntiva ora noteremo che è stato avviato il processo wscript.exe non solo, ma che è stato inoltre utilizzato per eseguire uno script VB.  
  
## <a name="configuration"></a>Configurazione  
Per osservare gli effetti di questo aggiornamento, è necessario abilitare due impostazioni dei criteri.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>È necessario che il controllo è attivato per verificare l'evento ID 4688 la creazione del processo di controllo.  
Per abilitare i criteri di creazione del processo di controllo, modificare i criteri di gruppo seguente:  
  
**Percorso di criteri:** Configurazione Computer > Criteri > Impostazioni di Windows > Impostazioni di sicurezza > Configurazione avanzata di controllo > analisi dettagliata  
  
**Nome del criterio:** la creazione del processo di controllo  
  
**Supportato in:** Windows 7 e versioni successive  
  
**Descrizione/guida:**  
  
Questa impostazione di criteri di protezione determina se il sistema operativo genera eventi di controllo quando viene creato un processo (inizio) e il nome del programma o dell'utente che l'ha creata.  
  
Questi eventi di controllo consente di comprendere come viene utilizzato un computer e tenere traccia delle attività dell'utente.  
  
Volume di eventi: bassa a supporto, a seconda dell'utilizzo del sistema  
  
**Valore predefinito:** non configurato  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Per visualizzare le aggiunte all'evento 4688 ID, è necessario abilitare la nuova impostazione di criteri: includono una riga di comando di elaborare gli eventi di creazione  
**Tabella SEQ tabella \\\* l'impostazione dei criteri del processo della riga di comando di 19 arabo**  
  
|Configurazione dei criteri|Dettagli|  
|------------------------|-----------|  
|**Percorso**|Creazione di processi amministrativi Templates\System\Audit|  
|**Impostazione**|**Includi riga di comando negli eventi di creazione del processo**|  
|**Impostazione predefinita**|Non configurata (non abilitata)|  
|**Supportato in:**|?|  
|**Descrizione**|Questa impostazione di criterio determina quali informazioni vengono registrate negli eventi di controllo di sicurezza quando viene creato un nuovo processo.<p>Questa impostazione si applica solo quando il criterio di creazione del processo di controllo è abilitato. Se si abilita questa impostazione di criteri, che le informazioni della riga di comando per ogni processo verranno registrate in testo normale nel registro eventi di protezione come parte dell'evento di creazione del processo di controllo 4688, "un nuovo processo creato," sulla workstation e server in cui viene applicata questa impostazione di criteri.<p>Se si disabilita o non si configura questa impostazione di criteri, le informazioni del processo della riga di comando non includerà negli eventi di creazione del processo di controllo.<p>Valore predefinito: Non configurato<p>Nota: Quando questa impostazione di criterio è abilitata, qualsiasi utente con accesso per leggere che gli eventi di sicurezza sarà in grado di leggere gli argomenti della riga di comando per qualsiasi correttamente creato processo. Argomenti della riga di comando possono contenere informazioni riservate o private, ad esempio password o dati utente.|  
  
![il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Quando si utilizzano le impostazioni di Configurazione avanzata dei criteri di controllo, è necessario confermare che tali impostazioni non vengano sovrascritte dalle impostazioni di base dei criteri di controllo.  Evento 4719 viene registrato quando le impostazioni vengono sovrascritte.  
  
![il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
La procedura seguente illustra come evitare conflitti, bloccando l'applicazione di eventuali impostazioni di base dei criteri di controllo.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Per verificare che le impostazioni di Configurazione avanzata dei criteri di controllo non vengano sovrascritte  
![il controllo della riga di comando](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Aprire la console Gestione criteri di gruppo  
  
2.  Fare clic con il pulsante destro del mouse su Criterio dominio predefinito, quindi scegliere Modifica.  
  
3.  Fare doppio clic su Configurazione computer, quindi su Criteri e infine su Impostazioni di Windows.  
  
4.  Fare doppio clic su impostazioni di sicurezza, fare doppio clic su criteri locali e quindi su opzioni di sicurezza.  
  
5.  Fare doppio clic su Audit: Controllo: imposizione delle impostazioni di sottocategoria del criterio di controllo (Windows Vista o versione successiva) per sostituire le impostazioni di categoria del criterio di controllo e quindi fare clic su Definisci le impostazioni relative al criterio.  
  
6.  Fare clic su attivato e quindi fare clic su OK.  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
[Creazione del processo di controllo](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guida dettagliata ai criteri avanzati di controllo della sicurezza](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: domande frequenti](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Procedura: Esplorare controllo dell'elaborazione della riga di comando  
  
1.  Abilitare **la creazione del processo di controllo** eventi e verificare la configurazione dei criteri di controllo avanzate non viene sovrascritto.  
  
2.  Creare uno script che genera alcuni eventi di interesse ed eseguire lo script.  Osservare gli eventi.  Lo script utilizzato per generare l'evento nella lezione simile al seguente:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Abilitare il controllo dell'elaborazione della riga di comando  
  
4.  Eseguire lo stesso script e osservare gli eventi  
  


