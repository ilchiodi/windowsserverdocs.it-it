---
title: PASSAGGIO 5 testare la connettività DirectAccess da Internet e tramite il Cluster
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23aed915edb827fd0cd61e6778167108647269ea
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283347"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>PASSAGGIO 5 testare la connettività DirectAccess da Internet e tramite il Cluster

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

CLIENT1 è ora pronto per il testing di DirectAccess.  
  
- Testare la connettività DirectAccess da Internet. Connettere CLIENT1 alla rete Internet simulata. Quando si è connessi alla rete Internet simulata, il client viene assegnato gli indirizzi IPv4 pubblici. Quando un client DirectAccess è assegnato un indirizzo IPv4 pubblico, tenta di stabilire una connessione al server Accesso remoto usando una tecnologia di transizione IPv6.  
  
- Testare la connettività dei client DirectAccess tramite il cluster. Verificare la funzionalità di cluster. Prima di iniziare i test, è consigliabile arrestarla sia EDGE1 ed EDGE2 per almeno cinque minuti. Esistono diversi motivi, tra cui timeout cache ARP e modifiche relative a bilanciamento carico di rete. Quando si convalida la configurazione di bilanciamento carico di rete in un lab di test, è necessario essere paziente, come nella configurazione verrà non essere immediatamente riflesse nel connettività fino a dopo che è trascorso un periodo di tempo. Questo è importante da tenere presenti quando effettuano le seguenti attività.  
  
    > [!TIP]  
    > È consigliabile cancellare la cache di Internet Explorer prima di eseguire questa procedura e ogni volta che si testare la connessione tramite un altro server di accesso remoto per assicurarsi che il test della connessione e non recupera le pagine Web dalla cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Testare la connettività DirectAccess da Internet  
  
1. Scollegare CLIENT1 dal commutatore della rete aziendale e connetterla al commutatore Internet. Attendere 30 secondi.  
  
2. In una finestra di Windows PowerShell con privilegi elevata, digitare **ipconfig /flushdns** e premere INVIO. Ciò elimina voci di risoluzione dei nomi ancora esistenti nella cache DNS del client da quando il computer client era connesso alla rete aziendale.  
  
3. Nella finestra di Windows PowerShell, digitare **Get-DnsClientNrptPolicy** e premere INVIO.  
  
   L'output visualizza le impostazioni correnti per la tabella dei criteri di risoluzione dei nomi (NRPT). Queste impostazioni indicano che tutte le connessioni a. corp.contoso.com devono essere risolte dal server DNS di accesso remoto, con 2001:db8:1::2 l'indirizzo IPv6. Inoltre, annotare la voce NRPT che indica un'esenzione per il nome nls.corp.contoso.com; i nomi nell'elenco delle esenzioni non ricevono risposta dal server DNS di Accesso remoto. È possibile effettuare il ping di indirizzo IP del server DNS di accesso remoto per confermare la connettività al server di accesso remoto. ad esempio, è possibile effettuare il ping di 2001:db8:1::2.  
  
4. Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Dovrebbe essere risposte dall'indirizzo IPv6 di APP1, che in questo caso è 2001:db8:1::3.  
  
5. Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2 che, in questo caso, corrisponde a fdc9:9f4e:eb1b:7777::a00:4.  
  
   La possibilità di effettuare il ping APP2 è importante, perché la riuscita indica che è stato possibile stabilire una connessione con NAT64 o DNS64, APP2 è una risorsa sola IPv4.  
  
6. Lasciare la finestra di Windows PowerShell aperta per la procedura successiva.  
  
7. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
8. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
9. Nel **avviare** digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato in grado di connettersi a un server solo IPv4 con SMB per ottenere una risorsa nel dominio delle risorse.  
  
10. Nel **avviare** digitare**WF. msc**, quindi premere INVIO.  
  
11. Nel **Windows Firewall con sicurezza avanzata** console, si noti che solo le **privato** o **profilo pubblico** è attiva. Il Firewall di Windows deve essere abilitato per DirectAccess a funzionare correttamente. Se il Firewall di Windows è disabilitato, la connettività DirectAccess non funziona.  
  
12. Nel riquadro sinistro della console, espandere la **Monitoring** e scegliere il **regole di sicurezza delle connessioni** nodo. Si dovrebbero vedere le regole di sicurezza della connessione attiva: **DirectAccess Policy-ClientToCorp**, **dei criteri DirectAccess-ClientToDNS64NAT64PrefixExemption**, **criteri DirectAccess-ClientToInfra**, e **DirectAccess Policy-ClientToNlaExempt**. Scorrere il riquadro al centro a destra per visualizzare il **metodi di autenticazione dal 1 °** e **2 metodi di autenticazione** colonne. Si noti che la prima regola (ClientToCorp) usa Kerberos V5 per stabilire il tunnel della intranet e la terza regola (ClientToInfra) Usa NTLMv2 per stabilire il tunnel dell'infrastruttura.  
  
13. Nel riquadro sinistro della console, espandere la **associazioni di sicurezza** e scegliere il **modalità principale** nodo. Si noti che le associazioni di sicurezza del tunnel dell'infrastruttura usando NTLMv2 e l'associazione di sicurezza del tunnel di intranet utilizzando Kerberos V5. Fare clic sulla voce che illustra **utente (Kerberos V5)** come la **2nd metodo di autenticazione** e fare clic su **proprietà**. Nel **generali** scheda, si noti il **seconda autenticazione ID locale** viene **CORP\User1**, che indica che User1 è stata in grado di eseguire l'autenticazione al dominio CORP utilizzando Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Testare la connettività dei client DirectAccess tramite il cluster  
  
1. Eseguire una chiusura normale su EDGE2.  
  
   È possibile utilizzare Gestione bilanciamento carico di rete per visualizzare lo stato dei server durante l'esecuzione dei test.  
  
2. In CLIENT1 nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO. Ciò elimina voci di risoluzione dei nomi ancora esistenti nella cache DNS al client.  
  
3. Nella finestra di Windows PowerShell, eseguire il ping APP1 e APP2. Si dovrebbero ricevere risposte da entrambe queste risorse.  
  
4. Nel **avviare** digitare<strong>\\\app2\files</strong>. Si dovrebbe essere la cartella condivisa nel computer APP2. La possibilità di aprire la condivisione di file su APP2 indica che il secondo tunnel, che richiede l'autenticazione Kerberos per l'utente, funzioni correttamente.  
  
5. Aprire Internet Explorer e quindi aprire i siti Web https://app1/ e https://app2/. La possibilità di aprire entrambi i siti Web conferma che entrambi i primi e il secondo tunnel sono backup e il funzionamento. Chiudere Internet Explorer.  
  
6. Avviare il computer EDGE2.  
  
7. Eseguire un arresto normale su EDGE1.  
  
8. Attendere 5 minuti e quindi tornare a CLIENT1. Eseguire i passaggi 2 e 5. In questo modo viene confermato che CLIENT1 è stato in grado di eseguire in modo trasparente failover EDGE2 dopo EDGE1 non erano più disponibili.
