---
title: PASSAGGIO 11 configurare la distribuzione multisito
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d90b20716c49b2ea0b1cd002a1c1933fbd6e26e5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314571"
---
# <a name="step-11-configure-the-multisite-deployment"></a>PASSAGGIO 11 configurare la distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per configurare una distribuzione multisito, apportare modifiche alla configurazione guidata accesso remoto corrente in EDGE1, abilitare la funzionalità multisito e quindi aggiungere 2-EDGE1 come secondo punto di ingresso.  
  
- Configurare accesso remoto in EDGE1  
  
- Abilitare la configurazione multisito in EDGE1  
  
- Aggiungere 2-EDGE1 come secondo punto di ingresso  
  
## <a name="configure-remote-access-on-edge1"></a><a name="configDA"></a>Configurare accesso remoto in EDGE1  
  
1.  Nella schermata **Start** digitare**RAMgmtUI. exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**.  
  
3.  Nel riquadro centrale della console, nell'area **passaggio 2 server di accesso remoto** , fare clic su **modifica**.  
  
4.  Fare clic su **Configurazione prefissi**. Nella pagina **configurazione prefisso** , in **prefissi IPv6 della rete interna**, immettere **2001: DB8:1::/64; 2001: DB8:2::/64**. In **prefisso IPv6 assegnato ai computer client DirectAccess**immettere **2001: DB8:1: 1000::/64**, fare clic su **Avanti**, quindi fare clic su **fine**.  
  
5.  Nel riquadro centrale della console, nell'area **passaggio 3 server di infrastruttura** , fare clic su **modifica**.  
  
6.  Fare clic su **elenco di ricerca suffissi DNS**. Nella pagina **elenco di ricerca suffissi DNS** assicurarsi che la casella di controllo **Configura client DirectAccess con suffisso client DNS** sia selezionata e che i suffissi di dominio **Corp.contoso.com** e **Corp2.Corp.contoso.com** siano visualizzati nell'elenco **suffissi di dominio da utilizzare** , fare clic su **Avanti**e quindi su fine.  
  
7.  Nel riquadro centrale della console, fare clic su **Fine**.  
  
8.  Nel **controllare l'accesso remoto** la finestra di dialogo, rivedere le impostazioni di configurazione e quindi fare clic su **Applica**. Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
9. Nel riquadro **attività** fare clic su **Aggiorna server di gestione**, quindi fare clic su **Chiudi** al termine dell'operazione.  
  
## <a name="enable-multisite-configuration-on-edge1"></a><a name="EnabledMultisite"></a>Abilitare la configurazione multisito in EDGE1  
  
1.  Nel riquadro **attività** della console di gestione accesso remoto fare clic su **Abilita multisito**.  
  
2.  Nella pagina **prima di iniziare** della procedura guidata Abilita distribuzione multisito fare clic su **Avanti**.  
  
3.  Nella pagina **nome distribuzione** digitare **Contoso**in **nome distribuzione multisito**, digitare **Edge1-site**, quindi fare clic su **Avanti**. **First entry point name**  
  
4.  Nella pagina **selezione punto di ingresso** fare clic su **assegna automaticamente i punti di ingresso e consentire ai client di selezionare manualmente**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina **bilanciamento del carico globale** fare clic su **No, non utilizzare il bilanciamento del carico globale**e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **supporto client** fare clic su **Consenti ai computer client che eseguono Windows 7 di accedere a questo punto di ingresso**, quindi fare clic su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Seleziona gruppi** , in **immettere i nomi degli oggetti da selezionare**, digitare **Win7_Clients_Site1**, fare clic su **OK**, quindi fare clic su **Avanti**.  
  
8.  Nella pagina **Impostazioni oggetto Criteri** di gruppo client fare clic su **Avanti**.  
  
9. Nel **riepilogo** pagina, fare clic su **Commit**.  
  
10. Nel **Abilitazione della distribuzione multisito** la finestra di dialogo, fare clic su **Chiudi** e quindi scegliere la procedura guidata Abilita distribuzione multisito, **Chiudi**.  
  
## <a name="add-2-edge1-as-a-second-entry-point"></a><a name="AddEP"></a>Aggiungere 2-EDGE1 come secondo punto di ingresso  
  
1.  Nel riquadro **attività** della console di gestione accesso remoto fare clic su **Aggiungi punto di ingresso**.  
  
2.  Nella procedura guidata Aggiungi punto di ingresso, nella pagina **Dettagli punto di ingresso** , in **server di accesso remoto**digitare **2-Edge1.Corp2.Corp.contoso.com**, in **nome punto di ingresso**, digitare **2-Edge1-sito**, quindi fare clic su **Avanti**.  
  
3.  Nella pagina **topologia di rete** fare clic su **Edge**, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **nome di rete o indirizzo IP** , in **digitare il nome pubblico o l'indirizzo IP utilizzato dai client per la connessione al server di accesso remoto**, digitare **2-Edge1.contoso.com**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina **schede di rete** verificare che la **scheda esterna** sia **Internet**, che la **scheda interna** sia **2-corpnet**, che il certificato sia **CN = 2-Edge1.contoso.com**e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **configurazione prefisso** , in **prefisso IPv6 assegnato ai computer client DirectAccess**, digitare **2001: DB8:2: 2000::/64**, quindi fare clic su **Avanti**.  
  
7.  Nella pagina **supporto client** fare clic su **Consenti ai computer client che eseguono Windows 7 di accedere a questo punto di ingresso**, quindi fare clic su **Aggiungi**.  
  
8.  Nella finestra di dialogo **Seleziona gruppi** , in **immettere i nomi degli oggetti da selezionare**, digitare **Win7_Clients_Site2**, fare clic su **OK**, quindi fare clic su **Avanti**.  
  
9. Nella pagina **Impostazioni oggetto Criteri** di gruppo client fare clic su **Avanti**.  
  
10. Nella pagina **Impostazioni oggetto Criteri** di gruppo Server fare clic su **Avanti**.  
  
11. Nella pagina **Riepilogo** fare clic su **commit**.  
  
12. Nella finestra di dialogo Aggiungi **punto di ingresso** fare clic su **Chiudi** e quindi, nella procedura guidata Aggiungi punto di ingresso, fare clic su **Chiudi**.  
  


