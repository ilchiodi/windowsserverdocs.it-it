---
title: Configure a Multi-Forest Deployment
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto in un ambiente con più foreste in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c8feff2-cae1-4376-9dfa-21ad3e4d5d99
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5321cc5339bdc38eec0726c108ec42fe0112c772
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812192"
---
# <a name="configure-a-multi-forest-deployment"></a>Configure a Multi-Forest Deployment

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare una distribuzione a più foreste di Accesso remoto in diversi scenari. In tutti gli scenari si presuppone che DirectAccess sia attualmente distribuito in una sola foresta, Forest1, e venga configurato per essere utilizzato in una nuova foresta, Forest2.  
  
## <a name="AccessForest2"></a>Accedere alle risorse da Forest2  
In questo scenario, DirectAccess è già distribuito in Forest1 e viene configurato per consentire ai client di Forest1 di accedere alla rete aziendale. Per impostazione predefinita, i client connessi tramite DirectAccess possono accedere solo alle risorse presenti in Forest1 e non sono in grado di accedere ad alcun server contenuto in Forest2.  
  
#### <a name="to-enable-directaccess-clients-to-access-resources-from-forest2"></a>Per abilitare l'accesso alle risorse di Forest2 da parte dei client DirectAccess  
  
1.  Se il suffisso DNS di Forest2 non fa parte del suffisso DNS di Forest1, aggiungere regole NRPT con i suffissi dei domini contenuti in Forest2 e, facoltativamente, aggiungere i suffissi dei domini in Forest2 all'elenco di ricerca dei suffissi DNS.  
  
2.  Se IPv6 viene distribuito nella rete interna, aggiungere i prefissi IPv6 interni pertinenti in Forest2.  
  
## <a name="EnableForest2DA"></a>Abilitare i client da Forest2 alla connessione tramite DirectAccess  
In questo scenario, viene configurata la distribuzione di Accesso remoto per consentire ai client di Forest2 di accedere alla rete aziendale. Si presuppone che i gruppi di sicurezza richiesti per i computer client di Forest2 siano già stati creati.   
  
#### <a name="to-allow-clients-from-forest2-to-access-the-corporate-network"></a>Per consentire ai client di Forest2 di accedere alla rete aziendale  
  
1.  Aggiungere il gruppo di sicurezza dei client da Forest2.  
  
2.  Se il suffisso DNS di Forest2 non fa parte del suffisso DNS di Forest1, aggiungere regole NRPT con i suffissi di dominio dei client contenuti in Forest2 per abilitare l'accesso ai controller di dominio per l'autenticazione e, facoltativamente, aggiungere i suffissi dei domini in Forest2 al suf DNS correggere l'elenco di ricerca. 
  
3.  Aggiungere i prefissi IPv6 interni in Forest2 per consentire a DirectAccess di creare il tunnel IPsec ai controller di dominio per l'autenticazione.  
  
4.  Aggiornare l'elenco dei server di gestione.  
  
## <a name="AddEPForest2"></a>Aggiungere punti di ingresso da Forest2  
In questo scenario, DirectAccess viene distribuito in una configurazione multisito in Forest1 e si desidera aggiungere un server di Accesso remoto, DA2, da Forest2 come punto di ingresso alla distribuzione multisito DirectAccess esistente.  
  
#### <a name="to-add-a-remote-access-server-from-forest2-as-an-entry-point"></a>Per aggiungere un server di Accesso remoto come punto di ingresso da Forest2  
  
1.  Verificare che l'amministratore di Accesso remoto disponga delle autorizzazioni sufficienti per scrivere oggetti Criteri di gruppo nel dominio di DA2 e che sia un amministratore locale in DA2.  
  
2.  Aggiungere DA2 come punto di ingresso.   
  
3.  Aggiungere regole NRPT con i suffissi dei domini contenuti in Forest2 per consentire di accedere ai controller di dominio per l'autenticazione e, facoltativamente, aggiungere i suffissi dei domini in Forest2 all'elenco di ricerca dei suffissi DNS.  
  
