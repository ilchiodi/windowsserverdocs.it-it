---
title: Configurare WEB1 per distribuire gli elenchi di revoche di certificati (CRL)
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839192"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurare WEB1 per distribuire gli elenchi di revoche di certificati (CRL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare il server web WEB1 per la distribuzione di CRL.  
  
Nelle estensioni della CA radice, è stato dichiarato che il CRL della CA radice deve essere disponibile tramite https://pki.corp.contoso.com/pki. Attualmente, non c'è una directory virtuale PKI in WEB1, in modo che ne deve essere creato.  
  
Per eseguire questa procedura, è necessario essere un membro di **Domain Admins**.  
  
> [!NOTE]  
> Nella procedura seguente, sostituire il nome dell'account utente, il nome del server Web, i nomi delle cartelle e percorsi e gli altri valori con quelli appropriati per la distribuzione.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Per configurare WEB1 per distribuire i certificati e CRL  
  
1.  In WEB1 eseguire Windows PowerShell come amministratore, digitare `explorer c:\`, quindi premere INVIO. Si apre Esplora Windows sull'unità C.   
  
2.  Creare una nuova cartella denominata PKI sull'unità c. A tale scopo, fare clic su **casa**, quindi fare clic su **nuova cartella**. Viene creata una nuova cartella con il nome temporaneo evidenziato. Tipo di **pki** e quindi premere INVIO.  
  
3.  In Windows Explorer, fare doppio clic la cartella appena creata, posizionare il cursore del mouse sullo **Condividi con**, quindi fare clic su **persone specifiche**. Verrà visualizzata la finestra di dialogo **Condivisione file**.  
  
4.  Nella **condivisione File**, digitare **Cert Publishers**e quindi fare clic su **Aggiungi**. Il gruppo Cert Publishers viene aggiunto all'elenco. Nell'elenco, nella **livello di autorizzazione**, fare clic sulla freccia accanto a **Cert Publishers**, quindi fare clic su **lettura/scrittura**. Fare clic su **Share**, quindi fare clic su **eseguita**.  
  
5.  Chiudere Esplora risorse.  
  
6.  Aprire la console IIS. In Server Manager fare clic su **Strumenti** e quindi su **Gestione Internet Information Services (IIS)**.  
  
7.  Nell'albero della console Gestione Internet Information Services (IIS), espandere **WEB1**. Se viene visualizzato un messaggio per iniziare con la piattaforma Web Microsoft, fare clic su **Annulla**.  
  
8.  Espandere **Siti**, fare clic con il pulsante destro del mouse su **Sito Web predefinito**, quindi scegliere **Aggiungi directory virtuale**.  
  
9. Nelle **Alias**, digitare **pki**. Nelle **percorso fisico** tipo **C:\pki**, quindi fare clic su **OK**.  
  
10. Abilitare Anonymous accedere alla directory virtuale pki, in modo che qualsiasi client può controllare la validità dei certificati della CA e CRL. A tale scopo:  
  
    1.  Nel riquadro **Connessioni** verificare che **pki** sia selezionato.  
  
    2.  In **Home page di pki** fare clic su **Autenticazione**.  
  
    3.  Nel riquadro **Azioni** fare clic su **Modifica autorizzazioni**.  
  
    4.  Nella scheda **Sicurezza** fare clic su **Modifica**.  
  
    5.  Nella finestra di dialogo **Autorizzazioni per pki** fare clic su **Aggiungi**.  
  
    6.  Nel **Seleziona utenti, computer, account del servizio o gruppi**, tipo **accesso anonimo; Tutti gli utenti** e quindi fare clic su **Controlla nomi**. Fare clic su **OK**.  
  
    7.  Fare clic su **OK** nel **Seleziona utenti, computer, account del servizio o gruppi** nella finestra di dialogo.  
  
    8.  Fare clic su **OK** nel **autorizzazioni per pki** nella finestra di dialogo.  
  
11. Fare clic su **OK** nel **pki proprietà** nella finestra di dialogo.  
  
12. Nel riquadro **Home page di pki** fare doppio clic su **Filtro richieste**.  
  
13. La scheda **Estensioni nomi file** è selezionata per impostazione predefinita nel riquadro **Filtro richieste**. Nel riquadro **Azioni** fare clic su **Modifica impostazioni funzionalità**.  
  
14. In **Modifica impostazioni di filtro richieste**selezionare **Consenti doppio escape** , quindi fare clic su **OK**.  
  
15. In MMC Gestione Internet Information Services (IIS), fare clic sul nome del server Web. Ad esempio, se il server Web è denominato WEB1, fare clic su **WEB1**.  
  
16. Nelle **azioni**, fare clic su **riavviare**. Servizi Internet vengono arrestati e successivamente riavviati.  
  

