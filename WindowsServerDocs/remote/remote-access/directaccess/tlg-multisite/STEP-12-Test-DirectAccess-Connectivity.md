---
title: PASSAGGIO 12 testare la connettività DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3f67ef0131d5fc765c3fe99fdff85d93e869902e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308839"
---
# <a name="step-12-test-directaccess-connectivity"></a>PASSAGGIO 12 testare la connettività DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Prima di testare la connettività dai computer client quando si trovano in Internet o in reti HomeNET, è necessario assicurarsi che dispongano delle impostazioni di criteri di gruppo corrette.  
  
- Per verificare che i client dispongano dei criteri di gruppo corretti  
  
- Testare la connettività DirectAccess da Internet tramite EDGE1  
  
- Spostare CLIENT2 nel gruppo di sicurezza Win7_Clients_Site2  
  
- Testare la connettività DirectAccess da Internet tramite 2-EDGE1  
  
## <a name="prerequisites"></a>Prerequisiti  
Connettere entrambi i computer client alla rete corpnet, quindi riavviare entrambi i computer client.  
  
## <a name="verify-clients-have-the-correct-group-policy"></a><a name="policy"></a>Verificare che i client dispongano dei criteri di gruppo corretti  
  
1.  In CLIENT1 fare clic sul pulsante **Start**, digitare **PowerShell. exe**, fare clic con il pulsante destro del mouse su **PowerShell**, scegliere **Avanzate**e quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nella finestra di Windows PowerShell digitare **ipconfig** e premere INVIO.  
  
    Verificare che l'indirizzo IPv4 dell'adapter corpnet inizi con 10.0.0.  
  
3.  Nella finestra di Windows PowerShell digitare **Get-DnsClientNrptPolicy** e premere INVIO. Vengono visualizzate le voci della tabella dei criteri di risoluzione dei nomi per DirectAccess.  
  
    -   . corp.contoso.com: queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolte da uno dei server DNS DirectAccess, con l'indirizzo IPv6 2001: DB8:1:: 2 o 2001: DB8:2:: 20.  
  
    -   nls.corp.contoso.com: queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
4.  Lasciare aperta la finestra di Windows PowerShell per la procedura successiva.  
  
5.  In CLIENT2 fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **Accessori**, **Windows PowerShell**, fare clic con il pulsante destro del mouse su **Windows PowerShell**, quindi scegliere **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
6.  Nella finestra di Windows PowerShell digitare **ipconfig** e premere INVIO.  
  
    Verificare che l'indirizzo IPv4 dell'adapter corpnet inizi con 10.0.0.  
  
7.  Nella finestra di Windows PowerShell digitare **netsh namespace show policy** e premere INVIO.  
  
    Nell'output devono essere presenti due sezioni:  
  
    -   . corp.contoso.com-queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolte dal server DNS DirectAccess, con l'indirizzo IPv6 2001: DB8:1:: 2.  
  
    -   nls.corp.contoso.com: queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
8.  Lasciare aperta la finestra di Windows PowerShell per la procedura successiva.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-edge1"></a><a name="EDGE1"></a>Testare la connettività DirectAccess da Internet tramite EDGE1  
  
1. Scollegare 2 EDGE1 dalla rete Internet.  
  
2. Scollegare CLIENT1 e CLIENT2 dall'opzione corpnet e connetterli al Commuter Internet. Attendere 30 secondi.  
  
3. In CLIENT1, nella finestra di Windows PowerShell, digitare **ipconfig** e premere INVIO.  
  
4. Esaminare l'output del comando ipconfig.  
  
   Il computer client è ora connesso a Internet e dispone di un indirizzo IPv4 pubblico. Quando il client DirectAccess dispone di un indirizzo IPv4 pubblico, USA le tecnologie di transizione IPv6 Teredo o IP-HTTPS per eseguire il tunneling dei messaggi IPv6 su una rete Internet IPv4 tra il client DirectAccess e il server di accesso remoto. Si noti che Teredo è la tecnologia di transizione preferita.  
  
5. Nella finestra di Windows PowerShell digitare **ipconfig/flushdns** e premere INVIO. Questa operazione Scarica le voci di risoluzione dei nomi che potrebbero ancora esistere nella cache DNS del client da quando il computer client era connesso a Corpnet.  
  
