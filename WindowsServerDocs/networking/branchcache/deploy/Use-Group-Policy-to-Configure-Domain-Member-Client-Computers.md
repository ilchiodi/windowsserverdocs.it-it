---
title: Utilizzare criteri di gruppo per configurare computer Client membri del dominio
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 96bf84df14eac8016a898e92eb96f90435c53c98
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Utilizzare criteri di gruppo per configurare computer Client membri del dominio

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare queste procedure per creare un oggetto Criteri di gruppo per tutti i computer nell'organizzazione, configurare i computer client membri del dominio con la modalità cache distribuita o modalità cache ospitata e configurare Windows Firewall con sicurezza avanzata per consentire il traffico BranchCache.  
  
Questa sezione contiene le procedure seguenti.  
  
1.  [Per creare un oggetto Criteri di gruppo e configurare la modalità di BranchCache](#bkmk_gp)  
  
2.  [Per configurare Windows Firewall con Advanced regole di traffico in ingresso di sicurezza](#bkmk_inbound)  
  
3.  [Per configurare Windows Firewall con Advanced regole di traffico in uscita di sicurezza](#bkmk_outbound)  
  
> [!TIP]  
> Nella procedura seguente, viene richiesto di creare un oggetto Criteri di gruppo in criterio dominio predefinito, tuttavia è possibile creare l'oggetto in un'unità organizzativa (OU) o altri contenitori che è appropriato per la distribuzione.  
  
È necessario essere un membro del **Domain Admins**, o equivalente a eseguire queste procedure.  
  
## <a name="bkmk_gp"></a>Per creare un oggetto Criteri di gruppo e configurare la modalità di BranchCache  
  
1.  In un computer su cui è installato il ruolo server Servizi di dominio Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**. Apre la console Gestione criteri di gruppo.  
  
2.  Nella console Gestione criteri di gruppo, espandere il percorso seguente: **foresta:** *example.com*, **domini**, *example.com*, **oggetti Criteri di gruppo**, dove *example.com* è il nome del dominio in cui si trovano gli account dei computer client BranchCache che si desidera configurare.  
  
3.  Fare doppio clic su **oggetti Criteri di gruppo**, quindi fare clic su **New**. Il **nuovo oggetto Criteri di gruppo** apre la finestra di dialogo. In **nome**, digitare un nome per il nuovo oggetto (criteri di gruppo). Ad esempio, se si desidera assegnare all'oggetto computer Client con BranchCache, digitare **computer Client con BranchCache**. Fare clic su **OK**.  
  
4.  Nella console di gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** sia selezionata e nel riquadro dei dettagli fare doppio clic su oggetto Criteri di gruppo appena creato. Ad esempio, se il computer Client con BranchCache oggetto Criteri di gruppo denominata, fare doppio clic su **computer Client con BranchCache**. Fare clic su **modifica**. Apre la console di Editor Gestione criteri di gruppo.  
  
5.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **configurazione Computer**, **criteri**, **modelli amministrativi: definizione dei criteri (file ADMX) recuperate dal computer locale**, **rete**, **BranchCache**.  
  
6.  Fare clic su **BranchCache**e quindi nel riquadro dei dettagli fare doppio clic su **attiva BranchCache**. Apre la finestra di dialogo di impostazione dei criteri.  
  
7.  Nel **attiva BranchCache** la finestra di dialogo, fare clic su **abilitato**, quindi fare clic su **OK**.  
  
8.  Per abilitare la modalità cache distribuita BranchCache, nel riquadro dei dettagli fare doppio clic su **modalità Cache distribuita BranchCache impostare**. Apre la finestra di dialogo di impostazione dei criteri.  
  
9. Nel **modalità Cache distribuita BranchCache impostare** la finestra di dialogo, fare clic su **abilitato**e quindi fare clic su **OK**.  
  
10. Se si dispongono di uno o più succursali in cui si distribuisce BranchCache in modalità cache ospitata e aver distribuito il server di cache in tali uffici ospitata, fare doppio clic su **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio**. Apre la finestra di dialogo di impostazione dei criteri.  
  
11. Nel **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** la finestra di dialogo, fare clic su **abilitato**e quindi fare clic su **OK**.  
  
    > [!NOTE]  
    > Quando si abilita sia il **modalità Cache distribuita BranchCache impostare** e **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** impostazioni dei criteri, i computer client operano in modalità cache distribuita BranchCache a meno che non si trova un server cache ospitata nella succursale, a quel punto operano in modalità cache ospitata.  
  
12. Utilizzare le procedure seguenti per configurare le impostazioni del firewall nei computer client tramite criteri di gruppo.  
  
## <a name="bkmk_inbound"></a>Per configurare Windows Firewall con Advanced regole di traffico in ingresso di sicurezza  
  
1.  Nella console Gestione criteri di gruppo, espandere il percorso seguente: **foresta:** *example.com*, **domini**, *example.com*, **oggetti Criteri di gruppo**, dove *example.com* è il nome del dominio in cui si trovano gli account dei computer client BranchCache che si desidera configurare.  
  
2.  Nella console di gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** sia selezionata e nel riquadro dei dettagli fare doppio clic su computer client con BranchCache oggetto Criteri di gruppo creato in precedenza. Ad esempio, se il computer Client con BranchCache oggetto Criteri di gruppo denominata, fare doppio clic su **computer Client con BranchCache**. Fare clic su **modifica**. Apre la console di Editor Gestione criteri di gruppo.  
  
3.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **configurazione Computer**, **criteri**, **le impostazioni di Windows**, **le impostazioni di sicurezza**, **Windows Firewall con sicurezza avanzata**, **Windows Firewall con sicurezza avanzata - LDAP**, **regole in entrata**.  
  
4.  Fare doppio clic su **regole in entrata**, quindi fare clic su **nuova regola**. Verrà visualizzata la creazione guidata nuova regola connessioni in entrata.  
  
5.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - recupero contenuto (utilizza HTTP)**. Fare clic su **Avanti**.  
  
6.  In **regole predefinite**, fare clic su **Avanti**.  
  
7.  In **azione**, assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache siano in grado di ricevere traffico su questa porta.  
  
8.  Per creare l'eccezione del firewall WS-Discovery, fare nuovamente clic sulla **regole in entrata**, quindi fare clic su **nuova regola**. Verrà visualizzata la creazione guidata nuova regola connessioni in entrata.  
  
9. In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - individuazione Peer (utilizza WSD)**. Fare clic su **Avanti**.  
  
10. In **regole predefinite**, fare clic su **Avanti**.  
  
11. In **azione**, assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache siano in grado di ricevere traffico su questa porta.  
  
## <a name="bkmk_outbound"></a>Per configurare Windows Firewall con Advanced regole di traffico in uscita di sicurezza  
  
1.  Nella console di Editor Gestione criteri di gruppo, fare doppio clic su **regole in uscita**, quindi fare clic su **nuova regola**. Apre la creazione guidata regola in uscita.  
  
2.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - recupero contenuto (utilizza HTTP)**. Fare clic su **Avanti**.  
  
3.  In **regole predefinite**, fare clic su **Avanti**.  
  
4.  In **azione**, assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache siano in grado di inviare traffico su questa porta.  
  
5.  Per creare l'eccezione del firewall WS-Discovery, fare nuovamente clic sulla **regole in uscita**, quindi fare clic su **nuova regola**. Apre la creazione guidata regola in uscita.  
  
6.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - individuazione Peer (utilizza WSD)**. Fare clic su **Avanti**.  
  
7.  In **regole predefinite**, fare clic su **Avanti**.  
  
8.  In **azione**, assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache siano in grado di inviare traffico su questa porta.  
  


