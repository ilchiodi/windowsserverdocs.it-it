---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Distribuire i controlli di sicurezza con criteri di controllo centrale (procedura dimostrativa)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Distribuire i controlli di sicurezza con criteri di controllo centrale (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo scenario verrà controllato l'accesso ai file nella cartella Finance Documents utilizzando il criterio finanziario creato in [distribuire un criterio di accesso centrale & #40; passaggi & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Se un utente non autorizzato di accedere alla cartella tenta di accedervi, l'attività viene registrata nel Visualizzatore eventi.   
 I passaggi seguenti sono necessari per testare questo scenario.  
  
|Attività|Descrizione|  
|--------|---------------|  
|[Configurare l'accesso agli oggetti globale](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|In questo passaggio, si configura il criterio di accesso agli oggetti globale nel controller di dominio.|  
|[Impostazioni di criteri di gruppo di aggiornamento](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Accedere al file server e applicare l'aggiornamento di criteri di gruppo.|  
|[Verificare che sia stato applicato il criterio di accesso agli oggetti globale](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Visualizzare gli eventi pertinenti nel Visualizzatore eventi. Gli eventi devono includere i metadati per il tipo di paese e dei documenti.|  
  
## <a name="BKMK_1"></a>Configurare il criterio di accesso agli oggetti globale  
In questo passaggio, si configura il criterio di accesso agli oggetti globale nel controller di dominio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Per configurare un criterio di accesso agli oggetti globale  
  
1.  Accedere al controller di dominio DC1 specificando contoso\administrator e la password **pass@word1**.  
  
2.  In Server Manager fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo **.  
  
3.  Nell'albero della console, fare doppio clic su **domini**, fare doppio clic su **contoso.com**, fare clic su **Contoso**e quindi fare doppio clic su **File server **.  
  
4.  Fare doppio clic su **FlexibleAccessGPO**e fare clic su **modifica **.  
  
5.  Fare doppio clic su **configurazione Computer**, fare doppio clic su **criteri**e quindi fare doppio clic su **le impostazioni di Windows **.  
  
6.  Fare doppio clic su **le impostazioni di sicurezza**, fare doppio clic su **configurazione avanzata dei criteri di controllo**e quindi fare doppio clic su **criteri di controllo **.  
  
7.  Fare doppio clic su **accesso agli oggetti**e quindi fare doppio clic su **controlla File System **.  
  
8.  Selezionare il **configura gli eventi seguenti** casella di controllo, seleziona il **successo** e **errore** caselle di controllo, quindi fare clic su **OK **.  
  
9. Nel riquadro di spostamento, fare doppio clic su **globale accesso agli oggetti**e quindi fare doppio clic su **File system **.  
  
10. Selezionare il **definire questa impostazione di criteri** casella di controllo e fare clic su **configura **.  
  
11. Nel **impostazioni avanzate di sicurezza per Global File SACL** fare clic su **Aggiungi**, quindi fare clic su **seleziona un'entità**, tipo **Everyone**e quindi fare clic su **OK **.  
  
12. Nel **voci di controllo per Global File SACL**, quindi selezionare **controllo completo** nel **autorizzazioni** casella.  
  
13. Nel **aggiungere una condizione:** fare clic su **aggiungere una condizione** e nell'elenco a discesa selezionare   
    [**Risorse**] [**Reparto**] [**Any of**] [**Value**] [**Finance**].  
  
14. Fare clic su **OK** criterio di controllo tre volte per completare la configurazione di accesso agli oggetti globale.  
  
15. Nel riquadro di spostamento, fare clic su **accesso agli oggetti**, nel riquadro dei risultati, fare doppio clic su **controllo della modifica Handle **. Fare clic su **configura gli eventi di controllo seguenti**, **successo**, e **errore**, fare clic su **OK**e quindi chiudere l'oggetto Criteri di gruppo di accesso flessibile.  
  
## <a name="BKMK_2"></a>Aggiornare le impostazioni di criteri di gruppo  
In questo passaggio, aggiornare le impostazioni di criteri di gruppo dopo aver creato il criterio di controllo.  
  
#### <a name="to-update-group-policy-settings"></a>Per aggiornare le impostazioni di criteri di gruppo  
  
1.  Accedere al file server FILE1 come contoso\Administrator, con la password **pass@word1**.  
  
2.  Premere il tasto Windows + R, quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
    > [!NOTE]  
    > Se il **controllo dell'Account utente** viene visualizzata la finestra di dialogo, verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì **.  
  
3.  Tipo **gpupdate /force** e quindi premere INVIO.  
  
## <a name="BKMK_3"></a>Verificare che sia stato applicato il criterio di accesso agli oggetti globale  
Dopo avranno applicate le impostazioni di criteri di gruppo, è possibile verificare che le impostazioni di criteri di controllo siano state applicate correttamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Per verificare che sia stato applicato il criterio di accesso agli oggetti globale  
  
1.  Accedere al computer client CLIENT1 come contoso\mreid.. Passare a HYPERLINK "file:///\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance la cartella documenti e modificare documento 2 di Word.  
  
2.  Accedere a file server FILE1 come contoso\administrator. Aprire il Visualizzatore eventi, passare a **registri di Windows**selezionare **sicurezza**e verificare che le attività abbiano generato negli eventi di controllo **4656** e **4663** (anche se non è stato SACL di controllo espliciti dei file o cartelle create, modificate ed eliminate).  
  
> [!IMPORTANT]  
> Nel computer in cui la risorsa si trova, per conto dell'utente per il quale viene verificata accesso valido, viene generato un nuovo evento di accesso. Durante l'analisi dei log di controllo di sicurezza per utente attività di accesso, per distinguere tra eventi di accesso generati dall'accesso valido e quelli generati da un utente interattivo alla rete effettuare l'accesso, informazioni sul livello rappresentazione sono incluse. Quando viene generato l'evento di accesso a causa di accesso valido, il livello di rappresentazione è identità. Un accesso utente interattivo rete genera normalmente un evento di accesso con il livello di rappresentazione = rappresentazione oppure delega.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: File di controllo dell'accesso](Scenario--File-Access-Auditing.md)  
  
-   [Pianificare per File di controllo dell'accesso](Plan-for-File-Access-Auditing.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

