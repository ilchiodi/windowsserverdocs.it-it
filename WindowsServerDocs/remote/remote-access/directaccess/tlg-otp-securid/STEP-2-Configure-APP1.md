---
title: PASSAGGIO 2 configurare APP1
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6d8720c71efba6f461aa0789fc2a143d1b1dab3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855584"
---
# <a name="step-2-configure-app1"></a>PASSAGGIO 2 configurare APP1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per preparare APP1 per il supporto OTP, attenersi alla procedura seguente:  
  
1. Per creare e distribuire un modello di certificato usato per firmare le richieste di certificati OTP. Configurare un modello di certificato usato per firmare le richieste di certificati OTP.  
  
2. Per creare e distribuire un modello di certificato per i certificati OTP rilasciati dalla CA aziendale. Configurare un modello di certificato per i certificati OTP rilasciati dalla CA aziendale.  
  
> [!WARNING]  
> La progettazione di questa guida al Lab di test include server di infrastruttura, ad esempio un controller di dominio e un'autorità di certificazione (CA) che eseguono Windows Server 2012 R2 o Windows Server 2012. L'utilizzo di questa guida al Lab di test per configurare i server di infrastruttura che eseguono altri sistemi operativi non è stato testato e le istruzioni per la configurazione di altri sistemi operativi non sono inclusi in questa guida.  
  
## <a name="to-create-and-deploy-a-certificate-template-used-to-sign-otp-certificate-requests"></a><a name="DAOTPRA"></a>Per creare e distribuire un modello di certificato usato per firmare le richieste di certificati OTP  
  
1.  Eseguire **certtmpl. msc**, quindi premere INVIO.  
  
2.  Nella console modelli di certificato, nel riquadro dei dettagli, fare clic con il pulsante destro del mouse sul modello di **computer** e scegliere **Duplica modello**.  
  
3.  Nella finestra **di dialogo Proprietà nuovo modello** , nella scheda **compatibilità** , nell'elenco autorità di **certificazione** selezionare il sistema operativo desiderato e nella finestra di dialogo **modifiche risultanti** fare clic su **OK**. Nell'elenco **destinatario certificato** selezionare il sistema operativo desiderato e nella finestra di dialogo **modifiche risultanti** fare clic su **OK**.  
  
4.  Nella finestra **di dialogo Proprietà nuovo modello** fare clic sulla scheda **generale** .  
  
5.  Nella scheda **generale** , in **nome visualizzato modello**, digitare **DAOTPRA**. Impostare **periodo di validità** su 2 giorni e impostare periodo di **rinnovo** su 1 giorno. Se viene visualizzato l'avviso **modelli di certificato** , fare clic su **OK**.  
  
6.  Fare clic sulla scheda **Sicurezza** , quindi su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Seleziona utenti, computer, account servizio o gruppi** fare clic su **tipi di oggetto**. Nella finestra di dialogo **tipi di oggetto** selezionare **computer**e quindi fare clic su **OK**. Nella casella **immettere i nomi degli oggetti da selezionare** digitare **Edge1**, fare clic su **OK**e nella colonna **Consenti** selezionare le caselle di controllo **lettura**, **registrazione**e **registrazione automatica** . Fare clic su **utenti autenticati**, selezionare la casella di controllo **lettura** nella colonna **Consenti** e deselezionare tutte le altre caselle di controllo. Fare clic su **computer del dominio**e deselezionare **registra** nella colonna **Consenti** . Fare clic su **Domain Admins** ed **Enterprise Admins** , quindi fare clic su **controllo completo** nella colonna **Consenti** per entrambi. Fare clic su **Applica**  
  
8.  Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**. Nel **formato nome soggetto:** elenco selezionare **nome DNS**, assicurarsi che la casella **nome DNS** sia selezionata e fare clic su **applica**.  
  
9. Fare clic sulla scheda **estensioni** , selezionare **criteri di applicazione** e quindi fare clic su **modifica**. Rimuovere tutti i criteri dell'applicazione esistenti. Fare clic su **Aggiungi**e nella finestra di dialogo **Aggiungi criterio di applicazione** fare clic su **nuovo**, immettere **da OTP ra** nel campo **Nome:** e **1.3.6.1.4.1.311.81.1.1** nel campo **identificatore oggetto:** e fare clic su **OK**. Nella finestra di dialogo **Aggiungi criterio di applicazione** fare clic su **OK**. In **modifica estensione criteri di applicazione**fare clic su **OK**. Nella finestra **di dialogo Proprietà nuovo modello** fare clic su **OK**.  
  
## <a name="to-create-and-deploy-a-certificate-template-for-otp-certificates-issued-by-the-corporate-ca"></a><a name="DAOTPLogon"></a>Per creare e distribuire un modello di certificato per i certificati OTP rilasciati dalla CA aziendale  
  
1.  Nella console modelli di certificato, nel riquadro dei dettagli, fare clic con il pulsante destro del mouse sul modello di **accesso smartcard** e scegliere **Duplica modello**.  
  
