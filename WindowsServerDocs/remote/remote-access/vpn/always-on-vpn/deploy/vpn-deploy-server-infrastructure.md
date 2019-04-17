---
title: Configurare l'infrastruttura di Server
description: In questo passaggio, installare e configurare i componenti sul lato server necessari per supportare la connessione VPN. I componenti sul lato server includono la configurazione dell'infrastruttura PKI per distribuire i certificati usati da utenti, il server VPN e il server dei criteri di rete.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 21b494bea1990fb8424537205db483d977331465
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067406"
---
# Passaggio 2. Configurare l'infrastruttura di server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 1. Pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)<br>
& #187;  [ **Successivo:** passaggio 3. Configurare il Server di accesso remoto per sempre su VPN](vpn-deploy-ras.md)


In questo passaggio, installare e configurare i componenti sul lato server necessari per supportare la connessione VPN. I componenti sul lato server includono la configurazione dell'infrastruttura PKI per distribuire i certificati usati da utenti, il server VPN e il server dei criteri di rete.  Anche configurare RRAS per supportare le connessioni IKEv2 e il server dei criteri di rete per eseguire l'autorizzazione per le connessioni VPN.

## Configurare la registrazione automatica certificato in Criteri di gruppo
In questa procedura, configurare criteri di gruppo nel controller di dominio in modo che i membri del dominio richiedono automaticamente i certificati dell'utente e computer. In questo modo consente agli utenti VPN richiedere e recuperare i certificati utente che l'autenticazione automaticamente le connessioni VPN. Analogamente, questo criterio consente di server dei criteri di rete per richiedere server i certificati di autenticazione automaticamente. 

Registrare manualmente i certificati nei server VPN.

