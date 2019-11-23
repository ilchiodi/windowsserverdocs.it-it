---
title: Utilizzare criteri di gruppo per configurare computer Client membri del dominio
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: dougkim
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.date: 06/02/2018
ms.openlocfilehash: 6f093e605ce735d8f86f7f4d479a646d144e8829
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356524"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Utilizzare criteri di gruppo per configurare computer Client membri del dominio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa sezione viene creato un oggetto Criteri di gruppo per tutti i computer dell'organizzazione, vengono configurati i computer client membri del dominio con la modalità cache distribuita o la modalità cache ospitata e viene configurato Windows Firewall con sicurezza avanzata per consentire BranchCache traffico.  
  
In questa sezione contiene le procedure seguenti.  
  
1.  [Per creare un oggetto Criteri di gruppo e configurare le modalità BranchCache](#bkmk_gp)  
  
2.  [Per configurare Windows Firewall con regole di traffico in ingresso con sicurezza avanzata](#bkmk_inbound)  
  
3.  [Per configurare Windows Firewall con regole di traffico in uscita con sicurezza avanzata](#bkmk_outbound)  
  
> [!TIP]  
> Nella procedura seguente viene illustrato come creare un oggetto Criteri di gruppo nei criteri di dominio predefiniti, tuttavia, è possibile creare l'oggetto in un'unità organizzativa (OU) o in un altro contenitore appropriato per la distribuzione.  
  
È necessario essere un membro di **Domain Admins**, o equivalente a eseguire queste procedure.  
  
## <a name="bkmk_gp"></a>Per creare un oggetto Criteri di gruppo e configurare le modalità BranchCache  
  
1.  In un computer su cui è installato il ruolo server Servizi di dominio Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**. Viene aperta la console di gestione Criteri di gruppo.  
  
2.  Nella console di gestione di Criteri di gruppo espandere il percorso seguente: **foresta:** *example.com*, **domini**, *example.com*, **criteri di gruppo oggetti**, dove *example.com* è il nome del dominio in cui si trovano gli account computer client BranchCache che si desidera configurare.  
  
3.  Fare doppio clic su **oggetti Criteri di gruppo**, quindi fare clic su **nuovo**. Verrà visualizzata la finestra di dialogo **Nuovo oggetto Criteri di gruppo**. In **nome**, digitare un nome per il nuovo oggetto (criteri di gruppo). Ad esempio, se si desidera assegnare all'oggetto il nome Computer client con BranchCache, digitare **Computer client con BranchCache**. Fai clic su **OK**.  
  
4.  Nella console Gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** sia selezionata e nel riquadro dei dettagli fare doppio clic su oggetto Criteri di gruppo appena creato. Ad esempio, se è denominato computer Client BranchCache oggetto Criteri di gruppo, fare doppio clic su **computer Client con BranchCache**. Fare clic su **modificare**. Si apre la console Editor Gestione Criteri di gruppo.  
  
5.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **Configurazione Computer**, **criteri**, **modelli amministrativi: definizioni di criteri (file ADMX) recuperate dal computer locale**, **rete**, **BranchCache**.  
  
6.  Fare clic su **BranchCache**, quindi nel riquadro dei dettagli, fare doppio clic su **Attiva BranchCache**. Verrà visualizzata la finestra di dialogo impostazione criterio.  
  
7.  Nella finestra di dialogo **Attiva BranchCache** fare clic su **Abilitato** e quindi su **OK**.  
  
8.  Per abilitare la modalità cache distribuita BranchCache, nel riquadro dei dettagli, fare doppio clic su **la modalità Cache distribuita BranchCache impostare**. Verrà visualizzata la finestra di dialogo impostazione criterio.  
  
9. Nella finestra di dialogo **Imposta modalità Cache distribuita di BranchCache** fare clic su **Abilitato** e quindi su **OK**.  
  
10. Se si dispongono di uno o più succursali in cui si distribuisce BranchCache in modalità cache ospitata e aver distribuito il server di cache in tali uffici ospitata, fare doppio clic su **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio**. Verrà visualizzata la finestra di dialogo impostazione criterio.  
  
11. Nel **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** la finestra di dialogo, fare clic su **abilitato**, e quindi fare clic su **OK**.  
  
    > [!NOTE]  
    > Quando si abilita sia il **la modalità Cache distribuita BranchCache impostare** e **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** impostazioni dei criteri, i computer client operano in modalità cache distribuita BranchCache a meno che non si trova un server cache ospitata nella succursale, momento in cui operano in modalità cache ospitata.  
  
12. Utilizzare le procedure seguenti per configurare le impostazioni del firewall nei computer client utilizzando criteri di gruppo.  
  
## <a name="bkmk_inbound"></a>Per configurare Windows Firewall con regole di traffico in ingresso con sicurezza avanzata  
  
1.  Nella console di gestione di Criteri di gruppo espandere il percorso seguente: **foresta:** *example.com*, **domini**, *example.com*, **criteri di gruppo oggetti**, dove *example.com* è il nome del dominio in cui si trovano gli account computer client BranchCache che si desidera configurare.  
  
2.  Nella console Gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** sia selezionata e nel riquadro dei dettagli fare doppio clic su computer client con BranchCache oggetto Criteri di gruppo creato in precedenza. Ad esempio, se è denominato computer Client BranchCache oggetto Criteri di gruppo, fare doppio clic su **computer Client con BranchCache**. Fare clic su **modificare**. Si apre la console Editor Gestione Criteri di gruppo.  
  
3.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **Configurazione Computer**, **criteri**, **le impostazioni di Windows**, **le impostazioni di sicurezza**, **Windows Firewall con sicurezza avanzata**, **Windows Firewall con sicurezza avanzata - LDAP**, **Regole connessioni in entrata**.  
  
4.  Fare clic con il pulsante destro del mouse su **Regole in entrata** e quindi scegliere **Nuova regola**. Verrà visualizzata la Creazione guidata nuova regola connessioni in entrata.  
  
5.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - recupero contenuto (utilizza HTTP)** . Fai clic su **Next**.  
  
6.  In **regole predefinite**, fare clic su **Avanti**.  
  
7.  In **Azione** assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **Fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache sia in grado di ricevere traffico su questa porta.  
  
8.  Per creare l'eccezione del firewall WS-Discovery, fare nuovamente clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola**. Verrà visualizzata la Creazione guidata nuova regola connessioni in entrata.  
  
9. In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - individuazione Peer (utilizza WSD)** . Fai clic su **Next**.  
  
10. In **regole predefinite**, fare clic su **Avanti**.  
  
11. In **Azione** assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **Fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache sia in grado di ricevere traffico su questa porta.  
  
## <a name="bkmk_outbound"></a>Per configurare Windows Firewall con regole di traffico in uscita con sicurezza avanzata  
  
1.  Nella console Editor Gestione Criteri di gruppo fare clic con il pulsante destro del mouse su **Regole in uscita** e quindi scegliere **Nuova regola**. Verrà visualizzata la Creazione guidata nuova regola connessioni in uscita.  
  
2.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - recupero contenuto (utilizza HTTP)** . Fai clic su **Next**.  
  
3.  In **regole predefinite**, fare clic su **Avanti**.  
  
4.  In **Azione** assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **Fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache sia in grado di inviare traffico su questa porta.  
  
5.  Per creare l'eccezione del firewall WS-Discovery, fare nuovamente clic con il pulsante destro del mouse su **Regole in uscita**, quindi scegliere **Nuova regola**. Verrà visualizzata la Creazione guidata nuova regola connessioni in uscita.  
  
6.  In **tipo di regola**, fare clic su **predefinito**, espandere l'elenco di scelte e quindi fare clic su **BranchCache - individuazione Peer (utilizza WSD)** . Fai clic su **Next**.  
  
7.  In **regole predefinite**, fare clic su **Avanti**.  
  
8.  In **Azione** assicurarsi che **Consenti la connessione** sia selezionata e quindi fare clic su **Fine**.  
  
    > [!IMPORTANT]  
    > È necessario selezionare **Consenti la connessione** per il client con BranchCache sia in grado di inviare traffico su questa porta.  
  


