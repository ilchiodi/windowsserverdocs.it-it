---
title: Passaggio 1 configurare l'infrastruttura DirectAccess avanzato
description: Questo argomento fa parte della Guida di distribuire un DirectAccess Server singolo con Advanced le impostazioni per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: edd59208237f9b1042427dfecba0a407a34b30a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835812"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>Passaggio 1 configurare l'infrastruttura DirectAccess avanzato

>Si applica a: Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come configurare l'infrastruttura richiesta per una distribuzione di Accesso remoto avanzata che usa un server DirectAccess singolo in un ambiente misto IPv4 e IPv6. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [pianificare una distribuzione avanzata DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Attività|Descrizione|  
|----|--------|  
|1.1 Configurare le impostazioni di rete del server|Consente di configurare le impostazioni di rete del server nel server DirectAccess.|  
|1.2 Configurare il tunneling forzato|Consente di configurare il tunneling forzato.|  
|1.3 Configurare il routing nella rete aziendale|Consente di configurare il routing nella rete aziendale.|  
|1.4 Configurare i firewall|Consente di configurare i firewall aggiuntivi eventualmente necessari.|  
|1.5 Configurare CA e certificati|Consente di configurare un'autorità di certificazione (CA), se necessaria, ed eventuali altri modelli di certificato richiesti nella distribuzione.|  
|1.6 Configurare il server DNS|Consente di configurare le impostazioni DNS (Domain Name System) per il server DirectAccess.|  
|1.7 Configurare Active Directory|Consente di aggiungere i computer client e il server DirectAccess al dominio Active Directory.|  
|1.8 Configurare gli oggetti Criteri di gruppo|Consente di configurare gli oggetti Criteri di gruppo per la distribuzione, se necessario.|  
|1.9 Configurare i gruppi di sicurezza|Consente di configurare i gruppi di sicurezza che conterranno i computer client DirectAccess e qualsiasi altro gruppo di sicurezza necessario nella distribuzione.|  
|1.10 Configurare il server dei percorsi di rete|Consente di configurare il server dei percorsi di rete, inclusa l'installazione del certificato del sito Web del server dei percorsi di rete.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="ConfigNetworkSettings"></a>1.1 configurare le impostazioni di rete di server  
Le seguenti impostazioni dell'interfaccia di rete sono necessarie per la distribuzione di un singolo server in un ambiente con IPv4 e IPv6. Tutti gli indirizzi IP vengono configurati tramite **modificare le impostazioni della scheda** nel **Windows Networking and Sharing Center**.  
  
**Topologia perimetrale**  
  
-   Due indirizzi IPv4 o IPv6 statici pubblici consecutivi con connessione Internet  
  
    > [!NOTE]  
    > Per Teredo sono necessari due indirizzi pubblici. Se non si usa Teredo, è possibile configurare un solo indirizzo statico pubblico IPv4.  
  
-   Un indirizzo IPv4 o IPv6 statico interno  
  
**Dietro il dispositivo NAT (con due schede di rete)**  
  
-   Un indirizzo IPv4 o IPv6 statico con connessione Internet  
  
-   Un indirizzo IPv4 o IPv6 statico con connessione alla rete interna  
  
**Dietro il dispositivo NAT (con una scheda di rete)**  
  
-   Un indirizzo IPv4 o IPv6 statico con connessione alla rete interna  
  