>[!TIP]
>Per i computer aggiunti a un non domained, vedi [configurazione autorità di certificazione per non appartenenti al dominio aggiunti a un computer](#ca-configuration-for-non-domain-joined-computers) seguente. Dato che il server RRAS non è aggiunto a un dominio, la registrazione automatica non può essere usata per registrare il certificato di gateway VPN.  Di conseguenza, utilizzare una routine di richiesta di certificato offline. 


1.  In un controller di dominio, Apri Gestione criteri di gruppo.

2.  Nel riquadro di spostamento, destro del mouse sul tuo dominio (ad esempio, corp.contoso.com) e fai clic su**Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.

3.  Nella finestra di dialogo Nuovo oggetto Criteri di gruppo, digita **La registrazione automatica dei criteri**e fai clic su **OK**.

4.  Nel riquadro di spostamento, destro **La registrazione automatica dei criteri**e fare clic su **Modifica**.

5.  Nell'Editor Gestione criteri di gruppo, completa i passaggi seguenti per configurare la registrazione automatica certificato di computer:

    1.  Nel riquadro di spostamento, fai clic sul **Computer Configuration\\Policies\\Windows Settings\\Security Settings\\Public chiave criteri**.

    2.  Nel riquadro dei dettagli, fai**Servizi Client-registrazione automatica del certificato -** e fare clic su **proprietà**.

    3.  Scegliere il Client Servizi certificati-registrazione automatica proprietà della finestra di dialogo nel **Modello di configurazione**, **abilitato**.

    4.  Seleziona**Rinnova i certificati scaduti, aggiornare, certificati in sospeso e Rimuovi certificati revocati**e**Aggiorna i certificati che utilizzano modelli di certificato**.

    5.  Fai clic su**OK**.

6.  Nell'Editor Gestione criteri di gruppo, completa la procedura seguente per configurare la registrazione automatica certificato di utente:

    1.  Nel riquadro di spostamento, fai clic su **Criteri di chiave Settings\\Public Settings\\Security Configuration\\Policies\\Windows utente**.

    2.  Nel riquadro dei dettagli, fai**Servizi Client-registrazione automatica del certificato -** e fare clic su **proprietà**.

    3.  Scegliere il Client Servizi certificati-registrazione automatica proprietà della finestra di dialogo nel **Modello di configurazione**, **abilitato**.

    4.  Seleziona**Rinnova i certificati scaduti, aggiornare, certificati in sospeso e Rimuovi certificati revocati**e**Aggiorna i certificati che utilizzano modelli di certificato**.

    5.  Fai clic su**OK**.

    6.  Chiudi l'Editor Gestione Criteri di gruppo.

7.  Gestione di criteri di gruppo di chiusura.

### Configurazione della CA per i computer aggiunti a un non appartenenti al dominio

Dato che il server RRAS non è aggiunto a un dominio, la registrazione automatica non può essere usata per registrare il certificato di gateway VPN.  Di conseguenza, utilizzare una routine di richiesta di certificato offline. 

1. Nel server RRAS, genera un file in base a criteri del certificato di esempio denominata **VPNGateway.inf** richiedere forniti nell'appendice (sezione 0) e personalizzare le voci seguenti: 

   - Nella sezione [NewRequest], Sostituisci vpn.contoso.com usato per il nome del soggetto con l'endpoint [_cliente_] VPN prescelto FQDN.

   - Nella sezione [estensioni], Sostituisci vpn.contoso.com utilizzato per nome alternativo del soggetto con l'endpoint [_cliente_] VPN prescelto FQDN.

2. Salva o copiare il file **VPNGateway.inf** in un percorso selezionato.

3. Da un prompt dei comandi con privilegi elevati, passa alla cartella contenente il file **VPNGateway.inf** e digita:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copia il file di output **VPNGateway.req** appena creato per un server di autorità di certificazione o Privileged Access Workstation (IMPRONTE). 

5. Salva o copiare il file **VPNGateway.req** in un percorso selezionato nel server di autorità di certificazione, o Privileged Access Workstation (IMPRONTE).

6. Da un prompt dei comandi con privilegi elevati, passa alla cartella contenente il file VPNGateway.req creato nel passaggio precedente e tipo: 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Se richiesto dalla finestra dell'elenco di autorità di certificazione, seleziona la CA Enterprise appropriate per soddisfare la richiesta di certificato.

8. Copia il file di output **VPNGateway.cer** appena creato al server RRAS. 

9. Salva o copiare il file **VPNGateway.cer** in un percorso per il server RRAS selezionato.

10. Da un prompt dei comandi con privilegi elevati, passa alla cartella contenente il file VPNGateway.cer creato nel passaggio precedente e tipo:
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Esegui lo snap-in MMC certificati come descritto [di seguito](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) si seleziona l'opzione di **account del Computer** .

12. Assicurarsi che sia presente un certificato valido per il server RRAS con le proprietà seguenti:

   - **Scopi:** Server di autenticazione, sicurezza IP IKE intermedio 

   - **Modello di certificato:** [_Cliente_] Server VPN

#### Esempio: Script VPNGateway.inf

Qui puoi vedere un esempio di script di un criterio di richiesta di certificato usato per richiedere un certificato di gateway VPN usando un processo fuori banda.

>[!TIP]
>Puoi trovare una copia dello script VPNGateway.inf nel VPN offrendo IP Kit all'interno della cartella di criteri di richiesta di certificato. Aggiornare solo il 'Soggetto' e '\_continue\_' con valori specifici dei clienti.

```
[Version] 

Signature="$Windows NT$"

[NewRequest]
Subject = "CN=vpn.contoso.com"
Exportable = FALSE   
KeyLength = 2048     
KeySpec = 1          
KeyUsage = 0xA0      
MachineKeySet = True
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
RequestType = PKCS10 

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=vpn.contoso.com&"

```

## Creare gli utenti della VPN, il server VPN e gruppi di server dei criteri di rete

In questa procedura, è possibile aggiungere un nuovo gruppo di Active Directory (AD) che contiene gli utenti autorizzati a usare la connessione VPN per connettersi alla rete dell'organizzazione.

Questo gruppo ha due scopi:

-   Definisce quali utenti è consentito eseguire la registrazione automatica per i certificati utente che richiede la connessione VPN.

-   Definisce quali utenti autorizza i criteri di rete per l'accesso VPN.

Usando un gruppo personalizzato, se si desidera mai revocare l'accesso VPN dell'utente, è possibile rimuovere tale utente dal gruppo.

Anche aggiungere un gruppo di server VPN e un altro gruppo contenente i server dei criteri di rete. Utilizzare questi gruppi per limitare le richieste di certificati ai relativi membri.

>[!NOTE] 
>Ti consigliamo di VPN server che si trovano nel/perimetro della DMA non essere aggiunti al dominio. Tuttavia, se si preferisce avere il server VPN appartenenti al dominio per una migliore gestibilità (criteri di gruppo, Backup/Monitoring agent, nessun utente locale per gestire e così via), quindi Aggiungi un gruppo di Active Directory per il modello di certificato del server VPN.

### Configurare il gruppo utenti VPN

1.  In un controller di dominio, Apri agli utenti di Active Directory e i computer.

2.  Fai un contenitore o unità organizzativa, scegli **Nuovo** e fai clic sul **gruppo**.

3.  **Nome del gruppo**, digita **Gli utenti della VPN**e fai clic su **OK**.

4.  **Gli utenti della VPN**di mouse e scegliere **le proprietà**.

5.  Nella scheda **membri** della finestra di dialogo VPN le proprietà di utenti, fai clic su **Aggiungi**.

6.  Nella finestra di dialogo Selezione utenti, aggiungere tutti gli utenti che hanno l'esigenza di VPN accedere e fai clic su **OK**.

7.  Chiudere i computer e utenti di Active Directory.

### Configurare i gruppi di server VPN e i server dei criteri di rete

1.  In un controller di dominio, Apri agli utenti di Active Directory e i computer.

2.  Fai un contenitore o unità organizzativa, scegli **Nuovo** e fai clic sul **gruppo**.

3.  Nel **nome del gruppo**, digita **I server VPN**e fai clic su **OK**.

4.  Destro del mouse sul **Server VPN**e fare clic su **proprietà**.

5.  Nella scheda **membri** della finestra di dialogo Proprietà server VPN, fai clic su **Aggiungi**.

6.  Fai clic su **Tipi di oggetto**, seleziona la casella di controllo di **computer** e fai clic su **OK**.

7.  In **Immettere i nomi degli oggetti da selezionare**, digita i nomi dei server VPN e fai clic su **OK**.

8.  Fai clic su **OK** per chiudere la finestra di dialogo Proprietà server VPN.

9.  Ripeti i passaggi precedenti per il gruppo di server dei criteri di rete.

10. Chiudere i computer e utenti di Active Directory.

## Creare il modello di autenticazione utente

In questa procedura, configurare un modello di autenticazione client-server personalizzato. Questo modello è necessario perché si desidera migliorare la sicurezza complessiva del certificato selezionando i livelli di compatibilità aggiornato e scegliendo il Provider di crittografia della piattaforma Microsoft. Questa ultima modifica consente di usare il modulo TPM nei computer client per proteggere il certificato. Per una panoramica del TPM, vedere [Panoramica di Trusted Platform Module tecnologia](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedura:**

1.  Nella CA, aprire l'autorità di certificazione.

2.  Nel riquadro di spostamento, destro**Modelli di certificato**e fai clic su**Gestisci**.

3.  Nella console di modelli di certificato, fai**utente**e fai clic su**Duplica modello**.

    >[!WARNING]
    >Non selezionare **Applica** o **OK** in qualsiasi momento prima del passaggio 10.  Se si sceglie questi pulsanti prima che inizi tutti i parametri, numerose opzioni diventano fissa e non è più modificabile. Ad esempio, nella scheda **crittografia** , se _Il Provider di archiviazione crittografia Legacy_ Mostra nel campo di categoria Provider, risulta disabled, impedendo le eventuali ulteriori modifiche. L'unica alternativa consiste nell'eliminare il modello e ricrearli.  

4.  Nella finestra di proprietà del nuovo modello finestra di dialogo in**Generale**scheda, Esegui le operazioni seguenti:

    1.  Nel **modello di nome visualizzato**, digita **L'autenticazione utente VPN**.

    2.  Deseleziona la casella di controllo **Pubblica certificato in Active Directory** .

5.  Sulla**sicurezza**scheda, Esegui le operazioni seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  A Seleziona utenti, computer, account del servizio o nella finestra di dialogo gruppi, digita **Gli utenti della VPN**e fai clic su **OK**.

    3.  Nel **gruppo di utenti e gruppi**, fai clic sugli **Utenti della VPN**.

    4.  **Le autorizzazioni per gli utenti della VPN**, seleziona le caselle di controllo di **registrazione** e **registrazione automatica** nella colonna **Consenti** .

       >[!TIP]
       >Assicurati di mantenere la casella di controllo di lettura selezionata. In altre parole, è necessario le autorizzazioni di lettura per la registrazione. 

    5.  Nel **gruppo di utenti e gruppi**, scegliere **Gli utenti del dominio**, quindi **rimuovere**.

6.  Nella scheda **compatibilità** , procedi come segue:

    1.  **Autorità di certificazione**, fai clic su **Windows Server2012R2**.

    2.  Nella finestra di dialogo **modifiche risultanti** , fai clic su **OK**.

    3.  Nel **destinatario certificato**, fai clic su **Windows 8.1/Windows Server2012R2**.

    4.  Nella finestra di dialogo **modifiche risultanti** , fai clic su **OK**.

7.  Nella scheda **Gestione richiesta** , deselezionare la casella di controllo **Consenti la chiave privata esportabile** .

8.  Nella scheda **crittografia** , procedi come segue:

    1.  Nella **Categoria Provider**, fai clic su **Provider di archiviazione chiavi**.

    2.  Fai clic su **richieste necessario utilizzare uno dei seguenti provider**.

    3.  Seleziona la casella di controllo di **Provider di crittografia della piattaforma Microsoft** .

9.  Nella scheda **Nome soggetto** , se non hai un indirizzo di posta elettronica elencato in tutti gli account utente, deseleziona le caselle di controllo **Includi nome di posta elettronica nel nome soggetto** e il **nome di posta elettronica** .

10. Fai clic su**OK** per salvare il modello di certificato di autenticazione utente VPN.

11. Console modelli di certificato Chiudi.

12. Nel riquadro di spostamento di theCertification Authoritysnap-in, fai**Modelli di certificato**, scegli **Nuovo** e quindi fai clic sul**Modello di certificato da rilasciare**.

13. Fai clic su**Autenticazione utente VPN**e fai clic su**OK**.

14. Chiudi snap-in autorità theCertification.

## Creare il modello di autenticazione Server VPN

In questa procedura, è possibile configurare un nuovo modello di autenticazione Server per il server VPN. Aggiungere che il criterio di applicazione Intermediate IKE IPsec (IP Security) consente al server di filtro dei certificati, se più di un certificato è disponibile con l'autenticazione Server Utilizzo chiavi avanzato.

>[!IMPORTANT] 
>Dato che i client VPN accedono a questo server dalla rete Internet pubblica, l'oggetto e i nomi alternativi sono diversi rispetto al nome del server interni. Di conseguenza, non è possibile registrare automaticamente questo certificato nel server VPN.

**Prerequisiti:**<p>
Server VPN appartenenti al dominio

**Procedura:** 

1.  Nella CA, aprire l'autorità di certificazione.

2.  Nel riquadro di spostamento, destro **Modelli di certificato**e fai clic su**Gestisci**.

3.  Nella console di modelli di certificato, fai**Server RAS e IAS**e fai clic su**Duplica modello**.

4.  Nella finestra di proprietà del nuovo modello finestra di dialogo in**Generale**della scheda nel **nome visualizzato modello**, Immetti un nome descrittivo per il server VPN, ad esempio, **L'autenticazione Server VPN** o **Server RADIUS**.

5.  Sulle**estensioni**scheda, Esegui le operazioni seguenti:

    1.  Fai clic su**Criteri delle applicazioni**e fare clic su**Modifica**.

    2.  Nella finestra di dialogo **Estensione Modifica criteri di applicazione** , fai clic su**Aggiungi**.

    3.  Nella finestra di dialogo **Aggiungi criterio di applicazione** , fai clic su **sicurezza IP IKE intermedi**e fai clic su **OK**.<p>Aggiunta di IP sicurezza IKE intermedio per l'EKU utili per gli scenari in cui è presente più di un certificato di autenticazione server sul server VPN. Quando è presente la protezione IP IKE intermedi, IPSec utilizza solo il certificato con entrambe le opzioni EKU. In caso contrario, IKEv2 autenticazione potrebbe avere esito negativo con errore 13801: le credenziali di autenticazione IKE sono semplicemente inaccettabile.

    4.  Fai clic su **OK** per tornare alla**Proprietà del nuovo modello**nella finestra di dialogo.

6.  Sulla**sicurezza**scheda, Esegui le operazioni seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  Nella finestra di dialogo **Selezione utenti, computer, account di servizio o gruppi** , digita **I server VPN**e fai clic su **OK**.

    3.  Nel **gruppo di utenti e gruppi**, fai clic su **Server VPN**.

    4.  **Le autorizzazioni per i server VPN**, seleziona la casella di controllo nella colonna **Consenti** **registrazione** .

    5.  Nel **gruppo di utenti e gruppi**, fai clic su **RAS e IAS**e fai clic su **rimuovere**.

7.  Nella scheda **Nome soggetto** , procedi come segue:

    1.  Fai clic su **Inserisci nella richiesta**.

    2.  Nella finestra di dialogo di avviso **Modelli di certificato** , fai clic su **OK**.

8.  (Facoltativo) *Se vuoi configurare l'accesso condizionale per la connettività VPN*, fare clic sulla scheda **Gestione richiesta** e fai clic su **chiave privata esportabile** per selezionarlo.

9.  Fai clic su**OK** per salvare il modello di certificato del Server VPN.

10. Console modelli di certificato Chiudi.

11. Nel riquadro thenavigation della certificazione Authoritysnap-in fai**Modelli di certificato**, scegli **Nuovo** e quindi fai clic sul**Modello di certificato da rilasciare**.

12. Riavviare i servizi di autorità di certificazione.

13. Seleziona il nome che scelto nel passaggio 4 sopra e fai clic su**OK**.

14. Chiudi snap-in autorità theCertification.

## Creare il modello di autenticazione Server dei criteri di rete

Il modello di certificato terzo e l'ultimo per creare è il modello di autenticazione Server dei criteri di rete. Il modello di autenticazione Server dei criteri di rete è una semplice copia del modello di Server RAS e IAS protetto al gruppo di Server dei criteri di rete che creato in precedenza in questa sezione. 

È necessario configurare il certificato per la registrazione automatica.

**Procedura:**

1.  Nella CA, aprire l'autorità di certificazione.

2.  Nel riquadro di spostamento, destro **Modelli di certificato**e fai clic su**Gestisci**.

3.  Nella console di modelli di certificato, fai**Server RAS e IAS**e seleziona **Duplica modello**.

4.  Nella finestra di proprietà del nuovo modello finestra di dialogo in**Generale**, nella scheda **nome visualizzato del modello**, tipo di **Autenticazione Server dei criteri di rete**.

5.  Sulla**sicurezza**scheda, Esegui le operazioni seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  Nella finestra di dialogo **Selezione utenti, computer, account di servizio o gruppi** , digita **I server dei criteri di rete**e fai clic su **OK**.

    3.  Nel **gruppo di utenti e gruppi**, fai clic su **Server dei criteri di rete**.

    4.  **Le autorizzazioni per i server dei criteri di rete**, seleziona le caselle di controllo di **registrazione** e **registrazione automatica** nella colonna **Consenti** .

    5.  Nel **gruppo di utenti e gruppi**, fai clic su **RAS e IAS**e fai clic su **rimuovere**.

6.  Fai clic su**OK** per salvare il modello di certificato del Server dei criteri di rete.

7.  Console modelli di certificato Chiudi.

8.  Nel riquadro thenavigation della certificazione Authoritysnap-in fai**Modelli di certificato**, scegli **Nuovo** e quindi fai clic sul**Modello di certificato da rilasciare**.

9.  Fai clic su**Autenticazione Server dei criteri di rete**e fai clic su**OK**.

10. Chiudi snap-in autorità theCertification.

## Eseguire la registrazione e convalidare il certificato utente

Poiché stai usando criteri di gruppo per registrare automaticamente i certificati utente, è necessario aggiornare solo i criteri e Windows 10 verrà registrato automaticamente l'account utente per il certificato corretto. È quindi possibile convalidare il certificato nella console di certificati.

**Procedura:**

1.  Accedi a un computer client aggiunto al dominio in qualità di membro del gruppo di **Utenti VPN** .

2.  Premi il tasto Windows + R, digitare **gpupdate /force**e premi INVIO.

3.  Nel menu Start, digita **certmgr**e premi INVIO.

4.  Nello snap-in certificati in **personali**, fai clic su **certificati**. I certificati vengono visualizzati nel riquadro dei dettagli.

5.  Fai il certificato con il nome utente dominio corrente, quindi **Apri**.

6.  Nella scheda **Generale** , verifica che la data elencata nel **valido da** è la data odierna. In caso contrario, si potrebbe avere selezionato il certificato corretto.

7.  Fai clic su **OK**e Chiudi lo snap-in certificati.

## Eseguire la registrazione e convalidare i certificati server

A differenza del certificato utente, è necessario registrare manualmente il certificato del server VPN. Dopo che hai registrato, convalidarla tramite lo stesso processo che è utilizzato per il certificato dell'utente. Ad esempio il certificato dell'utente, il server dei criteri di rete registra automaticamente il proprio certificato di autenticazione, in modo che tutto ciò che devi fare è la convalida.

>[!NOTE] 
>Potrebbe essere necessario riavviare il server VPN e dei criteri di rete per consentire loro di aggiornare l'appartenenza al gruppo prima di poter completare questi passaggi.

### Eseguire la registrazione e convalidare il certificato del server VPN

1.  Nel menu di avvio del server VPN, digitare **certlm**e premi INVIO.

2.  Fai **personali**, fai clic su **Tutte le attività** e quindi fai clic su **Richiedi nuovo certificato** per avviare la procedura guidata registrazione certificato.

3.  Nella pagina Prima di iniziare fai clic su **Avanti**.

4.  Nella pagina Seleziona criteri di registrazione certificato, fai clic su **Avanti**.

5.  Nella pagina richiesta certificati, fai clic sulla casella di controllo accanto al server VPN per selezionarlo.

6.  Sotto la casella di controllo del server VPN, fai clic su **che sono necessarie ulteriori informazioni** per aprire la finestra di dialogo delle proprietà del certificato e completare la procedura seguente:

    1.  Fare clic sulla scheda **soggetto** , fai clic sul **Nome comune** sotto il **nome dell'oggetto**, nel **tipo**.

    2.  Nella casella **nome soggetto**, **valore**, Immetti il nome del client del dominio esterno utilizzato per connettersi alla VPN, ad esempio, vpn.contoso.com e fai clic su **Aggiungi**.

    3.  Nella casella **Nome alternativo**, **tipo**, fai clic su **DNS**.

    4.  Sotto il **Nome alternativo**, immettere nel **valore**, tutti i nomi client per connettersi alla VPN, ad esempio vpn.contoso.com, vpn, 132.64.86.2 server.

    5.  Dopo avere immesso ogni nome, fai clic su **Aggiungi** .

    6.  Al termine, fai clic su **OK**.

7.  Fai clic su **Registra**.

8.  Fai clic su **Fine**.

9.  Nello snap-in certificati in **personali**, fai clic su **certificati**.<p>I certificati elencati vengono visualizzati nel riquadro dei dettagli.

10. Fai il certificato con il nome del server VPN, quindi **Apri**.

11. Nella scheda **Generale** , verifica che la data elencata nel **valido da** è la data odierna. In caso contrario, è possibile il certificato corretto selezionata.

12. Nella scheda **Dettagli** , fai clic su **Utilizzo chiavi avanzato**e verificare che **la protezione IP IKE intermedi** e **Autenticazione Server** vengano visualizzati nell'elenco.

13. Fai clic su **OK** per chiudere il certificato.

14. Chiudere lo snap-in certificati.

### Convalidare il certificato del server dei criteri di rete

1.  Riavviare il server dei criteri di rete.

2.  Nel menu di avvio del server dei criteri di rete, digitare **certlm**e premi INVIO.

3.  Nello snap-in certificati in **personali**, fai clic su **certificati**.<p>I certificati elencati vengono visualizzati nel riquadro dei dettagli.

4.  Destro il certificato con il nome del server dei criteri di rete e quindi fai clic su **aperto**.

5.  Nella scheda **Generale** , verifica che la data elencata nel **valido da** è la data odierna. In caso contrario, è possibile il certificato corretto selezionata.

6.  Fai clic su **OK** per chiudere il certificato.

7.  Chiudere lo snap-in certificati.

## Passaggio successivo
[Passaggio 3. Configurare il Server di accesso remoto per VPN Always On](vpn-deploy-ras.md): In questo passaggio, configurare l'accesso remoto VPN per consentire le connessioni VPN IKEv2, Nega le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio di indirizzi IP per la connessione client VPN autorizzati.







---
