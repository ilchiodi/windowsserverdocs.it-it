---
title: Configurare WEB1 per la distribuzione degli elenchi di revoche di certificati (CRL)
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5d53cbba37699346db110f0748a9c3e0c834c18e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356291"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurare WEB1 per la distribuzione degli elenchi di revoche di certificati (CRL)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare il server Web WEB1 per la distribuzione di CRL.  
  
Nelle estensioni della CA radice è stato indicato che il CRL dalla CA radice sarà disponibile tramite https://pki.corp.contoso.com/pki. Attualmente, non esiste una directory virtuale PKI in WEB1, quindi è necessario crearne una.  
  
Per eseguire questa procedura, è necessario essere un membro di **Domain Admins**.  
  
> [!NOTE]  
> Nella procedura seguente sostituire il nome dell'account utente, il nome del server Web, i nomi delle cartelle e i percorsi e altri valori con quelli appropriati per la distribuzione.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Per configurare WEB1 per la distribuzione di certificati e CRL  
  
1.  In WEB1, eseguire Windows PowerShell come amministratore, digitare `explorer c:\` e quindi premere INVIO. Esplora risorse verrà aperto nell'unità C.   
  
2.  Creare una nuova cartella denominata PKI nell'unità C:. A tale scopo, fare clic su **Home**, quindi fare clic su **nuova cartella**. Viene creata una nuova cartella con il nome temporaneo evidenziato. Digitare **PKI** , quindi premere INVIO.  
  
3.  In Esplora risorse fare clic con il pulsante destro del mouse sulla cartella appena creata, posizionare il puntatore del mouse su **Condividi con**e quindi fare clic su **utenti specifici**. Verrà visualizzata la finestra di dialogo **Condivisione file**.  
  
4.  In **condivisione file**digitare **Cert Publishers**, quindi fare clic su **Aggiungi**. Il gruppo Cert Publishers viene aggiunto all'elenco. Nell'elenco, in **livello di autorizzazione**, fare clic sulla freccia accanto a **Cert Publishers**, quindi fare clic su **lettura/scrittura**. Fare clic su **Condividi**, quindi fare clic su **fine**.  
  
5.  Chiudere Esplora risorse.  
  
6.  Aprire la console IIS. In Server Manager fare clic su **Strumenti** e quindi su **Gestione Internet Information Services (IIS)** .  
  
7.  Nell'albero della console di gestione Internet Information Services (IIS) espandere **Web1**. Se viene visualizzato un messaggio per iniziare con la piattaforma Web Microsoft, fare clic su **Annulla**.  
  
8.  Espandere **Siti**, fare clic con il pulsante destro del mouse su **Sito Web predefinito**, quindi scegliere **Aggiungi directory virtuale**.  
  
9. In **alias**Digitare **PKI**. In **percorso fisico** digitare **C:\pki**, quindi fare clic su **OK**.  
  
10. Abilitare l'accesso anonimo alla directory virtuale dell'infrastruttura a chiave pubblica (PKI), in modo che qualsiasi client possa controllare la validità dei certificati della CA e dei CRL. A tale scopo:  
  
    1.  Nel riquadro **Connessioni** verificare che **pki** sia selezionato.  
  
    2.  In **Home page di pki** fare clic su **Autenticazione**.  
  
    3.  Nel riquadro **Azioni** fare clic su **Modifica autorizzazioni**.  
  
    4.  Nella scheda **Sicurezza** fare clic su **Modifica**.  
  
    5.  Nella finestra di dialogo **Autorizzazioni per pki** fare clic su **Aggiungi**.  
  
    6.  In **Seleziona utenti, computer, account servizio o gruppi**digitare **accesso anonimo; Tutti** , quindi fare clic su **Controlla nomi**. Fare clic su **OK**.  
  
    7.  Fare clic su **OK** nella finestra di dialogo **Seleziona utenti, computer, account di servizio o gruppi** .  
  
    8.  Fare clic su **OK** nella finestra di dialogo **autorizzazioni per PKI** .  
  
11. Fare clic su **OK** nella finestra di dialogo **Proprietà PKI** .  
  
12. Nel riquadro **Home page di pki** fare doppio clic su **Filtro richieste**.  
  
13. La scheda **Estensioni nomi file** è selezionata per impostazione predefinita nel riquadro **Filtro richieste**. Nel riquadro **Azioni** fare clic su **Modifica impostazioni funzionalità**.  
  
14. In **Modifica impostazioni di filtro richieste**selezionare **Consenti doppio escape** , quindi fare clic su **OK**.  
  
15. In MMC Gestione Internet Information Services (IIS) fare clic sul nome del server Web. Ad esempio, se il server Web è denominato WEB1, fare clic su **Web1**.  
  
16. In **azioni**fare clic su **Riavvia**. I servizi Internet vengono arrestati e quindi riavviati.  
  

