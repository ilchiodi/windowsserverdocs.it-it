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
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurare DirectAccess in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questo argomento include istruzioni dettagliate per la configurazione di DirectAccess in Windows Server Essentials per consentire alla forza lavoro mobile di connettersi alla rete s dell'organizzazione da qualsiasi posizione remota dotata di connessione Internet senza stabilire una connessione di rete privata virtuale (VPN). DirectAccess offre agli utenti mobili lo stesso livello di connettività dentro e fuori sede dai computer Windows 8.1, Windows 8 e Windows 7.  
  
 In Windows Server Essentials, se il dominio contiene più di un server Windows Server Essentials, DirectAccess deve essere configurato nel controller di dominio.  
  
> [!NOTE]
>  In questo argomento vengono fornite istruzioni per la configurazione di DirectAccess quando il server Windows Server Essentials è il controller di dominio. Se il server Windows Server Essentials è un membro del dominio, seguire le istruzioni per configurare DirectAccess in un membro del dominio in [aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN)](https://technet.microsoft.com/library/jj574220.aspx) invece.  
  
## <a name="process-overview"></a>Panoramica del processo  
 Per configurare DirectAccess in Windows Server Essentials, completare i passaggi seguenti.  
  
> [!IMPORTANT]
>  Prima di utilizzare le procedure in questa guida per configurare DirectAccess in Windows Server Essentials, è necessario abilitare la VPN sul server. Per istruzioni, vedere [gestire la VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Passaggio 1: Aggiungere strumenti di gestione accesso remoto al server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Passaggio 2: Modificare l'indirizzo della scheda di rete del server in un indirizzo IP statico](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato server s Web](#BKMK_GrantFullPermissions)  
  
    -   [Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
    -   [Passaggio 3c: aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Passaggio 4: Creare un gruppo di sicurezza per i computer client DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Passaggio 5: Abilitare e configurare DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Passaggio 5a: abilitare DirectAccess mediante la Console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Passaggio 5D: configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Passaggio 5e: aggiungere una chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Passaggio 6: Configurare le impostazioni di tabella criteri risoluzione nomi per il server DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Passaggio 7: Configurare regole del firewall TCP e UDP per il server DirectAccess oggetti Criteri di gruppo](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Passaggio 8: Modificare la configurazione DNS64 per l'ascolto per l'interfaccia IP-HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Passaggio 9: Riservare le porte per il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Passaggio 10: Riavviare il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Appendice: Installare DirectAccess tramite Windows PowerShell](#BKMK_AppendixBPowerShellScript) fornisce uno script di Windows PowerShell che è possibile utilizzare per eseguire la configurazione di DirectAccess.  
  
##  <a name="BKMK_AddRAM"></a>Passaggio 1: Aggiungere strumenti di gestione accesso remoto al server  
  
#### <a name="to-add-remote-access-management-tools"></a>Per aggiungere strumenti di gestione accesso remoto  
  
1.  Nel server, nell'angolo inferiore sinistro della pagina iniziale, scegliere il **Server Manager** icona.  
  
     In Windows Server Essentials, sarà necessario cercare Server Manager per aprirlo. Nella pagina iniziale, digitare **Server Manager**, quindi fare clic su **Server Manager** nei risultati della ricerca. Per aggiungere Server Manager per la pagina iniziale, fare doppio clic su Server Manager nei risultati della ricerca e fare clic su **Aggiungi a Start**.  
  
2.  Se un **controllo dell'Account utente** viene visualizzato il messaggio di avviso, fare clic su **Sì**.  
  
3.  Nel Dashboard di Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**.  
  
4.  Nell'Aggiunta guidata ruoli e funzionalità, eseguire le operazioni seguenti:  
  
    1.  Nel **tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**.  
  
    2.  Nel **pagina Selezione Server** (o **server di destinazione** pagina in Windows Server Essentials), fare clic su **selezionare un server dal pool di server**.  
  
    3.  Nel **funzionalità** espandere **strumenti di amministrazione remota Server (installati)**, espandere **strumenti di gestione accesso remoto (installati)**, espandere **strumenti di amministrazione ruoli (installati)**, espandere **strumenti di gestione accesso remoto**e quindi seleziona **dell'interfaccia utente di accesso remoto e strumenti da riga di comando**.  
  
    4.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_AddStaticIP"></a>Passaggio 2: Modificare l'indirizzo della scheda di rete del server in un indirizzo IP statico  
 DirectAccess richiede una scheda con un indirizzo IP statico. È necessario modificare l'indirizzo IP della scheda di rete locale sul server.  
  
#### <a name="to-add-a-static-ip-address"></a>Per aggiungere un indirizzo IP statico  
  
1.  Nella pagina iniziale, aprire **Pannello di controllo**.  
  
2.  Fare clic su **rete e Internet**, quindi fare clic su **visualizzare lo stato della rete e attività**.  
  
3.  Nel riquadro attività del **centro rete e condivisione**, fare clic su **modificare le impostazioni della scheda**.  
  
4.  Fare clic sulla scheda di rete locale e quindi fare clic su **proprietà**.  
  
5.  Nel **rete** scheda, fare clic su **Internet Protocol versione 4 (TCP/IPv4)**, quindi fare clic su **proprietà**.  
  
6.  Nel **generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**e quindi digitare l'indirizzo IP che si desidera utilizzare.  
  
     Un valore predefinito per la subnet mask verrà visualizzato automaticamente nel **Subnet mask** casella. Accettare il valore predefinito oppure digitare il valore della subnet mask che si desidera utilizzare.  
  
7.  Nel **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.  
  
8.  Nel **server DNS preferito**, digitare l'indirizzo IP del server DNS.  
  
    > [!NOTE]
    >  Utilizzare l'indirizzo IP assegnato alla scheda di rete dal protocollo DHCP (ad esempio 192.168) anziché una rete loopback (ad esempio, 127.0.0.1). Per conoscere l'indirizzo IP assegnato, eseguire **ipconfig** a un prompt dei comandi.  
  
9. Nel **Server DNS alternativo**, digitare l'indirizzo IP del server DNS alternativo, se presente.  
  
10. Fare clic su **OK**, quindi fare clic su **Chiudi**.  
  
> [!IMPORTANT]
>  Assicurarsi di configurare il router per l'inoltro delle porte 80 e 443 per il nuovo indirizzo IP statico del server.  
  
##  <a name="BKMK_DNS"></a>Passaggio 3: Preparare un certificato e il record DNS per il server dei percorsi di rete  
 Per preparare un certificato e il record DNS per il server dei percorsi di rete, eseguire le attività seguenti:  
  
-   [Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato server s Web](#BKMK_GrantFullPermissions)  
  
-   [Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
-   [Passaggio 3c: aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato server s Web  
 La prima attività è la concessione delle autorizzazioni complete per autenticare gli utenti per il modello di certificato s di server Web nell'autorità di certificazione.  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>Per concedere le autorizzazioni complete agli utenti autenticati per il server Web il modello di certificato s  
  
1.  Nel **Start** pagina, aprire **autorità di certificazione**.  
  
2.  Nell'albero della console, in **autorità di certificazione (locale)**, espandere **< servername\ >-CA**, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestisci**.  
  
3.  In **autorità di certificazione (locale)**, fare doppio clic su **Server Web**, quindi fare clic su **proprietà**.  
  
4.  In proprietà del Server Web, nel **sicurezza** scheda, fare clic su **Authenticated Users**, selezionare **controllo completo**, quindi fare clic su **OK**.  
  
5.  Riavviare **Servizi certificati Active Directory**. Nel Pannello di controllo, aprire **Visualizza servizi locali**. Nell'elenco dei servizi, fare doppio clic su **Servizi certificati Active Directory**, quindi fare clic su **riavviare**.  
  
###  <a name="BKMK_EnrollaCertificate"></a>Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna  
 Successivamente, registrare un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna.  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>Per registrare un certificato per il server dei percorsi di rete  
  
1.  Nel **Start** pagina, aprire **mmc** (Microsoft Management Console).  
  
2.  Se un **controllo dell'Account utente** viene visualizzato il messaggio di avviso, fare clic su **Sì**.  
  
     Verrà visualizzata la finestra di Microsoft Management Console (MMC).  
  
3.  Nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
4.  Nel **Aggiungi o Rimuovi Snap-in** fare clic su **certificati**, quindi fare clic su **Aggiungi**.  
  
5.  Nel **snap-in certificati** pagina, fare clic su **account Computer**, quindi fare clic su **Avanti**.  
  
6.  Nel **seleziona Computer** pagina, fare clic su **computer locale**, fare clic su **fine**, quindi fare clic su **OK**.  
  
7.  Nell'albero della console, espandere **certificati (Computer locale)**, espandere **personali**, fare doppio clic su **certificati**, quindi **tutte le attività**, fare clic su **Richiedi nuovo certificato**.  
  
8.  Quando viene visualizzata la procedura guidata registrazione certificato, fare clic su **Avanti**.  
  
9. Nel **Seleziona criteri di registrazione certificato** pagina, fare clic su **Avanti**.  
  
10. Nel **richiesta certificati** pagina, selezionare il **Server Web** casella di controllo, quindi fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
11. Nel **proprietà certificato**, immettere le impostazioni seguenti per **nome soggetto**:  
  
    1.  Per **tipo**selezionare **nome comune**.  
  
    2.  Per **valore**, digitare il nome del server del percorso di rete (ad esempio, DirectAccess-NLS) e quindi fare clic su **Aggiungi**.  
  
    3.  Fare clic su **OK**, quindi fare clic su **registrazione**.  
  
12. Al termine della registrazione del certificato, fare clic su **fine**.  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>Passaggio 3c: aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials  
 Per completare la configurazione DNS, aggiungere un nuovo host al server DNS e mapparlo all'indirizzo del server Windows Server Essentials.  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>Per mappare un nuovo host all'indirizzo del server Windows Server Essentials  
  
1.  Nella pagina iniziale, aprire Gestore DNS. Per aprire Gestore DNS, cercare **dnsmgmt.msc**, quindi fare clic su **dnsmgmt.msc** nei risultati.  
  
2.  Nell'albero della console Gestore DNS, espandere il server locale, **zone di ricerca diretta**, fare clic sulla zona con il suffisso del dominio server s e quindi fare clic su **nuovo Host (A o AAAA)**.  
  
3.  Digitare il nome e indirizzo IP del server (ad esempio, DirectAccess-NLS) e è corrispondente indirizzo del server (ad esempio 192.168).  
  
4.  Fare clic su **Aggiungi Host**, fare clic su **OK**, quindi fare clic su **eseguita**.  
  
##  <a name="BKMK_AddSecurityGroup"></a>Passaggio 4: Creare un gruppo di sicurezza per i computer client DirectAccess  
 Successivamente, creare un gruppo di sicurezza da utilizzare per i computer client DirectAccess e quindi aggiungere gli account computer al gruppo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Per aggiungere un gruppo di sicurezza per i computer client che usano DirectAccess  
  
1.  Nel Dashboard di Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**.  
  
    > [!NOTE]
    >  Se non vedi **Active Directory Users and Computers** nel **strumenti** menu, è necessario installare la funzionalità. Per installare Active Directory Users e gruppi, eseguire il seguente cmdlet Windows PowerShell come amministratore:`Install-WindowsFeature RSAT-ADDS-Tools`. Per ulteriori informazioni, vedere [installare o rimuovere il pacchetto di strumento di amministrazione Server remota](https://technet.microsoft.com/library/cc730825.aspx).  
  
2.  Nell'albero della console, espandere il server, fare doppio clic su **utenti**, fare clic su **New**, quindi fare clic su **gruppo**.  
  
3.  Immettere un nome di gruppo, l'ambito del gruppo e tipo di gruppo (creare un gruppo di sicurezza), quindi fare clic su **OK**.  
  
 Viene aggiunto il nuovo gruppo di sicurezza per il **utenti** cartella.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Per aggiungere gli account computer al gruppo di sicurezza  
  
1.  Nel Dashboard di Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**.  
  
2.  Nell'albero della console, espandere il server e quindi fare clic su **utenti**.  
  
3.  Nell'elenco di account utente e gruppi di sicurezza nel server, fare doppio clic su gruppo di sicurezza creato per DirectAccess, quindi fare clic su **proprietà**.  
  
4.  Nel **membri** scheda, fare clic su **Aggiungi**.  
  
5.  Nella finestra di dialogo, digitare i nomi degli account computer che si desidera aggiungere al gruppo, separandoli con un punto e virgola. Fare clic su **Controlla nomi**.  
  
6.  Dopo aver convalidati gli account computer, fare clic su **OK**. Fare clic su **OK** nuovamente.  
  
> [!NOTE]
>  È inoltre possibile utilizzare il **appartenente** scheda nelle proprietà dell'account computer per aggiungere l'account al gruppo di sicurezza.  
  
##  <a name="BKMK_EnableConfigureDA"></a>Passaggio 5: Abilitare e configurare DirectAccess  
 Per abilitare e configurare DirectAccess in Windows Server Essentials, è necessario completare i passaggi seguenti:  
  
-   [Passaggio 5a: abilitare DirectAccess mediante la Console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Passaggio 5D: configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Passaggio 5e: aggiungere una chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>Passaggio 5a: abilitare DirectAccess mediante la Console di gestione accesso remoto  
 Questa sezione vengono fornite le istruzioni dettagliate per l'abilitazione di DirectAccess in Windows Server Essentials. Se non è stata configurata VPN nel server ancora, è necessario farlo prima di iniziare questa procedura. Per istruzioni, vedere [gestire la VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Per abilitare DirectAccess mediante la Console di gestione accesso remoto  
  
1.  Nella pagina iniziale, aprire **Gestione accesso remoto**.  
  
2.  Nell'abilitazione guidata DirectAccess, eseguire le operazioni seguenti:  
  
    1.  Revisione **prerequisiti di DirectAccess**e fare clic su **Avanti**.  
  
    2.  Nel **Seleziona gruppi** scheda, aggiungere il gruppo di sicurezza creato in precedenza per i client DirectAccess. (Se non hai creato il gruppo di sicurezza, vedere [passaggio 4: creare un gruppo di sicurezza per i client DirectAccess computer](#BKMK_AddSecurityGroup) per le istruzioni.)  
  
    3.  Nel **Seleziona gruppi** scheda, fare clic su **Abilita DirectAccess solo per computer mobili** se si desidera consentire ai computer mobili di utilizzare DirectAccess per accedere in modalità remota al server, quindi fare clic su **Avanti**.  
  
    4.  In **topologia di rete**, selezionare la topologia del server e quindi fare clic su **Avanti**.  
  
    5.  In **elenco ricerca suffissi DNS**, aggiungere il suffisso DNS per i computer client, se necessario e quindi fare clic su **Avanti**.  
  
        > [!NOTE]
        >  Per impostazione predefinita, l'abilitazione guidata DirectAccess aggiunge il suffisso DNS per il dominio corrente. Tuttavia, è possibile aggiungere più se necessario.  
  
    6.  Esaminare l'oggetti Criteri di gruppo (GPO) che verrà applicato e modificarle se necessario.  
  
    7.  Fare clic su **Avanti**, quindi fare clic su **fine**.  
  
    8.  Riavviare il servizio di gestione accesso remoto eseguendo il comando seguente di Windows PowerShell con privilegi elevati:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)  
  In questa sezione si applica a un server che esegue Windows Server Essentials.  
  
 Aprire Windows PowerShell come amministratore ed eseguire i comandi seguenti:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess  
 Se si dispone di computer client che eseguono Windows 7 Enterprise, completare la procedura seguente per abilitare DirectAccess da tali computer.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Per abilitare i computer Windows 7 Enterprise per l'uso di DirectAccess  
  
1.  Nella pagina iniziale, server s aprire **Gestione accesso remoto**.  
  
2.  Nella Console di gestione accesso remoto, fare clic su **configurazione**. Quindi, nel **dettagli installazione** riquadro, in **passaggio 2**, fare clic su **modifica**.  
  
     Verrà visualizzata la configurazione guidata Server di accesso remoto.  
  
3.  Nel **autenticazione** scegliere il certificato di autorità di certificazione che sarà il certificato radice attendibile (è possibile scegliere il certificato della CA del server di Windows Server Essentials). Fare clic su **attivare Windows 7 i computer client di connettersi tramite DirectAccess**, quindi fare clic su **Avanti**.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!IMPORTANT]
>  Esiste un problema noto per i computer Windows 7 Enterprise si connettono tramite DirectAccess, se il server Windows Server Essentials non proviene preinstallato ur1. Per abilitare le connessioni DirectAccess in tale ambiente, è necessario eseguire questi passaggi aggiuntivi:  
>   
>  1.  Installare l'hotfix descritto nel [articolo della Microsoft Knowledge Base (KB) 2796394](https://support.microsoft.com/kb/2796394) nel server di Windows Server Essentials. Quindi riavviare il server.  
> 2.  Quindi installare l'hotfix descritto nel [articolo della Microsoft Knowledge Base (KB) 2615847](https://support.microsoft.com/kb/2615847) in ogni computer Windows 7.  
>   
>      Questo problema è stato risolto in Windows Server Essentials.  
  
###  <a name="BKMK_NLS"></a>Passaggio 5D: configurare il server dei percorsi di rete  
 In questa sezione fornisce istruzioni dettagliate per configurare le impostazioni del server di percorso di rete.  
  
> [!NOTE]
>  Prima di iniziare, copiare il contenuto della cartella < SystemDrive\ > \inetpub\wwwroot nella cartella < SystemDrive\ > \Programmi\Windows server\bin\webapps\site\insideoutside.. Inoltre è possibile copiare il file default.aspx dalla cartella < SystemDrive\ > \Programmi\Windows Server\Bin\WebApps\Site nella cartella < SystemDrive\ > \Programmi\Windows server\bin\webapps\site\insideoutside..  
  
##### <a name="to-configure-the-network-location-server"></a>Per configurare il server dei percorsi di rete  
  
1.  Nella pagina iniziale, aprire **Gestione accesso remoto**.  
  
2.  Nella Console di gestione accesso remoto, fare clic su **configurazione**e il **configurazione accesso remoto** altri dettagli, riquadro, vedi **passaggio 3**, fare clic su **modifica**.  
  
3.  Nel Server di configurazione guidata accesso remoto, nel **Server dei percorsi di rete** selezionare **il server dei percorsi di rete viene distribuito nel server di accesso remoto**e quindi selezionare il certificato emesso in precedenza (in [passaggio 3: preparare un certificato e il record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Seguire le istruzioni per completare la procedura guidata, quindi fare clic su **fine**.  
  
###  <a name="BKMK_CA"></a>Passaggio 5e: aggiungere una chiave del Registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec  
 Il passaggio successivo consiste nel configurare il server per ignorare la certificazione CA quando viene stabilito un canale IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Per aggiungere una chiave del Registro di sistema per ignorare la certificazione CA  
  
1.  Nella pagina iniziale, aprire **regedit** (l'Editor del Registro di sistema).  
  
2.  Nell'Editor del Registro di sistema espandere **HKEY_LOCAL_MACHINE**, espandere **sistema**, espandere **CurrentControlSet**, espandere **servizi**, espandere **IKEEXT**.  
  
3.  In **IKEEXT**, fare doppio clic su **parametri**, fare clic su **New**, quindi fare clic su **valore DWORD (32 bit)**.  
  
4.  Rinominare il valore appena aggiunto in **ikeflags**.  
  
5.  Fare doppio clic su **ikeflags**, impostare il **tipo** per **esadecimale**, impostare il valore su **8000**, quindi fare clic su **OK**.  
  
> [!NOTE]
>  È possibile utilizzare il seguente comando di Windows PowerShell con privilegi elevati per aggiungere questa chiave del Registro di sistema:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>Passaggio 6: Configurare le impostazioni di tabella criteri risoluzione nomi per il server DirectAccess  
 In questa sezione vengono fornite istruzioni per la modifica il nome risoluzione criteri tabella delle voci per gli indirizzi interni (ad esempio, le voci con un suffisso contoso. Local) per oggetti Criteri di gruppo client DirectAccess e quindi impostata l'indirizzo dell'interfaccia IPHTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Per configurare le voci della tabella dei criteri risoluzione nomi  
  
1.  Nella pagina iniziale, aprire **Gestione criteri di gruppo**.  
  
2.  Nella console Gestione criteri di gruppo, fare clic sulla foresta e predefiniti dominio, fare doppio clic su **impostazioni Client DirectAccess**, quindi fare clic su **modifica**.  
  
3.  Fare clic su **configurazioni Computer**, fare clic su **criteri**, fare clic su **le impostazioni di Windows**, quindi fare clic su **criteri risoluzione nomi**. Scegliere la voce con lo spazio dei nomi identico al suffisso DNS in uso, quindi fare clic su **Modifica regola**.  
  
4.  Fare clic su di **le impostazioni DNS per DirectAccess** scheda; Selezionare quindi **Abilita le impostazioni DNS per DirectAccess in questa regola**. Aggiungere l'indirizzo IPv6 per l'interfaccia IP-HTTPS nell'elenco del server DNS.  
  
    > [!NOTE]
    >  Per ottenere l'indirizzo IPv6, è possibile utilizzare il comando di Windows PowerShell seguente:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>Passaggio 7: Configurare regole del firewall TCP e UDP per il server DirectAccess oggetti Criteri di gruppo  
 Questa sezione include istruzioni dettagliate per configurare regole del firewall TCP e UDP per il server DirectAccess oggetti Criteri di gruppo.  
  
#### <a name="to-configure-firewall-rules"></a>Per configurare le regole del firewall  
  
1.  Nella pagina iniziale, aprire **Gestione criteri di gruppo**.  
  
2.  Nella console Gestione criteri di gruppo, fare clic sulla foresta e predefiniti dominio, fare doppio clic su **impostazioni Server DirectAccess**, quindi fare clic su **modifica**.  
  
3.  Fare clic su **configurazione Computer**, fare clic su **criteri**, fare clic su **le impostazioni di Windows**, fare clic su **le impostazioni di sicurezza**, fare clic su **Windows Firewall con sicurezza avanzata**, fare clic su livello successivo **Windows Firewall con sicurezza avanzata**e quindi fare clic su **regole in entrata**. Fare doppio clic su **Domain name Server (TCP-In)**, quindi fare clic su **proprietà**.  
  
4.  Fare clic su di **ambito** scheda e il **indirizzo IP locale** elenco, aggiungere l'indirizzo IPv6 dell'interfaccia IP-HTTPS.  
  
5.  Ripetere la stessa procedura per **Server Domain Name (UDP-In)**.  
  
##  <a name="BKMK_DNS64"></a>Passaggio 8: Modificare la configurazione DNS64 per l'ascolto per l'interfaccia IP-HTTPS  
 È necessario modificare la configurazione DNS64 per l'ascolto per l'interfaccia IP-HTTPS utilizzando il comando seguente di Windows PowerShell.  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>Passaggio 9: Riservare le porte per il servizio WinNat  
 Utilizzare il comando di Windows PowerShell seguente per riservare le porte per il servizio WinNat. Sostituire "192.168.1.100" con l'indirizzo IPv4 effettivo del server Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Per evitare conflitti di porte con le applicazioni, assicurarsi che l'intervallo di porte riservato per il servizio WinNat non includa la porta 6602.  
  
##  <a name="BKMK_WinNAT"></a>Passaggio 10: Riavviare il servizio WinNat  
 Riavviare il servizio Driver NAT Windows (WinNat) eseguendo il comando seguente di Windows PowerShell.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>Appendice: Installare DirectAccess tramite Windows PowerShell  
 In questa sezione viene descritto come installare e configurare DirectAccess tramite Windows PowerShell.  
  
### <a name="preparation"></a>Preparazione  
 Prima di iniziare la configurazione del server per DirectAccess, è necessario completare le operazioni seguenti:  
  
1.  Seguire la procedura descritta in [passaggio 3: preparare un certificato e il record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) per registrare un certificato denominato **DirectAccess-NLS.contoso.com** (in cui **contoso.com** è sostituito dal nome di dominio interno effettivo in uso) e per aggiungere un record DNS per il server dei percorsi di rete (NLS).  
  
2.  Aggiungere un gruppo di sicurezza denominato **DirectAccessClients** in Active Directory, quindi aggiungere i computer client per il quale si desidera fornire la funzionalità DirectAccess. Per ulteriori informazioni, vedere [passaggio 4: creare un gruppo di sicurezza per i client DirectAccess computer](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Comandi  
  
> [!IMPORTANT]
>  In Windows Server Essentials, non è necessario rimuovere l'oggetto Criteri di gruppo del prefisso IPv6 non necessario. Eliminare la sezione di codice con l'etichetta seguente:`# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
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
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
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
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
