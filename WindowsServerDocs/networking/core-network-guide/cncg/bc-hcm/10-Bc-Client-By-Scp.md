---
title: Configurare l'individuazione automatica della Cache ospitata Client dal punto di connessione servizio
description: Questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata in computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b12fa6f9e11c8816d74c9013dd80b3fa38d0a478
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurare l'individuazione automatica della Cache ospitata Client dal punto di connessione servizio

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con questa procedura è possibile utilizzare criteri di gruppo per abilitare e configurare la modalità cache ospitata di BranchCache nei computer appartenenti a un domain \ che eseguono sistemi operativi Windows in grado di supportare BranchCache\ seguenti.

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 Enterprise

> [!NOTE]  
> Per configurare i computer appartenenti a un dominio che eseguono Windows Server 2008 R2 o Windows 7, vedere Windows Server 2008 R2 [Guida alla distribuzione di BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Per utilizzare criteri di gruppo per configurare i client per la modalità cache ospitata

1. In un computer su cui è installato il ruolo server Servizi di dominio Active Directory, aprire Server Manager, selezionare il Server locale, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**. Apre la console Gestione criteri di gruppo.

2. Nella console Gestione criteri di gruppo, espandere il percorso seguente: **foresta:** *corp.contoso.com*, **domini**, *corp.contoso.com*, **oggetti Criteri di gruppo**, dove *corp.contoso.com* è il nome del dominio in cui si trovano gli account dei computer client BranchCache che si desidera configurare.

3. Fare clic con **oggetti Criteri di gruppo**, quindi fare clic su **New**. Il **nuovo oggetto Criteri di gruppo** apre la finestra di dialogo. In **nome**, digitare un nome per il nuovo criterio di gruppo dell'oggetto \(GPO\). Ad esempio, se si desidera assegnare all'oggetto computer Client con BranchCache, digitare **computer Client con BranchCache**. Fare clic su **OK**.

4. Nella console di gestione criteri di gruppo, assicurarsi che **oggetti Criteri di gruppo** è selezionata e i dettagli riquadro con-fare clic su oggetto Criteri di gruppo appena creato. Ad esempio, se è denominato computer Client BranchCache oggetto Criteri di gruppo, fare clic con **computer Client con BranchCache**. Fare clic su **modifica**. Apre la console di Editor Gestione criteri di gruppo.

5. Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **configurazione Computer**, **criteri**, **modelli amministrativi: \(ADMX files\) le definizioni dei criteri recuperate dal computer locale**, **rete**, **BranchCache**.

6. Fare clic su **BranchCache**e quindi nel riquadro dei dettagli fare doppio clic **attiva BranchCache**. Il **attiva BranchCache** apre la finestra di dialogo.
  
7.  Nel **attiva BranchCache** la finestra di dialogo, fare clic su **abilitato**, quindi fare clic su **OK**.

8. Nella console di Editor Gestione criteri di gruppo, assicurarsi che **BranchCache** ancora selezionato, quindi nel riquadro dettagli doppio-clic **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio**. Apre la finestra di dialogo di impostazione dei criteri.

9. Nel **abilitare ospitato Cache rilevamento automatico dal punto di connessione servizio** la finestra di dialogo, fare clic su **abilitato**e quindi fare clic su **OK**.

10. Per abilitare i computer client di scaricare e memorizzare nella cache il contenuto di BranchCache file server di contenuti basati su server \: nella console di Editor Gestione criteri di gruppo, assicurarsi che **BranchCache** ancora selezionato, quindi nel riquadro dettagli doppio-clic **BranchCache per file di rete**. Il **Configura BranchCache per file di rete** apre la finestra di dialogo. 
11. Nel **Configura BranchCache per file di rete** la finestra di dialogo, fare clic su **abilitato**. In **opzioni**, digitare un valore numerico, in millisecondi, per il tempo di latenza di rete di andata e ritorno massima e quindi fare clic su **OK**.
  
    > [!NOTE]
    > Per impostazione predefinita, i computer client memorizzano nella cache contenuto da file server se la latenza di rete di andata e ritorno contiene più di 80 millisecondi.
  
12. Per configurare la quantità di spazio su disco allocato in ogni computer client per la cache BranchCache: nella console di Editor Gestione criteri di gruppo, assicurarsi che **BranchCache** ancora selezionato, quindi nel riquadro dettagli doppio-clic **imposta percentuale di spazio su disco utilizzato per la cache del computer client**. Il **imposta percentuale di spazio su disco utilizzato per la cache del computer client** apre la finestra di dialogo. Fare clic su **abilitato**e quindi **opzioni** digitare un valore numerico che rappresenta la percentuale di spazio su disco utilizzato in ogni computer client per la cache BranchCache. Fare clic su **OK**.

13. Per specificare il periodo predefinito, in giorni, per cui sono validi i segmenti nella cache dei dati di BranchCache nei computer client: nella console di Editor Gestione criteri di gruppo, assicurarsi che **BranchCache** ancora selezionato, quindi nel riquadro dettagli doppio-clic **impostare una durata dei segmenti nella cache dei dati**. Il **impostare una durata dei segmenti nella cache dei dati** apre la finestra di dialogo. Fare clic su **abilitato**e quindi nel riquadro dei dettagli digitare il numero di giorni che preferisci. Fare clic su **OK**.

14. Configurare ulteriori impostazioni di criteri di BranchCache per i computer client come appropriato per la distribuzione.

15. Aggiornare i criteri di gruppo nel computer client della filiale eseguendo il comando **gpupdate /force**, o riavviando il computer client.

La distribuzione in modalità Cache ospitata di BranchCache è stata completata.

Per ulteriori informazioni sulle tecnologie in questa Guida, vedere [risorse aggiuntive](11-Bc-Hcm-additional-resources.md).