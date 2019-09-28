---
title: Configurare l'individuazione client automatica della cache ospitata in base al punto di connessione del servizio
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406399"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurare l'individuazione client automatica della cache ospitata in base al punto di connessione del servizio

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con questa procedura è possibile utilizzare criteri di gruppo per abilitare e configurare la modalità cache ospitata di BranchCache nel dominio\-unita in join i computer che eseguono il seguente BranchCache\-in grado di supportare i sistemi operativi Windows.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Per configurare i computer appartenenti a un dominio che eseguono Windows Server 2008 R2 o Windows 7, vedere Windows Server 2008 R2 [Guida alla distribuzione di BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Per usare Criteri di gruppo per configurare i client per la modalità cache ospitata

1. In un computer su cui è installato il ruolo server Servizi di dominio Active Directory, aprire Server Manager, selezionare il Server locale, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**. Viene aperta la console di gestione Criteri di gruppo.

2. Nella console Gestione Criteri di gruppo espandere il percorso seguente: **Foresta:** *Corp.contoso.com*, **domains**, *corp.contoso.com*, **criteri di gruppo Objects**, dove *Corp.contoso.com* è il nome del dominio in cui gli account del computer client con BranchCache che si desiderano si trovano le configure.

3. Destra\-fare clic su **oggetti Criteri di gruppo**, quindi fare clic su **nuovo**. Verrà visualizzata la finestra di dialogo **Nuovo oggetto Criteri di gruppo**. In **nome**, digitare un nome per il nuovo oggetto Criteri di gruppo \(oggetto Criteri di gruppo\). Ad esempio, se si desidera assegnare all'oggetto il nome Computer client con BranchCache, digitare **Computer client con BranchCache**. Fare clic su **OK**.

4. Nella console Gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** è selezionata e il diritto di riquadro dei dettagli\-il GPO appena creato. Ad esempio, se il computer Client con BranchCache oggetto Criteri di gruppo denominata, destra\-fare clic su **computer Client con BranchCache**. Fare clic su **modificare**. Si apre la console Editor Gestione Criteri di gruppo.

5. Nella console di Editor Gestione Criteri di gruppo espandere il percorso seguente: **Configurazione computer**, **Criteri**, **Modelli amministrativi: Definizioni dei criteri \(ADMX @ no__t-1 recuperato dal computer locale @ no__t-2, **rete**, **BranchCache**.

6. Fare clic su **BranchCache**, quindi nel riquadro dei dettagli, fare doppio\-fare clic su **attivare BranchCache**. Verrà visualizzata la finestra di dialogo **Attiva BranchCache**.
  
7.  Nella finestra di dialogo **Attiva BranchCache** fare clic su **Abilitato** e quindi su **OK**.

8. Nella console di Editor Gestione criteri di gruppo, assicurarsi che **BranchCache** sia ancora selezionato, quindi nel riquadro dettagli doppio\-fare clic su **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio**. Verrà visualizzata la finestra di dialogo impostazione criterio.

9. Nel **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** la finestra di dialogo, fare clic su **abilitato**, e quindi fare clic su **OK**.

10. Per consentire ai computer client di scaricare e memorizzare nella cache il contenuto da BranchCache file server @ no__t-0based content servers: Nella console di Editor Gestione Criteri di gruppo verificare che **BranchCache** sia ancora selezionato, quindi nel riquadro dei dettagli doppio @ no__t-1Click **BranchCache per file di rete**. Verrà visualizzata la finestra di dialogo **Configura BranchCache per file di rete**. 
11. Nel **Configura BranchCache per file di rete** la finestra di dialogo, fare clic su **Enabled**. In **Opzioni** digitare un valore numerico, in millisecondi, per il tempo massimo di latenza di andata e ritorno della rete, quindi fare clic su **OK**.
  
    > [!NOTE]
    > Per impostazione predefinita, i computer client memorizzano il contenuto dai file server se la latenza di rete round trip è superiore a 80 millisecondi.
  
12. Per configurare la quantità di spazio su disco rigido allocata su ogni computer client per la cache BranchCache: Nella console di Editor Gestione Criteri di gruppo verificare che **BranchCache** sia ancora selezionato, quindi nel riquadro dei dettagli Double @ no__t-1Click impostare la **percentuale di spazio su disco utilizzata per la cache del computer client**. Verrà visualizzata la finestra di dialogo **Imposta percentuale di spazio su disco utilizzata per la cache del computer client**. Fare clic su **Enabled**, quindi nella **Opzioni** digitare un valore numerico che rappresenta la percentuale di spazio su disco utilizzato in ogni computer client per la cache BranchCache. Fare clic su **OK**.

13. Per specificare l'età predefinita, in giorni, per cui i segmenti sono validi nella cache dei dati di BranchCache nei computer client: Nella console di Editor Gestione Criteri di gruppo verificare che **BranchCache** sia ancora selezionato, quindi nel riquadro dei dettagli Double @ no__t-1Click **impostare Age per i segmenti nella cache dei dati**. Il **impostare una durata dei segmenti nella cache dei dati** verrà visualizzata la finestra di dialogo. Fare clic su **Enabled**, quindi digitare il numero di giorni desiderato nel riquadro dei dettagli. Fare clic su **OK**.

14. Configurare le impostazioni dei criteri di BranchCache aggiuntive per i computer client nel modo appropriato per la distribuzione.

15. Aggiornare i criteri di gruppo sul computer client della filiale eseguendo il comando **gpupdate /force**, o riavviando il computer client.

La distribuzione in modalità Cache ospitata di BranchCache è stata completata.

Per ulteriori informazioni sulle tecnologie in questa Guida, vedere [risorse aggiuntive](11-Bc-Hcm-additional-resources.md).