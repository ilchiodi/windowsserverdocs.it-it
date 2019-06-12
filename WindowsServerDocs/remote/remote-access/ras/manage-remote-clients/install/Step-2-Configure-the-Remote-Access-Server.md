---
title: Passaggio 2 configurare il Server di accesso remoto
description: Questo argomento fa parte della Guida di client DirectAccess di gestire in remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85bc7385fc3413bedb539cdbe9982b33dc653d6f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446895"
---
# <a name="step-2-configure-the-remote-access-server"></a>Passaggio 2 configurare il Server di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare le impostazioni client e server necessari per la gestione remota dei client DirectAccess. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [passaggio 2 pianificare la distribuzione di accesso remoto](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Attività|Descrizione|  
|----|--------|  
|Installare il ruolo Accesso remoto|Consente di installare il ruolo Accesso remoto.|  
|Configurare il tipo di distribuzione|Consente di configurare il tipo di distribuzione come DirectAccess e VPN, solo DirectAccess o solo VPN.|  
|Configurare i client DirectAccess|Consente di configurare il server di Accesso remoto con i gruppi di sicurezza contenenti i client DirectAccess.|  
|Configurare il server di Accesso remoto|Configurare le impostazioni del server di accesso remoto.|  
|Configurare i server dell'infrastruttura|Consente di configurare i server dell'infrastruttura usati nell'organizzazione.|  
|Configurare i server applicazioni|Configurare i server di applicazione per richiedere l'autenticazione e crittografia.|  
|Riepilogo della configurazione e oggetti Criteri di gruppo alternativi|Consente di visualizzare il riepilogo della configurazione di Accesso remoto e modificare gli oggetti Criteri di gruppo, se si vuole.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Installare il ruolo Accesso remoto  
È necessario installare il ruolo Accesso remoto in un server nell'organizzazione che fungerà da server di accesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Per installare il ruolo Accesso remoto  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Per installare il ruolo Accesso remoto nel server DirectAccess  
  
1.  Nel server DirectAccess, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nel **Selezione ruoli Server** finestra di dialogo, selezionare **accesso remoto**, quindi fare clic su **Avanti**.  
  
4.  Fare clic su **Avanti** tre volte.  
  
5.  Nel **Selezione servizi ruolo** finestra di dialogo, selezionare **DirectAccess e VPN (RAS)** e quindi fare clic su **Aggiungi funzionalità**.  
  
6.  Selezionare **Routing**, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7. Fare clic su **Avanti**e quindi su **Installa**.  
  
8.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>Configurare il tipo di distribuzione  
Sono disponibili tre opzioni che è possibile utilizzare per distribuire accesso remoto dalla console di gestione accesso remoto:  
  
-   DirectAccess e VPN  
  
-   Solo DirectAccess  
  
-   Solo VPN  
  
> [!NOTE]  
> In questa guida viene utilizzato il DirectAccess solo metodo di distribuzione nelle procedure di esempio.  
  
#### <a name="to-configure-the-deployment-type"></a>Per configurare il tipo di distribuzione  
  
1.  Nel server di Accesso remoto, aprire la Console di gestione Accesso remoto: Nel **avviare** dello schermo, tipo, tipo **Console Gestione accesso remoto**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella Console di gestione accesso remoto, nel riquadro centrale, fare clic su **eseguire Configurazione guidata accesso remoto**.  
  
3.  Nel **Configura accesso remoto** la finestra di dialogo selezionare DirectAccess e VPN, solo DirectAccess o solo VPN.  
  
## <a name="BKMK_Clients"></a>Configurare i client DirectAccess  
Per effettuarne il provisioning allo scopo di usare DirectAccess, un computer client deve appartenere al gruppo di sicurezza selezionato. Dopo aver configurato DirectAccess, viene effettuato il provisioning di computer client nel gruppo di sicurezza per la ricezione di DirectAccess oggetti Criteri di gruppo (GPO) per la gestione remota.  
  
#### <a name="to-configure-directaccess-clients"></a>Per configurare i client DirectAccess  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 1 Client remoti**, fare clic su **Configura**.  
  
2.  Nella configurazione guidata Client DirectAccess sul **uno Scenario di distribuzione** pagina, fare clic su **distribuire DirectAccess per la gestione remota solo**, e quindi fare clic su **Avanti**.  
  
