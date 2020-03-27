---
title: Passaggio 1 configurare l'infrastruttura DirectAccess
description: Questo argomento fa parte della guida aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 09cfde5bdd9e213e166345fb6844dcff08275b3f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314762"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>Passaggio 1 configurare l'infrastruttura DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare l'infrastruttura necessaria per l'abilitazione di DirectAccess in una distribuzione VPN esistente. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [passaggio 1: pianificare infrastruttura DirectAccess](Step-1-Plan-DirectAccess-Infrastructure.md).  
  
|Attività|Descrizione|  
|----|--------|  
|Configurare le impostazioni di rete del server|Consente di configurare le impostazioni di rete del server nel server di Accesso remoto.|  
|Configurare il routing nella rete aziendale|Consente di configurare il routing nella rete aziendale per assicurarsi che il traffico sia indirizzato correttamente.|  
|Configurare i firewall|Consente di configurare i firewall aggiuntivi eventualmente necessari.|  
|Configurare CA e certificati|L'Abilitazione guidata DirectAccess consente di configurare un proxy Kerberos predefinito per autenticare l'uso dei nome utente e delle password. Consente di configurare anche un certificato IP-HTTPS sul server di Accesso remoto.|  
|Configurare il server DNS|Consente di configurare le impostazioni DNS per il server di Accesso remoto.|  
|Configurare Active Directory|Consente di aggiungere i computer client al dominio di Active Directory.|  
|Configurare gli oggetti Criteri di gruppo|Consente di configurare gli oggetti Criteri di gruppo per la distribuzione, se necessario.|  
|Configurare i gruppi di sicurezza|Consente di configurare i gruppi di sicurezza che conterranno i computer client DirectAccess e qualsiasi altro gruppo di sicurezza necessario nella distribuzione.|  
|Configurare il server dei percorsi di rete|L'Abilitazione guidata DirectAccess consente di configurare il server dei percorsi di rete sul server DirectAccess.|  
  
## <a name="configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>Configurare le impostazioni di rete del server  
Le seguenti impostazioni dell'interfaccia di rete sono necessarie per la distribuzione di un singolo server in un ambiente con IPv4 e IPv6. Tutti gli indirizzi IP vengono configurati usando **Modifica impostazioni scheda** in **Centro connessioni di rete e condivisione di Windows**.  
  
-   Topologia perimetrale  
  
    -   Un indirizzo IPv4 o IPv6 statico pubblico con connessione Internet.  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico pubblico interno.  
  
-   Dietro il dispositivo NAT (due schede di rete)  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico pubblico con connessione alla rete interna.  
  
-   Dietro il dispositivo NAT (una scheda di rete)  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico.  
  
