---
title: PASSAGGIO 12 testare la connettività DirectAccess
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
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4e45f0c3c988c86a2428c3beb8bafc29b7b16bc0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446930"
---
# <a name="step-12-test-directaccess-connectivity"></a>PASSAGGIO 12 testare la connettività DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Prima di testare la connettività tra i computer client quando si trovano nelle reti Internet o Homenet, è necessario che siano state installate le impostazioni dei criteri di gruppo corretto.  
  
- Per verificare i client hanno i criteri di gruppo corretta  
  
- Testare la connettività DirectAccess da Internet tramite EDGE1  
  
- Spostare il gruppo di sicurezza Win7_Clients_Site2 CLIENT2  
  
- Testare la connettività DirectAccess da Internet tramite EDGE1 2  
  
## <a name="prerequisites"></a>Prerequisiti  
Connettere entrambi i computer client alla rete Corpnet e quindi riavviare entrambi i computer client.  
  
## <a name="policy"></a>Verificare i client con i criteri di gruppo corretta  
  
1.  In CLIENT1 fare clic su **avviare**, digitare **powershell.exe**, fare doppio clic su **powershell**, fare clic su **avanzate**, quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella finestra di Windows PowerShell, digitare **ipconfig** e premere INVIO.  
  
    Assicurarsi che l'indirizzo IPv4 della scheda di rete aziendale inizia con 10.0.0.  
  
3.  Nel tipo di finestra di Windows PowerShell **Get-DnsClientNrptPolicy** e premere INVIO. Vengono visualizzate le voci della tabella dei criteri di risoluzione dei nomi per DirectAccess.  
  
    -   . corp.contoso.com-queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolto da uno dei server DNS di DirectAccess, con il 2001:db8:1::2 indirizzo IPv6 o 2001:db8:2::20.  
  
    -   NLS.corp.contoso.com queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
4.  Lasciare la finestra di Windows PowerShell aperta per la procedura successiva.  
  
5.  In CLIENT2, fare clic su **avviare**, fare clic su **tutti i programmi**, fare clic su **Accessori**, fare clic su **Windows PowerShell**, fare doppio clic su **Windows PowerShell**, quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
6.  Nella finestra di Windows PowerShell, digitare **ipconfig** e premere INVIO.  
  
    Assicurarsi che l'indirizzo IPv4 della scheda di rete aziendale inizia con 10.0.0.  
  
7.  Nella finestra di Windows PowerShell, digitare **netsh dello spazio dei nomi Mostra criteri** e premere INVIO.  
  
    Nell'output, dovrebbero essere presenti due sezioni:  
  
    -   . corp.contoso.com-queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolte dal server DNS di DirectAccess con 2001:db8:1::2 l'indirizzo IPv6.  
  
    -   NLS.corp.contoso.com queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
8.  Lasciare la finestra di Windows PowerShell aperta per la procedura successiva.  
  
## <a name="EDGE1"></a>Testare la connettività DirectAccess da Internet tramite EDGE1  
  
1. Scollegare 2-EDGE1 dalla rete Internet.  
  
2. Scollegare CLIENT1 e CLIENT2 dal commutatore della rete aziendale e connetterle al commutatore Internet. Attendere 30 secondi.  
  
3. In CLIENT1 nella finestra di Windows PowerShell, digitare **ipconfig /all** e premere INVIO.  
  
4. Esaminare l'output del comando ipconfig.  
  
   Il computer client è ora connesso a Internet e dispone di un indirizzo IPv4 pubblico. Quando il client DirectAccess ha un indirizzo IPv4 pubblico, Usa le tecnologie di transizione Teredo o IP-HTTPS IPv6 per eseguire il tunneling i messaggi di IPv6 tramite una rete Internet IPv4 tra il client DirectAccess e server di accesso remoto. Si noti che la tecnologia di transizione preferita è Teredo.  
  
5. Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO. Ciò elimina voci di risoluzione dei nomi ancora esistenti nella cache DNS del client da quando il computer client era connesso alla rete aziendale.  
  
