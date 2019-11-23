---
title: Passaggio 1 configurare l'infrastruttura di accesso remoto
description: Questo argomento fa parte della Guida relativa alla gestione remota dei client DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110696d9f1ff082cfae315632c78fddc14359d52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367316"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>Passaggio 1 configurare l'infrastruttura di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
In questo argomento viene descritto come configurare l'infrastruttura necessaria per una distribuzione avanzata di accesso remoto utilizzando un singolo server di accesso remoto in un ambiente misto IPv4 e IPv6. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [passaggio 1: pianificare l'infrastruttura di accesso remoto](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Attività|Descrizione|  
|----|--------|  
|Configurare le impostazioni di rete del server|Consente di configurare le impostazioni di rete del server nel server di Accesso remoto.|  
|Configurare il routing nella rete aziendale|Consente di configurare il routing nella rete aziendale per assicurarsi che il traffico sia indirizzato correttamente.|  
|Configurare i firewall|Consente di configurare i firewall aggiuntivi eventualmente necessari.|  
|Configurare CA e certificati|Configurare un'autorità di certificazione (CA), se necessario e altri modelli di certificato richiesto nella distribuzione.|  
|Configurare il server DNS|Consente di configurare le impostazioni DNS per il server di Accesso remoto.|  
|Configurare Active Directory|Aggiungere i computer client e server di accesso remoto al dominio Active Directory.|  
|Configurare gli oggetti Criteri di gruppo|Configurare oggetti Criteri di gruppo (GPO) per la distribuzione, se necessario.|  
|Configurare i gruppi di sicurezza|Consente di configurare i gruppi di sicurezza che conterranno i computer client DirectAccess e qualsiasi altro gruppo di sicurezza necessario nella distribuzione.|  
|Configurare il server dei percorsi di rete|Consente di configurare il server dei percorsi di rete, inclusa l'installazione del certificato del sito Web del server dei percorsi di rete.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigNetworkSettings"></a>Configurare le impostazioni di rete del server  
A seconda se si decide di collocare il server di accesso remoto alla periferia o dietro un dispositivo Network Address Translation (NAT), le seguenti impostazioni di indirizzi di interfaccia di rete sono necessari per una distribuzione a server singolo in un ambiente con IPv4 e IPv6. Tutti gli indirizzi IP vengono configurati tramite **modificare le impostazioni della scheda** nel **Windows Networking and Sharing Center**.  
  
**Topologia perimetrale**:  
  
È necessario quanto segue:  
  
-   Due con connessione Internet pubblico statico IPv4 o IPv6 indirizzi consecutivi.  
  
    > [!NOTE]  
    > Per Teredo sono necessari due indirizzi IPv4 pubblici consecutivi. Se non si usa Teredo, è possibile configurare un solo indirizzo statico pubblico IPv4.  
  
-   Un singolo indirizzo IPv4 o IPv6 statico pubblico interno.  
  
**Dietro il dispositivo NAT (due schede di rete)** :  
  
Richiede un interno con connessione alla rete statico indirizzo IPv4 o IPv6.  
  
**Dietro il dispositivo NAT (una scheda di rete)** :  
  
Richiede un singolo indirizzo IPv4 o IPv6.  
  
Se il server di accesso remoto dispone di due schede di rete (uno per il profilo di dominio e l'altro per un profilo pubblico o privato), ma si utilizza una topologia delle schede di rete, l'indicazione è come segue:  
  
1.  Assicurarsi che la seconda scheda di rete viene classificata anche nel profilo del dominio.  
  
2.  Se la seconda scheda di rete non può essere configurata per il profilo di dominio per qualsiasi motivo, il criterio IPsec di DirectAccess ambito deve essere impostato manualmente a tutti i profili utilizzando il seguente comando di Windows PowerShell:  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    I nomi dei criteri IPsec da utilizzare in questo comando sono **DirectAccess DaServerToInfra** e **DirectAccess-DaServerToCorp**.  
  
## <a name="BKMK_ConfigRouting"></a>Configurare il routing nella rete aziendale  
Configurare il routing nella rete aziendale come segue:  
  
-   Quando si distribuisce la connettività IPv6 nativa nell'organizzazione, aggiungere una route in modo che i router sulla rete interna reindirizzino il traffico IPv6 all'indietro con il server di Accesso remoto.  
  
-   Configurare manualmente le route IPv4 e IPv6 dell'organizzazione sui server di Accesso remoto. Aggiungere una route pubblicata in modo che tutto il traffico con un (/ 48) prefisso IPv6 viene inoltrato alla rete interna. In aggiunta, per il traffico IPv4, aggiungere route esplicite in modo che il traffico IPv4 venga inoltrato alla rete interna.  
  
## <a name="BKMK_ConfigFirewalls"></a>Configurare i firewall  
A seconda delle impostazioni di rete si è scelto, quando si usano altri firewall nella distribuzione, si applicano le seguenti eccezioni del firewall per il traffico di accesso remoto:  
  
### <a name="remote-access-server-on-ipv4-internet"></a>Server di accesso remoto nella rete Internet IPv4  
Si applicano le seguenti eccezioni del firewall a Internet per il traffico di accesso remoto quando il server di accesso remoto nella rete Internet IPv4:  
  
-   **Traffico Teredo**  
  
    Porta di destinazione UDP (User Datagram Protocol) 3544 in ingresso e porta UDP di origine 3544 in uscita. Applicare l'esenzione per entrambi gli indirizzi di IPv4 pubblici consecutivi con connessione Internet nel server di accesso remoto.  
  
-   **traffico 6to4**  
  
    Protocollo IP 41 in ingresso e in uscita. Applicare l'esenzione per entrambi gli indirizzi di IPv4 pubblici consecutivi con connessione Internet nel server di accesso remoto.  
  
-   **Traffico IP-HTTPS**  
  
    Porta di destinazione Transmission Control Protocol (TCP) 443 e porta TCP di origine 443 in uscita. Se il server di Accesso remoto dispone di un singolo adattatore di rete e il server dei percorsi di rete si trova sul server di Accesso remoto, è richiesta anche la porta TCP 62000. Applicare tali esenzioni solo per l'indirizzo a cui viene risolto il nome del server esterno.  
  
    > [!NOTE]  
    > Questa esenzione è configurata sul server di accesso remoto. Tutte le altre esenzioni vengono configurate sul firewall periferico.  
  
### <a name="remote-access-server-on-ipv6-internet"></a>Server di accesso remoto nella rete Internet IPv6  
Si applicano le seguenti eccezioni del firewall a Internet per il traffico di accesso remoto quando il server di accesso remoto nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
-   Internet Control Message Protocol per IPv6 (ICMPv6) il traffico in ingresso e in uscita - per solo le implementazioni Teredo.  
  
### <a name="remote-access-traffic"></a>Traffico di accesso remoto  
Si applicano le seguenti eccezioni firewall di rete interna per il traffico di accesso remoto:  
  
-   ISATAP: Protocollo 41 in entrata e in uscita  
  
-   TCP/UDP per tutto il traffico IPv4 o IPv6  
  
-   ICMP per tutto il traffico IPv4 o IPv6  
  
## <a name="BKMK_ConfigCAs"></a>Configurare CA e certificati  
Con accesso remoto in Windows Server 2012, è possibile scegliere tra l'utilizzo di certificati per l'autenticazione del computer o tramite un'autenticazione Kerberos incorporata che utilizza i nomi utente e password. È inoltre necessario configurare un certificato IP-HTTPS nel server di accesso remoto. In questa sezione viene illustrato come configurare questi certificati.  
  
Per informazioni sull'impostazione di un'infrastruttura a chiave pubblica (PKI), vedere [Servizi certificati Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="BKMK_ConfigIPsec"></a>Configurare l'autenticazione IPsec  
È necessario un certificato nel server di accesso remoto e tutti i client DirectAccess in modo da poter utilizzare l'autenticazione IPsec. Il certificato deve essere emesso da un'autorità di certificazione interna (CA). Server di accesso remoto e i client DirectAccess devono considerare attendibile la CA che rilascia i certificati intermedi e radice.  
  
##### <a name="to-configure-ipsec-authentication"></a>Per configurare l'autenticazione IPsec  
  
1.  Nella CA interna, decidere se si utilizza il modello di certificato di computer predefinito o se si crea un nuovo modello di certificato come descritto in [creazione di modelli di certificato](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Se si crea un nuovo modello, deve essere configurato per l'autenticazione client.  
  
2.  Distribuire il modello di certificato, se necessario. Per ulteriori informazioni, vedere [distribuzione di modelli di certificato](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Se necessario, configurare il modello per la registrazione automatica.  
  
4.  Se necessario, configurare la registrazione automatica del certificato. Per ulteriori informazioni, vedere [configurare la registrazione automatica del certificato](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="BKMK_ConfigCertTemp"></a>Configurare i modelli di certificato  
Quando si utilizza una CA interna per emettere certificati, è necessario configurare modelli di certificato per il certificato IP-HTTPS e il certificato di sito Web server percorsi di rete.  
  
##### <a name="to-configure-a-certificate-template"></a>Per configurare un modello di certificato  
  
1.  Nella CA interna creare un modello di certificato come descritto in [creazione di modelli di certificato](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Distribuire il modello di certificato come descritto in [distribuzione di modelli di certificato](https://technet.microsoft.com/library/cc770794.aspx).  
  
Dopo aver preparato i modelli, è possibile utilizzare per configurare i certificati. Vedere le procedure seguenti per informazioni dettagliate:  
  
-   [Configurare il certificato IP-HTTPS](#BKMK_IPHTTPS)  
  
-   [Configurare il server dei percorsi di rete](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>Configurare il certificato IP-HTTPS  
Accesso remoto richiede un certificato IP-HTTPS per autenticare le connessioni IP-HTTPS al server di Accesso remoto. Sono disponibili tre opzioni per il certificato IP-HTTPS:  
  
-   **Pubblico**  
  
    Forniti da terze parti.  
  
-   **Privata**  
  
    Il certificato è basato sul modello di certificato creato in [configurazione di modelli di certificato](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp). Viene richiesto, un certificato revoche di certificati (CRL) punto di distribuzione raggiungibile da un nome di dominio COMPLETO risolvibile pubblicamente.  
  
-   **Con firma automatica**  
  
    Questo certificato richiede un punto di distribuzione CRL raggiungibile da un nome di dominio COMPLETO risolvibile pubblicamente.  
  
    > [!NOTE]  
    > I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
Assicurarsi che il certificato del sito Web usato per l'autenticazione IP-HTTPS del server soddisfi i requisiti seguenti:  
  
-   Il nome soggetto del certificato deve essere il nome di dominio completo (FQDN) risolvibile esternamente e dell'URL IP-HTTPS (indirizzo ConnectTo) che viene utilizzato solo per le connessioni IP-HTTPS del server Accesso remoto.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Nel campo soggetto, specificare l'indirizzo IPv4 della scheda esterna del server di accesso remoto o il nome di dominio COMPLETO dell'URL IP-HTTPS.  
  
-   Per il **Enhanced Key Usage** campo, utilizzare l'identificatore di oggetto (OID) di autenticazione Server.  
  
-   Per il campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Per installare il certificato IP-HTTPS da una CA interna  
  
1.  Nel server di accesso remoto: sul **avviare** digitare**mmc.exe**, e quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare doppio clic su **certificati**, scegliere **tutte le attività**, fare clic su **Richiedi nuovo certificato**, quindi fare clic su **Avanti** due volte...  
  
6.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato creato per la configurazione di modelli di certificato e se necessario, fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
7.  Nel **Proprietà certificato** della finestra di dialogo di **soggetto** nella scheda il **nome soggetto** area, in **tipo**, selezionare **nome comune**.  
  
8.  In **valore**, specificare l'indirizzo IPv4 della scheda esterna del server di accesso remoto o il nome di dominio COMPLETO dell'URL IP-HTTPS e quindi fare clic su **Aggiungi**.  
  
9. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
10. In **valore**, specificare l'indirizzo IPv4 della scheda esterna del server di accesso remoto o il nome di dominio COMPLETO dell'URL IP-HTTPS e quindi fare clic su **Aggiungi**.  
  
11. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
12. Nel **estensioni** accanto alla scheda **utilizzo chiave esteso**, fare clic sulla freccia e assicurarsi che l'autenticazione Server è nel **selezione di opzioni** elenco.  
  
13. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
14. Nel riquadro dei dettagli dello snap-in certificati, verificare che il nuovo certificato è stato registrato con lo scopo di autenticazione server.  
  
## <a name="BKMK_ConfigDNS"></a>Configurare il server DNS  
È necessario configurare manualmente una voce DNS per il sito Web del server dei percorsi di rete per la rete interna nella distribuzione.  
  
### <a name="NLS_DNS"></a>Per aggiungere il server dei percorsi di rete e il probe Web  
  
1.  Nel server DNS interni: sul **avviare** digitare**dnsmgmt. msc**, e quindi premere INVIO.  
  
2.  Nel riquadro a sinistra della console **Gestore DNS** espandere la zona di ricerca diretta per il proprio dominio. Il pulsante destro del dominio e fare clic su **Nuovo Host (A o AAAA)** .  
  
3.  Nel **Nuovo Host** della finestra di dialogo di **nome (utilizza nome del dominio padre se vuoto)** immettere il nome DNS per il sito Web server percorsi di rete (questo è il nome utilizzano dai client DirectAccess di connettersi al server dei percorsi di rete). Nel **indirizzo IP** immettere l'indirizzo IPv4 del server dei percorsi di rete e scegliere **Aggiungi Host**, quindi fare clic su **OK**.  
  
4.  Nel **Nuovo Host** della finestra di dialogo di **nome (utilizza nome del dominio padre se vuoto)** immettere il nome DNS del probe web (il nome del probe web predefinito è directaccess-webprobehost). Nella casella **Indirizzo IP** immettere l'indirizzo IPv4 del probe Web e quindi fare clic su **Aggiungi host**.  
  
5.  Ripetere questa procedura per directaccess-corpconnectivityhost e per qualsiasi strumento di verifica della connettività creato manualmente. Nel **DNS** la finestra di dialogo, fare clic su **OK**.  
  
6.  Fare clic su **Fine**.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Le voci DNS devono essere configurate anche per quanto segue:  
  
-   **Server IP-HTTPS**  
  
    I client DirectAccess devono essere in grado di risolvere il nome DNS del server di accesso remoto da Internet.  
  
-   **Verifica revoche CRL**  
  
    DirectAccess utilizza revoche del certificato per la connessione IP-HTTPS tra i client DirectAccess e il server di accesso remoto e per la connessione basata su HTTPS tra il client DirectAccess e il server dei percorsi di rete. In entrambi i casi, i client DirectAccess devono poter risolvere e accedere al percorso del punto di distribuzione CRL.  
  
-   **ISATAP**  
  
    All'interno del sito ISATAP Automatic Tunnel Addressing Protocol () Usa tunnel per consentire ai client DirectAccess di connettersi al server di accesso remoto tramite la rete Internet IPv4, incapsulando i pacchetti IPv6 all'interno di un'intestazione IPv4. Viene usato da Accesso remoto per fornire la connettività IPv6 agli host ISATAP in una Intranet. In un ambiente di rete IPv6 non nativo, il server di accesso remoto configura automaticamente come router ISATAP. Per il nome ISATAP è richiesto il supporto della risoluzione.  
  
## <a name="BKMK_ConfigAD"></a>Configurare Active Directory  
Il server di Accesso remoto e tutti i computer client DirectAccess devono appartenere a un dominio Active Directory. I computer client DirectAccess devono essere membri di uno dei seguenti tipi di dominio:  
  
-   Domini appartenenti alla stessa foresta del server di Accesso remoto.  
  
-   Domini appartenenti alle foreste con un trust bidirezionale con la foresta del server di Accesso remoto.  
  
-   Domini che abbiano un trust bidirezionale al dominio del server di Accesso remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Per aggiungere il server di accesso remoto a un dominio  
  
1.  In Server Manager fare clic su **Server locale**. Nel riquadro dei dettagli fare clic sul collegamento accanto a **Nome computer**.  
  
2.  Nella finestra di dialogo **Proprietà del sistema** , fare clic sulla scheda **Nome computer** , quindi su **Cambia**.  
  
3.  Nel **nome Computer** digitare il nome del computer se si modifica anche il nome del computer quando si aggiunge il server al dominio. In **membro del**, fare clic su **dominio**, quindi digitare il nome del dominio a cui si desidera aggiungere il server, (ad esempio, corp.contoso.com) e quindi fare clic su **OK**.  
  
4.  Quando viene chiesto di immettere un nome utente e password, immettere il nome utente e password di un utente con autorizzazioni per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Per aggiungere i computer client al dominio  
  
1.  Nel **avviare** digitare**explorer.exe**, quindi premere INVIO.  
  
2.  Pulsante destro del mouse sull'icona del Computer e quindi fare clic su **proprietà**.  
  
3.  Nella pagina **Sistema** fare clic su **Impostazioni di sistema avanzate**.  
  
4.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
5.  Nel **nome Computer** digitare il nome del computer se si modifica anche il nome del computer quando si aggiunge il server al dominio. In **membro del**, fare clic su **dominio**, quindi digitare il nome del dominio a cui si desidera aggiungere il server (ad esempio, corp.contoso.com) e quindi fare clic su **OK**.  
  
6.  Quando viene chiesto di immettere un nome utente e password, immettere il nome utente e password di un utente con autorizzazioni per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo di benvenuto, fare clic su **OK**.  
  
8.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
9. Nel **le proprietà di sistema** la finestra di dialogo, fare clic su Chiudi.  
  
10. Fare clic su **Riavvia** quando richiesto.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
> [!NOTE]  
> Dopo avere immesso il comando seguente, è necessario fornire le credenziali di dominio.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>Configurare oggetti Criteri di gruppo  
Per distribuire accesso remoto, è necessario un minimo di due oggetti Criteri di gruppo. Un oggetto Criteri di gruppo contiene impostazioni per il server di accesso remoto e uno contenente le impostazioni per i computer client DirectAccess. Quando si configura accesso remoto, la procedura guidata crea automaticamente gli oggetti Criteri di gruppo necessari. Tuttavia, se l'organizzazione applica una convenzione di denominazione o non avere le autorizzazioni necessarie per creare o modificare oggetti Criteri di gruppo, è necessario creare prima di configurare accesso remoto.  
  
Per creare oggetti Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
Un amministratore può collegare manualmente gli oggetti Criteri di gruppo DirectAccess a un'unità organizzativa (OU). Considerare quanto segue:  
  
1.  Prima di configurare DirectAccess collegare oggetti Criteri di gruppo creato le rispettive unità organizzative.  
  
2.  Quando si configura DirectAccess specificare un gruppo di sicurezza per i computer client.  
  
3.  Oggetti Criteri di gruppo sono configurati automaticamente, indipendentemente dal fatto se l'amministratore dispone delle autorizzazioni per collegare oggetti Criteri di gruppo al dominio.  
  
4.  Se tali oggetti sono già collegati a un'unità Organizzativa, i collegamenti non verranno rimossi, ma non collegati al dominio.  
  
5.  Per oggetti Criteri di gruppo di server, l'unità Organizzativa deve contenere i server computer oggetto, in caso contrario, l'oggetto Criteri di gruppo verrà collegato alla radice del dominio.  
  
6.  Se l'unità Organizzativa non è stato collegato in precedenza eseguendo la configurazione guidata DirectAccess, una volta completata la configurazione, l'amministratore può collegare oggetti Criteri di gruppo DirectAccess alle unità organizzative necessarie e rimuovere il collegamento al dominio.  
  
    Per ulteriori informazioni, vedere [collegare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se un oggetto Criteri di gruppo è stato creato manualmente, è possibile che l'oggetto Criteri di gruppo non sarà disponibile durante la configurazione di DirectAccess. L'oggetto Criteri di gruppo potrebbe non essere stato replicato al controller di dominio più vicino al computer di gestione. L'amministratore può attendere il completamento o forzare la replica della replica.  
  
## <a name="BKMK_ConfigSGs"></a>Configurare i gruppi di sicurezza  
Le impostazioni di DirectAccess contenute nell'oggetto Criteri di gruppo di computer client vengono applicate solo ai computer che sono membri dei gruppi di sicurezza specificati durante la configurazione di accesso remoto.  
  
### <a name="Sec_Group"></a>Per creare un gruppo di sicurezza per i client DirectAccess  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO.  
  
2.  Nel **Active Directory Users and Computers** console, nel riquadro sinistro, espandere il dominio che contiene il gruppo di sicurezza, fare doppio clic su **utenti**, scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
3.  Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, immettere il nome del gruppo di sicurezza.  
  
4.  In **Ambito del gruppo** fare clic su **Globale**, in **Tipo gruppo** fare clic su **Sicurezza** e quindi fare clic su **OK**.  
  
5.  Fare doppio clic sul gruppo di sicurezza computer client DirectAccess e la **proprietà** nella finestra di dialogo fare clic su di **membri** scheda.  
  
6.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Selezionare utenti, contatti, computer o account di servizio** selezionare i computer client che si vogliono abilitare per DirectAccess e quindi fare clic su **OK**.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell** per Windows PowerShell  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>Configurare il server dei percorsi di rete  
Il server dei percorsi di rete deve essere in un server con disponibilità elevata e richiede un certificato Secure Sockets Layer (SSL) valido considerato attendibile dai client DirectAccess.  
  
> [!NOTE]  
> Se server dei percorsi di rete si trova nel server di accesso remoto, verrà automaticamente creato un sito Web quando si configura accesso remoto ed è associato al certificato del server fornito.  
  
Sono disponibili due opzioni per il certificato del server dei percorsi di rete:  
  
-   **Privata**  
  
    > [!NOTE]  
    > Il certificato è basato sul modello di certificato creato in [configurazione di modelli di certificato](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp).  
  
-   **Con firma automatica**  
  
    > [!NOTE]  
    > I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
Se si utilizza un certificato privato o un certificato autofirmato, è necessario quanto segue:  
  
-   Un certificato del sito Web usato per il server dei percorsi di rete. Il soggetto del certificato deve essere l'URL del server dei percorsi di rete.  
  
-   Un punto di distribuzione CRL con disponibilità elevata nella rete interna.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Per installare il certificato del server dei percorsi di rete da una CA interna  
  
1.  Sul server che ospiterà il sito Web server percorsi di rete: nel **avviare** digitare**mmc.exe**, quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare doppio clic su **certificati**, scegliere **tutte le attività**, fare clic su **Richiedi nuovo certificato**, quindi fare clic su **Avanti** due volte.  
  
6.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato creato per la configurazione di modelli di certificato e se necessario, fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
7.  Nel **Proprietà certificato** della finestra di dialogo di **soggetto** nella scheda il **nome soggetto** area, in **tipo**, selezionare **nome comune**.  
  
8.  In **valore**, immettere il nome FQDN del server dei percorsi di rete e quindi fare clic su **Aggiungi**.  
  
9. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
10. In **valore**, immettere il nome FQDN del server dei percorsi di rete e quindi fare clic su **Aggiungi**.  
  
11. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
12. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
13. Nel riquadro dei dettagli dello snap-in certificati, verificare che il nuovo certificato è stato registrato con l'obiettivo di autenticazione server.  
  
#### <a name="to-configure-the-network-location-server"></a>Per configurare il server dei percorsi di rete  
  
1.  Impostare un sito Web in un server a disponibilità elevata. Il sito Web non richiede contenuti, ma quando viene testato è possibile definire una pagina predefinita che fornisca un messaggio al momento della connessione dei client.  
  
    Questo passaggio non è obbligatorio se server dei percorsi di rete è ospitato nel server di accesso remoto.  
  
2.  Associare un certificato del server HTTPS al sito Web. Il nome comune del certificato deve corrispondere al nome del sito del server dei percorsi di rete. Verificare che il client DirectAccess consideri attendibile la CA emittente.  
  
    Questo passaggio non è obbligatorio se server dei percorsi di rete è ospitato nel server di accesso remoto.  
  
3.  Configurare un sito CRL con disponibilità elevata nella rete interna.  
  
    I punti di distribuzione Elenco di revoche di certificati (CRL) sono accessibili da:  
  
    -   Server Web che utilizzano un URL basato su HTTP, ad esempio: https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   File server che si accede tramite un percorso universal naming convention (UNC), ad esempio \\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    Se il punto di distribuzione CRL interno è raggiungibile solo su IPv6, è necessario configurare Windows Firewall con una regola di sicurezza della connessione di sicurezza avanzata. Ciò esonera protezione IPsec dallo spazio degli indirizzi IPv6 della intranet agli indirizzi IPv6 dei punti di distribuzione CRL.  
  
4.  Assicurarsi che i client DirectAccess nella rete interna possono risolvere il nome del server dei percorsi di rete e che i client DirectAccess su Internet non possono risolvere il nome.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 2: configurare il server di accesso remoto](Step-2-Configure-the-Remote-Access-Server.md)

