---
title: PASSAGGIO 2 configurare APP1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess con autenticazione OTP e SecurID RSA per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 612d3f4e9be71f60811b4499d9bdb8ae3bd217e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847492"
---
# <a name="step-2-configure-app1"></a>PASSAGGIO 2 configurare APP1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Usare la procedura seguente per preparare APP1 per il supporto OTP:  
  
1. Per creare e distribuire un modello di certificato usato per firmare le richieste di certificato OTP. Configurare un modello di certificato usato per firmare le richieste di certificato OTP.  
  
2. Per creare e distribuire un modello di certificato per OTP certificati emessi dall'autorità di certificazione aziendale. Configurare un modello di certificato per OTP certificati emessi dall'autorità di certificazione aziendale.  
  
> [!WARNING]  
> La progettazione di questa Guida al lab di test include i server di infrastruttura, ad esempio un controller di dominio e un'autorità di certificazione (CA) che eseguono Windows Server 2012 R2 o Windows Server 2012. Uso di questa Guida al lab di test per configurare i server di infrastruttura che eseguono altri sistemi operativi non è stato testato e istruzioni per la configurazione di altri sistemi operativi non sono inclusi in questa Guida.  
  
## <a name="DAOTPRA"></a>Per creare e distribuire un modello di certificato usato per firmare le richieste di certificato OTP  
  
1.  Eseguire **certtmpl. msc**, quindi premere INVIO.  
  
2.  Nella Console di modelli di certificato, nel riquadro dei dettagli, fare doppio clic il **Computer** e scegliere **Duplica modello**.  
  
3.  Nel **proprietà nuovo modello** finestra di dialogo il **compatibilità** nella scheda il **autorità di certificazione** elencare, selezionare il sistema operativo desiderato e il  **Le modifiche risultanti** fare clic su finestra di dialogo **OK**. Nel **destinatario certificato** elenco selezionare il sistema operativo desiderato, e il **modifiche risultanti** fare clic su finestra di dialogo **OK**.  
  
4.  Nel **proprietà nuovo modello** della finestra di dialogo fare clic sui **generali** scheda.  
  
5.  Nel **generali** nella scheda **nome visualizzato modello**, digitare **DAOTPRA**. Impostare il **periodo di validità** su 2 giorni e impostare le **periodo di rinnovo** pari a 1 giorno. Se il **modelli di certificato** avviso è visualizzato, fare clic su **OK**.  
  
6.  Fare clic sulla scheda **Sicurezza** , quindi su **Aggiungi**.  
  
7.  Nel **Seleziona utenti, computer, account del servizio o gruppi** finestra di dialogo, fare clic su **tipi di oggetto**. Nel **tipi di oggetti** finestra di dialogo **computer**e quindi fare clic su **OK**. Nel **immettere i nomi degli oggetti da selezionare** , digitare **EDGE1**, fare clic su **OK**e nel **Consenti** colonna, selezionare il **lettura** , **Enroll**, e **Autoenroll** caselle di controllo. Fare clic su **Authenticated Users**, selezionare la **lettura** casella di controllo sotto le **Consenti** colonna e deselezionare tutte le altre caselle di controllo. Fare clic su **i computer del dominio**e deselezionare **Enroll** sotto il **Consenti** colonna. Fare clic su **Domain Admins** e **Enterprise Admins** e fare clic su **controllo completo** sotto il **Consenti** colonna per entrambi. Fare clic su **Applica**.  
  
8.  Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**. Nel **formato nome soggetto:** elenco select **il nome DNS**, assicurarsi che il **nome DNS** casella è selezionata e fare clic su **applica**.  
  
9. Fare clic sui **estensioni** scheda, seleziona **criteri di applicazione** e quindi fare clic su **modifica**. Rimuovere tutti i criteri di applicazione esistente. Fare clic su **Add**e nella **Aggiunta criterio di applicazione** della finestra di dialogo fare clic su **New**, immettere **RA OTP DA** nel **nome:** campo e **1.3.6.1.4.1.311.81.1.1** nel **identificatore di oggetto:** campo e fare clic su **OK**. Nel **Aggiunta criterio di applicazione** finestra di dialogo, fare clic su **OK**. Nel **Modifica estensione criteri di applicazione**, fare clic su **OK**. Nel **proprietà nuovo modello** finestra di dialogo, fare clic su **OK**.  
  
## <a name="DAOTPLogon"></a>Per creare e distribuire un modello di certificato per OTP certificati emessi dall'autorità di certificazione aziendale  
  
1.  Nella Console di modelli di certificato, nel riquadro dei dettagli, fare doppio clic il **accesso con smart card** e scegliere **Duplica modello**.  
  