4.  Aggiungere i prefissi IPv6 interni pertinenti in Forest2 per abilitare lo stabilimento del tunnel IPsec alle risorse aziendali da parte di Accesso remoto e verificare il corretto funzionamento dei probe NCSI.  
  
5.  Aggiornare l'elenco dei server di gestione.  
  
## <a name="OTPMultiForest"></a>Configurare l'autenticazione OTP in una distribuzione a più foreste  
Per una configurazione OTP in una distribuzione a più foreste tenere presente i termini seguenti:  
  
-   Foreste autorità di certificazione, il principale infrastruttura a chiave pubblica dell'albero autorità di certificazione radice.  
  
-   Autorità di certificazione aziendale, tutti gli altri CAs.  
  
-   La foresta delle risorse dell'insieme di strutture che contiene la CA radice e viene considerato il 'gestione foresta/dominio'.  
  
-   Foresta account-All altre foreste nella topologia.  
  
Per questa procedura è necessario lo script PKISync.ps1 di PowerShell. Vedere [Servizi certificati Active Directory: Script PKISync.ps1 per registrazione certificato tra foreste](https://technet.microsoft.com/library/ff961506.aspx).  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
### <a name="BKMK_CertPub"></a>Configurare CA come autori di certificati  
  
1.  Abilitare il supporto di riferimento LDAP per tutte le CA globali (enterprise) in tutte le foreste eseguendo il comando seguente in un prompt dei comandi con privilegi elevati:  
  
    ```  
    certutil -setreg Policy\EditFlags +EDITF_ENABLELDAPREFERRALS  
    ```  
  
2.  Aggiungere tutti gli account computer della CA globale (enterprise) ai gruppi di sicurezza Cert Publishers di Active Directory in ogni foresta di account.  
  
3.  Riavviare tutti i servizi certsvc in tutti i computer dell'autorità di certificazione in tutte le foreste eseguendo il comando seguente in un prompt dei comandi con privilegi elevati:  
  
    ```  
    net stop certsvc && net start certsvc  
    ```  
  
4.  Estrarre il certificato della CA radice eseguendo questo comando da un prompt dei comandi con privilegi elevati:  
  
    ```  
    certutil -config <Computer-Name>\<Root-CA-Name> -ca.cert <root-ca-cert-filename.cer>  
    ```  
  
    (Se si esegue il comando nella CA radice è possibile omettere le informazioni di connessione - < nome-Computer > config\\< nome-CA-radice >)  
  
    1.  Importare il certificato CA radice estratto nel precedente passaggio nella CA della foresta degli account eseguendo il comando seguente in un prompt dei comandi con privilegi elevati:  
  
        ```  
        certutil -dspublish -f <root-ca-cert-filename.cer> RootCA  
        ```  
  
    2.  Foresta delle risorse GRANT certificato modelli delle autorizzazioni di scrittura per il \<foresta Account\>\\< account di amministratore\>.  
  
    3.  Estrarre tutti i certificati CA globale (enterprise) della foresta delle risorse eseguendo questo comando da un prompt dei comandi con privilegi elevati:  
  
        ```  
        certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
        ```  
  
        (Se si esegue il comando nella CA radice è possibile omettere le informazioni di connessione - < nome-Computer > config\\< nome-CA-radice >)  
  
    4.  Importare i certificati della CA globale (enterprise) estratti nel precedente passaggio nella CA della foresta degli account eseguendo il comando seguente in un prompt dei comandi con privilegi elevati:  
  
        ```  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> NTAuthCA  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> SubCA  
        ```  
  
    5.  Rimuovere i modelli di certificato OTP della foresta degli account dall'elenco dei modelli di certificato emessi.  
  
### <a name="BKMK_DelImp"></a>Eliminare e importare i modelli di certificato OTP  
  
1.  Eliminare i modelli di certificato OTP dalla foresta degli account, vale a dire Forest2.  
  
2.  Copiare i modelli di certificato e gli oggetti OID della foresta delle risorse in ogni foresta degli account utilizzando i seguenti comandi di PowerShell:  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <DA OTP registration authority template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <Secure DA OTP logon certificate template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Oid -f  
    ```  
  
### <a name="BKMK_Publish"></a>Pubblicare i modelli di certificato OTP  
  
-   Emettere i modelli di certificato nuovi importati in tutte le CA delle foreste degli account.  
  
### <a name="BKMK_Extract"></a>Estrarre e sincronizzare le autorità di certificazione  
  
1.  Estrarre tutti i certificati della CA globale (enterprise) dalle foreste degli account eseguendo il comando seguente in un prompt dei comandi con privilegi elevati:  
  
    ```  
    certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
    ```  
  
2.  Sincronizzare le autorità di certificazione tra le foreste, dalle foreste degli account alla foresta delle risorse, utilizzando il seguente comando di PowerShell:  
  
    ```  
    .\PKISync.ps1 -sourceforest <account forest DNS> -targetforest <resource forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
3.  Sincronizzare le autorità di certificazione tra le foreste, dalle foresta delle risorse alla foresta degli account, utilizzando il seguente comando di PowerShell:  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
## <a name="configuration-procedures"></a>Procedure di configurazione  
Nelle sezioni seguenti sono illustrate le procedure di configurazione per le distribuzioni di scenario sopra riportate. Dopo aver completato una procedura, tornare allo scenario per continuare.  
  
### <a name="NRPT_DNSSearchSuffix"></a>Aggiungere regole NRPT e suffissi DNS  
I client che si connettono alla rete aziendale per mezzo di DirectAccess utilizzano la tabella dei criteri di risoluzione dei nomi (NRPT) per determinare quale server DNS utilizzare per risolvere l'indirizzo di risorse diverse. In questo modo il client può risolvere gli indirizzi delle risorse aziendali e conservare una classificazione adeguata sia all'interno che all'esterno dell'azienda, indispensabile per continuare a utilizzare DirectAccess. Gli strumenti di configurazione di DirectAccess rilevano automaticamente il suffisso DNS radice di Forest1 e lo aggiungono alla tabella NRPT. I suffissi FQDN di Forest2, invece, non vengono aggiunti automaticamente alla tabella NRPT e l'amministratore di Accesso remoto deve aggiungerli manualmente.  
  
L'elenco di ricerca suffissi DNS consente ai client di utilizzare nomi di etichetta breve, anziché nomi di dominio completo. Gli strumenti di configurazione di Accesso remoto aggiungono automaticamente tutti i domini di Forest1 all'elenco di ricerca dei suffissi DNS. Se si desidera abilitare l'utilizzo di nomi di etichetta breve per le risorse di Forest2 nei client, è necessario aggiungerli automaticamente.  
  
##### <a name="to-add-a-dns-suffix-to-the-nrpt-table-and-domain-suffixes-to-the-dns-suffix-search-list"></a>Per aggiungere un suffisso DNS alla tabella NRPT e suffissi di dominio all'elenco di ricerca dei suffissi DNS  
  
1.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Modifica** nell'area **Passaggio 3 Server di infrastruttura**.  
  
2.  Nella pagina **Server dei percorsi di rete** fare clic su **Avanti**.  
  
3.  Nella tabella della pagina **DNS** immettere eventuali suffissi di nomi aggiuntivi che fanno parte della rete aziendale in Forest 2. In **Indirizzo del server DNS** inserire manualmente l'indirizzo del server DNS o fare clic su **Rileva**. Se non si immette l'indirizzo, le nuove voci vengono applicate come esenzioni della tabella NRPT. Fare quindi clic su **Avanti**.  
  
4.  Facoltativo: nella pagina **Elenco di ricerca suffissi DNS** aggiungere eventuali suffissi DNS inserendo il suffisso nella casella **Nuovo suffisso** e facendo clic su **Aggiungi**. Fare quindi clic su **Avanti**.  
  
5.  Nella pagina **Gestione** fare clic su **Fine**.  
  
6.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Fine**.  
  
7.  Nella finestra di dialogo **Controllo Accesso remoto** fare clic su **Applica**.  
  
8.  Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
### <a name="IPv6Prefix"></a>Aggiungere un prefisso IPv6 interno  
  
> [!NOTE]  
> È necessario aggiungere un prefisso IPv6 interno solo quando IPv6 viene distribuito nella rete interna.  
  
Accesso remoto gestisce un elenco di prefissi IPv6 per le risorse aziendali. È possibile accedere alle risorse utilizzando i prefissi IPv6 solo da client connessi tramite DirectAccess. Poiché la console Gestione accesso remoto e i comandi di Windows PowerShell automaticamente aggiungere i prefissi IPv6 di Forest1 e non potrebbero aggiungere i prefissi di altre foreste, è necessario aggiungere manualmente qualsiasi mancante i prefissi di Forest2.  
  
##### <a name="to-add-an-ipv6-prefix"></a>Per aggiungere un prefisso IPv6  
  
1.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Modifica** nell'area **Passaggio 2 Server di Accesso remoto**.  
  
2.  Nella Configurazione guidata server di Accesso remoto fare clic su **Configurazione prefissi**.  
  
3.  In **Prefissi IPv6 della rete interna** aggiungere eventuali prefissi IPv6 nella pagina **Configurazione prefissi** separandoli con un punto e virgola; ad esempio, 2001:db8:1::/64;2001:db8:2::/64. Fare quindi clic su **Avanti**.  
  
4.  Nella pagina **Autenticazione** fare clic su **Fine**.  
  
5.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Fine**.  
  
6.  Nella finestra di dialogo **Controllo Accesso remoto** fare clic su **Applica**.  
  
7.  Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
### <a name="SGs"></a>Aggiungere gruppi di sicurezza client  
Per consentire ai computer client Windows 8 di Forest2 di accedere alle risorse tramite DirectAccess, è necessario aggiungere il gruppo di sicurezza da Forest2 alla distribuzione di accesso remoto.  
  
##### <a name="to-add-windows-8-client-security-groups"></a>Per aggiungere gruppi di sicurezza client Windows 8  
  
1.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Modifica** nell'area **Passaggio 1 Client remoti**.  
  
2.  Nella Configurazione guidata client DirectAccess fare clic su **Seleziona gruppi** e nella pagina **Seleziona gruppi** scegliere **Aggiungi**.  
  
3.  Nella finestra di dialogo **Seleziona gruppi** selezionare i gruppi di sicurezza contenenti i computer client DirectAccess. Fare quindi clic su **Avanti**.  
  
4.  Nella pagina **Assistente connettività di rete** fare clic su **Fine**.  
  
5.  Nel riquadro centrale della console di gestione Accesso remoto fare clic su **Fine**.  
  
6.  Nella finestra di dialogo **Controllo Accesso remoto** fare clic su **Applica**.  
  
7.  Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
Per attivare Windows 7 ai computer client di Forest2 di accedere alle risorse tramite DirectAccess quando multisito è abilitato, è necessario aggiungere il gruppo di sicurezza da Forest2 alla distribuzione di accesso remoto per ogni punto di ingresso. Per informazioni sull'aggiunta di gruppi di sicurezza di Windows 7, vedere la descrizione del **supporto Client** pagina 3.6. Abilitare la distribuzione multisito.  
  
### <a name="RefreshMgmtServers"></a>Aggiornare l'elenco di server di gestione  
Accesso remoto rileva automaticamente i server di infrastruttura in tutte le foreste che contengono oggetti Criteri di gruppo per la configurazione di DirectAccess. Se DirectAccess è stato distribuito in un server di Forest1, l'oggetto Criteri di gruppo del server verrà scritto nel relativo dominio in Forest1. Se è abilitato l'accesso a DirectAccess per i client di Forest2, l'oggetto Criteri di gruppo del client verrà scritto in un dominio in Forest2.  
  
Il processo di individuazione automatica dei server di infrastruttura è necessario per consentire di accedere tramite DirectAccess ai controller di dominio e a System Center Configuration Manager. Il processo di individuazione deve essere avviato manualmente.  
  
##### <a name="to-refresh-the-management-servers-list"></a>Per aggiornare l'elenco dei server di gestione  
  
1.  Nella console di gestione Accesso remoto fare clic su **Configurazione**, quindi, nel riquadro **Attività** fare clic su **Aggiorna server di gestione**.  
  
2.  Nella finestra di dialogo **Aggiornamento server di gestione** fare clic su **Chiudi**.  
  