6. Disabilitare l'interfaccia Teredo per verificare che il computer client utilizzi IP-HTTPS per connettersi a corpnet con il comando seguente:  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Assicurarsi di essere connessi tramite EDGE1. Digitare **netsh interface httpstunnel show interfaces show interfaces** e premere INVIO.  
  
   L'output deve contenere URL: https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > In CLIENT1 è anche possibile eseguire il comando di Windows PowerShell seguente: **Get-NetIPHTTPSConfiguration**. L'output Mostra le connessioni URL server disponibili e il profilo attualmente attivo.  
  
8. Nella finestra di Windows PowerShell digitare **ping App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 assegnato a APP1, che in questo caso è 2001: DB8:1:: 3.  
  
9. Nella finestra di Windows PowerShell digitare **ping 2-App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 assegnato a 2-APP1, che in questo caso è 2001: DB8:2:: 3.  
  
10. Nella finestra di Windows PowerShell digitare **ping App2** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 a APP2, che in questo caso è FD**C9:9F4E: eb1b**: 7777:: A00:4. Si noti che i valori in grassetto variano a seconda della modalità di generazione dell'indirizzo.  
  
    La possibilità di eseguire il ping di APP2 è importante, poiché l'esito positivo indica che è stato possibile stabilire una connessione tramite NAT64/DNS64, perché APP2 è una risorsa solo IPv4.  
  
11. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
12. Nella barra degli indirizzi di Internet Explorer immettere **https://2-app1/** e premere INVIO. Il sito Web predefinito sarà visualizzato in APP1.  
  
13. Nella barra degli indirizzi di Internet Explorer immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
14. Nella schermata **Start** Digitare<strong>\\\2-App1\Files</strong>, quindi premere INVIO. Fare doppio clic sul file di testo di esempio.  
  
    Ciò dimostra che è stato possibile connettersi al file server nel dominio corp2.corp.contoso.com quando si è connessi tramite EDGE1.  
  
15. Nella schermata **Start** Digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato possibile connettersi a un server solo IPv4 usando SMB per ottenere una risorsa nel dominio delle risorse.  
  
16. Nella schermata **Start** Digitare**WF. msc**, quindi premere INVIO.  
  
17. Nel **Windows Firewall con la console di sicurezza avanzata** si noti che è attivo solo il **profilo pubblico** . Per il corretto funzionamento di DirectAccess, è necessario abilitare il Windows Firewall. Se la Windows Firewall è disabilitata, la connettività DirectAccess non funziona.  
  
18. Nel riquadro sinistro della console espandere il nodo **monitoraggio** , quindi fare clic sul nodo **regole di sicurezza della connessione** . Verranno visualizzate le regole di sicurezza della connessione attive, ovvero **Criteri DirectAccess-ClientToCorp**, **Criteri DirectAccess-ClientToDNS64NAT64PrefixExemption**, **Criteri DirectAccess-ClientToInfra**e **Criteri DirectAccess-ClientToNlaExempt**. Scorrere il riquadro centrale a destra per visualizzare i **primi metodi di autenticazione** e **2 colonne metodi di autenticazione** . Si noti che la prima regola (ClientToCorp) USA Kerberos V5 per stabilire il tunnel della Intranet e la terza regola (ClientToInfra) USA NTLMv2 per stabilire il tunnel dell'infrastruttura.  
  
19. Nel riquadro sinistro della console espandere il nodo Associazioni di **sicurezza** e fare clic sul nodo **modalità principale** . Si notino le associazioni di sicurezza del tunnel dell'infrastruttura usando NTLMv2 e l'associazione di sicurezza del tunnel Intranet tramite Kerberos V5. Fare clic con il pulsante destro del mouse sulla voce che mostra l' **utente (Kerberos V5)** come **secondo metodo di autenticazione** e fare clic su **proprietà**. Nella scheda **generale** si noti che il **secondo ID locale di autenticazione** è **Corp\user1.** , che indica che User1 è stato in grado di eseguire l'autenticazione al dominio Corp tramite Kerberos.  
  
20. Ripetere questa procedura dal passaggio 3 in CLIENT2.  
  
## <a name="move-client2-to-the-win7_clients_site2-security-group"></a><a name="secgroup"></a>Spostare CLIENT2 nel gruppo di sicurezza Win7_Clients_Site2  
  
1.  In DC1 fare clic sul pulsante **Start**, digitare **DSA. msc**, quindi premere INVIO.  
  
2.  Nella console Active Directory utenti e computer aprire **Corp.contoso.com/Users** e fare doppio clic su **Win7_Clients_Site1**.  
  
3.  Nella finestra di dialogo **proprietà Win7_Clients_Site1** fare clic sulla scheda **membri** , fare clic su **CLIENT2**, fare clic su **Rimuovi**, quindi su **Sì**e infine su **OK**.  
  
