---
title: Passaggio 3 configurare la distribuzione Multisita
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ac6e231ac797d1ba1e8dfb314c6aa3df99ff91d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313966"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Passaggio 3 configurare la distribuzione Multisita

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver configurato l'infrastruttura multisito seguire questi passaggi per configurare la distribuzione multisita di accesso remoto.  
  
|Attività|Descrizione|  
|----|--------|  
|3.1. Configurare i server di accesso remoto|Configurare gli indirizzi IP, in aggiunta al dominio e installando il ruolo Accesso remoto per configurare altri server di accesso remoto.|  
|3.2. Concedere l'accesso come amministratore|Concedere privilegi per gli altri server di accesso remoto all'amministratore DirectAccess.|  
|3.3. Configurare IP-HTTPS per una distribuzione multisito|Configurare il certificato IP-HTTPS utilizzato in una distribuzione multisito.|  
|3.4. Configurare il server del percorso di rete per una distribuzione multisito|Configurare il certificato server percorsi di rete utilizzato in una distribuzione multisito.|  
|3.5. Configurare i client DirectAccess per una distribuzione multisito|Rimuovere i computer client Windows 7 da gruppi di sicurezza di Windows 8.|  
|3.6. Abilitare la distribuzione multisito|Abilitare la distribuzione multisito sul primo server di accesso remoto.|  
|3,7. Aggiungere punti di ingresso alla distribuzione multisito|Aggiungere ulteriori punti di distribuzione multisito.|  
  
> [!NOTE]  
> In questo argomento sono inclusi cmdlet di Windows PowerShell di esempio che possono essere usati per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-configure-remote-access-servers"></a><a name="BKMK_ConfigServer"></a>3.1. Configurare i server di accesso remoto  

  
### <a name="to-install-the-remote-access-role"></a>Per installare il ruolo Accesso remoto  
  
1.  Assicurarsi che ogni server di accesso remoto è configurato con la topologia di distribuzione appropriata (bordo, dietro un NAT, una singola interfaccia di rete) e le route corrispondente.  
  
2.  Configurare gli indirizzi IP in ogni server di accesso remoto in base alla topologia dei siti e schema di indirizzamento IP dell'organizzazione.  
  
3.  Aggiungere ogni server di accesso remoto a un dominio di Active Directory.  
  
4.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
5.   Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
6.  Nel **Selezione ruoli Server** finestra di dialogo, selezionare **accesso remoto**, quindi fare clic su **Avanti**.  
  
7.  Fare clic su **Avanti** tre volte.  
  
8.  Nel **Selezione servizi ruolo** finestra di dialogo, selezionare **DirectAccess e VPN (RAS)** e quindi fare clic su **Aggiungi funzionalità**.  
  
9.  Selezionare **Routing**, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
10. Fare clic su **Avanti** e quindi su **Installa**.  
  
11.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione abbia avuto esito positivo e quindi fare clic su **Chiudi**.  
  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  

  
I passaggi 1-3 devono essere eseguiti manualmente e non vengono eseguiti utilizzando questo cmdlet Windows PowerShell.  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="32-grant-administrator-access"></a><a name="BKMK_Admin"></a>3.2. Concedere l'accesso come amministratore  
  
#### <a name="to-grant-administrator-permissions"></a>Per concedere le autorizzazioni di amministratore  
  
1.  Nel server di accesso remoto nel punto di ingresso aggiuntive: sul **avviare** digitare **Gestione Computer**, e quindi premere INVIO.  
  
2.  Nel riquadro a sinistra, fare clic su **utenti e gruppi locali**.  
  
3.  Fare doppio clic su **gruppi**, quindi fare doppio clic su **amministratori**.  
  
4.  Nel **proprietà amministratori** nella finestra di dialogo fare clic su **Aggiungi**, e il **Seleziona utenti, computer, account del servizio o gruppi** nella finestra di dialogo fare clic su **percorsi**.  
  
5.  Nel **percorsi** della finestra di dialogo di **percorso** struttura ad albero, fare clic sulla posizione che contiene l'account utente dell'amministratore di DirectAccess e quindi fare clic su **OK**.  
  
