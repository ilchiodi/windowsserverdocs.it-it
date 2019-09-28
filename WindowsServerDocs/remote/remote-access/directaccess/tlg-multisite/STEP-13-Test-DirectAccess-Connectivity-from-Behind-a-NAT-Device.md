---
title: PASSAGGIO 13 testare la connettività DirectAccess da dietro un dispositivo NAT
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41701592c0d9b143c84ad3fbad3fd77491eff5a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404721"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>PASSAGGIO 13 testare la connettività DirectAccess da dietro un dispositivo NAT

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando un client DirectAccess è connesso a Internet da dietro un dispositivo NAT o un server proxy Web, il client DirectAccess usa Teredo o IP-HTTPS per il collegamento al server di Accesso remoto. Se il dispositivo NAT Abilita la porta UDP in uscita 3544 all'indirizzo IP pubblico del server di accesso remoto, viene usato Teredo. Se l'accesso a Teredo non è disponibile, il client DirectAccess esegue il fallback a IP-HTTPS sulla porta TCP in uscita 443, che abilita l'accesso attraverso i firewall o i server proxy Web sulla porta SSL tradizionale. Se un proxy Web richiede l'autenticazione, la connessione IP-HTTPS non riesce. Le connessioni IP-HTTPS non riescono anche quando il proxy Web esegue un'ispezione SSL in uscita perché la sessione HTTPS viene terminata nel proxy Web invece che nel server di Accesso remoto.  
  
Le seguenti procedure vengono eseguite in entrambi i computer client:  
  
1. Testare la connettività Teredo. Il primo set di test viene eseguito quando il client DirectAccess è configurato per l'uso di Teredo. Si tratta dell'impostazione automatica quando il dispositivo NAT consente l'accesso in uscita sulla porta UDP 3544. Eseguire prima i test in CLIENT1, quindi eseguire i test in CLIENT2.  
  
2. Testare la connettività IP-HTTPS. Il secondo set di test viene eseguito quando il client DirectAccess è configurato per l'utilizzo di IP-HTTPS. Per verificare la connettività IP-HTTPS, Teredo viene disabilitato nei computer client. Eseguire prima i test in CLIENT1, quindi eseguire i test in CLIENT2.  
  
## <a name="prerequisites"></a>Prerequisiti  
Avviare EDGE1 e 2-EDGE1 se non sono già in esecuzione e verificare che siano connessi alla subnet Internet.  
  
Prima di eseguire questi test, scollegare CLIENT1 e CLIENT2 dal Commuter Internet e connetterli all'opzione HomeNET. Se viene chiesto quale tipo di rete si desidera definire la rete corrente, selezionare **rete domestica**.  
  
## <a name="TeredoCLIENT1"></a>Testare la connettività Teredo  
  
1. In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevati.  
  
2. Abilitare l'adapter Teredo, digitare **netsh interface teredo set state enterpriseclient**, quindi premere INVIO.  
  
3. Nella finestra di Windows PowerShell digitare **ipconfig** e premere INVIO.  
  
4. Esaminare l'output del comando ipconfig.  
  
   Viene stabilita una connessione Internet da dietro un dispositivo NAT per il computer, a cui viene assegnato un indirizzo IPv4 privato. Quando il client DirectAccess è si trova dietro un dispositivo NAT e viene assegnato un indirizzo IPv4 privato, la tecnologia di transizione IPv6 preferita è Teredo. Se si esamina l'output del comando ipconfig, viene visualizzata una sezione per la pseudo-interfaccia del tunneling Teredo della scheda tunnel e quindi una descrizione di Microsoft Teredo tunneling Adapter con un indirizzo IP che inizia con 2001:0 coerente con un Teredo Indirizzo. Verrà visualizzato il gateway predefinito elencato per la scheda tunnel Teredo come '::'.  
  
5. Nella finestra di Windows PowerShell digitare **ipconfig/flushdns** e premere INVIO.  
  
   Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso a Internet, vengono cancellate.  
  
6. Nella finestra di Windows PowerShell digitare **ping App1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
7. Nella finestra di Windows PowerShell digitare **ping App2** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 a APP2, che in questo caso è FD**C9:9F4E: eb1b**: 7777:: A00:4. Si noti che i valori in grassetto variano a seconda della modalità di generazione dell'indirizzo.  
  
8. Nella finestra di Windows PowerShell digitare **ping 2-App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 2-APP1, 2001: DB8:2:: 3.  
  
9. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Il sito Web IIS predefinito sarà visualizzato in APP1.  
  
10. Nella barra degli indirizzi di Internet Explorer immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
11. Nella schermata **Start** Digitare<strong>\\ \ App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.  
  
12. Ripetere questa procedura in CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Testare la connettività IP-HTTPS  
  
1. In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevati e digitare **netsh interface teredo set state disabled** e premere INVIO. Teredo viene disabilitato nel computer client, che può quindi eseguire la configurazione per l'uso di IP-HTTPS. Quando il comando viene completato, viene visualizzata la risposta **Ok** .  
  
2. Nella finestra di Windows PowerShell digitare **ipconfig** e premere INVIO.  
  
3. Esaminare l'output del comando ipconfig. Viene stabilita una connessione Internet da dietro un dispositivo NAT per il computer, a cui viene assegnato un indirizzo IPv4 privato. Teredo viene disabilitato e il client DirectAccess esegue il fallback a IP-HTTPS. Quando si esamina l'output del comando ipconfig, viene visualizzata una sezione per tunnel Adapter iphttpsinterface con un indirizzo IP che inizia con 2001: DB8:1: 1000 o 2001: DB8:2: 2000 coerenti con il fatto che si tratta di un indirizzo IP-HTTPS basato sui prefissi che erano configurato durante la configurazione di DirectAccess. Non viene visualizzato un gateway predefinito elencato per la scheda tunnel IPHTTPSInterface.  
  
4. Nella finestra di Windows PowerShell digitare **ipconfig/flushdns** e premere INVIO. Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso alla rete aziendale, vengono cancellate.  
  
5. Nella finestra di Windows PowerShell digitare **ping App1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
6. Nella finestra di Windows PowerShell digitare **ping App2** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 a APP2, che in questo caso è FD**C9:9F4E: eb1b**: 7777:: A00:4. Si noti che i valori in grassetto variano a seconda della modalità di generazione dell'indirizzo.  
  
7. Nella finestra di Windows PowerShell digitare **ping 2-App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 2-APP1, 2001: DB8:2:: 3.  
  
8. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Il sito Web IIS predefinito sarà visualizzato in APP1.  
  
9. Nella barra degli indirizzi di Internet Explorer immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
10. Nella schermata **Start** Digitare<strong>\\ \ App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.  
  
11. Ripetere questa procedura in CLIENT2.  
  


