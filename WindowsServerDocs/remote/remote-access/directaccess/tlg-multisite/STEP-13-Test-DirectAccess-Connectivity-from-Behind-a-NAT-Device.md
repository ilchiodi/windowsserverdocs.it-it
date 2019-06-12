---
title: PASSAGGIO 13 testare la connettività DirectAccess da dietro un dispositivo NAT
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d1661d43cd45614dfabc66fd9a737c55ab388ed
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446919"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>PASSAGGIO 13 testare la connettività DirectAccess da dietro un dispositivo NAT

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando un client DirectAccess è connesso a Internet da dietro un dispositivo NAT o un server proxy Web, il client DirectAccess usa Teredo o IP-HTTPS per il collegamento al server di Accesso remoto. Se il dispositivo NAT abilita la porta UDP in uscita 3544 per indirizzo IP pubblico del server Accesso remoto, viene usato Teredo. Se l'accesso a Teredo non è disponibile, il client DirectAccess esegue il fallback a IP-HTTPS sulla porta TCP in uscita 443, che abilita l'accesso attraverso i firewall o i server proxy Web sulla porta SSL tradizionale. Se un proxy Web richiede l'autenticazione, la connessione IP-HTTPS non riesce. Le connessioni IP-HTTPS non riescono anche quando il proxy Web esegue un'ispezione SSL in uscita perché la sessione HTTPS viene terminata nel proxy Web invece che nel server di Accesso remoto.  
  
Le seguenti procedure vengono eseguite in entrambi i computer client:  
  
1. Testare la connettività Teredo. Il primo set di test vengono eseguiti quando il client DirectAccess è configurato per usare Teredo. Si tratta dell'impostazione automatica quando il dispositivo NAT consente l'accesso in uscita sulla porta UDP 3544. Prima di tutto eseguire i test in CLIENT1 e quindi eseguire i test su CLIENT2.  
  
2. Testare la connettività IP-HTTPS. Il secondo set di test vengono eseguiti quando il client DirectAccess è configurato per utilizzare IP-HTTPS. Per verificare la connettività IP-HTTPS, Teredo viene disabilitato nei computer client. Prima di tutto eseguire i test in CLIENT1 e quindi eseguire i test su CLIENT2.  
  
## <a name="prerequisites"></a>Prerequisiti  
Avviare EDGE1 ed EDGE1 2 se non sono già in esecuzione e verificare che sono connesse alla subnet Internet.  
  
Prima di eseguire questi test, scollegare CLIENT1 e CLIENT2 dal commutatore Internet e connetterle al commutatore Homenet. Se viene richiesto il tipo di rete si desidera definire la rete corrente, selezionare **rete domestica**.  
  
## <a name="TeredoCLIENT1"></a>Testare la connettività Teredo  
  
1. In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevata.  
  
2. Abilita la scheda Teredo, digitare **netsh interface teredo impostato state enterpriseclient**, quindi premere INVIO.  
  
3. Nella finestra di Windows PowerShell, digitare **ipconfig /all** e premere INVIO.  
  
4. Esaminare l'output del comando ipconfig.  
  
   Viene stabilita una connessione Internet da dietro un dispositivo NAT per il computer, a cui viene assegnato un indirizzo IPv4 privato. Quando il client DirectAccess è si trova dietro un dispositivo NAT e viene assegnato un indirizzo IPv4 privato, la tecnologia di transizione IPv6 preferita è Teredo. Se si esamina l'output del comando ipconfig, si noterà una sezione per la scheda Tunnel Teredo pseudointerfaccia di Tunneling e quindi Microsoft Teredo Tunneling Adapter, una descrizione con un indirizzo IP che inizia con 2001: 0 conforme a un Teredo indirizzo. Verrà visualizzato il gateway predefinito per la scheda tunnel Teredo come ':: '.  
  
5. Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO.  
  
   Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso a Internet, vengono cancellate.  
  
6. Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
7. Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2, che in questo caso è fd**c9:9f4e:eb1b**: 7777::a00:4. Si noti che i valori in grassetto possono variare a causa di modalità di generazione dell'indirizzo.  
  
8. Nella finestra di Windows PowerShell, digitare **effettuare il ping 2-app1** e premere INVIO. Le risposte dall'indirizzo IPv6 di 2-APP1, 2001:db8:2::3 dovrebbe.  
  
9. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Verrà visualizzato il sito Web IIS predefinito in 2-APP1.  
  
10. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
11. Nel **avviare** digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.  
  
12. Ripetere questa procedura su CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Testare la connettività IP-HTTPS  
  
1. In CLIENT1 aprire con privilegi elevati finestra Windows PowerShell e digitare **netsh interface teredo impostato lo stato disabilitato** e premere INVIO. Teredo viene disabilitato nel computer client, che può quindi eseguire la configurazione per l'uso di IP-HTTPS. Quando il comando viene completato, viene visualizzata la risposta **Ok** .  
  
2. Nella finestra di Windows PowerShell, digitare **ipconfig /all** e premere INVIO.  
  
3. Esaminare l'output del comando ipconfig. Viene stabilita una connessione Internet da dietro un dispositivo NAT per il computer, a cui viene assegnato un indirizzo IPv4 privato. Teredo viene disabilitato e il client DirectAccess esegue il fallback a IP-HTTPS. Quando si esamina l'output del comando ipconfig, si vede una sezione per la scheda Tunnel iphttpsinterface con un indirizzo IP che inizia con 2001:db8:1:1000 o 2001:DB8:2:2000::/ coerenti con questo da un indirizzo IP-HTTPS in base i prefissi che sono stati configurato durante la configurazione di DirectAccess. Non si vedranno un gateway predefinito per la scheda tunnel IPHTTPSInterface.  
  
4. Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO. Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso alla rete aziendale, vengono cancellate.  
  
5. Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
6. Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2, che in questo caso è fd**c9:9f4e:eb1b**: 7777::a00:4. Si noti che i valori in grassetto possono variare a causa di modalità di generazione dell'indirizzo.  
  
7. Nella finestra di Windows PowerShell, digitare **effettuare il ping 2-app1** e premere INVIO. Le risposte dall'indirizzo IPv6 di 2-APP1, 2001:db8:2::3 dovrebbe.  
  
8. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Verrà visualizzato il sito Web IIS predefinito in 2-APP1.  
  
9. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
10. Nel **avviare** digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.  
  
11. Ripetere questa procedura su CLIENT2.  
  