2.  Nel **proprietà nuovo modello** finestra di dialogo il **compatibilità** scheda il **autorità di certificazione** elenco, fare clic su sistema operativo da usare e di **Modifiche risultanti** finestra di dialogo, fare clic su **OK**. Nel **destinatario certificato** elenco selezionare il sistema operativo da usare, e il **modifiche risultanti** fare clic su finestra di dialogo **OK**.  
  
3.  Nel **proprietà nuovo modello** della finestra di dialogo fare clic sui **generali** scheda.  
  
4.  Nel **generali** nella scheda **nome visualizzato modello**, digitare **DAOTPLogon**. Nella **periodo di validità**, nell'elenco a discesa elenco, fare clic su **ore**via il **i modelli di certificato** della finestra di dialogo fare clic su **OK**e assicurarsi che che il numero di ore è impostato su 1. Nelle **periodo di rinnovo**, digitare **0**.  
  
    > [!IMPORTANT]  
    > **Windows Server 2003 CA**. In situazioni in cui l'autorità di certificazione (CA) è in un computer che esegue Windows Server 2003, quindi il modello di certificato deve essere configurato in un computer diverso. Questa operazione è necessaria poiché l'impostazione di **periodo di validità** nelle ore non è possibile durante l'esecuzione di versioni di Windows precedenti a Windows Server 2008 e Windows Vista. Se il computer in uso per configurare il modello non ha il ruolo server Servizi certificati Active Directory o se si tratta di un computer client, è necessario installare lo snap-in modelli di certificato. Per altre informazioni, vedere [installare lo Snap-In modelli di certificato](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **Autorità di certificazione di Windows Server 2008 R2**. Se è già stata distribuita un'autorità di certificazione (CA) che esegue Windows Server 2008 R2, è necessario configurare il modello di certificato **Renewal Period** su 1 o 2 ore e il **periodo di validità** a superare le **Renewal Period**, ma non più di 4 ore. Se si configura un modello di certificato **periodo di validità** di maggiore di 4 ore con un'autorità di certificazione che esegue Windows Server 2008 R2, l'installazione guidata di DirectAccess non è possibile rilevare il modello di certificato e DirectAccess installazione non riesce.  
  
5.  Fare clic sui **sicurezza** scheda, seleziona **Authenticated Users**, nel **Consenti** colonna e selezionare il **lettura** e **registrazione**  caselle di controllo. Fare clic su **OK**. Fare clic su **Domain Admins** e **Enterprise Admins**, fare clic su **controllo completo** sotto il **Consenti** colonna per entrambi. Fare clic su **Applica**.  
  
6.  Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**. Nel **formato nome soggetto:** elenco select **nome distinto completo**, verificare che il **nome entità utente (UPN)** casella è selezionata e fare clic su **applica** .  
  
7.  Fare clic sui **Server** scheda, seleziona la **non archiviare certificati e richieste nel database CA** casella di controllo, deseleziona il **non includono le informazioni di revoca dei certificati emessi** casella di controllo e quindi scegliere il **proprietà nuovo modello** della finestra di dialogo fare clic su **applica**.  
  
8.  Fare clic sui **requisiti di rilascio** scheda, seleziona la **questo numero di firme digitali autorizzate:** casella di controllo, impostare il valore su 1. Nel **tipo di criterio richiesto nella firma:** elenco select **criterio di applicazione**e nel **criteri dell'applicazione** elenco select **RA OTP DA**. Nel **proprietà nuovo modello** finestra di dialogo, fare clic su **OK**.  
  
9. Fare clic sui **estensioni** scheda e nella **criteri di applicazione** fare clic su **modifica**. Eliminare **autenticazione Client**, mantenere **SmartCardLogon**, fare clic su **OK** due volte.  
  
10. Chiudere la Console dei modelli di certificato.  
  
11. Nel **avviare** digitare**certsrv. msc**, quindi premere INVIO.  
  
12. Nell'albero della console Autorità di certificazione, espandere **corp-APP1-CA-1**, fare clic su **modelli di certificato**, fare doppio clic su **i modelli di certificato**, scegliere **New**, fare clic su **modello di certificato da emettere**.  
  
13. Nell'elenco dei modelli di certificato, fare clic su **DAOTPRA** e **DAOTPLogon**, fare clic su **OK**.  
  
14. Nel riquadro dei dettagli della console dovrebbe essere il **DAOTPRA** modello di certificato con un **scopo designato** dei **RA OTP DA** e il **DAOTPLogon** modello di certificato con un **scopo designato** dei **accesso con Smart Card**.  
  
15. Riavviare i servizi.  
  
16. Chiudere la console Autorità di certificazione.  
  
17. Apri una finestra del prompt dei comandi con privilegi elevati. Tipo di **CertUtil.exe - SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**, quindi premere INVIO.  
  
18. Lasciare il prompt dei comandi aperta per il passaggio successivo.  
  


