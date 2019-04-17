---
title: Configurare WEB1 per la distribuzione di elenchi di revoche di certificati (CRL)
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurare WEB1 per la distribuzione di elenchi di revoche di certificati (CRL)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per configurare il server web WEB1 per la distribuzione di CRL.  
  
Nelle estensioni della CA radice, è stato indicato che il CRL dalla CA radice deve essere disponibile tramite http://pki.corp.contoso.com/pki. Attualmente, non c'è una directory virtuale PKI in WEB1, in modo crearne uno.  
  
Per eseguire questa procedura, è necessario essere un membro del **Domain Admins**.  
  
> [!NOTE]  
> Nella procedura seguente, sostituire il nome dell'account utente, il nome del server Web, i nomi delle cartelle e posizioni e altri valori con quelli che sono appropriati per la distribuzione.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Per configurare WEB1 per distribuire i certificati e CRL  
  
1.  In WEB1 eseguire Windows PowerShell come amministratore, digitare `explorer c:\`, quindi premere INVIO. Apre Esplora sull'unità C.   
  
2.  Creare una nuova cartella denominata PKI sull'unità c. A tale scopo, fare clic su **Home**, quindi fare clic su **nuova cartella**. Viene creata una nuova cartella il nome temporaneo evidenziato. Tipo **pki** e quindi premere INVIO.  
  
3.  In Esplora risorse, fare clic sulla cartella appena creato, posizionare il cursore del mouse su **condividere con**, quindi fare clic su **utenti specifici**. Il **la condivisione di File** apre la finestra di dialogo.  
  
4.  In **la condivisione di File**, tipo **Cert Publishers**, quindi fare clic su **Aggiungi**. Il gruppo Cert Publishers viene aggiunto all'elenco. Nell'elenco, in **livello di autorizzazione**, fare clic sulla freccia accanto a **Cert Publishers**, quindi fare clic su **lettura/scrittura**. Fare clic su **condivisione**, quindi fare clic su **eseguita**.  
  
5.  Chiudere Esplora risorse.  
  
6.  Aprire la console IIS. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione Internet Information Services (IIS)**.  
  
7.  Nell'albero della console Gestione Internet Information Services (IIS) espandere **WEB1**. Se viene visualizzato un messaggio per iniziare con la piattaforma Web Microsoft, fare clic su **Annulla**.  
  
8.  Espandere **siti** e quindi fare doppio clic su di **sito Web predefinito** e quindi fare clic su **Aggiungi Directory virtuale**.  
  
9. In **Alias**, tipo **pki**. In **percorso fisico** tipo **C:\pki**, quindi fare clic su **OK**.  
  
10. Abilitare anonimo accedere alla directory virtuale pki, in modo che qualsiasi client può controllare la validità del CRL e certificati CA. A tale scopo:  
  
    1.  Nel **connessioni** riquadro, assicurarsi che **pki** sia selezionata.  
  
    2.  In **home page di pki** fare clic su **autenticazione**.  
  
    3.  Nel **azioni** riquadro, fare clic su **Modifica autorizzazioni**.  
  
    4.  Nel **sicurezza** scheda, fare clic su **modifica**  
  
    5.  Nel **autorizzazioni per pki** la finestra di dialogo, fare clic su **Aggiungi**.  
  
    6.  Nel **Seleziona utenti, computer, account servizio o gruppi**, tipo **accesso anonimo; Tutti gli utenti** e quindi fare clic su **Controlla nomi**. Fare clic su **OK**.  
  
    7.  Fare clic su **OK** nel **Seleziona utenti, computer, account servizio o gruppi** la finestra di dialogo.  
  
    8.  Fare clic su **OK** nel **autorizzazioni per pki** la finestra di dialogo.  
  
11. Fare clic su **OK** nel **pki proprietà** la finestra di dialogo.  
  
12. Nel **home page di pki** riquadro, fare doppio clic su **filtro richieste**.  
  
13. Il **estensioni di File** è selezionata per impostazione predefinita nel **filtro richieste** riquadro. Nel **azioni** riquadro, fare clic su **Modifica impostazioni funzionalità**.  
  
14. In **Modifica impostazioni di filtro richieste**selezionare **consentire gli escape doppi** e quindi fare clic su **OK**.  
  
15. In MMC Gestione Internet Information Services (IIS), fare clic sul nome del server Web. Ad esempio, se il server Web è denominato WEB1, fare clic su **WEB1**.  
  
16. In **azioni**, fare clic su **riavviare**. I servizi Internet sono arrestati e riavviati.  
  

