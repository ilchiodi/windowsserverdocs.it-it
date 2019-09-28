---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurare un computer per il ruolo di proxy server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d47f7d3985aa779276f0712347eb9030857cefdb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359794"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurare un computer per il ruolo di proxy server federativo

Dopo aver configurato un computer con i certificati richiesti e aver installato il servizio ruolo Proxy servizio federativo, si è pronti per configurare il computer in modo che diventi un proxy server federativo. Affinché il computer possa essere utilizzato nel ruolo proxy server federativo, utilizzare la procedura seguente.  
  
> [!IMPORTANT]  
> Prima di usare questa procedura per configurare il computer proxy server federativo, assicurarsi di aver seguito tutti i passaggi descritti in [Checklist: Configurazione di un proxy server federativo @ no__t-0 nell'ordine in cui sono elencati. Assicurarsi che sia distribuito almeno un server federativo e che siano implementate tutte le credenziali necessarie per l'autorizzazione di una configurazione proxy server federativo. È inoltre necessario configurare le associazioni Secure Sockets Layer \(SSL @ no__t-1 nel sito Web predefinito, altrimenti questa procedura guidata non verrà avviata. Prima di poter utilizzare il proxy server federativo, tutte queste operazioni devono essere completate.  
  
Una volta terminata la configurazione del computer, verificare che il proxy server federativo funzioni come previsto. Per altre informazioni, vedere [Verificare che un proxy server federativo sia operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Per configurare un computer per il ruolo di proxy server federativo  
  
1.  Esistono due modi per avviare la configurazione guidata del server federativo di AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Nella schermata **Start** Digitare**ad FS configurazione guidata proxy server federativo**, quindi premere INVIO.  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C: \\Windows @ no__t-2ADFS** e quindi a Double @ no__t-3Cliccare **FspConfigWizard. exe**.  
  
2.  Usando uno dei due metodi, avviare la procedura guidata e nella pagina **iniziale** fare clic su **Avanti**.  
  
3.  Nella pagina **Nome servizio federativo**, in **Nome servizio federativo**, immettere il nome che rappresenta il servizio federativo per cui il computer agirà nel ruolo di proxy.  
  
4.  In base a specifici requisiti di rete, determinare se sarà necessario usare un server proxy HTTP per inoltrare le richieste al servizio federativo. In caso affermativo, selezionare la casella di controllo relativa **all'uso di un server proxy HTTP per l'invio di richieste al servizio federativo**, in **Indirizzo e porta server proxy HTTP** immettere l'indirizzo del server proxy, fare clic su **Test connessione** per verificare la connettività e quindi fare clic su **Avanti**.  
  
5.  Quando viene richiesto, specificare le credenziali necessarie per stabilire un trust tra il proxy server federativo e il Servizio federativo.  
  
    Per impostazione predefinita, solo l'account di servizio utilizzato dal Servizio federativo o un membro del gruppo locale BUILTin @ no__t-0Administrators può autorizzare un proxy server federativo.  
  
6.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare il computer con queste impostazioni proxy.  
  
7.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    Non esiste alcuna Microsoft Management Console \(MMC @ no__t-1 snap @ no__t-2in da usare per l'amministrazione dei proxy server federativi. Per configurare le impostazioni per ognuno dei proxy server federativi nell'organizzazione, usare i cmdlet di Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurazione di una porta TCP @ no__t-0IP alternativa per le operazioni proxy  
Per impostazione predefinita, il servizio proxy server federativo è configurato per usare la porta TCP 443 per il traffico HTTPS e la porta 80 per il traffico HTTP per la comunicazione con il server federativo. Per configurare porte diverse, ad esempio la porta TCP 444 per HTTPS e la porta 81 per HTTP, è necessario completare le attività seguenti.  
  
> [!NOTE]  
> Se si intende distribuire inizialmente AD FS in modo che funzionino con porte TCP @ no__t-0IP alternative, è necessario innanzitutto modificare le porte nei binding del protocollo IIS per HTTP e HTTPS nei computer del server federativo e del proxy server federativo. Questa operazione deve essere eseguita prima di eseguire la configurazione guidata di AD FS per la configurazione iniziale. Se si configura prima Internet Information Services \(IIS @ no__t-1, le impostazioni della porta TCP @ no__t-2IP alternative vengono individuate quando si verifica la configurazione della procedura guidata @ no__t-3based in AD FS e la procedura seguente non è necessaria. Se si vogliono modificare le impostazioni delle porte in seguito, aggiornare prima i binding del protocollo IIS e quindi usare la procedura seguente per aggiornare correttamente le impostazioni delle porte. Per ulteriori informazioni sulla modifica dei binding IIS, vedere l' [articolo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) della Microsoft Knowledge base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Per configurare le porte TCP @ no__t-0IP alternative che il proxy server federativo utilizzerà  
  
1.  Configurare il server federativo per l'uso di porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito, inserendolo con le opzioni *HttpsPort* e *httpport* come parte del cmdlet **set @ no__t-3ADFSProperties** . Per configurare queste porte, ad esempio, usare i comandi seguenti nella sessione di Windows PowerShell nel computer server federativo:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurare il proxy server federativo per l'utilizzo della porta non predefinita.  
  
    A tale scopo, specificare il numero di porta non predefinito, inserendolo con le opzioni *HttpsPort* e *httpport* come parte del cmdlet **set @ no__t-3ADFSProxyProperties** . Per configurare queste porte, ad esempio, usare i comandi seguenti nella sessione di Windows PowerShell nel computer server federativo:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Gli URL degli endpoint non sono abilitati per impostazione predefinita per il servizio proxy server federativo. Se si configura una nuova installazione server federativo, è necessario abilitare prima gli endpoint del servizio proxy server federativo. Si presuppone, ad esempio, che per tutti gli endpoint a cui si riferisce l'esempio in questa procedura siano stati abilitati per il proxy selezionandoli nello snap-in di gestione AD FS @ no__t-0cm e quindi selezionando **Abilita sul proxy**.  
  
3.  Aggiornare l'installazione di IIS nel proxy server federativo in modo che gli endpoint Security Assertion Markup Language \(SAML @ no__t-1 e WS @ no__t-2Trust siano configurati in modo da riflettere il numero di porta aggiornato. A tale scopo, è possibile utilizzare il blocco note per modificare il codice seguente nel file Web. config, che si trova in SystemDrive% \\inetpub @ no__t-1adfs @ no__t-2LS @ no__t-3 nel computer proxy server federativo. Ad esempio, supponendo di avere un server federativo denominato sts1.contoso.com e il nuovo numero di porta 444, individuare e aprire il file Web. config nel blocco note nel computer proxy server federativo, individuare la sezione seguente, modificare il numero di porta come evidenziato di seguito, quindi salvare e uscire dal blocco note.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Aggiungere l'account utente del servizio proxy server federativo all'elenco di controllo di accesso \(ACL @ no__t-1 per gli URL dell'endpoint correlati. Se, ad esempio, il numero di porta è 1234 e l'account utente usato per eseguire il servizio proxy del server AD FSfederation è l'account servizio di rete no__t-0cm compilato, digitare il comando seguente al prompt dei comandi:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    È necessario eseguire i comandi precedenti sia nel server federativo che nei computer proxy server federativi.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