6. Disabilitare l'interfaccia Teredo per assicurarsi che il computer client utilizza IP-HTTPS per connettersi alla rete aziendale con il comando seguente:  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Assicurarsi di che essere connessi tramite EDGE1. Tipo di **netsh interface httpstunnel Mostra interfacce** e premere INVIO.  
  
   L'output dovrebbe contenere URL: https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Su CLIENT1, è anche possibile eseguire il comando Windows PowerShell seguente: **Get-NetIPHTTPSConfiguration**. L'output mostra le connessioni di URL di server disponibili e il profilo attivo.  
  
8. Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Dovrebbe essere risposte dall'indirizzo IPv6 assegnato ad APP1, che in questo caso è 2001:db8:1::3.  
  
9. Nella finestra di Windows PowerShell, digitare **effettuare il ping 2-app1** e premere INVIO. Le risposte dall'indirizzo IPv6 assegnato a 2-APP1, che in questo caso è 2001:db8:2::3 dovrebbe.  
  
10. Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2, che in questo caso è fd**c9:9f4e:eb1b**: 7777::a00:4. Si noti che i valori in grassetto possono variare a causa di modalità di generazione dell'indirizzo.  
  
    La possibilità di effettuare il ping APP2 è importante, perché la riuscita indica che è stato possibile stabilire una connessione con NAT64 o DNS64, APP2 è una risorsa sola IPv4.  
  
11. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
12. Nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Verrà visualizzato il sito Web predefinito in 2-APP1.  
  
13. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
14. Nel **avviare** digitare<strong>\\\2-App1\Files</strong>, quindi premere INVIO. Fare doppio clic sul file di testo di esempio.  
  
    Ciò dimostra che è stato in grado di connettersi al file server nel dominio corp2.corp.contoso.com quando si è connessi tramite EDGE1.  
  
15. Nel **avviare** digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato in grado di connettersi a un server solo IPv4 con SMB per ottenere una risorsa nel dominio delle risorse.  
  
16. Nel **avviare** digitare**WF. msc**, quindi premere INVIO.  
  
17. Nel **Windows Firewall con sicurezza avanzata** console, si noti che solo le **profilo pubblico** è attiva. Il Firewall di Windows deve essere abilitato per DirectAccess a funzionare correttamente. Se il Firewall di Windows è disabilitato, la connettività DirectAccess non funziona.  
  
18. Nel riquadro sinistro della console, espandere la **Monitoring** e scegliere il **regole di sicurezza delle connessioni** nodo. Si dovrebbero vedere le regole di sicurezza della connessione attiva: **DirectAccess Policy-ClientToCorp**, **dei criteri DirectAccess-ClientToDNS64NAT64PrefixExemption**, **criteri DirectAccess-ClientToInfra**, e **DirectAccess Policy-ClientToNlaExempt**. Scorrere il riquadro al centro a destra per visualizzare il **metodi di autenticazione dal 1 °** e **2 metodi di autenticazione** colonne. Si noti che la prima regola (ClientToCorp) usa Kerberos V5 per stabilire il tunnel della intranet e la terza regola (ClientToInfra) Usa NTLMv2 per stabilire il tunnel dell'infrastruttura.  
  
19. Nel riquadro sinistro della console, espandere la **associazioni di sicurezza** e scegliere il **modalità principale** nodo. Si noti che le associazioni di sicurezza del tunnel dell'infrastruttura usando NTLMv2 e l'associazione di sicurezza del tunnel di intranet utilizzando Kerberos V5. Fare clic sulla voce che illustra **utente (Kerberos V5)** come la **2nd metodo di autenticazione** e fare clic su **proprietà**. Nel **generali** scheda, si noti il **seconda autenticazione ID locale** viene **CORP\User1**, che indica che User1 è stata in grado di eseguire l'autenticazione al dominio CORP utilizzando Kerberos.  
  
20. Ripetere questa procedura dal passaggio 3 su CLIENT2.  
  
## <a name="secgroup"></a>Spostare il gruppo di sicurezza Win7_Clients_Site2 CLIENT2  
  
1.  In DC1 fare clic su **avviare**, digitare **DSA. msc**, quindi premere INVIO.  
  
2.  Nella console di Active Directory Users and Computers, aprire **corp.contoso.com/Users** e fare doppio clic su **Win7_Clients_Site1**.  
  
3.  Nel **proprietà Win7_Clients_Site1** della finestra di dialogo fare clic sul **membri** scheda, fare clic su **CLIENT2**, fare clic su **rimuovere**, fare clic su **Yes**, quindi fare clic su **OK**.  
  
