---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Distribuzione dei controlli di sicurezza con criteri di accesso centrale (procedura dimostrativa)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b14ded98c4f1a340349119bd9f5f42e3a1bf9434
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445740"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Distribuzione dei controlli di sicurezza con criteri di accesso centrale (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo scenario, si controllerà l'accesso ai file nella cartella Finance Documents utilizzando il criterio finanziario creato nella [distribuire un criterio di accesso centrale &#40;passaggi&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Se un utente non autorizzato tenta di accedere alla cartella, l'attività viene registrata dal visualizzatore eventi.   
 Per testare questo scenario è necessario eseguire la procedura seguente.  
  
|Attività|Descrizione|  
|--------|---------------|  
|[Configurare l'accesso agli oggetti globale](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Questo passaggio consente di configurare il criterio di accesso agli oggetti globale nel controller di dominio.|  
|[Impostazioni dei criteri di gruppo di aggiornamento](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Accedere al file server e applicare l'aggiornamento dei Criteri di gruppo.|  
|[Verificare che sia stato applicato il criterio di accesso agli oggetti globale](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Utilizzare il visualizzatore eventi per visualizzare gli eventi pertinenti, tra cui i metadati relativi al Paese e al tipo di documento.|  
  
## <a name="BKMK_1"></a>Configura criterio accesso agli oggetti globale  
Questo passaggio consente di configurare il criterio di accesso agli oggetti globale nel controller di dominio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Per configurare un criterio di accesso agli oggetti globale  
  
1. Accedere al controller di dominio DC1 specificando Contoso\administrator con la password <strong>pass@word1</strong>.  
  
2. In Server Manager scegliere **Strumenti**, quindi fare clic su **Gestione Criteri di gruppo**.  
  
3. Nell'albero della console fare doppio clic su **Domini**, quindi su **contoso.com**, selezionare **Contoso** e quindi fare doppio clic su **File server**.  
  
4. Fare clic con il pulsante destro del mouse su **FlexibleAccessGPO**, quindi selezionare **Modifica**.  
  
5. Fare doppio clic su **Configurazione computer**, fare doppio clic su **Criteri** e quindi fare doppio clic su **Impostazioni di Windows**.  
  
6. Fare doppio clic su **Impostazioni sicurezza**, quindi su **Configurazione avanzata dei criteri di controllo** e infine su **Criteri di controllo**.  
  
7. Fare doppio clic su **Accesso agli oggetti** e quindi su **Controlla File system**.  
  
8. Selezionare la casella di controllo **Configura gli eventi di controllo seguenti**, quindi le caselle di controllo **Operazioni riuscite** e **Operazioni non riuscite** e infine fare clic su **OK**.  
  
9. Nel riquadro di spostamento fare doppio clic su **Controllo di accesso agli oggetti globale**, quindi su **File system**.  
  
10. Selezionare la casella di controllo **Definisci questa impostazione di criterio**, quindi fare clic su **Configura**.  
  
11. Nella casella **Impostazioni avanzate di sicurezza per Global File SACL** fare clic su **Aggiungi**, quindi su **Seleziona un'entità**, digitare **Everyone** e infine fare clic su **OK**.  
  
12. In **Voci di controllo per Global File SACL** selezionare **Controllo completo** nella casella **Autorizzazioni**.  
  
13. Nel **aggiungere una condizione:** fare clic su **Aggiungi una condizione** e nell'elenco a discesa Elenca select   
    [**Resource**] [**reparto**] [**uno qualsiasi dei**] [**valore**] [**Finance**].  
  
14. Fare clic tre volte su **OK** per completare la configurazione del criterio di controllo dell'accesso agli oggetti globale.  
  
15. Nel riquadro di spostamento fare clic su **Accesso agli oggetti**, quindi nel riquadro dei risultati fare doppio clic su **Controlla Modifica handle**. Fare clic su **Configura gli eventi di controllo seguenti**, **Operazione riuscita** ed **Errore**, fare clic su **OK** e quindi chiudere l'oggetto Criteri di gruppo ad accesso flessibile.  
  
## <a name="BKMK_2"></a>Aggiornare le impostazioni di criteri di gruppo  
Questo passaggio consente di aggiornare le impostazioni di Criteri di gruppo dopo aver creato il criterio di controllo.  
  
#### <a name="to-update-group-policy-settings"></a>Per aggiornare le impostazioni di Criteri di gruppo  
  
1. Accedere al file server FILE1 come contoso\Administrator, con la password <strong>pass@word1</strong>.  
  
2. Premere il tasto WINDOWS+R, quindi digitare **cmd** per aprire la finestra del prompt dei comandi.  
  
   > [!NOTE]  
   > Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3. Digitare **gpupdate /force** e quindi premere INVIO.  
  
## <a name="BKMK_3"></a>Verificare che sia stato applicato il criterio di accesso agli oggetti globale  
Dopo che le impostazioni di Criteri di gruppo sono state applicate, è possibile verificare che le impostazioni di controllo siano state applicate correttamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Per verificare che il criterio di controllo di accesso agli oggetti globale sia stato applicato  
  
1.  Accedere al computer client CLIENT1 come Contoso\MReid. Passare alla cartella HYPERLINK "file:///\\\\\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance documenti e modificare i 2 documenti di Word.  
  
2.  Accedere al file server FILE1 come contoso\administrator. Aprire il visualizzatore eventi, passare a **Registri di Windows**, selezionare **Sicurezza** e confermare che le attività eseguite siano segnalate negli eventi di controllo **4656** e **4663** (anche se non erano stati impostati SACL di controllo espliciti su cartelle o file creati, modificati ed eliminati).  
  
> [!IMPORTANT]  
> Nel computer che ospita la risorsa viene generato un nuovo evento di accesso per conto dell'utente per il quale vengono monitorati gli accessi validi. Quando i registri di controllo della sicurezza vengono analizzati per individuare le attività di accesso degli utenti, per differenziare tra gli eventi di accesso generati dall'accesso valido e gli eventi di accesso generati da un accesso utente interattivo alla rete vengono fornite informazioni sul Livello rappresentazione. Quando l'evento di accesso è generato da un accesso valido, il Livello rappresentazione è Identità. L'accesso utente interattivo alla rete genera normalmente un evento di accesso con Livello rappresentazione = Rappresentazione oppure Delega.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: Controllo dell'accesso ai file](Scenario--File-Access-Auditing.md)  
  
-   [Pianificare il controllo dell'accesso ai file](Plan-for-File-Access-Auditing.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

