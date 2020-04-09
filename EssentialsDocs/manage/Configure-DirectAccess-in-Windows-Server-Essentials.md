---
title: Configurare DirectAccess in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d8029b954a5957433fb0fcc71d3bef610a187939
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819705"
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurare DirectAccess in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questo argomento vengono fornite istruzioni dettagliate per la configurazione di DirectAccess in Windows Server Essentials per consentire alla forza lavoro mobile di connettersi facilmente alla rete dell'organizzazione da qualsiasi posizione remota dotata di connessione Internet senza stabilire una connessione VPN (Virtual Private Network). DirectAccess può offrire agli utenti mobili la stessa esperienza di connettività all'interno e all'esterno dell'ufficio dai computer Windows 8.1, Windows 8 e Windows 7.  
  
 In Windows Server Essentials, se il dominio contiene più di un server di Windows Server Essentials, DirectAccess deve essere configurato nel controller di dominio.  
  
> [!NOTE]
>  In questo argomento vengono fornite le istruzioni per la configurazione di DirectAccess quando il server Windows Server Essentials è il controller di dominio. Se il server di Windows Server Essentials è un membro del dominio, seguire le istruzioni per configurare DirectAccess in un membro del dominio in [aggiungere DirectAccess a una distribuzione di accesso remoto (VPN) esistente](https://technet.microsoft.com/library/jj574220.aspx) .  
  
## <a name="process-overview"></a>Panoramica del processo  
 Per configurare DirectAccess in Windows Server Essentials, completare i passaggi seguenti.  
  
> [!IMPORTANT]
>  Prima di usare le procedure descritte in questa guida per configurare DirectAccess in Windows Server Essentials, è necessario abilitare la VPN sul server. Per istruzioni, vedere [Manage VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Passaggio 1: aggiungere strumenti di gestione accesso remoto al server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Passaggio 2: impostare l'indirizzo della scheda di rete del server su un indirizzo IP statico](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Passaggio 3: preparare un certificato e un record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web](#BKMK_GrantFullPermissions)  
  
    -   [Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune irrisolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
    -   [Passaggio 3c: aggiungere un nuovo host nel server DNS ed eseguirne il mapping all'indirizzo del server di Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Passaggio 4: creare un gruppo di sicurezza per i computer client DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Passaggio 5: abilitare e configurare DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Passaggio 5a: abilitare DirectAccess con la console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Passaggio 5D: configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Passaggio 5e: aggiungere una chiave del registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Passaggio 6: configurare le impostazioni della tabella dei criteri di risoluzione dei nomi per il server DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Passaggio 7: configurare le regole del firewall TCP e UDP per gli oggetti Criteri di gruppo del server DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Passaggio 8: modificare la configurazione DNS64 per l'ascolto dell'interfaccia IP-HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Passaggio 9: riservare le porte per il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Passaggio 10: riavviare il servizio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Appendice: Installare DirectAccess tramite Windows PowerShell](#BKMK_AppendixBPowerShellScript) fornisce uno script di Windows PowerShell che è possibile usare per eseguire la configurazione di DirectAccess.  
  
##  <a name="step-1-add-remote-access-management-tools-to-your-server"></a><a name="BKMK_AddRAM"></a>Passaggio 1: aggiungere strumenti di gestione accesso remoto al server  
  
#### <a name="to-add-remote-accregss-management-tools--reg"></a>Per aggiungere gli strumenti di gestione di ACC&reg;SS remota &reg;
  
1.  Sul server, nell'angolo inferiore sinistro della pagina iniziale fare clic sull'icona **Server Manager**.  
  
     In Windows Server Essentials sarà necessario cercare Server Manager per aprirlo. Nella pagina iniziale digitare **Server Manager** e quindi fare clic su **Server Manager** nei risultati della ricerca. Per aggiungere Server Manager alla pagina iniziale, fare clic con il pulsante destro del mouse su Server Manager nei risultati della ricerca e scegliere **Aggiungi a Start**.  
  
2.  Se viene visualizzato un messaggio di avviso relativo al **Controllo dell'account utente**, fare clic su **Sì**.  
  
3.  Nel dashboard di Server Manager fare clic su **Gestione** e quindi su **Aggiungi ruoli e funzionalità**.  
  
4.  In Aggiunta guidata ruoli e funzionalità eseguire le operazioni seguenti:  
  
    1.  Nella pagina **Tipo di installazione** fare clic su **Installazione basata su ruoli o basata su funzionalità**.  
  
    2.  Nella **pagina Selezione server** (o nella pagina Selezione **server di destinazione** in Windows Server Essentials) fare clic su **selezionare un server dal pool di server**.  
  
    3.  Nella pagina **Funzionalità** espandere **Strumenti di amministrazione remota del server (installati)** , espandere **Strumenti di Gestione Accesso remoto (installati)** , espandere **Strumenti di amministrazione ruoli (installati)** , espandere **Strumenti di Gestione Accesso remoto** e quindi selezionare **Interfaccia utente grafica e strumenti da riga di comando di Accesso remoto**.  
  
    4.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="step-2-change-the-network-adapter-address-of-the-server-to-a-static-ip-address"></a><a name="BKMK_AddStaticIP"></a>Passaggio 2: impostare l'indirizzo della scheda di rete del server su un indirizzo IP statico  
 DirectAccess richiede una scheda con un indirizzo IP statico. È quindi necessario modificare l'indirizzo IP per la scheda di rete locale del server.  
  
#### <a name="to-add-a-static-ip-address"></a>Per aggiungere un indirizzo IP statico  
  
1.  Nella pagina iniziale aprire il **Pannello di controllo**.  
  
2.  Fare clic su **Rete e Internet** e quindi su **Visualizza attività e stato della rete**.  
  
3.  Nel riquadro attività di **Centro connessioni di rete e condivisione** fare clic su **Modifica impostazioni scheda**.  
  
4.  Fare clic con il pulsante destro del mouse sulla scheda di rete locale e quindi scegliere **Proprietà**.  
  
5.  Nella scheda **Rete** fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
6.  Nella scheda **Generale** fare clic su **Utilizza il seguente indirizzo IP** e quindi digitare l'indirizzo IP che si desidera utilizzare.  
  
     Nella casella **Subnet mask** viene automaticamente visualizzato un valore predefinito per la subnet mask. Accettare il valore predefinito oppure digitare il valore della subnet mask che si desidera utilizzare.  
  
7.  Nella casella **Gateway predefinito** digitare l'indirizzo IP del gateway predefinito.  
  
8.  Nella casella **Server DNS preferito** digitare l'indirizzo IP del server DNS in uso.  
  
    > [!NOTE]
    >  Usare l'indirizzo IP assegnato alla scheda di rete dal protocollo DHCP (ad esempio 192.168.X.X) invece di quello di una rete loopback (ad esempio 127.0.0.1). Per conoscere l'indirizzo IP assegnato, eseguire **ipconfig** in un prompt dei comandi.  
  
9. Nella casella **Server DNS alternativo** digitare l'indirizzo IP del server DNS alternativo, se disponibile.  
  
10. Fare clic su **OK** e quindi fare clic su **Chiudi**.  
  
> [!IMPORTANT]
>  Accertarsi di configurare il router per l'inoltro delle porte 80 e 443 al nuovo indirizzo IP statico del server.  
  
##  <a name="step-3-prepare-a-certificate-and-dns-record-for-the-network-location-server"></a><a name="BKMK_DNS"></a>Passaggio 3: preparare un certificato e un record DNS per il server dei percorsi di rete  
 Per preparare un certificato e il record DNS per il server dei percorsi di rete, eseguire le attività seguenti:  
  
-   [Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web](#BKMK_GrantFullPermissions)  
  
-   [Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune irrisolvibile dalla rete esterna](#BKMK_EnrollaCertificate)  
  
-   [Passaggio 3c: aggiungere un nuovo host nel server DNS ed eseguirne il mapping all'indirizzo del server di Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="step-3a-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_GrantFullPermissions"></a>Passaggio 3a: concedere le autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web  
 La prima attività consiste nel concedere le autorizzazioni complete per autenticare gli utenti per il modello di certificato del server Web nell'autorità di certificazione.  
  
####  <a name="to-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_ToGrantFullPermissions"></a>Per concedere autorizzazioni complete agli utenti autenticati per il modello di certificato del server Web  
  
1.  Nella pagina **iniziale** aprire **Autorità di certificazione**.  
  
2.  Nell'albero della console, sotto **autorità di certificazione (locale)** , espandere **< nomeserver\>-CA**, fare clic con il pulsante destro del mouse su **modelli di certificato**, quindi fare clic su **Gestisci**.  
  
3.  In **Autorità di certificazione (locale)** fare clic con il pulsante destro del mouse su **Server Web** e quindi scegliere **Proprietà.**  
  
4.  Nelle proprietà del server Web, nella scheda **Sicurezza** fare clic su **Utenti autenticati**, selezionare **Controllo completo** e quindi fare clic su **OK**.  
  
5.  Riavviare **Servizi certificati Active Directory**. Nel Panello di controllo aprire **Visualizza servizi locali**. Nell'elenco di servizi fare clic con il pulsante destro del mouse su **Servizi certificati Active Directory** e quindi scegliere **Riavvia**.  
  
###  <a name="step-3b-enroll-a-certificate-for-the-network-location-server-with-a-common-name-that-is-unresolvable-from-the-external-network"></a><a name="BKMK_EnrollaCertificate"></a>Passaggio 3b: registrare un certificato per il server dei percorsi di rete con un nome comune irrisolvibile dalla rete esterna  
 Registrare quindi un certificato per il server dei percorsi di rete con un nome comune non risolvibile dalla rete esterna.  
  
####  <a name="to-enroll-a-certificate-for-the-network-location-server"></a><a name="BKMK_ToEnrollaCertificate"></a>Per registrare un certificato per il server dei percorsi di rete  
  
1.  Nella pagina inizialeaprire **mmc** (Microsoft Management Console).  
  
2.  Se viene visualizzato un messaggio di avviso relativo al **Controllo dell'account utente**, fare clic su **Sì**.  
  
     Verrà aperto Microsoft Management Console (MMC).  
  
3.  Nel menu **File** fare clic su **Aggiungi/Rimuovi snap-in**.  
  
4.  Nella casella **Aggiungi o rimuovi snap-in** fare clic su **Certificati** e quindi fare clic su **Aggiungi**.  
  
5.  Nella pagina **Snap-in certificati** fare clic su **Account computer**, quindi su **Avanti**.  
  
6.  Nella pagina **Selezione computer** fare clic su **Computer locale**, fare clic su **Fine** e quindi su **OK**.  
  
7.  Nell'albero della console espandere **Certificati (computer locale)** , espandere **Personale**, fare clic con il pulsante destro del mouse su **Certificati** e quindi in **Tutte le attività** scegliere **Richiedi nuovo certificato**.  
  
8.  Quando viene visualizzata la procedura guidata Registrazione certificato, fare clic su **Avanti**.  
  
9. Nella pagina **Seleziona criteri di registrazione certificato** fare clic su **Avanti**.  
  
10. Nella pagina **Richiedi certificato** selezionare la casella di controllo **Server Web** e quindi fare clic su **Sono necessarie ulteriori informazioni per registrare il certificato**.  
  
11. Nella casella **Proprietà certificato** immettere le impostazioni seguenti per **Nome soggetto**:  
  
    1.  In **Tipo** selezionare **Nome comune**.  
  
    2.  In **Valore** digitare il nome del server dei percorsi di rete, ad esempio, DirectAccess-NLS.contoso.local, quindi fare clic su **Aggiungi**.  
  
    3.  Fare clic su **OK** e quindi fare clic su **Registrazione**.  
  
12. Una volta completata la registrazione certificato, fare clic su **Fine**.  
  
###  <a name="step-3c-add-a-new-host-on-the-dns-server-and-map-it-to-the-windows-server-essentials-server-address"></a><a name="BKMK_MapNewHosttoServerAddress"></a>Passaggio 3c: aggiungere un nuovo host nel server DNS ed eseguirne il mapping all'indirizzo del server di Windows Server Essentials  
 Per completare la configurazione DNS, aggiungere un nuovo host nel server DNS ed eseguirne il mapping all'indirizzo del server di Windows Server Essentials.  
  
####  <a name="to-map-a-new-host-to-the-windows-server-essentials-server-address"></a><a name="BKMK_ToMapNewHosttoServerAddress"></a>Per eseguire il mapping di un nuovo host all'indirizzo del server di Windows Server Essentials  
  
1.  Nella pagina iniziale aprire Gestore DNS. Per aprire Gestore DNS, cercare **dnsmgmt.msc**e quindi fare clic su **dnsmgmt.msc** nei risultati.  
  
2.  Nell'albero della console Gestore DNS espandere il server locale, espandere **zone di ricerca diretta**, fare clic con il pulsante destro del mouse sulla zona con il suffisso di dominio del server e quindi scegliere **nuovo host (A o AAAA)** .  
  
3.  Digitare il nome e l'indirizzo IP del server, ad esempio DirectAccess-NLS.contoso.local e l'indirizzo del server corrispondente, ad esempio 192.168.x.x.  
  
4.  Fare clic su **Aggiungi host**, fare clic su **OK** e quindi fare clic su **Chiudi**.  
  
##  <a name="step-4-create-a-security-group-for-directaccess-client-computers"></a><a name="BKMK_AddSecurityGroup"></a>Passaggio 4: creare un gruppo di sicurezza per i computer client DirectAccess  
 Creare poi un gruppo di sicurezza da usare per i computer client DirectAccess e quindi aggiungere gli account computer al gruppo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Per aggiungere un gruppo di sicurezza per i computer client che usano DirectAccess  
  
1. Nel dashboard di Server Manager fare clic su **Strumenti** e quindi fare clic su **Utenti e computer di Active Directory**.  
  
   > [!NOTE]
   >  Se **Utenti e computer di Active Directory** non è visibile nel menu **Strumenti**, è necessario installare la funzionalità. Per installare utenti e gruppi di Active Directory, eseguire il cmdlet di Windows PowerShell seguente come amministratore: `Install-WindowsFeature RSAT-ADDS-Tools`. Per altre informazioni, vedere [Installare o rimuovere il pacchetto Strumenti di amministrazione remota del server](https://technet.microsoft.com/library/cc730825.aspx).  
  
2. Nell'albero della console espandere il server, fare clic con il pulsante destro del mouse su **Utenti**, scegliere **Nuovo** e quindi **Gruppo**.  
  
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
  
##  <a name="step-5-enable-and-configure-directaccess"></a><a name="BKMK_EnableConfigureDA"></a>Passaggio 5: abilitare e configurare DirectAccess  
 Per abilitare e configurare DirectAccess in Windows Server Essentials, è necessario completare i passaggi seguenti:  
  
-   [Passaggio 5a: abilitare DirectAccess con la console di gestione accesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Passaggio 5D: configurare il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Passaggio 5e: aggiungere una chiave del registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="step-5a-enable-directaccess-by-using-the-remote-access-management-console"></a><a name="BKMK_EnableDA"></a>Passaggio 5a: abilitare DirectAccess con la console di gestione accesso remoto  
 In questa sezione vengono fornite istruzioni dettagliate per l'abilitazione di DirectAccess in Windows Server Essentials. Se la VPN sul server non è ancora stata configurata, è consigliabile farlo prima di iniziare questa procedura. Per istruzioni, vedere [Manage VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Per abilitare DirectAccess con la Console di gestione Accesso remoto  
  
1.  Nella pagina iniziale aprire **Gestione accesso remoto**.  
  
2.  Nell'Abilitazione guidata DirectAccess eseguire le operazioni seguenti:  
  
    1.  Rivedere **Prerequisiti DirectAccess** e fare clic su **Avanti**.  
  
    2.  Nella scheda **Seleziona gruppi** aggiungere il gruppo di sicurezza creato in precedenza per i client DirectAccess. Se il gruppo di sicurezza non è stato creato, vedere [passaggio 4: creare un gruppo di sicurezza per i computer client DirectAccess](#BKMK_AddSecurityGroup) per istruzioni.  
  
    3.  Sulla scheda **Seleziona gruppi** fare clic su **Abilita DirectAccess solo per computer mobili** se si desidera abilitare i computer mobili per l'utilizzo di DirectAccess per accedere in modalità remota al server, quindi fare clic su **Avanti**.  
  
    4.  In **Topologia di rete** selezionare la topologia del server e quindi fare clic su **Avanti**.  
  
    5.  In **Elenco di ricerca suffissi DNS** aggiungere il suffisso DNS per i computer client, se necessario, e quindi fare clic su **Avanti**.  
  
        > [!NOTE]
        >  Per impostazione predefinita, l'abilitazione guidata di DirectAccess aggiunge il suffisso DNS per il dominio corrente. È tuttavia possibile aggiungerne altri, se necessario.  
  
    6.  Verificare gli oggetti Criteri di gruppo che verranno applicati e, se necessario, modificarli.  
  
    7.  Fare clic su **Avanti**e quindi su **Fine**.  
  
    8.  Riavviare il servizio Gestione accesso remoto eseguendo il comando Windows PowerShell seguente con un account con privilegi elevati:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="step-5b-remove-the-invalid-ipv6prefix-in-rras-gpo-windows-server-essentials-only"></a><a name="BKMK_RemoveIPv6"></a>Passaggio 5b: rimuovere il IPv6Prefix non valido nell'oggetto Criteri di gruppo RRAS (solo Windows Server Essentials)  
  Questa sezione si applica a un server che esegue Windows Server Essentials.  
  
 Aprire Windows PowerShell come amministratore ed eseguire i comandi seguenti:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="step-5c-enable-client-computers-running-windows-7-enterprise-to-use-directaccess"></a><a name="BKMK_Step4cWindows7Setup"></a>Passaggio 5c: abilitare i computer client che eseguono Windows 7 Enterprise per l'uso di DirectAccess  
 Se sono presenti computer client che eseguono Windows 7 Enterprise, completare la procedura seguente per abilitare DirectAccess da tali computer.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Per consentire ai computer Windows 7 Enterprise di utilizzare DirectAccess  
  
1.  Nella pagina iniziale del server aprire **gestione accesso remoto**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**. Quindi nel riquadro **Dettagli installazione**, sotto **Passaggio 2** fare clic su **Modifica**.  
  
     Si apre la procedura guidata Installazione del server di Accesso remoto.  
  
3.  Nella scheda **autenticazione** scegliere il certificato dell'autorità di certificazione (CA) che sarà il certificato radice attendibile. è possibile scegliere il certificato della CA del server di Windows Server Essentials. Fare clic su **Abilita computer client Windows 7 alla connessione tramite DirectAccess** e quindi fare clic su **Avanti**.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!IMPORTANT]
>  Si è verificato un problema noto per i computer Windows 7 Enterprise che si connettono tramite DirectAccess se il server Windows Server Essentials non è stato preinstallato con UR1. Per abilitare le connessioni DirectAccess in tale ambiente, è necessario eseguire questi passaggi aggiuntivi:  
> 
> 1. Installare l'hotfix descritto nell' [articolo 2796394 della Microsoft Knowledge base (KB)](https://support.microsoft.com/kb/2796394) nel server Windows Server Essentials. Quindi riavviare il server.  
>    2. Installare quindi l'hotfix descritto nell' [articolo 2615847 della Microsoft Knowledge base (KB)](https://support.microsoft.com/kb/2615847) in ogni computer Windows 7.  
> 
>    Questo problema è stato risolto in Windows Server Essentials.  
  
###  <a name="step-5d-configure-the-network-location-server"></a><a name="BKMK_NLS"></a>Passaggio 5D: configurare il server dei percorsi di rete  
 In questa sezione vengono illustrate le istruzioni dettagliate per configurare le impostazioni del server dei percorsi di rete.  
  
> [!NOTE]
>  Prima di iniziare, copiare il contenuto della cartella < SystemDrive\>\Inetpub\Wwwroot nella cartella < SystemDrive\>\Program Server\bin\webapps\site\insideoutside. Copiare anche il file default. aspx dalla cartella < SystemDrive\>\Programmi\microsoft Server\bin\webapps\site alla nella cartella < SystemDrive\>\Program Server\bin\webapps\site\insideoutside.  
  
##### <a name="to-configure-the-network-location-server"></a>Per configurare il server dei percorsi di rete  
  
1.  Nella pagina iniziale aprire **Gestione accesso remoto**.  
  
2.  Nella console di Gestione Accesso remoto fare clic su **Configurazione**, quindi, nel riquadro dei dettagli **Configurazione Accesso remoto** fare clic su **Modifica** in **Passaggio 3**.  
  
3.  Nella Configurazione guidata server di Accesso remoto, nella scheda **Server dei percorsi di rete** selezionare **Il server dei percorsi di rete viene distribuito nel server di Accesso remoto**e quindi selezionare il certificato emesso in precedenza (in [Step 3: Prepare a certificate and DNS record for the network location server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Seguire le istruzioni per completare la procedura guidata e quindi fare clic su **Fine**.  
  
###  <a name="step-5e-add-a-registry-key-to-bypass-ca-certification-when-you-establish-an-ipsec-channel"></a><a name="BKMK_CA"></a>Passaggio 5e: aggiungere una chiave del registro di sistema per ignorare la certificazione CA quando si stabilisce un canale IPsec  
 Il passaggio successivo è la configurazione del server per ignorare la certificazione CA quando viene stabilito un canale IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Per aggiungere una chiave del Registro di sistema per ignorare la certificazione CA  
  
1.  Nella pagina iniziale aprire **regedit** (l'editor del Registro di sistema).  
  
2.  Nell'editor del Registro di sistema espandere **HKEY_LOCAL_MACHINE**, espandere **System**, espandere **CurrentControlSet**, espandere **Services** ed espandere **IKEEXT**.  
  
3.  Sotto **IKEEXT** fare clic con il pulsante destro del mouse su **Parameters**, scegliere **Nuovo** e quindi fare clic su **Valore DWORD (32 bit)** .  
  
4.  Rinominare il valore appena aggiunto in **ikeflags**.  
  
5.  Fare doppio clic su **ikeflags**, impostare **Tipo** su **Esadecimale**, impostare il valore su **8000** e quindi fare clic su **OK**.  
  
> [!NOTE]
>  È possibile usare il comando Windows PowerShell seguente con un account con privilegi elevati per aggiungere questa chiave del Registro di sistema:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="step-6-configure-name-resolution-policy-table-settings-for-the-directaccess-server"></a><a name="BKMK_NRPT"></a>Passaggio 6: configurare le impostazioni della tabella dei criteri di risoluzione dei nomi per il server DirectAccess  
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
  
##  <a name="step-7-configure-tcp-and-udp-firewall-rules-for-the-directaccess-server-gpos"></a><a name="BKMK_TCPUDP"></a>Passaggio 7: configurare le regole del firewall TCP e UDP per gli oggetti Criteri di gruppo del server DirectAccess  
 In questa sezione vengono fornite le istruzioni dettagliate per configurare le regole del firewall TCP e UDP per gli oggetti Criteri di gruppo del server DirectAccess  
  
#### <a name="to-configure-firewall-rules"></a>Per configurare le regole del firewall  
  
1.  Nella pagina iniziale aprire **Gestione Criteri di gruppo**.  
  
2.  Nella console Gestione Criteri di gruppo fare clic sulla foresta e sul dominio predefiniti, fare clic con il pulsante destro del mouse su **Impostazioni server DirectAccess** e quindi scegliere **Modifica**.  
  
3.  Fare clic su **Configurazioni computer**, su **Criteri**, su **Impostazioni di Windows**, su **Impostazioni di sicurezza**, su **Windows Firewall con sicurezza avanzata**, su **Windows Firewall con sicurezza avanzata** nel livello successivo e quindi su **Regole connessioni in entrata**. Fare clic con il pulsante destro del mouse su **DNS (Domain Name Server) (TCP-In)** e quindi scegliere **Proprietà**.  
  
4.  Fare clic sulla scheda **Ambito** e quindi aggiungere l'indirizzo IPv6 dell'interfaccia IP-HTTPS all'elenco **Indirizzo IP locale**.  
  
5.  Ripetere la stessa procedura per **DNS (Domain Name Server) (UDP-In)** .  
  
##  <a name="step-8-change-the-dns64-configuration-to-listen-to-the-ip-https-interface"></a><a name="BKMK_DNS64"></a>Passaggio 8: modificare la configurazione DNS64 per l'ascolto dell'interfaccia IP-HTTPS  
 È necessario modificare la configurazione DNS64 per l'ascolto dell'interfaccia IP-HTTPS mediante il comando di Windows PowerShell seguente:  
  
```powershell  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="step-9-reserve-ports-for-the-winnat-service"></a><a name="BKMK_ExemptPort"></a>Passaggio 9: riservare le porte per il servizio WinNat  
 Usare il comando Windows PowerShell seguente per riservare le porte per il servizio WinNat. Sostituire "192.168.1.100" con l'indirizzo IPv4 effettivo del server di Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Per evitare conflitti di porte con le applicazioni, assicurarsi che l'intervallo di porte riservato per il servizio WinNat non includa la porta 6602.  
  
##  <a name="step-10-restart-the-winnat-service"></a><a name="BKMK_WinNAT"></a>Passaggio 10: riavviare il servizio WinNat  
 Riavviare il servizio Driver NAT Windows (WinNat) eseguendo il comando Windows PowerShell seguente.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="appendix-set-up-directaccess-by-using-windows-powershell"></a><a name="BKMK_AppendixBPowerShellScript"></a>Appendice: configurare DirectAccess tramite Windows PowerShell  
 In questa sezione viene descritto come installare e configurare DirectAccess tramite Windows PowerShell.  
  
### <a name="preparation"></a>Preparazione  
 Prima di iniziare a configurare il server per DirectAccess, è necessario:  
  
1.  Attenersi alla procedura descritta in [passaggio 3: preparare un certificato e un record DNS per il server dei percorsi di rete](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) per registrare un certificato denominato **DirectAccess-NLS.contoso.com** (dove **contoso.com** viene sostituito dal nome di dominio interno effettivo) e per aggiungere un record DNS per il server dei percorsi di rete (NLS).  
  
2.  Aggiungere un gruppo di sicurezza denominato **DirectAccessClients** in Active Directory, e quindi aggiungere i computer client per i quali si desidera abilitare la funzionalità DirectAccess. Per ulteriori informazioni, vedere [passaggio 4: creare un gruppo di sicurezza per i computer client DirectAccess](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Commands  
  
> [!IMPORTANT]
>  In Windows Server Essentials non è necessario rimuovere l'oggetto Criteri di gruppo del prefisso IPv6 non necessario. Eliminare la sezione di codice con l'etichetta seguente: `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
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