6.  Nel **Immettere i nomi di oggetto da selezionare**, immettere il nome utente dell'amministratore di DirectAccess e quindi fare clic su **OK** due volte.  
  
7.  Nel **proprietà amministratori** la finestra di dialogo, fare clic su **OK**.  
  
8.  Chiudere la finestra Gestione computer.  
  
9. Ripetere questa procedura su tutti i server di accesso remoto che farà parte della distribuzione multisito.  
  
## <a name="33-configure-ip-https-for-a-multisite-deployment"></a><a name="BKMK_IPHTTPS"></a>3.3. Configurare IP-HTTPS per una distribuzione multisito  
In ogni server di accesso remoto che verrà aggiunto alla distribuzione multisito, un certificato SSL è necessario verificare la connessione HTTPS al server web IP-HTTPS. L'appartenenza al gruppo **Administrators** locale, o equivalente, è il requisito minimo per completare questa procedura.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Per ottenere un certificato IP-HTTPS  
  
1.  In ogni server di accesso remoto: sul **avviare** digitare **mmc**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
3.  Fare clic su **Certificati**, su **Aggiungi**, **Account del computer** e su **Avanti**. Selezionare **Computer locale**, fare clic su **Fine** e quindi su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti**.  
  
7.  Nel **richiesta certificati** pagina, fare clic sul modello di certificato Server Web e quindi fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
    Se non viene visualizzato il modello di certificato Server Web, assicurarsi che l'account computer del server Accesso remoto dispone di autorizzazioni di registrazione per il modello di certificato Server Web. Per ulteriori informazioni, vedere [configurare le autorizzazioni per il modello di certificato Server Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Nel **soggetto** scheda il **Proprietà certificato** della finestra di dialogo **nome soggetto**, per **tipo**, selezionare **nome comune**.  
  
9. In **valore**, digitare il nome di dominio completo (FQDN) del nome Internet del server di accesso remoto (ad esempio, Europe.contoso.com) e quindi fare clic su **Aggiungi**.  
  
10. Fare clic su **OK**, su **Registrazione** e quindi su **Fine**.  
  
11. Nel riquadro dei dettagli dello snap-in Certificati verificare che un nuovo certificato con il nome di dominio completo sia registrato come **Autenticazione server** in **Scopi designati**.  
  
12. Fare clic con il pulsante destro del mouse sul certificato e scegliere **Proprietà**.  
  
13. In **Nome descrittivo** digitare **Certificato IP-HTTPS**, quindi fare clic su **OK**.  
  
    > [!TIP]  
    > I passaggi 12 e 13 sono facoltativi, ma rendono più semplice per selezionare il certificato per IP-HTTPS durante la configurazione di accesso remoto.  
  
14. Ripetere questa procedura su tutti i server di accesso remoto nella distribuzione.  
  
## <a name="34-configure-the-network-location-server-for-a-multisite-deployment"></a><a name="BKMK_NLS"></a>3,4. Configurare il server del percorso di rete per una distribuzione multisito  
Se si sceglie di configurare il server dei percorsi rete sul server di accesso remoto quando si configura il primo server, ogni nuovo server di accesso remoto che si aggiunge deve essere configurato con un certificato Server Web con lo stesso nome di oggetto che è stato selezionato per il server del percorso di rete per il primo server. Ogni server richiede un certificato per autenticare la connessione al server dei percorsi di rete e i computer client che si trova nella rete interna devono essere in grado di risolvere il nome del sito Web in DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Per installare un certificato per il percorso di rete  
  
1.  Nel server di accesso remoto: sul **avviare** digitare **mmc**, e quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
3.  Fare clic su **Certificati**, su **Aggiungi**, **Account del computer** e su **Avanti**. Selezionare **Computer locale**, fare clic su **Fine** e quindi su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
    > [!NOTE]  
    > È inoltre possibile importare lo stesso certificato utilizzato per il server del percorso di rete per il primo server di accesso remoto.  
  
6.  Fare clic due volte su **Avanti**.  
  