> [!NOTE]  
> Nel caso in cui il server di Accesso remoto abbia due schede di rete (una classificata nel profilo del dominio e l'altra in un profilo pubblico/privato), ma sia in uso una singola topologia NIC, è consigliabile procedere come segue:  
>   
> 1.  Assicurarsi che anche il secondo NIC sia classificato nel profilo del dominio (consigliato).  
> 2.  Se per un motivo qualsiasi non fosse possibile configurare il secondo NIC per il profilo del dominio, l'ambito del criterio IPSec di DirectAccess deve essere impostato manualmente a tutti i profili con i comandi seguenti di Windows Powershell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>Configurare il routing nella rete aziendale  
Configurare il routing nella rete aziendale come segue:  
  
-   Quando si distribuisce la connettività IPv6 nativa nell'organizzazione, aggiungere una route in modo che i router sulla rete interna reindirizzino il traffico IPv6 all'indietro con il server di Accesso remoto.  
  
-   Configurare manualmente le route IPv4 e IPv6 dell'organizzazione sui server di Accesso remoto. Aggiungere una route pubblicata in modo che tutto il traffico con il prefisso IPv6 di un'organizzazione (/48) sia inoltrato alla rete interna. In aggiunta, per il traffico IPv4, aggiungere route esplicite in modo che il traffico IPv4 venga inoltrato alla rete interna.  
  
## <a name="configure-firewalls"></a><a name="ConfigFirewalls"></a>Configurare i firewall  
Se si utilizzano altri firewall nella propria distribuzione, applicare le eccezioni firewall con connessione Internet seguenti per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete IPv4 Internet:  
  
-   Protocollo IP traffico 6to4 41 in entrata e in uscita.  
  
-   Porta di destinazione IP-HTTPS-protocollo TCP (Transmission Control) 443 e porta TCP di origine 443 in uscita. Se il server di Accesso remoto dispone di un singolo adattatore di rete e il server dei percorsi di rete si trova sul server di Accesso remoto, è richiesta anche la porta TCP 62000.  
  
Se si utilizzano altri firewall, applicare le eccezioni firewall con connessione Internet seguenti per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete IPv6 Internet:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
Quando si utilizzano altri firewall, applicare le eccezioni firewall delle rete interna seguenti per il traffico di Accesso remoto:  
  
-   ISATAP-protocollo 41 in entrata e in uscita  
  
-   TCP/UDP per tutto il traffico IPv4/IPv6  
  
## <a name="configure-cas-and-certificates"></a><a name="ConfigCAs"></a>Configurare CA e certificati  
L'Abilitazione guidata DirectAccess consente di configurare un proxy Kerberos predefinito per autenticare l'uso dei nome utente e delle password. Consente di configurare anche un certificato IP-HTTPS sul server di Accesso remoto.  
  
### <a name="configure-certificate-templates"></a><a name="ConfigCertTemp"></a>Configurare i modelli di certificato  
Quando si usa una CA interna per l'emissione di certificati, è necessario configurare un modello di certificato per il certificato IP-HTTPS e il certificato del sito Web del server dei percorsi di rete.  
  
##### <a name="to-configure-a-certificate-template"></a>Per configurare un modello di certificato  
  
1.  Nella CA interna creare un modello di certificato come descritto in [creazione di modelli di certificato](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Distribuire il modello di certificato come descritto in [distribuzione di modelli di certificato](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="configure-the-ip-https-certificate"></a>Configurare il certificato IP-HTTPS  
Accesso remoto richiede un certificato IP-HTTPS per autenticare le connessioni IP-HTTPS al server di Accesso remoto. Sono disponibili tre opzioni per il certificato IP-HTTPS:  
  
-   **Pubblica**-fornito da 3 parti.  
  
    Un certificato usato per l'autenticazione IP-HTTPS. Nel caso in cui il nome del soggetto del certificato non fosse un carattere jolly, deve necessariamente essere l'URL del nome dominio completo risolvibile esternamente e usato solo per le connessioni IP-HTTPS del server di Accesso remoto.  
  
-   **Privato**-seguenti sono necessari, se non esistono già:  
  
    -   Un certificato di un sito Web usato per l'autenticazione IP-HTTPS. Il soggetto del certificato deve essere un nome di dominio completo (FQDN) risolvibile esternamente e raggiungibile da Internet.  
  
    -   Un punto di distribuzione dell'elenco di revoche di certificati (CRL) raggiungibile da un nome di dominio completo risolvibile pubblicamente.  
  
-   **Autofirmato**-seguenti sono necessari, se non esistono già:  
  
    > [!NOTE]  
    > I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
    -   Un certificato di un sito Web usato per l'autenticazione IP-HTTPS. Il soggetto del certificato deve essere un nome di dominio completo risolvibile esternamente e raggiungibile da Internet.  
  
    -   Un punto di distribuzione raggiungibile da un nome di dominio completo risolvibile pubblicamente.  
  
Assicurarsi che il certificato del sito Web usato per l'autenticazione IP-HTTPS del server soddisfi i requisiti seguenti:  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Nel campo Soggetto, specificare l'indirizzo IPv4 della scheda con connessione esterna del server di Accesso remoto o il nome di dominio completo dell'URL IP-HTTPS.  
  
-   Per il campo Utilizzo chiavi avanzato usare l'identificatore di oggetto (OID) di autenticazione server.  
  
-   Per il campo Punti di distribuzione Elenco di revoche di certificati (CRL), specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Per installare il certificato IP-HTTPS da una CA interna  
  
1.  Nel server di accesso remoto: sul **avviare** digitare**mmc.exe**, e quindi premere INVIO.  
  
2.  Nella console MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
3.  Nella finestra di dialogo **Aggiungi/Rimuovi snap-in**, fare clic su **Certificati**, **Aggiungi**, **Account del computer** e infine su **Avanti**. Fare clic su **Computer locale**, **Fine** e quindi su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti**.  
  
7.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato e se necessario, fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
8.  Nella scheda **Oggetto** della finestra di dialogo **Proprietà certificato** in **Nome soggetto** selezionare **Nome comune** per **Tipo**.  
  
9. Nel campo **Valore**, specificare l'indirizzo IPv4 della scheda con connessione esterna del server di Accesso remoto o il nome di dominio completo dell'URL IP-HTTPS e quindi fare clic su **Aggiungi**.  
  
10. Nella sezione **Nome alternativo** selezionare **DNS** per **Tipo**.  
  
11. Nel campo **Valore**, specificare l'indirizzo IPv4 della scheda con connessione esterna del server di Accesso remoto o il nome di dominio completo dell'URL IP-HTTPS e quindi fare clic su **Aggiungi**.  
  
12. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
13. Nella scheda **Estensioni**, accanto a **Utilizzo chiave esteso**, fare clic sulla freccia e assicurarsi che nell'elenco **Opzioni selezionate** sia presente Autenticazione server.  
  
14. Fare clic su **OK**, su **Registrazione** e quindi su **Fine**.  
  
15. Nel riquadro dei dettagli dello snap-in Certificati verificare che il nuovo certificato sia registrato come Autenticazione server in Scopi designati.  
  
## <a name="configure-the-dns-server"></a><a name="ConfigDNS"></a>Configurare il server DNS  
È necessario configurare manualmente una voce DNS per il sito Web del server dei percorsi di rete per la rete interna nella distribuzione.  
  
### <a name="to-create-the-network-location-server-and-web-probe-dns-records"></a><a name="NLS_DNS"></a>Per creare i record DNS del server dei percorsi di rete e del probe Web  
  
1.  Nel server DNS della rete interna: nella schermata **Start** Digitare * * dnsmgmt. msc * *, quindi premere INVIO.  
  
2.  Nel riquadro a sinistra della console **Gestore DNS** espandere la zona di ricerca diretta per il proprio dominio. Fare clic con il pulsante destro del mouse sul dominio e fare clic su **Nuovo host (A o AAAA)** .  
  
3.  Nella finestra di dialogo **Nuovo host**, nella casella **Nome (se vuoto, utilizza il nome del dominio padre)** , immettere il nome DNS del sito Web del server dei percorsi di rete (cioè il nome usato dai client DirectAccess per connettersi al server dei percorsi di rete). Nella casella **Indirizzo IP** immettere l'indirizzo IPv4 del server dei percorsi di rete e quindi fare clic su **Aggiungi host**. Nella finestra di dialogo **DNS** fare clic su **OK**.  
  
4.  Nella finestra di dialogo **Nuovo host**, nella casella **Nome (se vuoto, utilizza il nome del dominio padre)** , immettere il nome DNS del probe Web (il nome del probe Web predefinito è directaccess-webprobehost). Nella casella **Indirizzo IP** immettere l'indirizzo IPv4 del probe Web e quindi fare clic su **Aggiungi host**. Ripetere questa procedura per directaccess-corpconnectivityhost e per qualsiasi strumento di verifica della connettività creato manualmente. Nella finestra di dialogo **DNS** fare clic su **OK**.  
  
5.  Fare clic su **Done**.  

![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Le voci DNS devono essere configurate anche per quanto segue:  
  
-   **Il server IP-HTTPS**i client DirectAccess - devono essere in grado di risolvere il nome DNS del server di accesso remoto da Internet.  
  
-   **Rilevamento delle revoche CRL**- DirectAccess utilizza revoche del certificato per la connessione IP-HTTPS tra i client DirectAccess e il server di accesso remoto e per la connessione basata su HTTPS tra il client DirectAccess e il server dei percorsi di rete. In entrambi i casi, i client DirectAccess devono poter risolvere e accedere al percorso del punto di distribuzione CRL.  
  
## <a name="configure-active-directory"></a><a name="ConfigAD"></a>Configurare Active Directory  
Il server di Accesso remoto e tutti i computer client DirectAccess devono appartenere a un dominio Active Directory. I computer client DirectAccess devono essere membri di uno dei seguenti tipi di dominio:  
  
-   Domini appartenenti alla stessa foresta del server di Accesso remoto.  
  
-   Domini appartenenti alle foreste con un trust bidirezionale con la foresta del server di Accesso remoto.  
  
-   Domini che abbiano un trust bidirezionale al dominio del server di Accesso remoto.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Per aggiungere i computer client al dominio  
  
1.  Nel **avviare** digitare **explorer.exe**, quindi premere INVIO.  
  
2.  Fare clic con il pulsante destro sull'icona Computer e quindi scegliere **Proprietà**.  
  
3.  Nella pagina **Sistema** fare clic su **Impostazioni di sistema avanzate**.  
  
4.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
5.  Per cambiare anche il nome computer quando si aggiunge il server al dominio, digitarlo in **Nome computer**. In **Membro di** fare clic su **Dominio** e quindi digitare il nome del dominio a cui si desidera aggiungere il server, ad esempio corp.contoso.com, e quindi fare clic su **OK**.  
  
6.  Quando viene richiesta l'immissione di nome utente e password, digitare il nome utente e la password di un utente che disponga delle autorizzazioni necessarie per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo di benvenuto, fare clic su **OK**.  
  
8.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
9. Nella finestra di dialogo **Proprietà del sistema** fare clic su Chiudi. Quando viene richiesto, fare clic su **Riavvia ora**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Si noti che è necessario specificare le credenziali di dominio dopo aver immesso il comando Add-Computer seguente.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="configure-gpos"></a><a name="ConfigGPOs"></a>Configurare oggetti Criteri di gruppo  
Per distribuire accesso remoto, è necessario un minimo di due oggetti Criteri di gruppo: un oggetto Criteri di gruppo contiene impostazioni per il server di accesso remoto e l'altro contiene le impostazioni per i computer client DirectAccess. Quando si configura accesso remoto, la procedura guidata crea automaticamente gli oggetti Criteri di gruppo necessari. Tuttavia, se l'organizzazione applica una convenzione di denominazione o non avere le autorizzazioni necessarie per creare o modificare oggetti Criteri di gruppo, è necessario creare prima di configurare accesso remoto.  
  
Per creare oggetti Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> L'amministratore può collegare manualmente gli oggetti Criteri di gruppo DirectAccess a un'unità organizzativa attenendosi alla procedura seguente:  
>   
> 1.  Prima di configurare DirectAccess collegare gli oggetti Criteri di gruppo alle rispettive unità organizzative.  
> 2.  Configurare DirectAccess specificando un gruppo di sicurezza per i computer client.  
> 3.  L'amministratore di accesso remoto può o non dispone delle autorizzazioni necessarie per collegare oggetti Criteri di gruppo al dominio. In ogni caso, gli oggetti Criteri di gruppo verranno configurati automaticamente. Se gli oggetti Criteri di gruppo sono già collegati a un'unità organizzativa, i collegamenti non verranno rimossi e gli oggetti Criteri di gruppo non verranno collegati al dominio. Per un oggetto Criteri di gruppo del server, l'unità organizzativa deve contenere l'oggetto computer server oppure l'oggetto Criteri di gruppo verrà collegato alla radice del dominio.  
> 4.  Se il collegamento all'unità Organizzativa non è stato eseguito prima di eseguire la procedura guidata DirectAccess, quindi dopo la configurazione è stata completata, l'amministratore di dominio possibile collegare gli oggetti Criteri di gruppo DirectAccess a unità organizzative richieste. Il collegamento al dominio può essere rimosso. I passaggi per il collegamento di un oggetto Criteri di gruppo per un'unità organizzativa sono reperibili [qui](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se un oggetto Criteri di gruppo è stato creato manualmente, è possibile durante la configurazione di DirectAccess che l'oggetto Criteri di gruppo non sarà disponibile. L'oggetto Criteri di gruppo potrebbe non essere stato replicato sul controller di dominio più vicino al computer di gestione. In questo caso, l'amministratore può attendere il completamento della replica o forzarla.  
  
## <a name="configure-security-groups"></a><a name="ConfigSGs"></a>Configurare i gruppi di sicurezza  
Le impostazioni di DirectAccess contenute nell'oggetto Criteri di gruppo di computer client vengono applicate solo ai computer che sono membri dei gruppi di sicurezza specificati durante la configurazione di accesso remoto. Inoltre, se si usano i gruppi di sicurezza per gestire i server applicazioni, creare un gruppo di sicurezza per tali server.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Per creare un gruppo di sicurezza per i client DirectAccess  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO. Nella console **Utenti e computer di Active Directory**, nel riquadro a sinistra, espandere il dominio che conterrà il gruppo di sicurezza, fare clic con il pulsante destro del mouse su **Utenti**, scegliere **Nuovo** e quindi fare clic su **Gruppo**.  
  
2.  Nella finestra di dialogo **Nuovo oggetto - Gruppo**, in **Nome gruppo**, immettere il nome del gruppo di sicurezza.  
  
3.  In **Ambito del gruppo** fare clic su **Globale**, in **Tipo gruppo** fare clic su **Sicurezza** e quindi fare clic su **OK**.  
  
4.  Fare doppio clic sul gruppo di sicurezza dei computer client DirectAccess e, nella finestra di dialogo delle proprietà, fare clic sulla scheda **Membri**.  
  
5.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
6.  Nella finestra di dialogo **Selezionare utenti, contatti, computer o account di servizio** selezionare i computer client che si vogliono abilitare per DirectAccess e quindi fare clic su **OK**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell** per Windows PowerShell  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="configure-the-network-location-server"></a><a name="ConfigNLS"></a>Configurare il server dei percorsi di rete  
Il server dei percorsi di rete deve trovarsi su un server con elevata disponibilità e un certificato SSL valido attendibile dei client DirectAccess. Sono disponibili due opzioni per il certificato del server dei percorsi di rete:  
  
-   **Privato**-seguenti sono necessari, se non esistono già:  
  
    -   Un certificato del sito Web usato per il server dei percorsi di rete. Il soggetto del certificato deve essere l'URL del server dei percorsi di rete.   
  
    -   Un punto di distribuzione CRL a disponibilità elevata nella rete interna.  
  
-   **Autofirmato**-seguenti sono necessari, se non esistono già:  
  
    > [!NOTE]  
    > I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
    -   Un certificato del sito Web usato per il server dei percorsi di rete. Il soggetto del certificato deve essere l'URL del server dei percorsi di rete.  
  
> [!NOTE]  
> Se il sito Web del server dei percorsi di rete si trova sul server di Accesso remoto, verrà creato automaticamente un sito Web durante la configurazione di Accesso remoto associato al certificato del server fornito dall'utente.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Per installare il certificato del server dei percorsi di rete da una CA interna  
  
1.  Sul server che ospiterà il sito Web server percorsi di rete: nel **avviare** digitare**mmc.exe**, quindi premere INVIO.  
  
2.  Nella console MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
3.  Nella finestra di dialogo **Aggiungi/Rimuovi snap-in**, fare clic su **Certificati**, **Aggiungi**, **Account del computer** e infine su **Avanti**. Fare clic su **Computer locale**, **Fine** e quindi su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti**.  
  
7.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato e se necessario, fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
8.  Nella scheda **Oggetto** della finestra di dialogo **Proprietà certificato** in **Nome soggetto** selezionare **Nome comune** per **Tipo**.  
  
9. In **Valore** immettere il nome di dominio completo (FQDN) del server dei percorsi di rete, quindi fare clic su **Aggiungi**.  
  
10. Nella sezione **Nome alternativo** selezionare **DNS** per **Tipo**.  
  
11. In **Valore** immettere il nome di dominio completo (FQDN) del server dei percorsi di rete, quindi fare clic su **Aggiungi**.  
  
12. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
13. Fare clic su **OK**, su **Registrazione** e quindi su **Fine**.  
  
14. Nel riquadro dei dettagli dello snap-in Certificati verificare che il nuovo certificato sia registrato come Autenticazione server in Scopi designati.  
  

  


