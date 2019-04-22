---
title: Passaggio 1 configurare l'infrastruttura DirectAccess base
description: Questo argomento fa parte della Guida di distribuire un Server DirectAccess singolo con l'introduzione avvio procedura guidata per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba4de2a4-f237-4b14-a8a7-0b06bfcd89ad
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 75a47b6a6cb0a345d1d6e46ac8807bba6a895ace
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820942"
---
# <a name="step-1-configure-the-basic-directaccess-infrastructure"></a>Passaggio 1 configurare l'infrastruttura DirectAccess base

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive come configurare l'infrastruttura richiesta per una distribuzione di DirectAccess di base che usa un server DirectAccess singolo in un ambiente misto IPv4 e IPv6. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [pianificare una distribuzione DirectAccess base](../../../remote-access/directaccess/single-server-wizard/Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Attività|Descrizione|  
|----|--------|  
|Configurare le impostazioni di rete del server|Consente di configurare le impostazioni di rete del server nel server DirectAccess.|  
|Configurare il routing nella rete aziendale|Consente di configurare il routing nella rete aziendale per assicurarsi che il traffico sia indirizzato correttamente.|  
|Configurare i firewall|Consente di configurare i firewall aggiuntivi eventualmente necessari.|  
|Configurare il server DNS|Configurare le impostazioni DNS per il server DirectAccess.|  
|Configurare Active Directory|Consente di aggiungere i computer client e il server DirectAccess al dominio Active Directory.|  
|Configurare gli oggetti Criteri di gruppo|Consente di configurare gli oggetti Criteri di gruppo per la distribuzione, se necessario.|  
|Configurare i gruppi di sicurezza|Consente di configurare i gruppi di sicurezza che conterranno i computer client DirectAccess e qualsiasi altro gruppo di sicurezza necessario nella distribuzione.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="ConfigNetworkSettings"></a>Configurare le impostazioni di rete di server  
Le seguenti impostazioni dell'interfaccia di rete sono necessarie per la distribuzione di un singolo server in un ambiente con IPv4 e IPv6. Tutti gli indirizzi IP vengono configurati tramite **modificare le impostazioni della scheda** nel **Windows Networking and Sharing Center**.  
  
-   Topologia perimetrale  
  
    -   Un indirizzo IPv4 o IPv6 statico pubblico con connessione Internet.  
  
        > [!NOTE]  
        > Per Teredo sono necessari due indirizzi IPv4 pubblici consecutivi. Se non si usa Teredo, è possibile configurare un solo indirizzo statico pubblico IPv4.  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico pubblico interno.  
  
-   Dietro il dispositivo NAT (due schede di rete)  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico pubblico con connessione alla rete interna.  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico con connessione alla rete perimetrale.  
  
-   Dietro il dispositivo NAT (una scheda di rete)  
  
    -   Un singolo indirizzo IPv4 o IPv6 statico.  
  
