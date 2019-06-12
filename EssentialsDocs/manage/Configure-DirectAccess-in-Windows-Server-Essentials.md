---
title: Configurare DirectAccess in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2081473500a08776a1dc81a4fa443696b6fde0d6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433460"
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurare DirectAccess in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questo argomento vengono fornite istruzioni dettagliate per la configurazione di DirectAccess in Windows Server Essentials per consentire alla forza lavoro mobile di connettersi alla rete dell'organizzazione da qualsiasi posizione remota dotata di Internet senza stabilire una connessione di rete privata virtuale (VPN). DirectAccess offre agli utenti mobili la stessa esperienza di connettività in sede e fuori sede dai computer Windows 8.1, Windows 8 e Windows 7.  
  
 In Windows Server Essentials, se il dominio contiene più di un server di Windows Server Essentials, DirectAccess deve essere configurato nel controller di dominio.  
  
> [!NOTE]
>  Questo argomento fornisce istruzioni per la configurazione di DirectAccess quando il server di Windows Server Essentials è il controller di dominio. Se il server di Windows Server Essentials è un membro del dominio, seguire le istruzioni per configurare DirectAccess in un membro del dominio in [aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN)](https://technet.microsoft.com/library/jj574220.aspx) invece.  
  
## <a name="process-overview"></a>Panoramica del processo  
 Per configurare DirectAccess in Windows Server Essentials, completare i passaggi seguenti.  
  
> [!IMPORTANT]
>  Prima di usare le procedure descritte in questa guida per configurare DirectAccess in Windows Server Essentials, è necessario abilitare la VPN sul server. Per istruzioni, vedere [gestire la VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Passaggio 1: Aggiungere strumenti di gestione accesso remoto al server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Passaggio 2: Modificare l'indirizzo della scheda di rete del server in un indirizzo IP statico](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Passaggio 3a: Concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web](#BKMK_GrantFullPermissions)  
  
    -   [Passaggio 3b: Registrare un certificato per il server dei percorsi rete con un nome comune non risolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
    -   [Passaggio 3c: Aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Passaggio 4: Creare un gruppo di sicurezza per i computer client DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Passaggio 5: Abilitare e configurare DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Passaggio 5a: Abilitare DirectAccess mediante la Console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Passaggio 5b: Rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Passaggio 5c: Abilita computer client che eseguono Windows 7 Enterprise per l'utilizzo di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Passaggio 5D: Configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Passaggio 5e: Aggiungere la chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Passaggio 6: Configurare le impostazioni di tabella criteri risoluzione nomi per il server DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Passaggio 7: Configurare le regole del firewall TCP e UDP per il server DirectAccess oggetti Criteri di gruppo](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Passaggio 8: Modificare la configurazione DNS64 per l'ascolto per l'interfaccia IP-HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Passaggio 9: Riservare le porte per il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Passaggio 10: Riavviare il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Appendice: Installare DirectAccess tramite Windows PowerShell](#BKMK_AppendixBPowerShellScript) fornisce uno script di Windows PowerShell che è possibile usare per eseguire la configurazione di DirectAccess.  
  
##  <a name="BKMK_AddRAM"></a> Passaggio 1: Aggiungere Strumenti di Gestione Accesso remoto al server  
  
#### <a name="to-add-remote-access-management-tools"></a>Per aggiungere Strumenti di Gestione Accesso remoto  
  
1.  Sul server, nell'angolo inferiore sinistro della pagina iniziale fare clic sull'icona **Server Manager**.  
  
     In Windows Server Essentials, è necessario cercare Server Manager per aprirlo. Nella pagina iniziale digitare **Server Manager**e quindi fare clic su **Server Manager** nei risultati della ricerca. Per aggiungere Server Manager alla pagina iniziale, fare clic con il pulsante destro del mouse su Server Manager nei risultati della ricerca e scegliere **Aggiungi a Start**.  
  
2.  Se viene visualizzato un messaggio di avviso relativo al **Controllo dell'account utente** , fare clic su **Sì**.  
  
3.  Nel dashboard di Server Manager fare clic su **Gestione** e quindi su **Aggiungi ruoli e funzionalità**.  
  
4.  In Aggiunta guidata ruoli e funzionalità eseguire le operazioni seguenti:  
  
    1.  Nella pagina **Tipo di installazione** fare clic su **Installazione basata su ruoli o basata su funzionalità**.  
  
    2.  Nel **pagina di selezione del Server** (o il **Selezione server di destinazione** pagina in Windows Server Essentials), fare clic su **selezionare un server dal pool di server**.  
  
    3.  Nella pagina **Funzionalità** espandere **Strumenti di amministrazione remota del server (installati)** , espandere **Strumenti di Gestione Accesso remoto (installati)** , espandere **Strumenti di amministrazione ruoli (installati)** , espandere **Strumenti di Gestione Accesso remoto** e quindi selezionare **Interfaccia utente grafica e strumenti da riga di comando di Accesso remoto**.  
  
    4.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_AddStaticIP"></a> Passaggio 2: Impostare l'indirizzo della scheda di rete del server su un indirizzo IP statico  
 DirectAccess richiede una scheda con un indirizzo IP statico. È quindi necessario modificare l'indirizzo IP per la scheda di rete locale del server.  
  
#### <a name="to-add-a-static-ip-address"></a>Per aggiungere un indirizzo IP statico  
  
1.  Nella pagina iniziale aprire il **Pannello di controllo**.  
  
2.  Fare clic su **Rete e Internet**e quindi su **Visualizza attività e stato della rete**.  
  
3.  Nel riquadro attività di **Centro connessioni di rete e condivisione**fare clic su **Modifica impostazioni scheda**.  
  
4.  Fare clic con il pulsante destro del mouse sulla scheda di rete locale e quindi scegliere **Proprietà**.  
  
5.  Nella scheda **Rete** fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
6.  Nella scheda **Generale** fare clic su **Utilizza il seguente indirizzo IP**e quindi digitare l'indirizzo IP che si desidera utilizzare.  
  
     Nella casella **Subnet mask** viene automaticamente visualizzato un valore predefinito per la subnet mask. Accettare il valore predefinito oppure digitare il valore della subnet mask che si desidera utilizzare.  
  
7.  Nella casella **Gateway predefinito** digitare l'indirizzo IP del gateway predefinito.  
  
8.  Nella casella **Server DNS preferito** digitare l'indirizzo IP del server DNS in uso.  
  
    > [!NOTE]
    >  Usare l'indirizzo IP assegnato alla scheda di rete dal protocollo DHCP (ad esempio 192.168.X.X) invece di quello di una rete loopback (ad esempio 127.0.0.1). Per trovare l'indirizzo IP assegnato, eseguire **ipconfig** al prompt dei comandi.  
  
9. Nella casella **Server DNS alternativo** digitare l'indirizzo IP del server DNS alternativo, se disponibile.  
  
10. Fare clic su **OK** e quindi fare clic su **Chiudi**.  
  
> [!IMPORTANT]
>  Accertarsi di configurare il router per l'inoltro delle porte 80 e 443 al nuovo indirizzo IP statico del server.  
  
##  <a name="BKMK_DNS"></a> Passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi di rete  
 Per preparare un certificato e il record DNS per il server dei percorsi di rete, eseguire le attività seguenti:  
  
-   [Passaggio 3a: Concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web](#BKMK_GrantFullPermissions)  
  
-   [Passaggio 3b: Registrare un certificato per il server dei percorsi rete con un nome comune non risolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
-   [Passaggio 3c: Aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a> Passaggio 3a: Concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web  
 La prima attività consiste nella concessione delle autorizzazioni complete per autenticare gli utenti per il modello di certificato del server Web nell'autorità di certificazione.  
  
####  <a name="BKMK_ToGrantFullPermissions"></a> Per concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web  
  
1.  Nella pagina **iniziale** aprire **Autorità di certificazione**.  
  
2.  Nell'albero della console, sotto **autorità di certificazione (locale)** , espandere **< nomeserver\>-CA**, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestire**.  
  
3.  In **Autorità di certificazione (locale)** fare clic con il pulsante destro del mouse su **Server Web**e quindi scegliere **Proprietà.**  
  
4.  Nelle proprietà del server Web, nella scheda **Sicurezza** fare clic su **Utenti autenticati**, selezionare **Controllo completo**e quindi fare clic su **OK**.  
  
5.  Riavviare **Servizi certificati Active Directory**. Nel Panello di controllo aprire **Visualizza servizi locali**. Nell'elenco di servizi fare clic con il pulsante destro del mouse su **Servizi certificati Active Directory**e quindi scegliere **Riavvia**.  
  
###  <a name="BKMK_EnrollaCertificate"></a> Passaggio 3b: Registrare un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna  
 Registrare quindi un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna.  
  
####  <a name="BKMK_ToEnrollaCertificate"></a> Per registrare un certificato per il server dei percorsi di rete  
  
1.  Nella pagina **iniziale** aprire **mmc** (Microsoft Management Console).  
  
2.  Se viene visualizzato un messaggio di avviso relativo al **Controllo dell'account utente** , fare clic su **Sì**.  
  
     Verrà aperto Microsoft Management Console (MMC).  
  
3.  Nel menu **File** fare clic su **Aggiungi/Rimuovi snap-in**.  
  
4.  Nella casella **Aggiungi o rimuovi snap-in** fare clic su **Certificati**e quindi fare clic su **Aggiungi**.  
  
5.  Nella pagina **Snap-in certificati** fare clic su **Account computer**, quindi su **Avanti**.  
  
6.  Nella pagina **Selezione computer** fare clic su **Computer locale**, fare clic su **Fine**e quindi su **OK**.  
  
7.  Nell'albero della console espandere **Certificati (computer locale)** , espandere **Personale**, fare clic con il pulsante destro del mouse su **Certificati**e quindi in **Tutte le attività**scegliere **Richiedi nuovo certificato**.  
  
8.  Quando viene visualizzata la procedura guidata Registrazione certificato, fare clic su **Avanti**.  
  
9. Nella pagina **Seleziona criteri di registrazione certificato** fare clic su **Avanti**.  
  
10. Nella pagina **Richiedi certificato** selezionare la casella di controllo **Server Web** e quindi fare clic su **Sono necessarie ulteriori informazioni per registrare il certificato**.  
  
11. Nella casella **Proprietà certificato** immettere le impostazioni seguenti per **Nome soggetto**:  
  
    1.  In **Tipo** selezionare **Nome comune**.  
  
    2.  In **Valore**digitare il nome del server dei percorsi di rete, ad esempio, DirectAccess-NLS.contoso.local, quindi fare clic su **Aggiungi**.  
  
    3.  Fare clic su **OK** e quindi fare clic su **Registrazione**.  
  
12. Una volta completata la registrazione certificato, fare clic su **Fine**.  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a> Passaggio 3c: Aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials  
 Per completare la configurazione del DNS, aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials.  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a> Eseguire il mapping di un nuovo host all'indirizzo del server Windows Server Essentials  
  
1.  Nella pagina iniziale aprire Gestore DNS. Per aprire Gestore DNS, cercare **dnsmgmt.msc**e quindi fare clic su **dnsmgmt.msc** nei risultati.  
  
2.  Nell'albero della console Gestore DNS, espandere il server locale, espandere **zone di ricerca diretta**, fare clic sulla zona con il suffisso del dominio del server e quindi fare clic su **nuovo Host (A o AAAA)** .  
  
3.  Digitare il nome e l'indirizzo IP del server, ad esempio DirectAccess-NLS.contoso.local e l'indirizzo del server corrispondente, ad esempio 192.168.x.x.  
  
4.  Fare clic su **Aggiungi host**, fare clic su **OK** e quindi fare clic su **Chiudi**.  
  
##  <a name="BKMK_AddSecurityGroup"></a> Passaggio 4: Creare un gruppo di sicurezza per i computer client DirectAccess  
 Creare poi un gruppo di sicurezza da usare per i computer client DirectAccess e quindi aggiungere gli account computer al gruppo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Per aggiungere un gruppo di sicurezza per i computer client che usano DirectAccess  
  
1. Nel dashboard di Server Manager fare clic su **Strumenti** e quindi fare clic su **Utenti e computer di Active Directory**.  
  
   > [!NOTE]
   >  Se **Utenti e computer di Active Directory** non è visibile nel menu **Strumenti**, è necessario installare la funzionalità. Per installare utenti e gruppi di Active Directory, eseguire il cmdlet di Windows PowerShell seguente come amministratore: `Install-WindowsFeature RSAT-ADDS-Tools`. Per altre informazioni, vedere [Installare o rimuovere il pacchetto Strumenti di amministrazione remota del server](https://technet.microsoft.com/library/cc730825.aspx).  
  
2. Nell'albero della console espandere il server, fare clic con il pulsante destro del mouse su **Utenti**, scegliere **Nuovo**e quindi **Gruppo**.  
  
3. Immettere il nome del gruppo, l'ambito del gruppo e il tipo di gruppo (creare un gruppo di sicurezza) e quindi fare clic su **OK**.  
  
   Il nuovo gruppo di sicurezza viene aggiunto alla cartella **Utenti**.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Per aggiungere gli account computer al gruppo di sicurezza  
  
1.  Nel dashboard di Server Manager fare clic su **Strumenti** e quindi fare clic su **Utenti e computer di Active Directory**.  
  
2.  Nell'albero della console espandere il server e quindi fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente e di gruppi di sicurezza sul server fare clic con il pulsante destro del mouse sul gruppo di sicurezza creato per DirectAccess e quindi scegliere **Proprietà**.  
  
4.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
5.  Nella finestra di dialogo digitare i nomi degli account computer da aggiungere al gruppo, separandoli con un punto e virgola (;). Quindi fare clic su **Controlla nomi**.  
  
6.  Una volta convalidati gli account computer, fare clic su **OK**. Fare quindi ancora clic su **OK**.  
  
> [!NOTE]
>  È anche possibile usare la scheda **Membro di** nelle proprietà dell'account computer per aggiungere l'account al gruppo di sicurezza.  
  
##  <a name="BKMK_EnableConfigureDA"></a> Passaggio 5: Abilitare e configurare DirectAccess  
 Per abilitare e configurare DirectAccess in Windows Server Essentials, è necessario completare i passaggi seguenti:  
  
-   [Passaggio 5a: Abilitare DirectAccess mediante la Console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Passaggio 5b: Rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Passaggio 5c: Abilita computer client che eseguono Windows 7 Enterprise per l'utilizzo di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Passaggio 5D: Configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Passaggio 5e: Aggiungere la chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a> Passaggio 5a: Abilitare DirectAccess con la Console di gestione Accesso remoto  
 In questa sezione vengono fornite istruzioni dettagliate per abilitare DirectAccess in Windows Server Essentials. Se la VPN sul server non è ancora stata configurata, è consigliabile farlo prima di iniziare questa procedura. Per istruzioni, vedere [gestire la VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Per abilitare DirectAccess con la Console di gestione Accesso remoto  
  
1.  Nella pagina iniziale aprire **Gestione accesso remoto**.  
  
2.  Nell'Abilitazione guidata DirectAccess eseguire le operazioni seguenti:  
  
    1.  Rivedere **Prerequisiti DirectAccess** e fare clic su **Avanti**.  
  
    2.  Nella scheda **Seleziona gruppi** aggiungere il gruppo di sicurezza creato in precedenza per i client DirectAccess. (Se non è stato creato il gruppo di sicurezza, vedere [passaggio 4: Crea un gruppo di sicurezza per client DirectAccess a computer](#BKMK_AddSecurityGroup) per le istruzioni.)  
  
    3.  Sulla scheda **Seleziona gruppi** fare clic su **Abilita DirectAccess solo per computer mobili** se si desidera abilitare i computer mobili per l'utilizzo di DirectAccess per accedere in modalità remota al server, quindi fare clic su **Avanti**.  
  
    4.  In **Topologia di rete**selezionare la topologia del server e quindi fare clic su **Avanti**.  
  
    5.  In **Elenco di ricerca suffissi DNS**aggiungere il suffisso DNS per i computer client, se necessario, e quindi fare clic su **Avanti**.  
  
        > [!NOTE]
        >  Per impostazione predefinita, l'abilitazione guidata di DirectAccess aggiunge il suffisso DNS per il dominio corrente. È tuttavia possibile aggiungerne altri, se necessario.  
  
    6.  Verificare gli oggetti Criteri di gruppo che verranno applicati e, se necessario, modificarli.  
  
    7.  Fare clic su **Avanti**e quindi su **Fine**.  
  
    8.  Riavviare il servizio Gestione accesso remoto eseguendo il comando Windows PowerShell seguente con un account con privilegi elevati:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a> Passaggio 5b: Rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)  
  In questa sezione si applica a un server che esegue Windows Server Essentials.  
  
 Aprire Windows PowerShell come amministratore ed eseguire i comandi seguenti:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a> Passaggio 5c: Abilita computer client che eseguono Windows 7 Enterprise per l'utilizzo di DirectAccess  
 Se sono presenti computer client che eseguono Windows 7 Enterprise, completare la procedura seguente per abilitare DirectAccess da tali computer.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Per consentire ai computer Windows 7 Enterprise per l'utilizzo di DirectAccess  
  
1.  Nella pagina iniziale del server, aprire **Gestione accesso remoto**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**. Quindi nel riquadro **Dettagli installazione**, sotto **Passaggio 2** fare clic su **Modifica**.  
  
     Si apre la procedura guidata Installazione del server di Accesso remoto.  
  
3.  Nel **autenticazione** scheda, scegliere il certificato di autorità (CA) certificazione che sarà il certificato radice attendibile (è possibile scegliere il certificato della CA del server di Windows Server Essentials). Fare clic su **Abilita computer client Windows 7 alla connessione tramite DirectAccess** e quindi fare clic su **Avanti**.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!IMPORTANT]
>  Si verifica un problema noto per computer Windows 7 Enterprise che si connettono tramite DirectAccess se il server di Windows Server Essentials non proviene preinstallato ur1. Per abilitare le connessioni DirectAccess in tale ambiente, è necessario eseguire questi passaggi aggiuntivi:  
> 
> 1. Installare l'hotfix descritto nel [articolo 2796394 della Microsoft Knowledge Base (KB)](https://support.microsoft.com/kb/2796394) nel server di Windows Server Essentials. Quindi riavviare il server.  
>    2. Quindi installare l'hotfix descritto nel [articolo 2615847 della Microsoft Knowledge Base (KB)](https://support.microsoft.com/kb/2615847) in ogni computer Windows 7.  
> 
>    Questo problema è stato risolto in Windows Server Essentials.  
  
###  <a name="BKMK_NLS"></a> Passaggio 5D: Configurare il server dei percorsi di rete  
 In questa sezione vengono illustrate le istruzioni dettagliate per configurare le impostazioni del server dei percorsi di rete.  
  
> [!NOTE]
>  Prima di iniziare, copiare il contenuto del < unitàsistema\>cartella \inetpub\wwwroot il < unitàsistema\>\Programmi\Windows server\bin\webapps\site\insideoutside. Copiare anche il file default. aspx dal < unitàsistema\>\Programmi\Windows server\bin\webapps\site alla cartella di < unitàsistema\>\Programmi\Windows server\bin\webapps\site\insideoutside.  
  
##### <a name="to-configure-the-network-location-server"></a>Per configurare il server dei percorsi di rete  
  
1.  Nella pagina iniziale aprire **Gestione accesso remoto**.  
  
2.  Nella console di Gestione Accesso remoto fare clic su **Configurazione**, quindi, nel riquadro dei dettagli **Configurazione Accesso remoto** fare clic su **Modifica** in **Passaggio 3**.  
  
3.  Nel Server di configurazione guidata accesso remoto, nella **Server dei percorsi di rete** scheda, seleziona **il server dei percorsi di rete viene distribuito nel server di accesso remoto**e quindi selezionare il certificato che è stato emesso in precedenza (in [passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Seguire le istruzioni per completare la procedura guidata e quindi fare clic su **Fine**.  
  
###  <a name="BKMK_CA"></a> Passaggio 5e: Aggiungere la chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce il canale IPsec  
 Il passaggio successivo è la configurazione del server per ignorare la certificazione CA quando viene stabilito un canale IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Per aggiungere una chiave del Registro di sistema per ignorare la certificazione CA  
  
1.  Nella pagina iniziale aprire **regedit** (l'editor del Registro di sistema).  
  
2.  Nell'editor del Registro di sistema espandere **HKEY_LOCAL_MACHINE**, espandere **System**, espandere **CurrentControlSet**, espandere **Services** ed espandere **IKEEXT**.  
  
3.  Sotto **IKEEXT** fare clic con il pulsante destro del mouse su **Parameters**, scegliere **Nuovo** e quindi fare clic su **Valore DWORD (32 bit)** .  
  
4.  Rinominare il valore appena aggiunto in **ikeflags**.  
  
5.  Fare doppio clic su **ikeflags**, impostare **Tipo** su **Esadecimale**, impostare il valore su **8000**e quindi fare clic su **OK**.  
  
> [!NOTE]
>  È possibile usare il comando Windows PowerShell seguente con un account con privilegi elevati per aggiungere questa chiave del Registro di sistema:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a> Passaggio 6: Configurare le impostazioni della tabella dei criteri di risoluzione dei nomi per il server DirectAccess  
 In questa sezione vengono fornite le istruzioni per la modifica delle voci della tabella dei criteri di risoluzione dei nomi per gli indirizzi interni (ad esempio le voci con un suffisso contoso.local) per gli oggetti Criteri di gruppo del client DirectAccess e quindi impostare l'indirizzo dell'interfaccia IPHTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Per configurare le voci della tabella dei criteri di risoluzione dei nomi  
  
1.  Nella pagina iniziale aprire **Gestione Criteri di gruppo**.  
  
2.  Nella console Gestione Criteri di gruppo fare clic sulla foresta e sul dominio predefiniti, fare clic con il pulsante destro del mouse su **Impostazioni client DirectAccess** e quindi scegliere **Modifica**.  
  
3.  Fare clic su **Configurazioni computer**, su **Criteri**, su **Impostazioni di Windows** e quindi su **Criteri risoluzione nomi**. Scegliere la voce con lo spazio dei nomi identico al suffisso DNS in uso e quindi fare clic su **Modifica regola**.  
  
4.  Fare clic sulla scheda **Impostazioni DNS per DirectAccess**, quindi fare clic su **Abilita le impostazioni DNS per DirectAccess in questa regola**. Aggiungere l'indirizzo IPv6 per l'interfaccia IP-HTTPS nell'elenco del server DNS.  
  
    > [!NOTE]
    >  Per ottenere l'indirizzo IPv6, è possibile usare il comando Windows PowerShell seguente:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a> Passaggio 7: Configurare le regole del firewall TCP e UDP per gli oggetti Criteri di gruppo del server DirectAccess  
 In questa sezione vengono fornite le istruzioni dettagliate per configurare le regole del firewall TCP e UDP per gli oggetti Criteri di gruppo del server DirectAccess  
  
#### <a name="to-configure-firewall-rules"></a>Per configurare le regole del firewall  
  
1.  Nella pagina iniziale aprire **Gestione Criteri di gruppo**.  
  
2.  Nella console Gestione Criteri di gruppo fare clic sulla foresta e sul dominio predefiniti, fare clic con il pulsante destro del mouse su **Impostazioni server DirectAccess**e quindi scegliere **Modifica**.  
  
3.  Fare clic su **Configurazioni computer**, su **Criteri**, su **Impostazioni di Windows**, su **Impostazioni di sicurezza**, su **Windows Firewall con sicurezza avanzata**, su **Windows Firewall con sicurezza avanzata** nel livello successivo e quindi su **Regole connessioni in entrata**. Fare clic con il pulsante destro del mouse su **DNS (Domain Name Server) (TCP-In)** e quindi scegliere **Proprietà**.  
  
4.  Fare clic sulla scheda **Ambito** e quindi aggiungere l'indirizzo IPv6 dell'interfaccia IP-HTTPS all'elenco **Indirizzo IP locale** .  
  
5.  Ripetere la stessa procedura per **DNS (Domain Name Server) (UDP-In)** .  
  
##  <a name="BKMK_DNS64"></a> Passaggio 8: Modificare la configurazione DNS64 per l'ascolto dell'interfaccia IP-HTTPS  
 È necessario modificare la configurazione DNS64 per l'ascolto dell'interfaccia IP-HTTPS mediante il comando di Windows PowerShell seguente:  
  
```powershell  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a> Passaggio 9: Riservare le porte per il servizio WinNat  
 Usare il comando Windows PowerShell seguente per riservare le porte per il servizio WinNat. Sostituire "192.168.1.100" con l'indirizzo IPv4 effettivo del server di Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Per evitare conflitti di porte con le applicazioni, assicurarsi che l'intervallo di porte riservato per il servizio WinNat non includa la porta 6602.  
  
##  <a name="BKMK_WinNAT"></a> Passaggio 10: Riavviare il servizio WinNat  
 Riavviare il servizio Driver NAT Windows (WinNat) eseguendo il comando Windows PowerShell seguente.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a> Appendice: Installare DirectAccess tramite Windows PowerShell  
 In questa sezione viene descritto come installare e configurare DirectAccess tramite Windows PowerShell.  
  
### <a name="preparation"></a>Preparazione  
 Prima di iniziare a configurare il server per DirectAccess, è necessario:  
  
1.  Attenersi alla procedura di [passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) per registrare un certificato denominato **DirectAccess-NLS.contoso.com** (dove **contoso.com** viene sostituito dall'effettivo nome di dominio interno) e per aggiungere un record DNS per il server dei percorsi di rete (NLS).  
  
2.  Aggiungere un gruppo di sicurezza denominato **DirectAccessClients** in Active Directory, e quindi aggiungere i computer client per i quali si desidera abilitare la funzionalità DirectAccess. Per altre informazioni, vedere [passaggio 4: Crea un gruppo di sicurezza per client DirectAccess a computer](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Comandi  
  
> [!IMPORTANT]
>  In Windows Server Essentials, è necessario non rimuovere l'oggetto Criteri di gruppo del prefisso IPv6 non necessario. Eliminare la sezione di codice con l'etichetta seguente: `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer -IPSecRootCertificate $rootcert[0]  
Set -DAClient -OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