4.  Fare doppio clic su **Win7_Clients_Site2**, quindi nella finestra di dialogo **Proprietà Win7_Clients_Site2** fare clic sulla scheda **membri** .  
  
5.  Fare clic su **Aggiungi**e nella finestra di dialogo **Seleziona utenti, contatti, computer o account di servizio** fare clic su **tipi di oggetto**, selezionare **computer**, quindi fare clic su **OK**.  
  
6.  In **immettere i nomi degli oggetti da selezionare**digitare **CLIENT2**, quindi fare clic su **OK**.  
  
7.  Riavviare CLIENT2 e accedere usando l'account Corp/User1.  
  
8.  In CLIENT2 aprire una finestra di Windows PowerShell con privilegi elevati, digitare **netsh namespace show policy** e premere INVIO.  
  
    Nell'output devono essere presenti due sezioni:  
  
    -   . corp.contoso.com-queste impostazioni indicano che tutte le connessioni a corp.contoso.com devono essere risolte dal server DNS DirectAccess, con l'indirizzo IPv6 2001: DB8:2:: 20.  
  
    -   nls.corp.contoso.com: queste impostazioni indicano che è presente un'esenzione per il nome nls.corp.contoso.com.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-2-edge1"></a><a name="DAConnect"></a>Testare la connettività DirectAccess da Internet tramite 2-EDGE1  
  
1. Connettere 2-EDGE1 alla rete Internet.  
  
2. Scollegare EDGE1 dalla rete Internet.  
  
3. In CLIENT1 aprire una finestra di Windows PowerShell con privilegi elevati.  
  
4. Nella finestra di Windows PowerShell digitare **ipconfig/flushdns** e premere INVIO. Questa operazione Scarica le voci di risoluzione dei nomi che potrebbero ancora esistere nella cache DNS del client da quando il computer client era connesso a Corpnet.  
  
5. Assicurarsi di essere connessi tramite 2 EDGE1. Digitare **netsh interface httpstunnel show interfaces show interfaces** e premere INVIO.  
  
   L'output deve contenere URL: https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > In CLIENT1 è anche possibile eseguire il comando seguente: **Get-NetIPHTTPSConfiguration**. L'output Mostra le connessioni URL server disponibili e il profilo attualmente attivo.  
  
   > [!NOTE]  
   > CLIENT1 modifica automaticamente il server tramite cui si connette alle risorse aziendali. Se l'output del comando Mostra una connessione a EDGE1, attendere circa cinque minuti, quindi riprovare.  
  
6. Nella finestra di Windows PowerShell digitare **ping App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 assegnato a APP1, che in questo caso è 2001: DB8:1:: 3.  
  
7. Nella finestra di Windows PowerShell digitare **ping 2-App1** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo IPv6 assegnato a 2-APP1, che in questo caso è 2001: DB8:2:: 3.  
  
8. Nella finestra di Windows PowerShell digitare **ping App2** e premere INVIO. Verranno visualizzate le risposte dall'indirizzo NAT64 assegnato da EDGE1 a APP2, che in questo caso è FD**C9:9F4E: eb1b**: 7777:: A00:4. Si noti che i valori in grassetto variano a seconda della modalità di generazione dell'indirizzo.  
  
   La possibilità di eseguire il ping di APP2 è importante, poiché l'esito positivo indica che è stato possibile stabilire una connessione tramite NAT64/DNS64, perché APP2 è una risorsa solo IPv4.  
  
9. Aprire Internet Explorer, nella barra degli indirizzi di Internet Explorer immettere **https://app1/** e premere INVIO. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
10. Nella barra degli indirizzi di Internet Explorer immettere **https://2-app1/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP2.  
  
11. Nella barra degli indirizzi di Internet Explorer immettere **https://app2/** e premere INVIO. Viene visualizzato il sito Web predefinito in APP3.  
  
12. Nella schermata **Start** Digitare<strong>\\\App1\Files</strong>, quindi premere INVIO. Fare doppio clic sul file di testo di esempio.  
  
    Ciò dimostra che è stato possibile connettersi al file server nel dominio corp.contoso.com quando si è connessi tramite 2 EDGE1.  
  
13. Nella schermata **Start** Digitare<strong>\\\App2\Files</strong>, quindi premere INVIO. Fare doppio clic sul file Nuovo documento di testo.  
  
    Ciò dimostra che è stato possibile connettersi a un server solo IPv4 usando SMB per ottenere una risorsa nel dominio delle risorse.  
  
14. Ripetere questa procedura in CLIENT2 al passaggio 3.  
  


