---
title: PASSAGGIO 5 testare la connettività DirectAccess da Internet e tramite il cluster
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1b5708e51b2653444fb3eb636baac6a165dfc55d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404845"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>PASSAGGIO 5 testare la connettività DirectAccess da Internet e tramite il cluster

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

CLIENT1 è ora pronto per il test di DirectAccess.  
  
- Testare la connettività DirectAccess da Internet. Connettere CLIENT1 alla rete Internet simulata. Quando si è connessi alla rete Internet simulata, al client vengono assegnati indirizzi IPv4 pubblici. Quando un client DirectAccess viene assegnato a un indirizzo IPv4 pubblico, tenta di stabilire una connessione al server di accesso remoto utilizzando una tecnologia di transizione IPv6.  
  
- Testare la connettività del client DirectAccess tramite il cluster. Testare le funzionalità del cluster. Prima di iniziare il test, è consigliabile arrestare sia EDGE1 che EDGE2 per almeno cinque minuti. Esistono diversi motivi per questo, tra cui i timeout della cache ARP e le modifiche correlate a NLB. Quando si convalida la configurazione NLB in un Lab di test, è necessario essere paziente perché le modifiche nella configurazione non verranno immediatamente riflesse nella connettività fino a quando non è trascorso un periodo di tempo. Questo aspetto è importante da tenere presente quando si eseguono le attività seguenti.  
  
    > [!TIP]  
    > Si consiglia di cancellare la cache di Internet Explorer prima di eseguire questa procedura e ogni volta che si testa la connessione tramite un server di accesso remoto diverso per assicurarsi di testare la connessione e di non recuperare le pagine Web dalla cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Testare la connettività DirectAccess da Internet  
  
1. Scollegare CLIENT1 dall'opzione corpnet e connetterlo al Commuter Internet. Attendere 30 secondi.  
  
2. In una finestra di Windows PowerShell con privilegi elevati digitare **ipconfig/flushdns** e premere INVIO. Questa operazione Scarica le voci di risoluzione dei nomi che potrebbero ancora esistere nella cache DNS del client da quando il computer client era connesso a Corpnet.  
  
3. Nella finestra di Windows PowerShell digitare **Get-DnsClientNrptPolicy** e premere INVIO.  
  
   L'output visualizza le impostazioni correnti per la tabella dei criteri di risoluzione dei nomi (NRPT). Queste impostazioni indicano che tutte le connessioni a. corp.contoso.com devono essere risolte dal server DNS di accesso remoto, con l'indirizzo IPv6 2001: DB8:1:: 2. Inoltre, annotare la voce NRPT che indica un'esenzione per il nome nls.corp.contoso.com; i nomi nell'elenco delle esenzioni non ricevono risposta dal server DNS di Accesso remoto. È possibile effettuare il ping dell'indirizzo IP del server DNS di accesso remoto per confermare la connettività al server di accesso remoto; ad esempio, è possibile effettuare il ping di 2001: DB8:1:: 2.  
  
4. Nella finestra di Windows PowerShell digitare **ping App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 per APP1, che in questo caso è 2001: DB8:1:: 3.  
  
5. Nella finestra di Windows PowerShell digitare **ping App2** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2 che, in questo caso, corrisponde a fdc9:9f4e:eb1b:7777::a00:4.  
  
   La possibilità di eseguire il ping di APP2 è importante, poiché l'esito positivo indica che è stato possibile stabilire una connessione tramite NAT64/DNS64, perché APP2 è una risorsa solo IPv4.  
  
6. Lasciare aperta la finestra di Windows PowerShell per la procedura successiva.  
  
7. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
8. Nella barra degli indirizzi di Internet Explorer immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
9. Nella schermata **Start** Digitare<strong>\\ \ App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato possibile connettersi a un server solo IPv4 usando SMB per ottenere una risorsa nel dominio delle risorse.  
  
10. Nella schermata **Start** Digitare**WF. msc**, quindi premere INVIO.  
  
11. Nel **Windows Firewall con la console di sicurezza avanzata** si noti che è attivo solo il profilo **privato** o **pubblico** . Per il corretto funzionamento di DirectAccess, è necessario abilitare il Windows Firewall. Se la Windows Firewall è disabilitata, la connettività DirectAccess non funziona.  
  
12. Nel riquadro sinistro della console espandere il nodo **monitoraggio** , quindi fare clic sul nodo **regole di sicurezza della connessione** . Verranno visualizzate le regole di sicurezza della connessione attive: **Criteri DirectAccess-ClientToCorp**, **Criteri DirectAccess-ClientToDNS64NAT64PrefixExemption**, **Criteri DirectAccess-ClientToInfra**e **Criteri DirectAccess-ClientToNlaExempt**. Scorrere il riquadro centrale a destra per visualizzare i **primi metodi di autenticazione** e **2 colonne metodi di autenticazione** . Si noti che la prima regola (ClientToCorp) USA Kerberos V5 per stabilire il tunnel della Intranet e la terza regola (ClientToInfra) USA NTLMv2 per stabilire il tunnel dell'infrastruttura.  
  
13. Nel riquadro sinistro della console espandere il nodo Associazioni di **sicurezza** e fare clic sul nodo **modalità principale** . Si notino le associazioni di sicurezza del tunnel dell'infrastruttura usando NTLMv2 e l'associazione di sicurezza del tunnel Intranet tramite Kerberos V5. Fare clic con il pulsante destro del mouse sulla voce che mostra l' **utente (Kerberos V5)** come **secondo metodo di autenticazione** e fare clic su **proprietà**. Nella scheda **generale** si noti che il **secondo ID locale di autenticazione** è **Corp\user1.** , che indica che User1 è stato in grado di eseguire l'autenticazione al dominio Corp tramite Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Testare la connettività del client DirectAccess tramite il cluster  
  
1. Eseguire un arresto normale in EDGE2.  
  
   È possibile utilizzare Gestione bilanciamento carico di rete per visualizzare lo stato dei server durante l'esecuzione di questi test.  
  
2. In CLIENT1, nella finestra di Windows PowerShell, digitare **ipconfig/flushdns** e premere INVIO. Questa operazione Scarica le voci di risoluzione dei nomi che potrebbero ancora esistere nella cache DNS del client.  
  
3. Nella finestra di Windows PowerShell, effettuare il ping di APP1 e APP2. Si dovrebbero ricevere risposte da entrambe le risorse.  
  
4. Nella schermata **Start** Digitare<strong>\\ \ app2\files</strong>. Dovrebbe essere visualizzata la cartella condivisa nel computer APP2. La possibilità di aprire la condivisione file in APP2 indica che il secondo tunnel, che richiede l'autenticazione Kerberos per l'utente, funziona correttamente.  
  
5. Aprire Internet Explorer, quindi aprire i siti Web https://app1/ e https://app2/. La possibilità di aprire entrambi i siti Web conferma che il primo e il secondo tunnel sono operativi. Chiudere Internet Explorer.  
  
6. Avviare il computer EDGE2.  
  
7. In EDGE1 eseguire un arresto normale.  
  
8. Attendere 5 minuti e tornare a CLIENT1. Eseguire i passaggi 2-5. Questo conferma che CLIENT1 è stato in grado di eseguire il failover in modo trasparente in EDGE2 dopo che EDGE1 è diventato non disponibile.
