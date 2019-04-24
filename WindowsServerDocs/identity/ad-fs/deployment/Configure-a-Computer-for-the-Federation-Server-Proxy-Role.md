---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurare un computer per il ruolo di proxy server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861802"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurare un computer per il ruolo di proxy server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver configurato un computer con i certificati necessari e aver installato il servizio ruolo Proxy servizio federativo, si è pronti a configurare il computer come un proxy server federativo. Affinché il computer possa essere utilizzato nel ruolo proxy server federativo, utilizzare la procedura seguente.  
  
> [!IMPORTANT]  
> Prima di usare questa procedura per configurare il computer proxy server federativo, assicurarsi di aver seguito tutti i passaggi [elenco di controllo: Impostazione di un Server Proxy federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md) nell'ordine in cui sono elencati. Assicurarsi che sia distribuito almeno un server federativo e che siano implementate tutte le credenziali necessarie per l'autorizzazione di una configurazione proxy server federativo. È inoltre necessario configurare Secure Sockets Layer \(SSL\) associazioni del sito Web predefinito o la procedura guidata non verrà avviato. Prima di poter utilizzare il proxy server federativo, tutte queste operazioni devono essere completate.  
  
Una volta terminata la configurazione del computer, verificare che il proxy server federativo funzioni come previsto. Per altre informazioni, vedere [Verificare che un proxy server federativo sia operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Per configurare un computer per il ruolo di proxy server federativo  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Nel **avviare** digitare**configurazione guidata di AD FS Federation Server Proxy**, quindi premere INVIO.  
  
    -   In qualsiasi momento dopo l'installazione guidata è completata, aprire Esplora Windows, passare al **c:\\Windows\\ADFS** cartella e quindi fare doppio\-fare clic su **FspConfigWizard.exe**.  
  
2.  Usando uno dei due metodi, avviare la procedura guidata e nella pagina iniziale **** fare clic su **Avanti**.  
  
3.  Nella pagina **Nome servizio federativo**, in **Nome servizio federativo**, immettere il nome che rappresenta il servizio federativo per cui il computer agirà nel ruolo di proxy.  
  
4.  In base a specifici requisiti di rete, determinare se sarà necessario usare un server proxy HTTP per inoltrare le richieste al servizio federativo. In caso affermativo, selezionare la casella di controllo relativa all'uso di un server proxy HTTP per l'invio di richieste al servizio federativo,**** in **Indirizzo e porta server proxy HTTP** immettere l'indirizzo del server proxy, fare clic su **Test connessione** per verificare la connettività e quindi fare clic su **Avanti**.  
  
5.  Quando viene richiesto, specificare le credenziali necessarie per stabilire un trust tra il proxy server federativo e il Servizio federativo.  
  
    Per impostazione predefinita, solo l'account del servizio utilizzato dal servizio federativo o un membro del locale BUILTIN\\gruppo Administrators possa autorizzare un proxy server federativo.  
  
6.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare il computer con queste impostazioni proxy.  
  
7.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    Non vi è alcun Microsoft Management Console \(MMC\) blocca\-in da utilizzare per l'amministrazione di proxy server federativo. Per configurare le impostazioni per tutti i proxy server federativo nell'organizzazione, usare i cmdlet di Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurazione di un'alternativa TCP\/porta IP per le operazioni Proxy  
Per impostazione predefinita, il servizio di proxy server federativo è configurato per usare la porta TCP 443 per il traffico HTTPS e la porta 80 per il traffico HTTP per la comunicazione con il server federativo. Per configurare porte diverse, ad esempio la porta TCP 444 per HTTPS e la porta 81 per HTTP, è necessario completare le attività seguenti.  
  
> [!NOTE]  
> Se si intende distribuire inizialmente AD FS per operare con TCP alternativo\/porte IP, è necessario prima modificare porte di binding del protocollo IIS per HTTP e HTTPS sul computer proxy server federativo sia nel server federativo. Questo dovrebbe avvenire prima di eseguire le configurazioni guidate di AD FS per la configurazione iniziale. Se si configura Internet Information Services \(IIS\) primo, il TCP alternativo\/impostazioni della porta IP individuate quando guidata\-basato su configurazione viene eseguita all'interno di ADFS e la procedura seguente non è necessario. Se si vogliono modificare le impostazioni delle porte in seguito, aggiornare prima i binding del protocollo IIS e quindi usare la procedura seguente per aggiornare correttamente le impostazioni delle porte. Per altre informazioni sulla modifica dei binding IIS, vedere [articolo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) nella Microsoft Knowledge Base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Per configurare TCP alternativo\/porte IP per il proxy server federativo da usare  
  
1.  Configurare il server federativo per l'uso di porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito includendolo con le *HttpsPort* e *HttpPort* opzioni come parte del **impostare\-ADFSProperties** cmdlet. Ad esempio, per configurare queste porte, usare i comandi seguenti nella sessione di Windows PowerShell nel computer del server federativo:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurare il proxy server federativo per usare porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito includendolo con le *HttpsPort* e *HttpPort* opzioni come parte del **impostare\-ADFSProxyProperties** cmdlet. Ad esempio, per configurare queste porte, usare i comandi seguenti nella sessione di Windows PowerShell nel computer del server federativo:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URL dell'endpoint non sono abilitati per impostazione predefinita per il servizio di proxy server federativo. Se si sta configurando una nuova installazione di server federativo, è necessario abilitare innanzitutto l'endpoint del servizio proxy server federativo. Ad esempio, si presuppone che per tutti gli endpoint che l'esempio in questa procedura si riferisce a siano stati abilitati per il proxy selezionandoli nello snap di gestione di AD FS\-in e quindi selezionando **Abilita su proxy**.  
  
3.  Aggiornare l'installazione di IIS al proxy server federativo in modo tale Security Assertion Markup Language \(SAML\) WS e\-attendibili gli endpoint vengono configurati in modo da riflettere il numero di porta aggiornato. A tale scopo, è possibile usare blocco note per modificare quanto segue nel file Web. config, che si trova in % systemdrive\\inetpub\\adfs\\ls\\ nel computer proxy server federativo. Ad esempio, supponendo che si dispone di un server di federazione denominato sts1.contoso.com e che il nuovo numero di porta sia 444, individuare e aprire il file Web. config nel blocco note nel computer proxy server federativo, individuare la sezione seguente, modificare il numero di porta evidenziato di seguito, quindi salvare e chiudere il blocco note.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Aggiungere l'account utente di servizio di federazione server proxy all'elenco di controllo di accesso \(ACL\) per gli URL dell'endpoint correlati. Ad esempio, se il numero di porta è 1234 e l'account utente usato per eseguire l'Active Directory FSfederation server proxy servizio è incorporato\-nell'account di servizio di rete, digitare il comando seguente al prompt dei comandi:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    I comandi precedenti devono essere eseguiti nel server federativo sia i computer proxy server federativi.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