7.  Nel **richiesta certificati** pagina, fare clic sul modello di certificato Server Web e quindi fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
    Se non viene visualizzato il modello di certificato Server Web, assicurarsi che l'account computer del server Accesso remoto dispone di autorizzazioni di registrazione per il modello di certificato Server Web. Per ulteriori informazioni, vedere [configurare le autorizzazioni per il modello di certificato Server Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Nel **soggetto** scheda il **Proprietà certificato** della finestra di dialogo **nome soggetto**, per **tipo**, selezionare **nome comune**.  
  
9. In **valore**, digitare il nome di dominio completo (FQDN) che è stato configurato per il certificato server percorsi di rete del primo server di accesso remoto (ad esempio, NLS) e quindi fare clic su **Aggiungi**.  
  
10. Fare clic su **OK**, su **Registrazione** e quindi su **Fine**.  
  
11. Nel riquadro dei dettagli dello snap-in Certificati verificare che un nuovo certificato con il nome di dominio completo sia registrato come **Autenticazione server** in **Scopi designati**.  
  
12. Fare clic con il pulsante destro del mouse sul certificato e scegliere **Proprietà**.  
  
13. In **Nome descrittivo** digitare **Certificato del percorso di rete**, quindi fare clic su **OK**.  
  
    > [!TIP]  
    > I passaggi 12 e 13 sono facoltativi, ma rendono più semplice per selezionare il certificato per il percorso di rete quando si configura accesso remoto.  
  
14. Ripetere questa procedura su tutti i server di accesso remoto nella distribuzione.  
  
### <a name="to-create-the-network-location-server-dns-records"></a><a name="NLS"></a>Per creare i record DNS del server dei percorsi di rete  
  
1.  Nel server DNS: nel **avviare** digitare **dnsmgmt. msc**, quindi premere INVIO.  
  
2.  Nel riquadro a sinistra di **gestore DNS** console, aprire l'area di ricerca diretta per la rete interna. Fare clic con il pulsante destro zona pertinente e fare clic su **Nuovo Host (A o AAAA)** .  
  
3.  Nel **Nuovo Host** della finestra di dialogo di **nome (utilizza nome del dominio padre se vuoto)** immettere il nome utilizzato per il server del percorso di rete per il primo server di accesso remoto. Nel **indirizzo IP** casella, immettere l'indirizzo IPv4 con connessione intranet del server di accesso remoto e quindi fare clic su **Aggiungi Host**. Nella finestra di dialogo **DNS** fare clic su **OK**.  
  
4.  Nel **Nuovo Host** della finestra di dialogo di **nome (utilizza nome del dominio padre se vuoto)** immettere il nome utilizzato per il server del percorso di rete per il primo server di accesso remoto. Nel **indirizzo IP** casella, immettere l'indirizzo IPv6 con connessione intranet del server di accesso remoto e quindi fare clic su **Aggiungi Host**. Nella finestra di dialogo **DNS** fare clic su **OK**.  
  
5.  Ripetere i passaggi 3 e 4 per ogni server di accesso remoto nella distribuzione.  
  
6.  Fare clic su **Done**.  
  
7.  Ripetere questa procedura prima di aggiungere server come punti di ingresso aggiuntive nella distribuzione.  
  
## <a name="35-configure-directaccess-clients-for-a-multisite-deployment"></a><a name="BKMK_Client"></a>3,5. Configurare i client DirectAccess per una distribuzione multisito  
I computer client DirectAccess Windows devono essere membri di gruppi di sicurezza che definiscono l'associazione di DirectAccess. Prima di abilitata la funzionalità multisito, questi gruppi di sicurezza possono contenere sia i client Windows 8 e Windows 7 (se è stata selezionata la modalità "legacy" appropriato). Dopo aver abilitata la funzionalità multisito, gruppi di sicurezza client esistente, in modalità server singolo, vengono convertiti in gruppi di sicurezza per Windows 8 solo. Dopo aver abilitata la funzionalità multisito, i computer client DirectAccess Windows 7 devono essere spostati dedicato Windows 7 client gruppi di protezione (che sono associati a specifici punti di ingresso) o non sarà in grado di connettersi tramite DirectAccess. I client Windows 7, è necessario rimuovere dai gruppi di protezione esistente che ora sono gruppi di sicurezza di Windows 8. Attenzione: Windows 7 i computer client membri di gruppi di sicurezza in Windows 7 e Windows 8 client perderanno la connettività remota e i client Windows 7 senza SP1 installato perderanno la connettività dell'organizzazione. Pertanto, tutti i computer client Windows 7 devono essere rimossa dai gruppi di protezione di Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Rimuovere i client Windows 7 da gruppi di sicurezza di Windows 8  
  