4.  Fare doppio clic su **Win7_Clients_Site2**e quindi sul **proprietà Win7_Clients_Site2** della finestra di dialogo fare clic sul **membri** scheda.  
  
5.  Fare clic su **Add**e nella **Seleziona utenti, contatti, computer o gli account del servizio** della finestra di dialogo fare clic su **tipi di oggetti**, selezionare **computer**, quindi fare clic su **OK**.  
  
6.  Nelle **immettere i nomi degli oggetti da selezionare**, digitare **CLIENT2**, quindi fare clic su **OK**.  
  
7.  Riavviare CLIENT2 e accedere utilizzando l'account User1/corp.  
  
8.  In CLIENT2 aprire una finestra di Windows PowerShell con privilegi elevata, digitare **netsh dello spazio dei nomi Mostra criteri** e premere INVIO.  
  
    Nell'output, dovrebbero essere presenti due sezioni:  
  
    -   . corp.contoso.com-queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolte dal server DNS di DirectAccess con 2001:db8:2::20 l'indirizzo IPv6.  
  
    -   NLS.corp.contoso.com queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
## <a name="DAConnect"></a>Testare la connettività DirectAccess da Internet tramite EDGE1 2  
  
1. Connettere la rete Internet EDGE1 2.  
  
2. Scollegare EDGE1 dalla rete Internet.  
  
3. In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevata.  
  
4. Nella finestra di Windows PowerShell, digitare **ipconfig /flushdns** e premere INVIO. Ciò elimina voci di risoluzione dei nomi ancora esistenti nella cache DNS del client da quando il computer client era connesso alla rete aziendale.  
  
5. Assicurarsi di che essere connessi tramite EDGE1 2. Tipo di **netsh interface httpstunnel Mostra interfacce** e premere INVIO.  
  
   L'output dovrebbe contenere URL: https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Su CLIENT1, è anche possibile eseguire il comando seguente: **Get-NetIPHTTPSConfiguration**. L'output mostra le connessioni di URL di server disponibili e il profilo attivo.  
  
   > [!NOTE]  
   > CLIENT1 cambia automaticamente il server tramite il quale si connette alle risorse aziendali. Se l'output del comando Mostra una connessione a EDGE1, attendere circa cinque minuti e ripetere l'operazione.  
  
6. Nella finestra di Windows PowerShell, digitare **ping app1** e premere INVIO. Dovrebbe essere risposte dall'indirizzo IPv6 assegnato ad APP1, che in questo caso è 2001:db8:1::3.  
  
7. Nella finestra di Windows PowerShell, digitare **effettuare il ping 2-app1** e premere INVIO. Le risposte dall'indirizzo IPv6 assegnato a 2-APP1, che in questo caso è 2001:db8:2::3 dovrebbe.  
  
8. Nella finestra di Windows PowerShell, digitare **ping app2** e premere INVIO. Le risposte dall'indirizzo NAT64 assegnato da EDGE1 ad APP2, che in questo caso è fd**c9:9f4e:eb1b**: 7777::a00:4. Si noti che i valori in grassetto possono variare a causa di modalità di generazione dell'indirizzo.  
  
   La possibilità di effettuare il ping APP2 è importante, perché la riuscita indica che è stato possibile stabilire una connessione con NAT64 o DNS64, APP2 è una risorsa sola IPv4.  
  
9. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer, immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
10. Nella barra degli indirizzi di Internet Explorer, immettere **https://2-app1/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
11. Nella barra degli indirizzi di Internet Explorer, immettere **https://app2/** e premere INVIO. Verrà visualizzato il sito Web predefinito in APP3.  
  
12. Nel **avviare** digitare<strong>\\\App1\Files</strong>, quindi premere INVIO. Fare doppio clic sul file di testo di esempio.  
  
    Ciò dimostra che è stato in grado di connettersi al file server nel dominio corp.contoso.com quando si è connessi tramite EDGE1 2.  
  
13. Nel **avviare** digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato in grado di connettersi a un server solo IPv4 con SMB per ottenere una risorsa nel dominio delle risorse.  
  
14. Ripetere questa procedura su CLIENT2 ottenuto nel passaggio 3.  
  