> [!NOTE]  
> Nel caso in cui un server DirectAccess con due o più schede di rete (una classificata nel profilo del dominio e l'altra in un profilo pubblico o privato), sia configurato con una singola topologia delle schede di rete, è consigliabile procedere come segue:  
>   
> -   Verificare che la seconda scheda di rete ed eventuali altre schede di rete siano classificate nel profilo del dominio.  
> -   Se la seconda scheda di rete non può essere configurata per il profilo di dominio, il criterio IPsec di DirectAccess ambito deve essere impostato manualmente a tutti i profili utilizzando il seguente comando di Windows PowerShell dopo aver configurato DirectAccess:  
>   
>     ```  
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any  
>     Save-NetGPO "GPOSession $gposession  
>     ```  
  
## <a name="BKMK_forcetunnel"></a>1.2 configurare il tunneling forzato  
Il tunneling forzato può essere configurato con la Configurazione guidata Accesso remoto, visualizzata come casella di controllo nella Configurazione guidata dei client remoti. Questa impostazione interessa solo i client DirectAccess. Se VPN è abilitato, i client VPN usano il tunneling forzato per impostazione predefinita. Gli amministratori possono modificare l'impostazione per i client VPN dal profilo del client.  
  
La selezione della casella di controllo per il tunneling forzato:  
  
-   Abilita il tunneling forzato nei client DirectAccess  
  
-   Aggiunge un **qualsiasi** voce nel criterio di tabella (Risoluzione dei nomi) per i client DirectAccess, il che significa che tutto il traffico DNS entra nei server DNS della rete interna  
  
-   Configura i client DirectAccess in modo che usino sempre la tecnologia di transizione IP-HTTPS  
  
Per rendere disponibili le risorse Internet ai client DirectAccess che usano il tunneling forzato, è possibile usare un server proxy che può ricevere richieste basate su IPv6 per le risorse Internet e tradurle in richieste per le risorse Internet basate per IPv4. Per configurare un server proxy per le risorse Internet, è necessario modificare la voce predefinita nella tabella dei criteri di risoluzione dei nomi per aggiungere il server proxy. A questo scopo, usare i cmdlet PowerShell di Accesso remoto o i cmdlet PowerShell DNS. Ad esempio, usare il cmdlet PowerShell di Accesso remoto come segue:  
  
```  
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>  
```  
  
> [!NOTE]  
> Se DirectAccess e VPN sono abilitati nello stesso server, VPN è in modalità di tunneling forzato e il server è distribuito in una topologia perimetrale o dietro una topologia NAT (con due schede di rete, una connessa al dominio e una a una rete privata), il traffico Internet VPN non può essere inoltrato attraverso l'interfaccia esterna del server DirectAccess. Per abilitare questo scenario, le organizzazioni devono distribuire Accesso remoto nel server protetto da firewall in una topologia con una sola scheda di rete. In alternativa, le organizzazioni possono usare un server proxy distinto nella rete interna per inoltrare il traffico Internet proveniente dai client VPN.  
  
> [!NOTE]  
> Se un'organizzazione sta usando un proxy Web per i client DirectAccess per accedere alle risorse Internet e il proxy aziendale non riesce a gestire le risorse della rete interna, i client DirectAccess non riusciranno ad accedere alle risorse interne se si trovano fuori dalla Intranet. In questo scenario, per abilitare i client DirectAccess ad accedere alle risorse interne, creare manualmente le voci NRPT per i suffissi della rete interna usando la pagina DNS della procedura guidata dell'infrastruttura. Non applicare le impostazioni proxy a questi suffissi NRPT. I suffissi devono essere popolati con le voci del server DNS predefinito.  
  
## <a name="ConfigRouting"></a>1.3 configurare il routing nella rete aziendale  
Configurare il routing nella rete aziendale come segue:  
  
-   Quando si distribuisce la connettività IPv6 nativa nell'organizzazione, aggiungere una route in modo che i router nella rete interna reindirizzino il traffico IPv6 all'indietro con il server DirectAccess.  
  
-   Configurare manualmente l'organizzazione "s route IPv4 e IPv6 nel server DirectAccess. Aggiungere una route pubblicata in modo che tutto il traffico con il prefisso IPv6 di un'organizzazione (/48) sia inoltrato alla rete interna. Per il traffico IPv4, aggiungere route esplicite in modo che il traffico IPv4 venga inoltrato alla rete interna.  
  
## <a name="ConfigFirewalls"></a>1.4 configurare i firewall  
Se si usano altri firewall nella propria distribuzione, applicare le seguenti eccezioni firewall con connessione Internet per il traffico di Accesso remoto quando il server DirectAccess si trova nella rete Internet IPv4:  
  
-   Il traffico Teredo "porta di destinazione di protocollo UDP (User Datagram) 3544 in entrata e porta UDP di origine 3544 in uscita.  
  
-   traffico 6to4 "protocollo 41 in entrata e in uscita.  
  
-   IP-HTTPS "porta di destinazione di controllo TCP (Transmission Protocol) 443 e porta TCP di origine 443 in uscita. Se il server DirectAccess ha una sola scheda di rete e il server dei percorsi di rete si trova sul server DirectAccess, è richiesta anche la porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione deve essere configurata sul server DirectAccess, diversamente da tutte le altre che devono essere configurate sul firewall periferico.  
  
> [!NOTE]  
> Per il traffico Teredo e 6to4, queste eccezioni vanno applicate a entrambi gli indirizzi Ipv4 pubblici consecutivi con connessione Internet nel server DirectAccess. Per IP-HTTPS le esenzioni devono essere applicate solo all'indirizzo nel quale viene risolto il nome pubblico del server.  
  
Se si usano altri firewall, applicare le seguenti eccezioni firewall con connessione Internet per il traffico di Accesso remoto quando il server DirectAccess si trova nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
-   Internet Control Message Protocol per IPv6 (ICMPv6) il traffico in ingresso e in uscita "per solo le implementazioni Teredo.  
  
Quando si utilizzano altri firewall, applicare le eccezioni firewall delle rete interna seguenti per il traffico di Accesso remoto:  
  
-   ISATAP "protocollo 41 in entrata e in uscita  
  
-   TCP/UDP per tutto il traffico IPv4/IPv6  
  
-   ICMP per tutto il traffico IPv4/IPv6  
  
## <a name="ConfigCAs"></a>1.5 configurare CA e certificati  
Accesso remoto in Windows Server 2012 consente di scegliere tra l'utilizzo di certificati per l'autenticazione del computer o tramite un proxy Kerberos viene autenticato mediante nomi utente e password. È necessario configurare anche un certificato IP-HTTPS nel server DirectAccess.  
  
Per ulteriori informazioni, vedere [Servizi certificati Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="151-configure-ipsec-authentication"></a>1.5.1 Configurare l'autenticazione IPsec  
È necessario un certificato del computer nel server DirectAccess e in tutti i client DirectAccess per usare l'autenticazione IPsec. Il certificato deve essere emesso da un'autorità di certificazione (CA) interna e i server e i client DirectAccess devono considerare attendibile la catena di CA che emette i certificati radice e intermedi.  
  
##### <a name="to-configure-ipsec-authentication"></a>Per configurare l'autenticazione IPsec  
  
1.  Nella CA interna, decidere se si utilizza il **Computer** del modello di certificato, o se si crea un nuovo modello di certificato come descritto in [creazione di modelli di certificato](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Se si crea un nuovo modello, deve essere configurato per l'autenticazione client.  
  
2.  Distribuire il modello di certificato, se necessario. Per ulteriori informazioni, vedere [distribuzione di modelli di certificato](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Configurare il modello di certificato per la registrazione automatica, se necessario. Per ulteriori informazioni, vedere [configurare la registrazione automatica del certificato](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="ConfigCertTemp"></a>1.5.2 configurare modelli di certificato  
Quando si usa una CA interna per l'emissione di certificati, è necessario configurare un modello di certificato per il certificato IP-HTTPS e il certificato del sito Web del server dei percorsi di rete.  
  
##### <a name="to-configure-a-certificate-template"></a>Per configurare un modello di certificato  
  
1.  Nella CA interna, creare un modello di certificato come descritto in [creazione di modelli di certificato](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Distribuire il modello di certificato come descritto in [distribuzione di modelli di certificato](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 Configurare il certificato IP-HTTPS  
Accesso remoto richiede un certificato IP-HTTPS per autenticare le connessioni IP-HTTPS al server DirectAccess. Sono disponibili tre opzioni di certificato per l'autenticazione IP-HTTPS:  
  
**Certificato pubblico**  
  
Un certificato pubblico viene fornito da terze parti. Se il nome soggetto del certificato non contiene caratteri jolly, deve essere l'URL del nome di dominio completo risolvibile esternamente usato solo per le IP-HTTPS del server DirectAccess.  
  
**Certificato privato**  
  
Se si usa un certificato privato, gli elementi seguenti sono necessari, se non esistono già:  
  
-   Un certificato del sito Web usato per l'autenticazione IP-HTTPS. Il soggetto del certificato deve essere un nome di dominio completo risolvibile esternamente raggiungibile da Internet. Il certificato è basato sul modello di certificato creato seguendo le istruzioni in 1.5.2 configurare modelli di certificato.  
  
-   Un punto di distribuzione dell'elenco di revoche di certificati (CRL) raggiungibile da un nome di dominio completo risolvibile pubblicamente.  
  
**Certificato autofirmato**  
  
Se si usa un certificato autofirmato, gli elementi seguenti sono necessari, se non esistono già:  
  
-   Un certificato del sito Web usato per l'autenticazione IP-HTTPS. Il soggetto del certificato deve essere un nome di dominio completo risolvibile esternamente raggiungibile da Internet.  
  
-   Un punto di distribuzione dell'elenco di revoche di certificati (CRL) raggiungibile da un nome di dominio completo risolvibile pubblicamente.  
  
> [!NOTE]  
> I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
Assicurarsi che il certificato del sito Web usato per l'autenticazione IP-HTTPS soddisfi i requisiti seguenti:  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Nel **soggetto** campo, specificare il nome FQDN dell'URL IP-HTTPS.  
  
-   Per il **Enhanced Key Usage** campo, utilizzare l'identificatore di oggetto (OID) autenticazione server.  
  
-   Per il campo **Punti di distribuzione Elenco di revoche di certificati (CRL)**, specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Per installare il certificato IP-HTTPS da una CA interna  
  
1.  Nel server DirectAccess: Nel **avviare** digitare**mmc.exe**, quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti** .  
  
7.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato creato in precedenza (per ulteriori informazioni, vedere 1.5.2 configurare modelli di certificato). Se necessario, **Sono necessarie ulteriori informazioni per registrare il certificato**.  
  
8.  Nel **Proprietà certificato** della finestra di dialogo di **soggetto** nella scheda il **nome soggetto** area, in **tipo**, selezionare **nome comune**.  
  
9. In **Valore**, specificare l'indirizzo IPv4 della scheda con connessione esterna del server DirectAccess o il nome di dominio completo dell'URL IP-HTTPS, quindi fare clic su **Add**.  
  
10. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
11. In **Valore**, specificare l'indirizzo IPv4 della scheda con connessione esterna del server DirectAccess o il nome di dominio completo dell'URL IP-HTTPS, quindi fare clic su **Add**.  
  
12. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
13. Nella scheda **Estensioni**, fare clic sulla freccia accanto a **Utilizzo chiave esteso** e assicurarsi che **Server autenticazione** sia visualizzato nell'elenco **Opzioni selezionate**.  
  
14. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
15. Nel riquadro dei dettagli dello snap-in Certificati verificare che il nuovo certificato sia registrato come Autenticazione server in Scopi designati.  
  
## <a name="ConfigDNS"></a>1.6 configurare il server DNS  
È necessario configurare manualmente una voce DNS per il sito Web del server dei percorsi di rete per la rete interna nella distribuzione.  
  
### <a name="NLS_DNS"></a>Per creare il server dei percorsi di rete  
  
1.  Nel server DNS della rete interna: Nel **avviare** digitare**dnsmgmt. msc**, quindi premere INVIO.  
  
2.  Nel riquadro a sinistra della console **Gestore DNS** espandere la zona di ricerca diretta per il proprio dominio. Fare clic con il pulsante destro del mouse sul dominio e fare clic su **Nuovo host (A o AAAA)**.  
  
3.  Nella casella **Indirizzo IP** della finestra di dialogo **Nuovo host**:  
  
    -   Nella casella **Nome (se vuoto, utilizza il nome del dominio padre)**, immettere il nome DNS del sito Web del server dei percorsi di rete (cioè il nome usato dai client DirectAccess per connettersi al server dei percorsi di rete).  
  
    -   Immettere l'indirizzo IPv4 o IPv6 del server dei percorsi di rete, quindi fare clic su **Aggiungi host** e **OK**.  
  
4.  Nella finestra di dialogo **Nuovo host**:  
  
    -   Nel **nome (utilizza nome del dominio padre se vuoto)** immettere il nome DNS del probe web (il nome del probe web predefinito è **directaccess-webprobehost**).  
  
    -   Nella casella **Indirizzo IP**, immettere l'indirizzo IPv4 o IPv6 del probe Web, quindi fare clic su **Aggiungi host**.  
  
    -   Ripetere questo processo per **directaccess-corpconnectivityhost** e qualsiasi strumento di verifica della connettività creato manualmente.  
  
5.  Nel **DNS** la finestra di dialogo, fare clic su **OK**, e quindi fare clic su **eseguita**.  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Le voci DNS devono essere configurate anche per quanto segue:  
  
-   **Il server IP-HTTPS**  
  
    I client DirectAccess devono poter risolvere il nome DNS del server DirectAccess da Internet.  
  
-   **Delle revoche CRL**  
  
    DirectAccess usa il rilevamento delle revoche del certificato per la connessione IP-HTTPS tra i client DirectAccess e il server DirectAccess e per la connessione basata su HTTPS tra il client DirectAccess e il server dei percorsi di rete. In entrambi i casi, i client DirectAccess devono poter risolvere e accedere al percorso del punto di distribuzione CRL.  
  
-   **ISATAP**  
  
    Il protocollo ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) usa il tunneling per abilitare i client DirectAccess per la connessione al server DirectAccess su Internet IPv4 , incapsulando i pacchetti IPv6 in un'intestazione IPv4. Viene usato da Accesso remoto per fornire la connettività IPv6 agli host ISATAP in una Intranet. In un ambiente di rete IPv6 non nativo, il server DirectAccess si configura automaticamente come router ISATAP. Per il nome ISATAP è richiesto il supporto della risoluzione.  
  
## <a name="ConfigAD"></a>1.7 configurare Active Directory  
Il server DirectAccess e tutti i computer client DirectAccess devono essere aggiunti a un dominio Active Directory. I computer client DirectAccess devono essere membri di uno dei seguenti tipi di dominio:  
  
-   Domini appartenenti alla stessa foresta del server DirectAccess.  
  
-   Domini appartenenti a foreste con un trust bidirezionale con la foresta del server DirectAccess.  
  
-   Domini che abbiano un trust bidirezionale al dominio del server DirectAccess.  
  
#### <a name="to-join-the-directaccess-server-to-a-domain"></a>Per aggiungere il server DirectAccess a un dominio  
  
1.  In Server Manager fare clic su **Server locale**. Nel riquadro dei dettagli fare clic sul collegamento accanto a **Nome computer**.  
  
2.  Nella finestra di dialogo **Proprietà del sistema** , fare clic sulla scheda **Nome computer** , quindi su **Cambia**.  
  
3.  Per cambiare anche il nome computer quando si aggiunge il server al dominio, digitarlo in **Nome computer**. In **membro del**, fare clic su **dominio**, quindi digitare il nome del dominio a cui si desidera aggiungere il server (ad esempio, corp.contoso.com) e quindi fare clic su **OK**.  
  
4.  Quando viene richiesta l'immissione di nome utente e password, digitare il nome utente e la password di un utente che disponga delle autorizzazioni necessarie per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo che consente di iniziare il dominio, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Per aggiungere i computer client al dominio  
  
1.  Nel **avviare** digitare**explorer.exe**, quindi premere INVIO.  
  
2.  Pulsante destro del mouse sull'icona del Computer e quindi fare clic su **proprietà**.  
  
3.  Nella pagina **Sistema** fare clic su **Impostazioni di sistema avanzate**.  
  
4.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
5.  In **nome Computer**, digitare il nome del computer se si modifica anche il nome del computer quando si aggiunge il server al dominio. In **membro del**, fare clic su **dominio**, quindi digitare il nome del dominio a cui si desidera aggiungere il server (ad esempio, corp.contoso.com) e quindi fare clic su **OK**.  
  
6.  Quando viene richiesta l'immissione di nome utente e password, digitare il nome utente e la password di un utente che disponga delle autorizzazioni necessarie per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo che consente di iniziare il dominio, fare clic su **OK**.  
  
8.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
9. Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
10. Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
> [!NOTE]  
> È necessario fornire delle credenziali di dominio quando si immette il comando **Add-Computer** seguente.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>1.8 configurare oggetti Criteri di gruppo  
Sono necessari almeno due oggetti Criteri di gruppo per distribuire accesso remoto:  
  
-   Uno contiene le impostazioni per il server DirectAccess  
  
-   L'altro contiene le impostazioni per i computer client DirectAccess  
  
Quando si configura accesso remoto, la procedura guidata crea automaticamente gli oggetti Criteri di gruppo necessari. Tuttavia, se l'organizzazione applica una convenzione di denominazione, è possibile digitare un nome nella finestra di dialogo dell'oggetto Criteri di gruppo nella Console di gestione Accesso remoto. Per ulteriori informazioni, vedere 2.7. Configurazione alternativa e riepilogo oggetti Criteri di gruppo. Se si hanno le autorizzazioni di creazione, l'oggetto Criteri di gruppo viene creato. Se non si hanno le autorizzazioni necessarie per la creazione di oggetti Criteri di gruppo, è necessario crearli prima di configurare Accesso remoto.  
  
Per creare oggetti Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> Gli amministratori possono collegare manualmente gli oggetti Criteri di gruppo DirectAccess a un'unità organizzativa (OU) attenendosi alla procedura seguente:  
>   
> 1.  Prima di configurare DirectAccess collegare gli oggetti Criteri di gruppo alle rispettive unità organizzative.  
> 2.  Quando si configura DirectAccess specificare un gruppo di sicurezza per i computer client.  
> 3.  L'amministratore di accesso remoto può o non dispone delle autorizzazioni necessarie per collegare oggetti Criteri di gruppo al dominio. In ogni caso, gli oggetti Criteri di gruppo verranno configurati automaticamente. Se gli oggetti Criteri di gruppo sono già collegati a un'unità organizzativa, i collegamenti non verranno rimossi e gli oggetti Criteri di gruppo non verranno collegati al dominio. Per un oggetto Criteri di gruppo del server, l'unità organizzativa deve contenere l'oggetto computer server oppure l'oggetto Criteri di gruppo verrà collegato alla radice del dominio.  
> 4.  Se non si collegamento all'unità Organizzativa prima di eseguire la procedura guidata DirectAccess, una volta completata la configurazione, l'amministratore di dominio è possibile collegare gli oggetti Criteri di gruppo DirectAccess a unità organizzative richieste. Il collegamento al dominio può essere rimosso. Per ulteriori informazioni, vedere [collegare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se un oggetto Criteri di gruppo è stato creato manualmente, è possibile che l'oggetto Criteri di gruppo non sarà disponibile durante la configurazione di DirectAccess. L'oggetto Criteri di gruppo potrebbe non essere stato replicato sul controller di dominio più vicino al computer di gestione. In questo caso, l'amministratore può attendere il completamento della replica o forzarla.  
  
### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 Configurare gli oggetti Criteri di gruppo di Accesso remoto con autorizzazioni limitate  
In una distribuzione che usa oggetti Criteri di gruppo di gestione temporanea e di produzione, l'amministratore di dominio deve:  
  
1.  Ottenere un elenco degli oggetti Criteri di gruppo necessari per la distribuzione di Accesso remoto dall'amministratore di Accesso remoto. Per ulteriori informazioni, vedere 1.8 pianificare oggetti Criteri di gruppo.  
  
2.  Per ogni oggetto Criteri di gruppo richiesto dall'amministratore di Accesso remoto, creare una coppia di oggetti Criteri di gruppo con nomi diversi. Il primo viene usato come oggetto Criteri di gruppo di gestione temporanea, mentre il secondo come oggetto Criteri di gruppo di produzione.  
  
    Per creare oggetti Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
3.  Per collegare oggetti Criteri di gruppo di produzione, vedere [collegare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc732979).  
  
4.  Concedere all'amministratore di Accesso remoto le autorizzazioni **Modifica impostazioni, elimina, modifica sicurezza** su tutti gli oggetti Criteri di gruppo di gestione temporanea. Per ulteriori informazioni, vedere [delegare le autorizzazioni per un gruppo o utente in un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754542).  
  
5.  Nega le autorizzazioni di amministratore di accesso remoto per collegare oggetti Criteri di gruppo in tutti i domini (o verificare che non di amministratore di accesso remoto "t disponga di tali autorizzazioni). Per ulteriori informazioni, vedere [delegato le autorizzazioni per oggetti Criteri di gruppo collegamento](https://technet.microsoft.com/library/cc755086).  
  
Quando gli amministratori di Accesso remoto configurano Accesso remoto devono sempre specificare solo gli oggetti Criteri di gruppo di gestione temporanea (non gli oggetti Criteri di gruppo di produzione). Questo si applica alla configurazione iniziale di Accesso remoto e quando si eseguono operazioni di configurazione aggiuntive in cui sono richiesti altri oggetti Criteri di gruppo, ad esempio quando si aggiungono punti di ingresso in una distribuzione multisito o si abilitano i computer client in domini aggiuntivi.  
  
Dopo che l'amministratore di Accesso remoto completa le modifiche alla configurazione di Accesso remoto, l'amministratore di dominio deve rivedere le impostazioni degli oggetti Criteri di gruppo di gestione temporanea e usare la seguente procedura per copiare le impostazioni negli oggetti Criteri di gruppo di produzione.  
  
> [!TIP]  
> Eseguire la seguente procedura dopo ogni modifica alla configurazione di Accesso remoto.  
  
##### <a name="to-copy-settings-to-the-production-gpos"></a>Per copiare le impostazioni negli oggetti Criteri di gruppo di produzione  
  
1.  Verificare che tutti gli oggetti Criteri di gruppo di gestione temporanea nella distribuzione di Accesso remoto siano stati replicati in tutti i controller di dominio nel dominio. È un passaggio necessario per assicurare che negli oggetti Criteri di gruppo di produzione sia importata la configurazione più aggiornata. Per ulteriori informazioni, vedere Verifica stato di infrastruttura Criteri di gruppo.  
  
2.  Esportare le impostazioni eseguendo il backup di tutti gli oggetti Criteri di gruppo di gestione temporanea nella distribuzione di Accesso remoto. Per ulteriori informazioni, vedere eseguire il backup di un oggetto Criteri di gruppo.  
  
3.  Per ogni oggetto Criteri di gruppo di produzione, modificare i filtri di sicurezza in modo che corrispondano ai filtri di sicurezza dell'oggetto Criteri di gruppo di gestione temporanea corrispondente. Per ulteriori informazioni, vedere gruppi di sicurezza tramite filtro.  
  
    > [!NOTE]  
    > Questo passaggio è necessario perché **Importa impostazioni** non copia il filtro di sicurezza dell'oggetto Criteri di gruppo di origine.  
  
4.  Per ogni oggetto Criteri di gruppo di produzione, importare le impostazioni dal backup dell'oggetto Criteri di gruppo di gestione temporanea corrispondente come segue:  
  
    1.  Nel gruppo di CONSOLE Gestione criteri (), espandere il nodo oggetti Criteri di gruppo nella foresta e nel dominio che contiene l'oggetto Criteri di gruppo di produzione in cui verranno importate le impostazioni.  
  
    2.  Fare doppio clic su oggetto Criteri di gruppo e fare clic su **Importa impostazioni**.  
  
    3.  Nel **Importazione guidata impostazioni**, via il **iniziale** pagina, fare clic su **Avanti**.  
  
    4.  Nella pagina **Esegui backup dell'oggetto Criteri di gruppo**, scegliere **Backup**.  
  
    5.  Nella finestra di dialogo **Backup oggetto Criteri di gruppo**, nella casella **Percorso**, immettere il percorso in cui archiviare i backup dell'oggetto Criteri di gruppo oppure fare clic su **Sfoglia** per individuare la cartella.  
  
    6.  Nella casella **Descrizione**, digitare una descrizione per l'oggetto Criteri di gruppo di produzione, quindi scegliere **Backup**.  
  
    7.  Al termine del backup, fare clic su **OK**, quindi, nella pagina **Esegui backup dell'oggetto Criteri di gruppo**, scegliere **Avanti**.  
  
    8.  Nel **percorso Backup** nella pagina di **cartella Backup** casella immettere il percorso per il percorso in cui è stato archiviato il backup dell'oggetto di gestione temporanea corrispondente nel passaggio 2 oppure fare clic su **Sfoglia** per individuare la cartella e quindi fare clic su **Avanti**.  
  
    9. Nella pagina **Oggetto Criteri di gruppo di origine**, selezionare la casella di controllo **Mostra solo l'ultima versione di ogni oggetto** per nascondere i backup meno recenti, quindi selezionare l'oggetto Criteri di gruppo di gestione temporanea corrispondente. Fare clic su **Visualizza impostazioni** per rivedere le impostazioni di accesso remoto prima di applicarle al GPO in produzione e quindi fare clic su **Avanti**.  
  
    10. Nella pagina **Analisi del backup**, scegliere **Avanti**, quindi fare clic su **Fine**.  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
-   Per eseguire il backup il GPO client di gestione temporanea "DirectAccess Client impostazioni - gestione temporanea" nel dominio "corp.contoso.com" della cartella di backup "C:\Backups\":  
  
    ```  
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'  
    ```  
  
-   Per visualizzare i filtri di sicurezza del client di gestione temporanea oggetto Criteri di gruppo "DirectAccess Client impostazioni - gestione temporanea" nel dominio "corp.contoso.com":  
  
    ```  
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}  
    ```  
  
-   Per aggiungere il gruppo di sicurezza "corp.contoso.com\DirectAccess client" per il filtro di sicurezza dell'oggetto Criteri di gruppo del client di produzione "impostazioni DirectAccess Client"Produzione"nel dominio"corp.contoso.com":  
  
    ```  
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group  
    ```  
  
-   Per importare le impostazioni dal backup oggetto Criteri di gruppo del client di produzione "impostazioni DirectAccess Client"Produzione"nel dominio"corp.contoso.com":  
  
    ```  
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'  
    ```  
  
## <a name="ConfigSGs"></a>1.9 configurare i gruppi di sicurezza  
Le impostazioni di DirectAccess contenute nell'oggetto Criteri di gruppo di computer client vengono applicate solo ai computer che sono membri dei gruppi di sicurezza specificate quando si configura accesso remoto. Inoltre, se si usano i gruppi di sicurezza per gestire i server applicazioni, creare un gruppo di sicurezza per tali server.  
  
### <a name="Sec_Group"></a>Per creare un gruppo di sicurezza per i client DirectAccess  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO. Nel **Active Directory Users and Computers** console, nel riquadro sinistro, espandere il dominio che contiene il gruppo di sicurezza, fare doppio clic su **utenti**, scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
2.  Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, immettere il nome del gruppo di sicurezza.  
  
3.  In **Ambito del gruppo** fare clic su **Globale**, in **Tipo gruppo** fare clic su **Sicurezza** e quindi fare clic su **OK**.  
  
4.  Fare doppio clic sul gruppo di sicurezza computer client DirectAccess e, nella finestra di dialogo Proprietà fare clic su di **membri** scheda.  
  
5.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
6.  Nella finestra di dialogo **Selezionare utenti, contatti, computer o account di servizio** selezionare i computer client che si vogliono abilitare per DirectAccess e quindi fare clic su **OK**.  
  
![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell**  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>1.10 configurare il server dei percorsi di rete  
Il server dei percorsi di rete deve trovarsi su un server a disponibilità elevata e un certificato SSL valido considerato attendibile dai client DirectAccess. Sono disponibili due opzioni per il certificato del server dei percorsi di rete:  
  
-   **Certificato privato**  
  
    Questo certificato è basato sul modello di certificato creato seguendo le istruzioni in [1.5.2 configurare modelli di certificato](#ConfigCertTemp).  
  
-   **Certificato autofirmato**  
  
    > [!NOTE]  
    > I certificati autofirmati non possono essere usati in distribuzioni multisito.  
  
Per entrambi i tipi di certificato, sono necessari i seguenti elementi, se non esistono già:  
  
-   Un certificato del sito Web usato per il server dei percorsi di rete. Il soggetto del certificato deve essere l'URL del server dei percorsi di rete.  
  
-   Un punto di distribuzione CRL a disponibilità elevata nella rete interna.  
  
> [!NOTE]  
> Se il sito Web del server dei percorsi di rete si trova sul server DirectAccess, viene creato automaticamente un sito Web quando si configura Accesso remoto. Questo sito è associato al certificato del server fornito.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Per installare il certificato del server dei percorsi di rete da una CA interna  
  
1.  Sul server che ospiterà il sito Web del server dei percorsi di rete: Nel **avviare** digitare**mmc.exe**, quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti** .  
  
7.  Nel **richiesta certificati** pagina, selezionare la casella di controllo per il modello di certificato creato seguendo le istruzioni in 1.5.2 configurare modelli di certificato. Se necessario, **Sono necessarie ulteriori informazioni per registrare il certificato**.  
  
8.  Nel **Proprietà certificato** della finestra di dialogo di **soggetto** nella scheda il **nome soggetto** area, in **tipo**, selezionare **nome comune**.  
  
9. In **valore**, immettere il nome FQDN del server dei percorsi di rete e quindi fare clic su **Aggiungi**.  
  
10. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
11. In **valore**, immettere il nome FQDN del server dei percorsi di rete e quindi fare clic su **Aggiungi**.  
  
12. Nella casella **Nome descrittivo** della scheda **Generale** è possibile immettere un nome che consentirà di identificare il certificato.  
  
13. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
14. Nel riquadro dei dettagli dello snap-in Certificati verificare che il nuovo certificato sia registrato come Autenticazione server in Scopi designati.  
  
#### <a name="to-configure-the-network-location-server"></a>Per configurare il server dei percorsi di rete  
  
1.  Impostare un sito Web in un server a disponibilità elevata. Il sito Web non richiede contenuti, ma quando viene testato è possibile definire una pagina predefinita che fornisca un messaggio al momento della connessione dei client.  
  
    > [!NOTE]  
    > Questo passaggio non è necessario se il sito Web del server dei percorsi di rete è ospitato nel server DirectAccess.  
  
2.  Associare un certificato del server HTTPS al sito Web. Il nome comune del certificato deve corrispondere al nome del sito del server dei percorsi di rete. Verificare che il client DirectAccess consideri attendibile la CA emittente.  
  
    > [!NOTE]  
    > Questo passaggio non è necessario se il sito Web del server dei percorsi di rete è ospitato nel server DirectAccess.  
  
3.  Impostare un sito CRL a disponibilità elevata nella rete interna.  
  
    I punti di distribuzione Elenco di revoche di certificati (CRL) sono accessibili da:  
  
    -   Server Web tramite un URL basato su HTTP, ad esempio: https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   File server che si accede tramite un percorso universal naming convention (UNC), ad esempio \\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    Se il punto di distribuzione CRL interno è raggiungibile con IPv6, è necessario configurare una regola di sicurezza della connessione di Windows Firewall con sicurezza avanzata per rendere la protezione IPsec esente dall'indirizzo IPv6 della Intranet agli indirizzi IPv6 dei punti di distribuzione Elenco di revoche di certificati (CRL).  
  
4.  I client DirectAccess nella rete interna devono poter risolvere il nome del server dei percorsi di rete. Verificare che il nome non sia risolvibile dal client DirectAccess nella Internet.  
  
## <a name="BKMK_Links"></a>Passaggio successivo  
  
-   [Passaggio 2: Configurare i server DirectAccess avanzato](da-adv-configure-s2-servers.md)  
  