1.  Nel controller di dominio primario, fare clic su **avviare**, quindi fare clic su **Active Directory Users and Computers**.  
  
2.  Per rimuovere computer dal gruppo di sicurezza, fare doppio clic sul gruppo di sicurezza e il **< nome_gruppo > proprietà** nella finestra di dialogo fare clic su di **membri** scheda.  
  
3.  Selezionare il computer client Windows 7 e fare clic su **rimuovere**.  
  
4.  Ripetere questa procedura per rimuovere i computer client Windows 7 dai gruppi di protezione di Windows 8.  
  
> [!IMPORTANT]  
> Quando si abilita una configurazione multisito di accesso remoto tutti i computer client (Windows 7 e Windows 8) perderanno la connettività remota fino a quando non sono in grado di connettersi alla rete aziendale, direttamente o tramite VPN per aggiornare i propri criteri di gruppo. Questo vale quando si abilita la funzionalità multisito per la prima volta e anche quando si disabilita multisito.  
  
## <a name="36-enable-the-multisite-deployment"></a><a name="BKMK_Enable"></a>3,6. Abilitare la distribuzione multisito  
Per configurare una distribuzione multisito, abilitare la funzionalità multisita sul server di accesso remoto esistente. Prima di abilitare multisito nella distribuzione, assicurarsi di che avere le seguenti informazioni:  
  
1.  Le impostazioni del servizio di bilanciamento carico globale e gli indirizzi IP se si desidera caricare bilanciare le connessioni client DirectAccess tutti punti di ingresso nella distribuzione.  
  
2.  La sicurezza del gruppo che contiene i computer client Windows 7 per il primo punto di ingresso nella distribuzione, se si desidera consentire ai computer client di accesso remoto per Windows 7.  
  
3.  I nomi degli oggetti Criteri di gruppo se è necessario utilizzare oggetti Criteri di gruppo di non predefiniti che vengono applicate nei computer client Windows 7 per il primo punto di ingresso nella distribuzione, se è necessario il supporto per i computer client Windows 7.  
  
### <a name="to-enable-a-multisite-configuration"></a><a name="EnabledMultisite"></a>Per abilitare una configurazione multisito  
  
1.  Sul server di accesso remoto esistente: nel **avviare** digitare **RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nella Console di gestione accesso remoto, fare clic su **configurazione**, e quindi la **attività** riquadro, fare clic su **abilitare multisito**.  
  
3.  Nel **Abilita distribuzione multisito** procedura guidata di **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **nome distribuzione** pagina **nome distribuzione multisito**, immettere un nome per la distribuzione. In **nome del punto di ingresso primo**, immettere un nome per identificare il punto di ingresso prima che il server di accesso remoto corrente e quindi fare clic su **Avanti**.  
  
5.  Nel **la selezione del punto di ingresso** pagina, effettuare una delle operazioni seguenti:  
  
    -   Fare clic su **assegnare automaticamente i punti di ingresso e consentire ai client di selezionare manualmente** per indirizzare automaticamente i computer client al punto di ingresso più adatto, consentendo anche di selezionare un punto di ingresso manualmente i computer client. Selezione del punto di inserimento manuale è disponibile solo per i computer Windows 8. Fare clic su **Avanti**.  
  
    -   Fare clic su **assegna automaticamente i punti di ingresso** automaticamente indirizzare i computer client al punto di ingresso più adatto e quindi fare clic su **Avanti**.  
  