2.  Nella finestra **di dialogo Proprietà nuovo modello** , nella scheda **compatibilità** nell'elenco autorità di **certificazione** , fare clic sul sistema operativo che si desidera utilizzare e nella finestra di dialogo **modifiche risultanti** fare clic su **OK**. Nell'elenco **destinatario certificato** selezionare il sistema operativo che si desidera utilizzare e nella finestra di dialogo **modifiche risultanti** fare clic su **OK**.  
  
3.  Nella finestra **di dialogo Proprietà nuovo modello** fare clic sulla scheda **generale** .  
  
4.  Nella scheda **generale** , in **nome visualizzato modello**, digitare **DAOTPLogon**. In **periodo di validità**, nell'elenco a discesa fare clic su **ore**, nella finestra di dialogo **modelli di certificato** , fare clic su **OK**e verificare che il numero di ore sia impostato su 1. In **periodo di rinnovo**Digitare **0**.  
  
    > [!IMPORTANT]  
    > **CA di Windows Server 2003**. Nelle situazioni in cui l'autorità di certificazione (CA) si trova in un computer che esegue Windows Server 2003, il modello di certificato deve essere configurato in un computer diverso. Questa operazione è necessaria perché non è possibile impostare il **periodo di validità** in ore quando si eseguono versioni di Windows precedenti a windows Server 2008 e Windows Vista. Se nel computer utilizzato per configurare il modello non è installato il ruolo server Servizi certificati Active Directory o se si tratta di un computer client, potrebbe essere necessario installare lo snap-in modelli di certificato. Per ulteriori informazioni, vedere [installare lo snap-in modelli di certificato](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **CA di Windows Server 2008 R2**. Se è già stata distribuita un'autorità di certificazione (CA) che esegue Windows Server 2008 R2, è necessario configurare il **periodo di rinnovo** del modello di certificato su 1 o 2 ore e il **periodo di validità** più lungo del periodo di **rinnovo**, ma non più di 4 ore. Se si configura un periodo di **validità** del modello di certificato di più di 4 ore con un'autorità di certificazione che esegue Windows Server 2008 R2, l'installazione guidata DirectAccess non è in grado di rilevare il modello di certificato e l'installazione di DirectAccess non riesce.  
  
5.  Fare clic sulla scheda **sicurezza** , selezionare **utenti autenticati**, nella colonna **Consenti** e selezionare le caselle di controllo **lettura** e **registrazione** . Fare clic su **OK**. Fare clic su **Domain Admins** ed **Enterprise Admins**, quindi fare clic su **controllo completo** nella colonna **Consenti** per entrambi. Fare clic su **Applica**  
  
6.  Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**. Nel **formato nome soggetto:** elenco selezionare **nome distinto completo**, assicurarsi che la casella **nome entità utente (UPN)** sia selezionata e fare clic su **applica**.  
  
7.  Fare clic sulla scheda **Server** , selezionare la casella di controllo non **archiviare certificati e richieste nel database CA** , deselezionare la **casella di controllo non includere le informazioni di revoca nei certificati rilasciati** , quindi nella finestra di dialogo **Proprietà nuovo modello** fare clic su **applica**.  
  
8.  Fare clic sulla scheda **requisiti di rilascio** , selezionare la casella **di controllo numero di firme autorizzate:** , impostare il valore su 1. Nel **tipo di criteri richiesto in firma:** selezionare **criteri applicazione**e nell'elenco **criteri applicazione** selezionare **da OTP ra**. Nella finestra **di dialogo Proprietà nuovo modello** fare clic su **OK**.  
  
9. Fare clic sulla scheda **estensioni** e, in **criteri di applicazione** , fare clic su **modifica**. Eliminare **l'autenticazione client**, Keep **SmartCardLogon**e fare clic su **OK** due volte.  
  
10. Chiudere la Console dei modelli di certificato.  
  
11. Nella schermata **Start** Digitare**certsrv. msc**, quindi premere INVIO.  
  
12. Nell'albero della console autorità di certificazione espandere **Corp-App1-CA-1**, fare clic su **modelli di certificato**, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo**e fare clic su **modello di certificato da emettere**.  
  
13. Nell'elenco dei modelli di certificato fare clic su **DAOTPRA** e **DAOTPLogon**e quindi su **OK**.  
  
14. Nel riquadro dei dettagli della console dovrebbe essere visualizzato il modello di certificato **DAOTPRA** con lo **scopo designato** **da OTP ra** e il modello di certificato **DAOTPLogon** con uno **scopo designato** per l'accesso con **Smart Card**.  
  
15. Riavviare i servizi.  
  
16. Chiudere la console Autorità di certificazione.  
  
17. Aprire un prompt dei comandi con privilegi elevati. Digitare **certutil. exe-SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**e premere INVIO.  
  
18. Lasciare aperta la finestra del prompt dei comandi per il passaggio successivo.  
  


