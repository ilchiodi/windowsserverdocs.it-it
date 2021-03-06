---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurare un computer per il ruolo di proxy server federativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: a8bfe21f50a68edfcdbc7c937dc914ff1e1d94c3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854904"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurare un computer per il ruolo di proxy server federativo

Dopo aver configurato un computer con i certificati richiesti e aver installato il servizio ruolo Proxy servizio federativo, si è pronti per configurare il computer in modo che diventi un proxy server federativo. Affinché il computer possa essere utilizzato nel ruolo proxy server federativo, utilizzare la procedura seguente.  
  
> [!IMPORTANT]  
> Prima di utilizzare questa procedura per configurare il computer proxy server federativo, assicurarsi di aver seguito tutti i passaggi in elenco di [controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md) nell'ordine in cui sono elencati. Assicurarsi che sia distribuito almeno un server federativo e che siano implementate tutte le credenziali necessarie per l'autorizzazione di una configurazione proxy server federativo. È inoltre necessario configurare Secure Sockets Layer \(binding SSL\) nel sito Web predefinito, altrimenti questa procedura guidata non verrà avviata. Prima di poter utilizzare il proxy server federativo, tutte queste operazioni devono essere completate.  
  
Una volta terminata la configurazione del computer, verificare che il proxy server federativo funzioni come previsto. Per altre informazioni, vedere [Verificare che un proxy server federativo sia operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Configurazione di un computer il ruolo proxy server federativo  
  
1.  Sono due i modi per avviare la configurazione guidata del server federativo di ADFS. Per avviare la procedura guidata, procedere in uno dei seguenti modi:  
  
    -   Nella schermata **Start** Digitare**ad FS configurazione guidata proxy server federativo**, quindi premere INVIO.  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** , quindi fare\-doppio clic su **FspConfigWizard. exe**.  
  
2.  Nell'uno o nell'altro modo avviare la procedura guidata, quindi nella pagina **Welcome** fare clic su **Next**.  
  
3.  Nella pagina **Specify Federation Service Name**, in **Federation Service name**, digitare il nome che rappresenta il servizio federativo per cui il computer opererà nel ruolo di proxy.  
  
4.  Sulla base dei requisiti specifici di rete, determinare se è necessario un server proxy HTTP per inoltrare le richieste al servizio federativo. In caso affermativo, selezionare la casella di controllo **Use an HTTP proxy server when sending requests to this Federation Service**, in **HTTP proxy server address** digitare l'indirizzo del server proxy, fare clic su **Test Connection** per verificare la connettività, quindi fare clic su **Next**.  
  
5.  Quando viene richiesto, specificare le credenziali necessarie a stabilire un trust tra il proxy server federativo e il servizio federativo.  
  
    Per impostazione predefinita, solo l'account di servizio utilizzato dal Servizio federativo o un membro del gruppo locale BUILTin\\Administrators può autorizzare un proxy server federativo.  
  
6.  Nella pagina **Ready to Apply Settings** esaminare i dettagli. Se le impostazioni risultano corrette, fare clic su **Next** per avviare la configurazione del computer con queste impostazioni di proxy.  
  
7.  Nella pagina **Configuration Results** esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    Non sono presenti Microsoft Management Console \(MMC\) snap\-in da usare per l'amministrazione dei proxy server federativi. Per configurare le impostazioni per ognuno dei proxy server federativi nell'organizzazione, usare i cmdlet di Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurazione di una porta TCP\/IP alternativa per le operazioni proxy  
Per impostazione predefinita, il servizio proxy server federativo è configurato per usare la porta TCP 443 per il traffico HTTPS e la porta 80 per il traffico HTTP per la comunicazione con il server federativo. Per configurare porte diverse, ad esempio la porta TCP 444 per HTTPS e la porta 81 per HTTP, è necessario completare le attività seguenti.  
  
> [!NOTE]  
> Se si intende distribuire inizialmente AD FS in modo che funzionino con porte IP\/TCP alternative, è necessario innanzitutto modificare le porte nei binding del protocollo IIS per HTTP e HTTPS sul server federativo e sui computer proxy server federativi. Questa operazione deve essere eseguita prima di eseguire la configurazione guidata di AD FS per la configurazione iniziale. Se si configura Internet Information Services prima \(IIS\), le impostazioni della porta IP\/TCP alternative vengono individuate quando si verifica la configurazione basata su\-della procedura guidata in AD FS e la procedura seguente non è necessaria. Se si vogliono modificare le impostazioni delle porte in seguito, aggiornare prima i binding del protocollo IIS e quindi usare la procedura seguente per aggiornare correttamente le impostazioni delle porte. Per ulteriori informazioni sulla modifica dei binding IIS, vedere l' [articolo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) della Microsoft Knowledge base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Per configurare porte IP\/TCP alternative che il proxy server federativo utilizzerà  
  
1.  Configurare il server federativo per l'uso di porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito, inserendolo con le opzioni *HttpsPort* e *httpport* come parte del cmdlet **set\-ADFSProperties** . Per configurare queste porte, ad esempio, usare i comandi seguenti nella sessione di Windows PowerShell nel computer server federativo:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurare il proxy server federativo per l'utilizzo della porta non predefinita.  
  
    A tale scopo, specificare il numero di porta non predefinito, inserendolo con le opzioni *HttpsPort* e *httpport* come parte del cmdlet **set\-ADFSProxyProperties** . Per configurare queste porte, ad esempio, usare i comandi seguenti nella sessione di Windows PowerShell nel computer server federativo:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Gli URL degli endpoint non sono abilitati per impostazione predefinita per il servizio proxy server federativo. Se si configura una nuova installazione server federativo, è necessario abilitare prima gli endpoint del servizio proxy server federativo. Si presuppone, ad esempio, che per tutti gli endpoint a cui si riferisce l'esempio in questa procedura siano stati abilitati per il proxy selezionandoli nello snap-in di gestione AD FS\-in e quindi selezionando **Abilita sul proxy**.  
  
3.  Aggiornare l'installazione di IIS nel proxy server federativo in modo che Security Assertion Markup Language \(SAML\) e WS\-Trust endpoint siano configurati in modo da riflettere il numero di porta aggiornato. A tale scopo, è possibile utilizzare il blocco note per modificare il codice seguente nel file Web. config, che si trova in SystemDrive%\\Inetpub\\ADFS\\LS\\ nel computer proxy server federativo. Ad esempio, supponendo che si disponga di un server federativo denominato sts1.contoso.com e che il nuovo numero di porta sia 444, individuare e aprire il file Web. config nel blocco note sul computer proxy server federativo, individuare la sezione seguente, modificare il numero di porta come evidenziato di seguito, quindi salvare e chiudere il blocco note.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Aggiungere l'account utente del servizio proxy server federativo all'elenco di controllo di accesso \(ACL\) per gli URL dell'endpoint correlati. Se, ad esempio, il numero di porta è 1234 e l'account utente usato per eseguire il servizio proxy del server AD FSfederation in è il\-compilato in account servizio di rete, digitare il comando seguente al prompt dei comandi:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    È necessario eseguire i comandi precedenti sia nel server federativo che nei computer proxy server federativi.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