3.  Nel **Seleziona gruppi** fare clic su **Aggiungi**.  
  
4.  Nel **Seleziona gruppi** finestra di dialogo, selezionare i gruppi di sicurezza contenenti i computer client DirectAccess e quindi fare clic su **Avanti**.  
  
5.  Nel **Assistente connettività di rete** pagina:  
  
    -   Nella tabella, aggiungere le risorse utilizzate per determinare la connettività alla rete interna. Se non sono configurate altre risorse, viene creato automaticamente un probe Web predefinito. Quando si configurano i percorsi di probe web per determinare la connettività alla rete aziendale, assicurarsi di aver configurato almeno un probe di basate su HTTP. La configurazione solo una verifica ping non è sufficiente, e ciò potrebbe causare un'una determinazione non corretta dello stato di connettività. Questo avviene perché ping è esente da IPsec. Di conseguenza, il comando ping non garantisce che i tunnel IPsec siano correttamente impostati.  
  
    -   Aggiungere un indirizzo di posta elettronica del supporto tecnico per consentire agli utenti di inviare informazioni se incontrano difficoltà di connettività.  
  
    -   Fornire un nome descrittivo per la connessione DirectAccess.  
  
    -   Selezionare il **DirectAccess consente ai client di utilizzare la risoluzione dei nomi locale** casella di controllo, se necessario.  
  
        > [!NOTE]  
        > Quando è abilitata la risoluzione dei nomi locali, gli utenti che eseguono il NCA possono risolvere nomi con i server DNS configurati nel computer client DirectAccess.  
  
6.  Scegliere **Fine**.  
  
## <a name="BKMK_Server"></a>Configurare il server di accesso remoto  
Per distribuire accesso remoto, è necessario configurare il server che fungerà da server di accesso remoto con il codice seguente:  
  
1.  Schede di rete corrette  
  
