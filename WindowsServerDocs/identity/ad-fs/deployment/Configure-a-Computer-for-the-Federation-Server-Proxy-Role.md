---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurare un Computer per il ruolo di Proxy Server federativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurare un Computer per il ruolo di Proxy Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver configurato un computer con i certificati necessari e aver installato il servizio ruolo Proxy servizio federativo, si è pronti a configurare il computer come un proxy server federativo. È possibile utilizzare la procedura seguente in modo che il computer agisca nel ruolo proxy server federativo.  
  
> [!IMPORTANT]  
> Prima di utilizzare questa procedura per configurare il computer proxy server federativo, assicurarsi di avere seguito tutti i passaggi in [elenco di controllo: impostazione di un Server Proxy federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md) nell'ordine in cui sono elencati. Assicurarsi che viene distribuito che almeno un server federativo e che tutte le credenziali necessarie per autorizzare una configurazione di proxy server federativo sono implementati. È inoltre necessario configurare le associazioni di Secure Sockets Layer \(SSL\) al sito Web predefinito o la procedura guidata non verrà avviato. Tutte queste attività devono essere completate prima che il proxy server federativo possa funzionare.  
  
Dopo aver completato la configurazione del computer, verificare che il proxy server federativo funzioni come previsto. Per ulteriori informazioni, vedere [verificare che una federazione Server Proxy sia operativo](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Per configurare un computer per il ruolo di proxy server federativo  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, effettuare una delle operazioni seguenti:  
  
    -   Nel **Start** digitare**configurazione guidata di AD FS Federation Server Proxy**, quindi premere INVIO.  
  
    -   In qualsiasi momento dopo l'installazione guidata, aprire Esplora risorse, passare al **C:\\Windows\\ADFS** cartella, quindi fare doppio clic **FspConfigWizard.exe**.  
  
2.  Con entrambi i metodi, avviare la procedura guidata e il **iniziale** pagina, fare clic su **Avanti**.  
  
3.  Nel **Specifica nome servizio federativo** nella pagina **nome servizio federativo**, digitare il nome che rappresenta il servizio federativo per cui il computer agirà nel ruolo proxy.  
  
4.  In base a specifici requisiti di rete, determinare se è necessario utilizzare un server proxy HTTP per inoltrare le richieste al servizio federativo. In caso affermativo, selezionare il **utilizzare un server proxy HTTP per l'invio di richieste al servizio federativo** casella di controllo in **indirizzo del server proxy HTTP** digitare l'indirizzo del server proxy, fare clic su **Test connessione** per verificare la connettività e quindi fare clic su **Avanti**.  
  
5.  Quando viene richiesto, specificare le credenziali necessarie stabilire un trust tra il proxy server federativo e il servizio federativo.  
  
    Per impostazione predefinita, solo l'account di servizio utilizzato dal servizio federativo o un membro del gruppo locale BUILTIN\\Administrators. può autorizzare un proxy server federativo.  
  
6.  Nel **applicazione delle impostazioni** pagina, esaminare i dettagli. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare il computer con queste impostazioni proxy.  
  
7.  Nel **risultati di configurazione** pagina, esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    Non esiste alcun \(MMC\) snap\ Microsoft Management Console-in da utilizzare per l'amministrazione di proxy server federativo. Per configurare le impostazioni per ognuno del proxy server federativo nell'organizzazione, utilizzare i cmdlet di Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurazione di una porta TCP/IP alternativa per le operazioni Proxy  
Per impostazione predefinita, il servizio proxy server federativo è configurato per utilizzare la porta TCP 443 per il traffico HTTPS e la porta 80 per il traffico HTTP per la comunicazione con il server federativo. Per configurare porte diverse, ad esempio la porta TCP 444 per HTTPS e la porta 81 per HTTP, è necessario completare le attività seguenti.  
  
> [!NOTE]  
> Se si intende distribuire inizialmente AD FS per operare con porte TCP/IP alternative, è necessario prima modificare le porte in binding del protocollo IIS per HTTP e HTTPS nel server federativo sia nel computer proxy server federativo. Questo deve essere eseguita prima di eseguire la configurazione guidata di ADFS per la configurazione iniziale. Se si configura \(IIS\) Internet Information Services prima di tutto, le impostazioni della porta TCP/IP alternative vengono individuate durante la configurazione basata su wizard\ all'interno di ADFS e la procedura seguente non è necessaria. Se si desidera modificare le impostazioni delle porte in un secondo momento, aggiornare i binding del protocollo IIS prima di tutto, quindi utilizzare la procedura seguente per aggiornare le impostazioni delle porte in modo appropriato. Per ulteriori informazioni sulla modifica dei binding IIS, vedere [articolo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) della Microsoft Knowledge Base.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Per configurare porte TCP/IP alternative per i proxy server federativo da utilizzare  
  
1.  Configurare il server federativo per l'utilizzo di porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito includendolo con il *HttpsPort* e *HttpPort* come parte di opzioni di **set-ADFSProperties** cmdlet. Ad esempio, per configurare queste porte, usare i comandi seguenti nella sessione di Windows PowerShell nel computer del server federativo:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurare il proxy server federativo per l'utilizzo di porte non predefinite.  
  
    A tale scopo, specificare il numero di porta non predefinito includendolo con il *HttpsPort* e *HttpPort* come parte di opzioni di **set-ADFSProxyProperties** cmdlet. Ad esempio, per configurare queste porte, usare i comandi seguenti nella sessione di Windows PowerShell nel computer del server federativo:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URL dell'endpoint non sono abilitati per impostazione predefinita per il servizio proxy server federativo. Se si configura una nuova installazione di server federativo, è necessario abilitare prima gli endpoint servizio di proxy server federativo. Ad esempio, si presuppone che tutti gli endpoint a cui fa riferimento nell'esempio di questa procedura si sono abilitati per proxy selezionandoli nello snap-in di gestione di ADFS e quindi selezionando **Abilita su proxy**.  
  
3.  Aggiornare l'installazione di IIS nel proxy server federativo in modo che \(SAML\) Security Assertion Markup Language e gli endpoint WS-Trust siano configurati per riflettere il numero di porta aggiornato. A tale scopo, è possibile utilizzare Blocco note per modificare le operazioni seguenti nel file Web. config, che si trova in systemdrive%\\inetpub\\adfs\\ls\\ sul computer proxy server federativo. Ad esempio, supponendo che si dispone di un server federativo denominato sts1.contoso.com e il nuovo numero di porta sia 444, Sfoglia per aprire il file Web. config nel blocco note nel computer proxy server federativo, individuare la sezione seguente, modificare il numero di porta come evidenziato qui sotto e quindi salvare e chiudere il blocco note.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Aggiungere l'account utente del servizio proxy server federativo per il controllo di accesso elenco \(ACL\) per gli URL dell'endpoint correlati. Ad esempio, se il numero di porta è 1234 e l'account utente utilizzato per eseguire il servizio proxy di Active Directory FSfederation server in è l'account del servizio di rete predefinita, digitare il comando seguente al prompt dei comandi:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Il server federativo sia i computer proxy server federativi, è necessario eseguire i comandi precedenti.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

