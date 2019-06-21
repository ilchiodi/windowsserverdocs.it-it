---
title: PASSAGGIO 11 configurare la distribuzione Multisita
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c95fb641c0d0fa3161caadfa2eb769e12b47672d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283228"
---
# <a name="step-11-configure-the-multisite-deployment"></a>PASSAGGIO 11 configurare la distribuzione Multisita

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Per configurare una distribuzione multisito, apportare modifiche alla configurazione guidata accesso remoto corrente su EDGE1, abilitare la funzionalità multisita e quindi aggiungere EDGE1 2 come un secondo punto di ingresso.  
  
- Configurare l'accesso remoto EDGE1  
  
- Abilitare la configurazione multisito in EDGE1  
  
- Aggiungere EDGE1 2 come un secondo punto di ingresso  
  
## <a name="configDA"></a>Configurare l'accesso remoto EDGE1  
  
1.  Nel **avviare** digitare**RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**.  
  
3.  Nel riquadro centrale della console, nelle **passaggio 2 Server di accesso remoto** area, fare clic su **modificare**.  
  
4.  Fare clic su **Configurazione prefissi**. Nel **configurazione prefissi** nella pagina **prefissi IPv6 della rete interna**, immettere **2001:db8:1:: / 64; 2001:DB8:2:::/::/ 64**. Nella **prefisso IPv6 assegnato ai computer client DirectAccess**, immettere **2001:db8:1:1000:: 64**, fare clic su **successiva**, quindi fare clic su **fine** .  
  
5.  Nel riquadro centrale della console, nelle **passaggio 3 server di infrastruttura** area, fare clic su **modificare**.  
  
6.  Fare clic su **elenco di ricerca suffissi DNS**. Nel **elenco ricerca suffissi DNS** pagina, assicurarsi che le **i client di configurare DirectAccess con elenco di ricerca suffissi client DNS** casella di controllo è selezionata e che il **corp.contoso.com** e **corp2.corp.contoso.com** suffissi di dominio vengono visualizzati nei **suffissi di dominio da usare** fare clic su **successivo**e quindi fare clic su Fine.  
  
7.  Nel riquadro centrale della console, fare clic su **Fine**.  
  
8.  Nel **controllare l'accesso remoto** la finestra di dialogo, rivedere le impostazioni di configurazione e quindi fare clic su **Applica**. Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
9. Nel **attività** riquadro, fare clic su **Aggiorna server di gestione**, fare clic su **Chiudi** al termine.  
  
## <a name="EnabledMultisite"></a>Abilitare la configurazione multisito in EDGE1  
  
1.  Nella Console di gestione accesso remoto, nella **attività** riquadro, fare clic su **abilitare multisito**.  
  
2.  Nella procedura guidata Abilita distribuzione multisito, sul **prima di iniziare** pagina, fare clic su **successivo**.  
  
3.  Nel **nome della distribuzione** nella pagina **nome distribuzione multisito**, tipo **Contoso**nella **nome del punto di ingresso primo**, tipo **Edge1-Site**, quindi fare clic su **successiva**.  
  
4.  Nel **selezione del punto di ingresso** pagina, fare clic su **assegnare automaticamente i punti di ingresso e consentire ai client di selezionare manualmente**e quindi fare clic su **Next**.  
  
5.  Nel **globali del bilanciamento del carico** pagina, fare clic su **No, non usare il bilanciamento del carico globale**e quindi fare clic su **Avanti**.  
  
6.  Nel **supporto Client** pagina, fare clic su **consentire ai computer che eseguono Windows 7 per accedere a questo punto di ingresso client**e fare clic su **Add**.  
  
7.  Nel **Seleziona gruppi** nella finestra di dialogo **immettere i nomi degli oggetti da selezionare**, digitare **Win7_Clients_Site1**, fare clic su **OK**e quindi fare clic su **Successivo**.  
  
8.  Nel **impostazioni oggetto Criteri di gruppo Client** pagina, fare clic su **successivo**.  
  
9. Nel **riepilogo** pagina, fare clic su **Commit**.  
  
10. Nel **Abilitazione della distribuzione multisito** la finestra di dialogo, fare clic su **Chiudi** e quindi scegliere la procedura guidata Abilita distribuzione multisito, **Chiudi**.  
  
## <a name="AddEP"></a>Aggiungere EDGE1 2 come un secondo punto di ingresso  
  
1.  Nella Console di gestione accesso remoto, nella **attività** riquadro, fare clic su **aggiungere un punto di ingresso**.  
  
2.  Aggiungi un punto di ingresso procedura guidata, nel **dettagli dei punti di ingresso** nella pagina **server di accesso remoto**, tipo **2 edge1.corp2.corp.contoso.com**, in **voce Nome punto**, digitare **2-Edge1-Site**, quindi fare clic su **successivo**.  
  
3.  Nel **topologia di rete** pagina, fare clic su **Edge**e quindi fare clic su **Next**.  
  
4.  Nel **nome di rete o indirizzo IP** nella pagina **digitare il nome pubblico o l'indirizzo IP utilizzato dai client per connettersi al server di accesso remoto**, digitare **2 edge1.contoso.com**, e quindi fare clic su **successivo**.  
  
5.  Nel **schede di rete** pagina, assicurarsi che il **adapter esterno** viene **Internet**, il **scheda interna** è **2 -Corpnet**, il certificato viene **CN = 2-edge1.contoso.com**, quindi fare clic su **successivo**.  
  
6.  Nel **configurazione prefissi** nella pagina **prefisso IPv6 assegnato ai computer client DirectAccess**, tipo **2001:DB8:2:2000::/:: 64**, quindi fare clic su **successivo** .  
  
7.  Nel **supporto Client** pagina, fare clic su **consentire ai computer che eseguono Windows 7 per accedere a questo punto di ingresso client**e fare clic su **Add**.  
  
8.  Nel **Seleziona gruppi** nella finestra di dialogo **immettere i nomi degli oggetti da selezionare**, digitare **Win7_Clients_Site2**, fare clic su **OK**e quindi fare clic su **Successivo**.  
  
9. Nel **impostazioni oggetto Criteri di gruppo Client** pagina, fare clic su **successivo**.  
  
10. Nel **impostazioni oggetto Criteri di gruppo del Server** pagina, fare clic su **successivo**.  
  
11. Nel **Summary** pagina fare clic su **Commit**.  
  
12. Nel **aggiunta punto di ingresso** finestra di dialogo, fare clic su **Chiudi** e quindi su Aggiungi una creazione guidata punto di ingresso, fare clic su **Chiudi**.  
  


