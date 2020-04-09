---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Distribuzione dei controlli di sicurezza con criteri di accesso centrale (procedura dimostrativa)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 9ad3f069eaea6917d29f56a00c6ecde035ecb01d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861194"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Distribuzione dei controlli di sicurezza con criteri di accesso centrale (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo scenario, si verificherà l'accesso ai file nella cartella Finance Documents usando il criterio Finance creato in [distribuire una procedura &#40;&#41;dimostrativa per i criteri di accesso centrale](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Se un utente non autorizzato tenta di accedere alla cartella, l'attività viene registrata dal visualizzatore eventi.   
 Per testare questo scenario è necessario eseguire la procedura seguente.  
  
|Attività|Descrizione|  
|--------|---------------|  
|[Configurare l'accesso agli oggetti globali](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Questo passaggio consente di configurare il criterio di accesso agli oggetti globale nel controller di dominio.|  
|[Aggiorna impostazioni Criteri di gruppo](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Accedere al file server e applicare l'aggiornamento dei Criteri di gruppo.|  
|[Verificare che il criterio di accesso agli oggetti globale sia stato applicato](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Utilizzare il visualizzatore eventi per visualizzare gli eventi pertinenti, tra cui i metadati relativi al Paese e al tipo di documento.|  
  
## <a name="configure-global-object-access-policy"></a><a name="BKMK_1"></a>Configurare i criteri di accesso agli oggetti globali  
Questo passaggio consente di configurare il criterio di accesso agli oggetti globale nel controller di dominio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Per configurare un criterio di accesso agli oggetti globale  
  
1. Accedere al controller di dominio DC1 come CONTOSO\Administrator con la password <strong>pass@word1</strong>.  
  
2. In Server Manager scegliere **Strumenti**, quindi fare clic su **Gestione Criteri di gruppo**.  
  
3. Nell'albero della console fare doppio clic su **Domini**, quindi su **contoso.com**, selezionare **Contoso** e quindi fare doppio clic su **File server**.  
  
4. Fare clic con il pulsante destro del mouse su **FlexibleAccessGPO**, quindi selezionare **Modifica**.  
  
5. Fare doppio clic su **Configurazione computer**, quindi su **Criteri** e infine su **Impostazioni di Windows**.  
  
6. Fare doppio clic su **Impostazioni sicurezza**, quindi su **Configurazione avanzata dei criteri di controllo** e infine su **Criteri di controllo**.  
  
7. Fare doppio clic su **Accesso agli oggetti** e quindi su **Controlla File system**.  
  
8. Selezionare la casella di controllo **Configura gli eventi di controllo seguenti**, quindi le caselle di controllo **Operazione riuscita** ed **Errore** e infine fare clic su **OK**.  
  
9. Nel riquadro di spostamento fare doppio clic su **Controllo di accesso agli oggetti globale**, quindi su **File system**.  
  
10. Selezionare la casella di controllo **Definisci questa impostazione di criterio**, quindi fare clic su **Configura**.  
  
11. Nella casella **Impostazioni avanzate di sicurezza per Global File SACL** fare clic su **Aggiungi**, quindi su **Seleziona un'entità**, digitare **Everyone** e infine fare clic su **OK**.  
  
12. In **Voci di controllo per Global File SACL** selezionare **Controllo completo** nella casella **Autorizzazioni**.  
  
13. Nella sezione **Aggiungi una condizione:** fare clic su **Aggiungi una condizione** e negli elenchi a discesa selezionare   
    [**Risorsa**] [**Reparto**] [**Any of**] [**Valore**] [**Finance**].  
  
14. Fare clic tre volte su **OK** per completare la configurazione del criterio di controllo dell'accesso agli oggetti globale.  
  
15. Nel riquadro di spostamento fare clic su **Accesso agli oggetti**, quindi nel riquadro dei risultati fare doppio clic su **Controlla Modifica handle**. Fare clic su **Configura gli eventi di controllo seguenti**, **Operazione riuscita** ed **Errore**, fare clic su **OK** e quindi chiudere l'oggetto Criteri di gruppo ad accesso flessibile.  
  
## <a name="update-group-policy-settings"></a><a name="BKMK_2"></a>Aggiorna impostazioni Criteri di gruppo  
Questo passaggio consente di aggiornare le impostazioni di Criteri di gruppo dopo aver creato il criterio di controllo.  
  
#### <a name="to-update-group-policy-settings"></a>Per aggiornare le impostazioni di Criteri di gruppo  
  
1. Accedere al file server, FILE1 come contoso\Administrator, con la password <strong>pass@word1</strong>.  
  
2. Premere il tasto WINDOWS+R, quindi digitare **cmd** per aprire la finestra del prompt dei comandi.  
  
   > [!NOTE]  
   > Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
3. Digitare **gpupdate /force** e quindi premere INVIO.  
  
## <a name="verify-that-the-global-object-access-policy-has-been-applied"></a><a name="BKMK_3"></a>Verificare che il criterio di accesso agli oggetti globale sia stato applicato  
Dopo che le impostazioni di Criteri di gruppo sono state applicate, è possibile verificare che le impostazioni di controllo siano state applicate correttamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Per verificare che il criterio di controllo di accesso agli oggetti globale sia stato applicato  
  
1.  Accedere al computer client CLIENT1 come Contoso\MReid. Passare alla cartella HYPERLINK "file:///\\\\\\\ ID_AD_FILE1\\\Finance" \\\ FILE1\Finance Documents e modificare Word Document 2.  
  
2.  Accedere al file server FILE1 come contoso\administrator. Aprire il visualizzatore eventi, passare a **Registri di Windows**, selezionare **Sicurezza** e confermare che le attività eseguite siano segnalate negli eventi di controllo **4656** e **4663** (anche se non erano stati impostati SACL di controllo espliciti su cartelle o file creati, modificati ed eliminati).  
  
> [!IMPORTANT]  
> Nel computer che ospita la risorsa viene generato un nuovo evento di accesso per conto dell'utente per il quale vengono monitorati gli accessi validi. Quando i registri di controllo della sicurezza vengono analizzati per individuare le attività di accesso degli utenti, per differenziare tra gli eventi di accesso generati dall'accesso valido e gli eventi di accesso generati da un accesso utente interattivo alla rete vengono fornite informazioni sul Livello rappresentazione. Quando l'evento di accesso è generato da un accesso valido, il Livello rappresentazione è Identità. L'accesso utente interattivo alla rete genera normalmente un evento di accesso con Livello rappresentazione = Rappresentazione oppure Delega.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: controllo di accesso ai file](Scenario--File-Access-Auditing.md)  
  
-   [Pianificare il controllo dell'accesso ai file](Plan-for-File-Access-Auditing.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