6.  Nel **il bilanciamento del carico globale** pagina, effettuare una delle operazioni seguenti:  
  
    -   Fare clic su **No, non utilizzare il bilanciamento del carico globale** Se si desidera utilizzare un bilanciamento del carico globale e quindi fare clic su **Avanti**.  
  
        > [!NOTE]  
        > Quando si seleziona questo client opzione computer connettono automaticamente al relativo punto di ingresso più vicino.  
  
    -   Fare clic su **Sì, è possibile usare il bilanciamento del carico globale** Se si desidera caricare bilanciare il traffico a livello globale tra tutti i punti di ingresso. In **Digitare FQDN utilizzabile da tutti i punti di ingresso di bilanciamento del carico globale**, immettere il FQDN, di bilanciamento del carico globale e in **digitare l'indirizzo IP per il punto di ingresso di bilanciamento del carico globale** che contiene il primo server di accesso remoto, immettere l'indirizzo IP per il punto di ingresso di bilanciamento del carico globale e quindi fare clic su **Avanti**.  
  
7.  Nel **supporto Client** pagina, effettuare una delle operazioni seguenti:  
  
    -   Per limitare l'accesso al computer client che eseguono Windows 8 o versioni successive, fare clic su **limitare l'accesso al computer client che eseguono Windows 8 o un sistema operativo successivo**, quindi fare clic su **Avanti**.  
  
    -   Per consentire a client di computer che eseguono Windows 7 per accedere a questo punto di ingresso, fare clic su **consentono ai computer che eseguono Windows 7 per accedere a questo punto di ingresso client**, fare clic su **Aggiungi**. Nel **Seleziona gruppi** la finestra di dialogo, selezionare i gruppi di sicurezza che contiene i computer client Windows 7, fare clic su **OK**, e quindi fare clic su **Avanti**.  
  
8.  Nel **Impostazioni oggetto Criteri di gruppo Client** accettare i computer client oggetto Criteri di gruppo per Windows 7 predefinito per il punto di ingresso, digitare il nome dell'oggetto Criteri di gruppo per l'accesso remoto per creare automaticamente oppure fare clic su **Sfoglia** consente di individuare i computer client l'oggetto Criteri di gruppo per Windows 7 e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > -   Il **Impostazioni oggetto Criteri di gruppo Client** verrà visualizzata la pagina solo quando si configura il punto di ingresso per consentire ai computer di accedere al punto di ingresso di client Windows 7.  
    > -   È possibile scegliere facoltativamente **convalidare oggetti Criteri di gruppo** per assicurarsi di disporre delle autorizzazioni appropriate per i criteri di gruppo selezionato o oggetti Criteri di gruppo per il punto di ingresso. Se l'oggetto Criteri di gruppo non esiste, verrà creato automaticamente, quindi creare e collegare sono necessarie autorizzazioni. Nel caso in cui i GPO sono stati creati manualmente, quindi modificare, modificare la sicurezza e sono necessarie le autorizzazioni di eliminazione.  
  
9. Nel **riepilogo** pagina, fare clic su **Commit**.  
  
10. Nel **Abilitazione della distribuzione multisito** la finestra di dialogo, fare clic su **Chiudi** e quindi scegliere la procedura guidata Abilita distribuzione multisito, **Chiudi**.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Per attivare una distribuzione multisito denominata 'Contoso' sul primo punto di ingresso denominato "Edge1-US". La distribuzione consente ai client di selezionare manualmente il punto di ingresso e non utilizza un bilanciamento del carico globale.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Per consentire l'accesso dei computer client Windows 7 tramite il primo punto di ingresso tramite il gruppo di sicurezza DA_Clients_US e l'utilizzo di DA_W7_Clients_GPO_US l'oggetto Criteri di gruppo.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="37-add-entry-points-to-the-multisite-deployment"></a><a name="BKMK_EntryPoint"></a>3,7. Aggiungere punti di ingresso alla distribuzione multisito  
Dopo l'abilitazione della distribuzione multisito, è possibile aggiungere ulteriori punti mediante l'aggiunta guidata di un punto di ingresso. Prima di aggiungere punti di ingresso, accertarsi di avere le seguenti informazioni:  
  
