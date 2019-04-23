---
title: PASSAGGIO 6 di testare la connettività Client DirectAccess da dietro un dispositivo NAT
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a48bb9b012a81e91fbbe0ad914637dacac5a8a5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883002"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>PASSAGGIO 6 di testare la connettività Client DirectAccess da dietro un dispositivo NAT

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando un client DirectAccess è connesso a Internet da dietro un dispositivo NAT o un server proxy Web, il client DirectAccess usa Teredo o IP-HTTPS per il collegamento al server di Accesso remoto. 

Se il dispositivo NAT abilita la porta UDP in uscita 3544 per indirizzo IP pubblico del server Accesso remoto, viene usato Teredo. Se l'accesso a Teredo non è disponibile, il client DirectAccess esegue il fallback a IP-HTTPS sulla porta TCP in uscita 443, che abilita l'accesso attraverso i firewall o i server proxy Web sulla porta SSL tradizionale. 

Se un proxy Web richiede l'autenticazione, la connessione IP-HTTPS non riesce. Le connessioni IP-HTTPS non riescono anche quando il proxy Web esegue un'ispezione SSL in uscita perché la sessione HTTPS viene terminata nel proxy Web invece che nel server di Accesso remoto. In questa sezione vengono eseguiti gli stessi test usati per la connessione 6to4 descritta nella precedente sezione.  
  
Le seguenti procedure vengono eseguite in entrambi i computer client:  
  
1. Testare la connettività Teredo. Il primo set di test vengono eseguiti quando il client DirectAccess è configurato per usare Teredo. Si tratta dell'impostazione automatica quando il dispositivo NAT consente l'accesso in uscita sulla porta UDP 3544.  
  
2. Testare la connettività IP-HTTPS. Il secondo set di test vengono eseguiti quando il client DirectAccess è configurato per utilizzare IP-HTTPS. Per verificare la connettività IP-HTTPS, Teredo viene disabilitato nei computer client.  
  
> [!TIP]  
> Si consiglia di cancellare la cache di Internet Explorer prima di eseguire queste procedure per assicurarsi di testare la connessione e non il recupero delle pagine del sito Web dalla cache.  
  
## <a name="prerequisites"></a>Prerequisiti

Prima di eseguire i test, scollegare CLIENT1 dal commutatore Internet e collegarlo al commutatore Homenet. Se viene chiesto di definire il tipo di rete corrente, selezionare **Rete domestica**.  
  
Avviare EDGE1 ed EDGE2 se non sono già in esecuzione.  
  
## <a name="test-teredo-connectivity"></a>Testare la connettività Teredo  
  
1.  In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevata, digitare **ipconfig /all** e premere INVIO.  
  
2.  Esaminare l'output del comando ipconfig.  
  
    Viene stabilita una connessione Internet da dietro un dispositivo NAT per CLIENT1, a cui viene assegnato un indirizzo IPv4 privato. Quando il client DirectAccess è si trova dietro un dispositivo NAT e viene assegnato un indirizzo IPv4 privato, la tecnologia di transizione IPv6 preferita è Teredo. Se si esamina l'output del comando ipconfig, si noterà una sezione per la scheda Tunnel Teredo pseudointerfaccia di Tunneling e quindi Microsoft Teredo Tunneling Adapter, una descrizione con un indirizzo IP che inizia con 2001: conforme a un Teredo indirizzo. Se la sezione Teredo non è visualizzata, abilitare Teredo con il comando **netsh interface Teredo set state enterpriseclient** e quindi eseguire nuovamente il comando ipconfig. Per la scheda tunnel Teredo non viene visualizzato un gateway predefinito.  
  
3.  Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO.  
  
    Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso a Internet, vengono cancellate.  
  
4.  Nella finestra di Windows PowerShell, digitare **Get-DnsClientNrptPolicy** e premere INVIO.  
  
    L'output visualizza le impostazioni correnti per la tabella dei criteri di risoluzione dei nomi (NRPT). Queste impostazioni indicano che tutte le connessioni a .corp.contoso.com devono essere risolte dal server DNS di Accesso remoto, con l'indirizzo IPv6 2001:db8:1::2. Inoltre, annotare la voce NRPT che indica un'esenzione per il nome nls.corp.contoso.com; i nomi nell'elenco delle esenzioni non ricevono risposta dal server DNS di Accesso remoto. È possibile eseguire il ping dell'indirizzo IP del server DNS di Accesso remoto per confermare la connettività al server di Accesso remoto; in questo esempio, viene eseguito il ping di 2001:db8:1::2.  
  
5.  Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
6.  Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2 che, in questo caso, corrisponde a fdc9:9f4e:eb1b:7777::a00:4.  
  
7.  Lasciare la finestra di Windows PowerShell aperta per la procedura successiva.  
  
8.  Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
9. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
10. Nel **avviare** digitare**\\\App2\Files**, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.  
  
## <a name="test-ip-https-connectivity"></a>Testare la connettività IP-HTTPS  
  
1.  Aprire una finestra di Windows PowerShell con privilegi elevata, digitare **netsh interface teredo impostato lo stato disabilitato** e premere INVIO. Teredo viene disabilitato nel computer client, che può quindi eseguire la configurazione per l'uso di IP-HTTPS. Quando il comando viene completato, viene visualizzata la risposta **Ok** .  
  
2.  Nella finestra di Windows PowerShell, digitare **ipconfig /all** e premere INVIO.  
  
3.  Esaminare l'output del comando ipconfig. Viene stabilita una connessione Internet da dietro un dispositivo NAT per il computer, a cui viene assegnato un indirizzo IPv4 privato. Teredo viene disabilitato e il client DirectAccess esegue il fallback a IP-HTTPS. Quando si esamina l'output del comando ipconfig, si vede una sezione per la scheda Tunnel iphttpsinterface con un indirizzo IP che inizia con 2001:db8:1:100 vale a dire con un indirizzo IP-HTTPS basato sul prefisso configurato durante la configurazione DirectAccess. Per la scheda tunnel IP-HTTPS non viene visualizzato un gateway predefinito.  
  
4.  Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO. Le eventuali voci di risoluzione dei nomi ancora esistenti nella cache DNS del client, create quando il computer client era connesso alla rete aziendale, vengono cancellate.  
  
5.  Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo IPv6 di APP1, 2001:db8:1::3.  
  
6.  Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Vengono visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2 che, in questo caso, corrisponde a fdc9:9f4e:eb1b:7777::a00:4.  
  
7.  Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito IIS predefinito in APP1.  
  
8.  Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
9. Nel **avviare** digitare**\\\App2\Files**, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo. Ciò dimostra che è stato possibile connettersi a un server solo IPv4 con SMB per ottenere una risorsa disponibile in un host solo IPv4.