2.  Un URL pubblico per il server di accesso remoto per il client che i computer possano connettersi (l'indirizzo ConnectTo)  
  
3.  Un certificato IP-HTTPS con un oggetto che corrisponde all'indirizzo ConnectTo  
  
4.  Impostazioni IPv6  
  
5.  Autenticazione del computer client  
  
#### <a name="to-configure-the-remote-access-server"></a>Per configurare il server di Accesso remoto  
  
1.  Nel riquadro centrale della console di gestione accesso remoto, nel **passaggio 2 Server di accesso remoto** area, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata del server di Accesso remoto, in **Topologia di rete**, scegliere la topologia di distribuzione da usare nell'organizzazione. In **Digitare il nome pubblico o l'indirizzo IPv4 usati dai client per connettersi al server di Accesso remoto** immettere il nome pubblico per la distribuzione (che corrisponde al nome soggetto del certificato IP-HTTPS, ad esempio edge1.contoso.com) e quindi fare clic su **Avanti**.  
  
3.  Nel **schede di rete** pagina, la procedura guidata rileva automaticamente:  
  
    -   Schede di rete per le reti di distribuzione. Se la procedura guidata non rileva le schede di rete corrette, selezionarle manualmente.  
  
    -   Certificato IP-HTTPS. Questo è in base al nome pubblico per la distribuzione impostate durante il passaggio precedente della procedura guidata. Se la procedura guidata non rileva il certificato IP-HTTPS corretto, fare clic su **Sfoglia** selezionare manualmente il certificato corretto.  
  
4.  Fare clic su **Avanti**.  
  
5.  Nel **Configurazione prefissi** pagina (in questa pagina è visibile solo se viene rilevato IPv6 nella rete interna), la procedura guidata rileva automaticamente le impostazioni IPv6 utilizzati nella rete interna. Se la distribuzione richiede prefissi aggiuntivi, configurare i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN.  
  
6.  Nel **autenticazione** pagina:  
  
    -   Per le distribuzioni multisito e a due fattori, è necessario usare l'autenticazione del certificato computer. Selezionare il **Usa certificati computer** casella di controllo per utilizzare l'autenticazione del certificato computer e selezionare il certificato radice IPsec.  
  
    -   Per abilitare i computer client che eseguono Windows 7 alla connessione tramite DirectAccess, selezionare il **attivare Windows 7 ai computer client di connettersi tramite DirectAccess** casella di controllo. In questo tipo di distribuzione deve essere usata anche l'autenticazione del certificato computer.  
  
7.  Scegliere **Fine**.  
  
## <a name="BKMK_Infra"></a>Configurare i server dell'infrastruttura  
Per configurare i server dell'infrastruttura in una distribuzione di accesso remoto, è necessario configurare quanto segue:  
  
-   Server dei percorsi di rete  
  
-   Elenco di ricerca suffissi impostazioni DNS, tra cui il DNS  
  
-   I server di gestione che non vengono rilevati automaticamente da accesso remoto  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Per configurare i server dell'infrastruttura  
  
1.  Nel riquadro centrale della console di gestione accesso remoto, nel **passaggio 3 server di infrastruttura** area, fare clic su **Configura**.  
  
2.  Nella configurazione guidata Server di infrastruttura, nel **Server dei percorsi di rete** pagina, fare clic sull'opzione che corrisponde al percorso del server dei percorsi di rete nella distribuzione.  
  
    -   Se il server dei percorsi di rete è in un server web remoto, immettere l'URL e quindi fare clic su **convalida** prima di continuare.  
  
    -   Se il server dei percorsi di rete nel server di accesso remoto, fare clic su **Sfoglia** per individuare il certificato rilevante e quindi fare clic su **Avanti**.  
  
3.  Nel **DNS** pagina, nella tabella, immettere i suffissi di nomi aggiuntivi che verranno applicati come esenzioni criteri tabella (Risoluzione dei nomi). Selezionare un'opzione di risoluzione del nome locale e quindi fare clic su **Avanti**.  
  
4.  Nel **elenco ricerca suffissi DNS** pagina, il server di accesso remoto rileva automaticamente i suffissi di dominio nella distribuzione. Utilizzare il **Aggiungi** e **rimuovere** pulsanti per creare l'elenco dei suffissi di dominio che si desiderano utilizzare. Per aggiungere un nuovo suffisso di dominio, in **nuovo suffisso**, immettere il suffisso e quindi fare clic su **Aggiungi**. Fare clic su **Avanti**.  
  
5.  Nel **Management** pagina, aggiungere server di gestione che non vengono rilevate automaticamente e quindi fare clic su **Avanti**. Accesso remoto aggiunge automaticamente i controller di dominio e i server System Center Configuration Manager.  
  
6.  Scegliere **Fine**.  
  
## <a name="BKMK_App"></a>Configurare i server applicazioni  
In una distribuzione completa di accesso remoto, la configurazione server applicazioni è un'attività facoltativa. In questo scenario per la gestione remota dei client DirectAccess, server applicazioni non vengono utilizzati e questo passaggio è grigio per indicare che non è attivo. Fare clic su **Fine** per applicare la configurazione.  
  
## <a name="BKMK_GPO"></a>Configurazione alternativa e riepilogo oggetti Criteri di gruppo  
Al termine, la configurazione di accesso remoto di **revisione di accesso remoto** viene visualizzato. È possibile controllare tutte le impostazioni selezionate in precedenza, incluse:  
  
-   **Impostazioni oggetto Criteri di gruppo**  
  
    Sono elencati il nome oggetto Criteri di gruppo di server DirectAccess e il nome oggetto Criteri di gruppo Client. È possibile fare clic il **Modifica** collegamento accanto al **Impostazioni oggetto Criteri di gruppo** sull'intestazione per modificare le impostazioni di GPO.  
  
-   **Client remoti**  
  
    Verrà visualizzata la configurazione del client DirectAccess, inclusi il gruppo di sicurezza, strumenti di verifica della connettività e nome della connessione DirectAccess.  
  
-   **Server di accesso remoto**  
  
    Viene visualizzata la configurazione di DirectAccess, inclusi il nome pubblico e l'indirizzo, configurazione scheda di rete e informazioni sul certificato.  
  
-   **Server di infrastruttura**  
  
    questo elenco comprende l'URL del server dei percorsi di rete, i suffissi DNS usati dal client DirectAccess e le informazioni sul server di gestione.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 3: Verificare la distribuzione](Step-3-Verify-the-Deployment_2.md)  
  
  