-   Indirizzi IP di bilanciamento del carico globale per ogni nuova voce di scegliere se si utilizza il bilanciamento del carico globale.  
  
-   I gruppi di sicurezza contenente i computer client Windows 7 per ogni punto di ingresso che verrà aggiunto se si desidera consentire ai computer client di accesso remoto per Windows 7.  
  
-   I nomi degli oggetti Criteri di gruppo se viene richiesto di utilizzare oggetti Criteri di gruppo di non predefiniti che vengono applicate nei computer client Windows 7 per ogni punto di ingresso che verranno aggiunti, se è necessario il supporto per i computer client Windows 7.  
  
-   Nel caso in cui IPv6 è distribuito nella rete dell'organizzazione, è necessario preparare il prefisso IP-HTTPS per il nuovo punto di ingresso.  
  
### <a name="to-add-entry-points-to-your-multisite-deployment"></a><a name="AddEP"></a>Per aggiungere punti di ingresso alla distribuzione multisito  
  
1.  Sul server di accesso remoto esistente: nel **avviare** digitare **RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nella Console di gestione accesso remoto, fare clic su **configurazione**, e quindi la **attività** riquadro, fare clic su **aggiungere un punto di ingresso**.  
  
3.  Nella finestra Aggiungi un punto di ingresso di procedura guidata, nel **Dettagli dei punti di ingresso** pagina **server di accesso remoto**, immettere il nome di dominio completo (FQDN) del server da aggiungere. In **nome del punto di ingresso**, immettere il nome del punto di ingresso e quindi fare clic su **Avanti**.  
  
4.  Nel **globale impostazioni bilanciamento di carico** pagina, immettere l'indirizzo IP del punto di ingresso di bilanciamento del carico globale e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Il **globale impostazioni bilanciamento di carico** verrà visualizzata la pagina solo quando la configurazione multisito utilizza un bilanciamento del carico globale.  
  
5.  Nel **topologia di rete** pagina, scegliere la topologia corrispondente con la topologia di rete del server di accesso remoto che si desidera aggiungere e quindi fare clic su **Avanti**.  
  
6.  Nel **nome di rete o indirizzo IP** pagina **digitare il nome pubblico o l'indirizzo IP utilizzato dai client per connettersi al server di accesso remoto**, immettere il nome pubblico o l'indirizzo IP utilizzato dai client per connettersi al server di accesso remoto. Il nome pubblico corrisponde al nome soggetto del certificato IP-HTTPS. Nel caso in cui è stata implementata la topologia della rete perimetrale, l'indirizzo IP è quello della scheda esterna del server di accesso remoto. Fare clic su **Avanti**.  
  
7.  Nel **schede di rete** pagina, eseguire le operazioni seguenti:  
  
    -   Se si distribuisce una topologia con due schede di rete **adapter esterno**, selezionare la scheda connessa alla rete esterna. In **scheda interna**, selezionare la scheda connessa alla rete interna.  
  
    -   Se si distribuisce una topologia con una scheda di rete, in **scheda di rete**, selezionare la scheda connessa alla rete interna.  
  
8.  Nel **schede di rete** pagina **Selezionare il certificato utilizzato per autenticare le connessioni IP-HTTPS**, fare clic su **Sfoglia** per individuare e selezionare il certificato IP-HTTPS. Fare clic su **Avanti**.  
  
9. Se IPv6 è configurato nella rete aziendale, nel **Configurazione prefissi** pagina **prefisso IPv6 assegnato ai computer client**, immettere un prefisso IP-HTTPS per assegnare indirizzi IPv6 ai computer client DirectAccess e fare clic su **Avanti**.  
  
10. Nel **supporto Client** pagina, effettuare una delle operazioni seguenti:  
  
    -   Per limitare l'accesso al computer client che eseguono Windows 8 o versioni successive, fare clic su **limitare l'accesso al computer client che eseguono Windows 8 o un sistema operativo successivo**, quindi fare clic su **Avanti**.  
  
    -   Per consentire a client di computer che eseguono Windows 7 per accedere a questo punto di ingresso, fare clic su **consentono ai computer che eseguono Windows 7 per accedere a questo punto di ingresso client**, fare clic su **Aggiungi**. Nel **Seleziona gruppi** finestra di dialogo, selezionare i gruppi di sicurezza che contengono i computer client Windows 7 che consentiranno di connettersi a questo punto di ingresso, fare clic su **OK**, fare clic su **Avanti**.  
  