> [!NOTE]  
> Se il server di DirectAccess ha due o più schede di rete (una classificata nel profilo del dominio e l'altra in un profilo pubblico/privato), ma si vuole usare una singola topologia NIC, è consigliabile procedere come segue:  
>   
> 1.  Verificare che anche la seconda NIC ed eventuali altre NIC siano classificate nel profilo del dominio.  
> 2.  Se per un motivo qualsiasi non fosse possibile configurare la seconda NIC per il profilo del dominio, l'ambito del criterio IPSec di DirectAccess deve essere impostato manualmente su tutti i profili con i comandi seguenti di Windows PowerShell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
>   
>     I nomi dei criteri IPsec sono DirectAccess-DaServerToInfra e DirectAccess-DaServerToCorp.  
  
## <a name="ConfigRouting"></a>Configurare il routing nella rete aziendale  
Configurare il routing nella rete aziendale come segue:  
  
-   Quando si distribuisce la connettività IPv6 nativa nell'organizzazione, aggiungere una route in modo che i router sulla rete interna reindirizzino il traffico IPv6 all'indietro con il server di Accesso remoto.  
  
-   Configurare manualmente le route IPv4 e IPv6 dell'organizzazione sui server di Accesso remoto. Aggiungere una route pubblicata in modo che tutto il traffico con il prefisso IPv6 di un'organizzazione (/48) sia inoltrato alla rete interna. In aggiunta, per il traffico IPv4, aggiungere route esplicite in modo che il traffico IPv4 venga inoltrato alla rete interna.  
  
## <a name="ConfigFirewalls"></a>Configurare i firewall  
Se si utilizzano altri firewall nella propria distribuzione, applicare le eccezioni firewall con connessione Internet seguenti per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete IPv4 Internet:  
  
-   6to4 il traffico IP - protocollo 41 in entrata e in uscita.  
  
-   IP-HTTPS-protocollo TCP (Transmission Control) destinazione porta 443 e porta TCP di origine 443 in uscita. Se il server di Accesso remoto dispone di un singolo adattatore di rete e il server dei percorsi di rete si trova sul server di Accesso remoto, è richiesta anche la porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione deve essere configurata sul server di accesso remoto. Tutte le altre esenzioni devono essere configurate sul firewall di confine.  
  
> [!NOTE]  
> Per il traffico Teredo e 6to4, tali esenzioni devono essere applicate per entrambi gli indirizzi IPv4 pubblici consecutivi con connessione Internet sul server di Accesso remoto. Per IP-HTTPS le esenzioni devono essere applicate solo per l'indirizzo nel quale viene risolto il nome esterno del server.  
  
Se si utilizzano altri firewall, applicare le eccezioni firewall con connessione Internet seguenti per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete IPv6 Internet:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
Quando si utilizzano altri firewall, applicare le eccezioni firewall delle rete interna seguenti per il traffico di Accesso remoto:  
  
-   ISATAP-protocollo 41 in entrata e in uscita  
  
-   TCP/UDP per tutto il traffico IPv4/IPv6  
  
## <a name="ConfigDNS"></a>Configurare il server DNS  
È necessario configurare manualmente una voce DNS per il sito Web del server dei percorsi di rete per la rete interna nella distribuzione.  
  
### <a name="NLS_DNS"></a>Per creare il percorso di rete server e del probe NCSI i record DNS  
  
1.  Nel server DNS di rete interna, eseguire **dnsmgmt. msc** e quindi premere INVIO.  
  
2.  Nel riquadro a sinistra della console **Gestore DNS** espandere la zona di ricerca diretta per il proprio dominio. Fare clic con il pulsante destro del mouse sul dominio e fare clic su **Nuovo host (A o AAAA)**.  
  
3.  Nella finestra di dialogo **Nuovo host**, nella casella **Nome (se vuoto, utilizza il nome del dominio padre)**, immettere il nome DNS del sito Web del server dei percorsi di rete (cioè il nome usato dai client DirectAccess per connettersi al server dei percorsi di rete). Nel **indirizzo IP** casella, immettere l'indirizzo IPv4 del server dei percorsi di rete e quindi fare clic su **Aggiungi Host**. Nel **DNS** la finestra di dialogo, fare clic su **OK**.  
  
4.  Nella finestra di dialogo **Nuovo host**, nella casella **Nome (se vuoto, utilizza il nome del dominio padre)**, immettere il nome DNS del probe Web (il nome del probe Web predefinito è directaccess-webprobehost). Nella casella **Indirizzo IP** immettere l'indirizzo IPv4 del probe Web e quindi fare clic su **Aggiungi host**. Ripetere questa procedura per directaccess-corpconnectivityhost e per qualsiasi strumento di verifica della connettività creato manualmente. Nel **DNS** la finestra di dialogo, fare clic su **OK**.  
  
5.  Fare clic su **Fine**.  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Le voci DNS devono essere configurate anche per quanto segue:  
  
-   **Il server IP-HTTPS** i client DirectAccess - devono essere in grado di risolvere il nome DNS del server di accesso remoto da Internet.  
  
-   **Rilevamento delle revoche CRL** - DirectAccess utilizza revoche del certificato per la connessione IP-HTTPS tra i client DirectAccess e il server di accesso remoto e per la connessione basata su HTTPS tra il client DirectAccess e il server dei percorsi di rete. In entrambi i casi, i client DirectAccess devono poter risolvere e accedere al percorso del punto di distribuzione CRL.  
  
## <a name="ConfigAD"></a>Configurare Active Directory  
Il server di Accesso remoto e tutti i computer client DirectAccess devono appartenere a un dominio Active Directory. I computer client DirectAccess devono essere membri di uno dei seguenti tipi di dominio:  
  
-   Domini appartenenti alla stessa foresta del server di accesso remoto.  
  
-   Domini appartenenti alle foreste con un trust bidirezionale con la foresta del server di Accesso remoto.  
  
-   Domini che abbiano un trust bidirezionale al dominio del server di Accesso remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Per aggiungere il server di accesso remoto a un dominio  
  
1.  In Server Manager fare clic su **Server locale**. Nel riquadro dei dettagli fare clic sul collegamento accanto a **Nome computer**.  
  
2.  Nella finestra di dialogo **Proprietà del sistema** fare clic sulla scheda **Nome computer** . Nella scheda **Nome computer** , fare clic su **Modifica**.  
  
3.  Per cambiare anche il nome computer quando si aggiunge il server al dominio, digitarlo in **Nome computer**. In **Membro di** fare clic su **Dominio** e quindi digitare il nome del dominio a cui si desidera aggiungere il server, ad esempio corp.contoso.com, e quindi fare clic su **OK**.  
  
4.  Quando viene richiesta l'immissione di nome utente e password, digitare il nome utente e la password di un utente che disponga delle autorizzazioni necessarie per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Per aggiungere i computer client al dominio  
  
1.  Eseguire **explorer.exe**.  
  
2.  Pulsante destro del mouse sull'icona del Computer e quindi fare clic su **proprietà**.  
  
3.  Nella pagina **Sistema** fare clic su **Impostazioni di sistema avanzate**.  
  
4.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
5.  In **nome Computer**, digitare il nome del computer se si modifica anche il nome del computer quando si aggiunge il server al dominio. In **Membro di** fare clic su **Dominio** e quindi digitare il nome del dominio a cui si desidera aggiungere il server, ad esempio corp.contoso.com, e quindi fare clic su **OK**.  
  
6.  Quando viene richiesta l'immissione di nome utente e password, digitare il nome utente e la password di un utente che disponga delle autorizzazioni necessarie per aggiungere computer al dominio e quindi fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo di benvenuto, fare clic su **OK**.  
  
8.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
9. Nel **le proprietà di sistema** la finestra di dialogo, fare clic su Chiudi. Fare clic su **Riavvia** quando richiesto.  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Si noti che è necessario specificare le credenziali di dominio dopo aver immesso il comando Add-Computer seguente.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurare oggetti Criteri di gruppo  
Per distribuire accesso remoto, è necessario un minimo di due oggetti Criteri di gruppo: un oggetto Criteri di gruppo contiene impostazioni per il server di accesso remoto e l'altro contiene le impostazioni per i computer client DirectAccess. Quando si configura accesso remoto, la procedura guidata crea automaticamente l'oggetto Criteri di gruppo necessario. Tuttavia, se l'organizzazione applica una convenzione di denominazione o non avere le autorizzazioni necessarie per creare o modificare oggetti Criteri di gruppo, è necessario creare prima di configurare accesso remoto.  
  
Per creare un oggetto Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> L'amministratore può collegare manualmente l'oggetto Criteri di gruppo DirectAccess a un'unità organizzativa attenendosi alla procedura seguente:  
>   
> 1.  Prima di configurare DirectAccess collegare gli oggetti Criteri di gruppo alle rispettive unità organizzative.  
> 2.  Configurare DirectAccess specificando un gruppo di sicurezza per i computer client.  
> 3.  L'amministratore potrebbe avere o meno le autorizzazioni per collegare gli oggetti Criteri di gruppo al dominio. In ogni caso, gli oggetti Criteri di gruppo verranno configurati automaticamente. Se gli oggetti Criteri di gruppo sono già collegati a un'unità organizzativa, i collegamenti non verranno rimossi né gli oggetti Criteri di gruppo verranno collegati al dominio. Per l'oggetto Criteri di gruppo del server, l'unità organizzativa deve contenere l'oggetto computer server, altrimenti l'oggetto Criteri di gruppo verrà collegato alla radice del dominio.  
> 4.  Se il collegamento all'unità organizzativa non è stato eseguito prima di eseguire la procedura guidata DirectAccess, al termine della configurazione l'amministratore potrà collegare gli oggetti Criteri di gruppo DirectAccess alle unità organizzative richieste. Il collegamento al dominio può essere rimosso. I passaggi per il collegamento di un oggetto Criteri di gruppo per un'unità organizzativa sono reperibili [qui](https://technet.microsoft.com/library/cc732979.aspx)  
  
> [!NOTE]  
> Se un oggetto Criteri di gruppo è stato creato manualmente, è possibile durante la configurazione di DirectAccess che l'oggetto Criteri di gruppo non sarà disponibile. L'oggetto Criteri di gruppo potrebbe non essere stato replicato sul controller di dominio più vicino al computer di gestione. In questo caso, l'amministratore può attendere il completamento della replica o forzarla.  
  
## <a name="ConfigSGs"></a>Configurare gruppi di sicurezza  
Le impostazioni di DirectAccess contenute negli oggetti Criteri di gruppo di computer client vengono applicate solo ai computer che sono membri dei gruppi di sicurezza specificati durante la configurazione di accesso remoto.  
  
### <a name="Sec_Group"></a>Per creare un gruppo di sicurezza per i client DirectAccess  
  
1.  Eseguire **DSA. msc**. Nel **Active Directory Users and Computers** console, nel riquadro sinistro, espandere il dominio che contiene il gruppo di sicurezza, fare doppio clic su **utenti**, scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
2.  Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, immettere il nome del gruppo di sicurezza.  
  
3.  In **ambito del gruppo**, fare clic su **globale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
4.  Fare doppio clic sul gruppo di sicurezza computer client DirectAccess e nella finestra di dialogo proprietà, fare clic su di **membri** scheda.  
  
5.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
6.  Nel **Seleziona utenti, contatti, computer o gli account del servizio** finestra di dialogo, selezionare i computer client che si desidera abilitare per DirectAccess e quindi fare clic su **OK**.  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell**  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_Links"></a>Passaggio successivo  
  
-   [Passaggio 2: Configurare il Server DirectAccess di base](da-basic-configure-s2-server.md)  
  