11. Nel **Impostazioni oggetto Criteri di gruppo Client** accettare i computer client oggetto Criteri di gruppo per Windows 7 predefinito per il punto di ingresso, digitare il nome dell'oggetto Criteri di gruppo che si desidera l'accesso remoto per creare automaticamente oppure fare clic su **Sfoglia** consente di individuare i computer client l'oggetto Criteri di gruppo per Windows 7 e fare clic su **Avanti**.  
  
    > [!NOTE]  
    > -   Il **Impostazioni oggetto Criteri di gruppo Client** verrà visualizzata la pagina solo quando si configura il punto di ingresso per consentire ai computer di accedere al punto di ingresso di client Windows 7.  
    > -   È possibile scegliere facoltativamente **convalidare oggetti Criteri di gruppo** per assicurarsi di disporre delle autorizzazioni appropriate per i criteri di gruppo selezionato o oggetti Criteri di gruppo per il punto di ingresso. Se l'oggetto Criteri di gruppo non esiste, verrà creato automaticamente, quindi creare e collegare sono necessarie autorizzazioni. Nel caso in cui i GPO sono stati creati manualmente, quindi modificare, modificare la sicurezza e sono necessarie le autorizzazioni di eliminazione.  
  
12. Nel **Impostazioni oggetto Criteri di gruppo di Server** pagina accettare l'oggetto Criteri di gruppo predefinito per il server di accesso remoto, digitare il nome dell'oggetto Criteri di gruppo che si desidera l'accesso remoto per creare automaticamente oppure fare clic su **Sfoglia** consente di individuare l'oggetto Criteri di gruppo per il server e quindi fare clic su **Avanti**.  
  
13. Nel **Server dei percorsi di rete** pagina, fare clic su **Sfoglia** per selezionare il certificato per il sito Web server percorsi di rete in esecuzione sul server di accesso remoto e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Il **Server dei percorsi di rete** verrà visualizzata la pagina solo quando il server dei percorsi rete è in esecuzione sul server di accesso remoto.  
  
14. Nel **riepilogo** pagina, esaminare le impostazioni del punto di ingresso e quindi fare clic su **Commit**.  
  
15. Nel **aggiunta punto di ingresso** la finestra di dialogo, fare clic su **Chiudi** e fare clic su Aggiungi una creazione guidata punto di ingresso, **Chiudi**.  
  
    > [!NOTE]  
    > Se il punto di ingresso aggiunto si trova in una foresta diversa rispetto ai punti di ingresso o ai computer client esistenti, è necessario fare clic su **Aggiorna server di gestione** nel riquadro **attività** per individuare i controller di dominio e Configuration Manager nella nuova foresta.  
  
16. Ripetere questa procedura dal passaggio 2 per ogni punto di ingresso che si desidera aggiungere alla distribuzione multisita.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Per aggiungere edge2 il computer dal dominio corp2 come secondo punto di ingresso denominato Edge2 Europa. La configurazione del punto di ingresso è: un prefisso IPv6 client ' 2001:db8:2:2000::/ / 64', una connessione a 'edge2.contoso.com' indirizzo (il certificato IP-HTTPS nel computer edge2), un GPO server denominato "DirectAccess Server impostazioni – Edge2-Europe" e le interfacce interne ed esterne denominate rispettivamente Internet e Corpnet2:  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Per consentire l'accesso dei computer client Windows 7 tramite il secondo punto di ingresso tramite il gruppo di sicurezza DA_Clients_Europe e l'utilizzo di DA_W7_Clients_GPO_Europe l'oggetto Criteri di gruppo.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 2: configurare l'infrastruttura multisito](Step-2-Configure-the-Multisite-Infrastructure.md)
