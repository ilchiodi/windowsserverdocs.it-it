---
title: Configurare l'infrastruttura di Server
description: In questo passaggio installare e configurare i componenti lato server necessari per supportare la connessione VPN. I componenti lato server è inclusa la configurazione di infrastruttura a chiave pubblica per distribuire i certificati usati da utenti, il server VPN e il server NPS.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832932"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Passaggio 2. Configurare l'infrastruttura di server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Precedente:** Passaggio 1. Pianificare la distribuzione VPN Always On](always-on-vpn-deploy-planning.md)<br>
&#187;  [ **Next:** Passaggio 3. Configurare il Server di accesso remoto per sempre in una VPN](vpn-deploy-ras.md)


In questo passaggio installare e configurare i componenti lato server necessari per supportare la connessione VPN. I componenti lato server è inclusa la configurazione di infrastruttura a chiave pubblica per distribuire i certificati usati da utenti, il server VPN e il server NPS.  È anche possibile configurare RRAS per supportare connessioni IKEv2 e il server dei criteri di rete per eseguire l'autorizzazione per le connessioni VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurare la registrazione automatica del certificato in Criteri di gruppo
In questa procedura configurare criteri di gruppo nel controller di dominio in modo che i membri del dominio richiedere automaticamente i certificati utente e computer. In questo modo consente agli utenti VPN richiedere e recuperare i certificati utente che l'autenticazione delle connessioni VPN automaticamente. Analogamente, questo criterio consente i server dei criteri di rete per richiedere server i certificati di autenticazione automaticamente. 

Registrare manualmente i certificati nel server VPN.

>[!TIP]
>Per i computer non domained unita in join, vedere [configurazione della CA per non di dominio i computer aggiunti al](#ca-configuration-for-non-domain-joined-computers) sotto. Poiché il server RRAS non è aggiunto a un dominio, la registrazione automatica non è utilizzabile per registrare il certificato del gateway VPN.  Pertanto, utilizzare una procedura di richiesta certificato non in linea. 


1.  In un controller di dominio, aprire Gestione criteri di gruppo.

2.  Nel riquadro di spostamento, fare doppio clic su dominio (ad esempio, corp.contoso.com) e fare clic su **crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.

3.  Nella finestra di dialogo Nuovo oggetto Criteri di gruppo, digitare **criterio di registrazione automatica**, fare clic su **OK**.

4.  Nel riquadro di spostamento, fare doppio clic su **criterio di registrazione automatica**, fare clic su **modificare**.

5.  In Editor Gestione criteri di gruppo, completare i passaggi seguenti per configurare la registrazione automatica certificati computer:

    1.  Nel riquadro di spostamento, fare clic su **configurazione Computer\\i criteri\\le impostazioni di Windows\\le impostazioni di sicurezza\\criteri chiave pubblica**.

    2.  Nel riquadro dei dettagli, fare doppio clic su **Client Servizi certificati-registrazione automatica**, fare clic su **proprietà**.

    3.  Nel Client di servizi certificati-registrazione automatica delle proprietà nella finestra di dialogo **modello di configurazione**, fare clic su **abilitato**.

    4.  Selezionare **Rinnova i certificati scaduti, aggiorna quelli in sospeso, e rimuovere certificati revocati** e **Aggiorna i certificati che utilizzano modelli di certificato**.

    5.  Fare clic su **OK**.

6.  In Editor Gestione criteri di gruppo, completare i passaggi seguenti per configurare registrazione automatica dei certificati di utente:

    1.  Nel riquadro di spostamento, fare clic su **User Configuration\\i criteri\\le impostazioni di Windows\\le impostazioni di sicurezza\\criteri chiave pubblica**.

    2.  Nel riquadro dei dettagli, fare doppio clic su **Client Servizi certificati-registrazione automatica**, fare clic su **proprietà**.

    3.  Nel Client di servizi certificati-registrazione automatica delle proprietà nella finestra di dialogo **modello di configurazione**, fare clic su **abilitato**.

    4.  Selezionare **Rinnova i certificati scaduti, aggiorna quelli in sospeso, e rimuovere certificati revocati** e **Aggiorna i certificati che utilizzano modelli di certificato**.

    5.  Fare clic su **OK**.

    6.  Chiudi l'Editor Gestione Criteri di gruppo.

7.  Chiudere Gestione Criteri di gruppo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configurazione della CA per i computer unita in join non di dominio

Poiché il server RRAS non è aggiunto a un dominio, la registrazione automatica non è utilizzabile per registrare il certificato del gateway VPN.  Pertanto, utilizzare una procedura di richiesta certificato non in linea. 

1. Nel server RRAS, generare un file denominato **VPNGateway.inf** basato su richiesta dei criteri di certificato riportato di seguito viene fornita nella appendice (sezione 0) e personalizzare le voci seguenti: 

   - Nella sezione [NewRequest], vpn.contoso.com utilizzato per il nome di oggetto con la scelta di sostituire [_cliente_] endpoint VPN FQDN.

   - Nella sezione [estensioni], vpn.contoso.com usato per il nome alternativo del soggetto con la scelta di sostituire [_cliente_] endpoint VPN FQDN.

2. Salvare o copiare il **VPNGateway.inf** file da una posizione prescelta.

3. Da un prompt dei comandi con privilegi elevati, passare alla cartella che contiene il **VPNGateway.inf** file e digitare:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copiare l'oggetto appena creato **VPNGateway.req** file di output da un'autorità di certificazione server o Workstation di accesso con privilegi (PAW). 

5. Salvare o copiare il **VPNGateway.req** file in un percorso scelto nel server di autorità di certificazione o Workstation di accesso con privilegi (PAW).

6. Da un prompt dei comandi con privilegi elevati, passare alla cartella che contiene il file VPNGateway.req creato nel passaggio precedente e digitare: 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Se richiesto dalla finestra elenco Autorità di certificazione, selezionare la CA dell'organizzazione appropriato per soddisfare la richiesta di certificato.

8. Copiare l'oggetto appena creato **VPNGateway.cer** file di output per il server RRAS. 

9. Salvare o copiare il **VPNGateway.cer** file in un percorso scelto per il server RRAS.

10. Da un prompt dei comandi con privilegi elevati, passare alla cartella che contiene il file VPNGateway.cer creato nel passaggio precedente e digitare:
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Eseguire lo snap-in MMC certificati come descritto [in questo caso](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selezionando le **account del Computer** opzione.

12. Verificare l'esistenza di un certificato valido per il server RRAS con le proprietà seguenti:

   - **Scopi designati:** Autenticazione server, Mediatore IKE sicurezza IP 

   - **Modello di certificato:** [_cliente_] Server VPN

#### <a name="example-vpngatewayinf-script"></a>Esempio: VPNGateway.inf script

Qui è possibile visualizzare un esempio di script di un criterio di richiesta di certificato usato per richiedere un certificato di gateway VPN tramite un processo fuori banda.

>[!TIP]
>È possibile trovare una copia dello script VPNGateway.inf del Kit IP offerta VPN nella cartella Criteri di richiesta di certificato. Aggiornare solo il 'oggetto' e '\_continuare\_' con valori specifici del cliente.

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

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Creare gli utenti VPN, server VPN e i gruppi di server dei criteri di rete

In questa procedura, è possibile aggiungere un nuovo gruppo di Active Directory (AD) che contiene gli utenti possono usare la VPN per connettersi alla rete dell'organizzazione.

Questo gruppo ha due scopi:

-   Definisce quali utenti sono autorizzati alla registrazione automatica per i certificati utente che richiede la connessione VPN.

-   Definisce gli utenti che autorizza i criteri di rete per l'accesso VPN.

Usando un gruppo personalizzato, se si desidera revocare l'accesso VPN dell'utente, è possibile rimuovere tale utente dal gruppo.

È anche possibile aggiungere un gruppo che contiene i server VPN e un altro gruppo che contiene i server NPS. Usare questi gruppi per limitare le richieste di certificati per i relativi membri.

>[!NOTE] 
>È consigliabile VPN i server che si trovano in DMA/perimetro non essere aggiunti al dominio. Tuttavia, se si preferisce che il server VPN di dominio per una migliore gestibilità (criteri di gruppo, dell'agente di Backup/monitoraggio, nessun utente locale per gestire e così via), quindi aggiungere un gruppo di Active Directory per il modello di certificato del server VPN.

### <a name="configure-the-vpn-users-group"></a>Configurare il gruppo di utenti VPN

1.  In un controller di dominio aprire Active Directory Users and Computers.

2.  Fare clic su un contenitore o unità organizzativa, fare clic su **New** e fare clic su **gruppo**.

3.  In **nome gruppo**, digitare **gli utenti VPN**, fare clic su **OK**.

4.  Fare doppio clic su **gli utenti VPN**, fare clic su **proprietà**.

5.  Nel **membri** della scheda della finestra di dialogo Proprietà gli utenti VPN, fare clic su **Add**.

6.  Nella finestra di dialogo Seleziona utenti, aggiungere tutti gli utenti che necessitano di accesso VPN, quindi scegliere **OK**.

7.  Chiudere Utenti e computer di Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurare i gruppi di server dei criteri di rete e server VPN

1.  In un controller di dominio aprire Active Directory Users and Computers.

2.  Fare clic su un contenitore o unità organizzativa, fare clic su **New** e fare clic su **gruppo**.

3.  In **nome gruppo**, digitare **server VPN**, fare clic su **OK**.

4.  Fare doppio clic su **server VPN**, fare clic su **proprietà**.

5.  Nel **membri** della scheda della finestra di dialogo Proprietà server VPN, fare clic su **Add**.

6.  Fare clic su **tipi di oggetti**, selezionare la **computer** casella di controllo e fare clic su **OK**.

7.  Nelle **immettere i nomi degli oggetti da selezionare**, digitare i nomi dei server VPN e fare clic su **OK**.

8.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà server VPN.

9.  Ripetere i passaggi precedenti per il gruppo di server dei criteri di rete.

10. Chiudere Utenti e computer di Active Directory.

## <a name="create-the-user-authentication-template"></a>Creare il modello di autenticazione utente

In questa procedura si configura un modello di autenticazione client-server personalizzato. Questo modello è necessario perché si vuole migliorare la sicurezza complessiva del certificato, selezionare i livelli di compatibilità aggiornato e scegliendo Microsoft piattaforma Crypto Provider. Questa ultima modifica consente di utilizzare il TPM dei computer client per proteggere il certificato. Per una panoramica del TPM, vedere [Trusted Platform Module tecnologia Overview](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedura:**

1.  Nella CA inizialeaprire autorità di certificazione.

2.  Nel riquadro di spostamento, fare doppio clic su **modelli di certificato**, fare clic su **Gestisci**.

3.  Nella console di modelli di certificato, fare doppio clic su **utente**, fare clic su **Duplica modello**.

    >[!WARNING]
    >Non fare clic **Apply** oppure **OK** in qualsiasi momento prima del passaggio 10.  Se si fa clic su questi pulsanti prima di entrare in tutti i parametri, scegliere tra molte opzioni diventano fisso e non sarà più modificabile. Ad esempio, nella **Cryptography** scheda, se _Cryptographic Provider di archiviazione Legacy_ Mostra nel campo categoria Provider, viene disabilitata, impedendo eventuali ulteriori modifiche. L'unica alternativa consiste nell'eliminare il modello e ricrearlo.  

4.  Nella finestra di dialogo Proprietà nuovo modello sul **generali** scheda, completare i passaggi seguenti:

    1.  Nelle **nome visualizzato modello**, digitare **l'autenticazione utente VPN**.

    2.  Cancella il **pubblica certificato in Active Directory** casella di controllo.

5.  Nel **sicurezza** scheda, completare i passaggi seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  In Seleziona utenti, computer, account del servizio o nella finestra di dialogo, digitare **gli utenti VPN**, fare clic su **OK**.

    3.  Nelle **nomi utente o gruppo**, fare clic su **gli utenti VPN**.

    4.  In **le autorizzazioni per gli utenti VPN**, selezionare la **Enroll** e **registrazione automatica** caselle di controllo la **Consenti** colonna.

       >[!TIP]
       >Assicurarsi di mantenere selezionata la casella di controllo in lettura. In altre parole, sono necessarie le autorizzazioni di lettura per la registrazione. 

    5.  Nelle **nomi utente o gruppo**, fare clic su **Domain Users**, fare clic su **rimuovere**.

6.  Nel **compatibilità** scheda, completare i passaggi seguenti:

    1.  Nelle **autorità di certificazione**, fare clic su **Windows Server 2012 R2**.

    2.  Nel **modifiche risultanti** finestra di dialogo, fare clic su **OK**.

    3.  Nelle **destinatario certificato**, fare clic su **Windows 8.1 e Windows Server 2012 R2**.

    4.  Nel **modifiche risultanti** finestra di dialogo, fare clic su **OK**.

7.  Nel **Gestione richiesta** scheda, deseleziona le **Rendi la chiave privata esportabile** casella di controllo.

8.  Nel **Cryptography** scheda, completare i passaggi seguenti:

    1.  Nelle **categoria Provider**, fare clic su **Key Storage Provider**.

    2.  Fare clic su **le richieste devono utilizzare uno dei seguenti provider**.

    3.  Selezionare il **Microsoft piattaforma Crypto Provider** casella di controllo.

9.  Nel **nome soggetto** scheda, se non hai un indirizzo di posta elettronica elencato in tutti gli account utente, deselezionare le **Includi nome posta elettronica nel nome soggetto** e **indirizzo di posta elettronica** caselle di controllo.

10. Fare clic su **OK** per salvare il modello di certificato di autenticazione dell'utente di VPN.

11. Chiudere la console modelli di certificato.

12. Nel riquadro di spostamento dello snap-in Autorità di certificazione, fare doppio clic su **modelli di certificato**, fare clic su **New** e quindi fare clic su **modello di certificato da emettere**.

13. Fare clic su **l'autenticazione utente VPN**, fare clic su **OK**.

14. Chiudere lo snap-in Autorità di certificazione.

## <a name="create-the-vpn-server-authentication-template"></a>Creare il modello di autenticazione Server VPN

In questa procedura, è possibile configurare un nuovo modello di autenticazione Server per il server VPN. Aggiunta di che criteri dell'applicazione Intermediate IKE sicurezza IP (IPsec) consentono al server di filtro certificati se più di un certificato è disponibile con l'autenticazione Server Utilizzo chiavi avanzato.

>[!IMPORTANT] 
>Poiché i client VPN di accedono a questo server dalla rete Internet pubblica, il soggetto e nomi alternativi sono diversi dal nome interno del server. Di conseguenza, non è possibile registrare automaticamente questo certificato nel server VPN.

**Prerequisiti:**<p>
Server VPN di dominio

**Procedura:** 

1.  Nella CA inizialeaprire autorità di certificazione.

2.  Nel riquadro di spostamento, fare doppio clic su **modelli di certificato**, fare clic su **Gestisci**.

3.  Nella console di modelli di certificato, fare doppio clic su **Server RAS e IAS**, fare clic su **Duplica modello**.

4.  Nella finestra di dialogo Proprietà nuovo modello sul **generali** nella scheda **nome visualizzato modello**, immettere un nome descrittivo per il server VPN, ad esempio, **Server VPN L'autenticazione** oppure **Server RADIUS**.

5.  Nel **Extensions** scheda, completare i passaggi seguenti:

    1.  Fare clic su **i criteri di applicazione**, fare clic su **modificare**.

    2.  Nel **Modifica estensione criteri di applicazione** finestra di dialogo, fare clic su **Add**.

    3.  Nel **Aggiunta criterio di applicazione** finestra di dialogo, fare clic su **Mediatore IKE sicurezza IP**, fare clic su **OK**.<p>Aggiunta dell'indirizzo IP all'EKU Mediatore IKE sicurezza aiuta negli scenari in cui è presente più di un certificato di autenticazione server nel server VPN. Quando Mediatore IKE sicurezza IP è presente, IPSec utilizza solo il certificato con le opzioni di EKU. In caso contrario, l'autenticazione IKEv2 potrebbe non riuscire con l'errore 13801: Le credenziali di autenticazione IKE sono inaccettabili.

    4.  Fare clic su **OK** da restituire per il **proprietà nuovo modello** nella finestra di dialogo.

6.  Nel **sicurezza** scheda, completare i passaggi seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  Nel **Seleziona utenti, computer, account del servizio o gruppi** finestra di dialogo, digitare **server VPN**, fare clic su **OK**.

    3.  Nelle **nomi utente o gruppo**, fare clic su **i server VPN**.

    4.  In **le autorizzazioni per i server VPN**, selezionare la **Enroll** casella di controllo la **Consenti** colonna.

    5.  Nelle **nomi utente o gruppo**, fare clic su **server RAS e IAS**, fare clic su **Rimuovi**.

7.  Nel **nome del soggetto** scheda, completare i passaggi seguenti:

    1.  Fare clic su **Inserisci nella richiesta**.

    2.  Nel **modelli di certificato** della finestra di dialogo di avviso fare clic su **OK**.

8.  (Facoltativo) *Se si configura l'accesso condizionale per la connettività VPN*, fare clic sui **Gestione richiesta** scheda, quindi scegliere **Rendi la chiave privata esportabile** per selezionarlo.

9.  Fare clic su **OK** per salvare il modello di certificato Server VPN.

10. Chiudere la console modelli di certificato.

11. Nel riquadro di spostamento dello snap-in Autorità di certificazione, fare doppio clic su **modelli di certificato**, fare clic su **New** e quindi fare clic su **modello di certificato da emettere**.

12. Riavviare i servizi autorità di certificazione.

13. Selezionare il nome scelto nel passaggio 4 sopra descritto e fare clic su **OK**.

14. Chiudere lo snap-in Autorità di certificazione.

## <a name="create-the-nps-server-authentication-template"></a>Creare il modello di autenticazione del Server dei criteri di rete

Modello di certificato terzo e ultimo da creare è il modello di autenticazione del Server dei criteri di rete. Il modello di autenticazione del Server dei criteri di rete è una semplice copia del modello di Server RAS e IAS protetto al gruppo di Server dei criteri di rete creato in precedenza in questa sezione. 

Si configurerà questo certificato per la registrazione automatica.

**Procedura:**

1.  Nella CA inizialeaprire autorità di certificazione.

2.  Nel riquadro di spostamento, fare doppio clic su **modelli di certificato**, fare clic su **Gestisci**.

3.  Nella console di modelli di certificato, fare doppio clic su **Server RAS e IAS**e selezionare **Duplica modello**.

4.  Nella finestra di dialogo Proprietà nuovo modello nel **generali** nella scheda **nome visualizzato del modello**, digitare **l'autenticazione del Server dei criteri di rete**.

5.  Nel **sicurezza** scheda, completare i passaggi seguenti:

    1.  Fai clic su **Aggiungi**.

    2.  Nel **Seleziona utenti, computer, account del servizio o gruppi** finestra di dialogo, digitare **i server NPS**e fare clic su **OK**.

    3.  Nelle **nomi utente o gruppo**, fare clic su **i server NPS**.

    4.  In **le autorizzazioni per i server NPS**, selezionare il **Enroll** e **registrazione automatica** caselle di controllo il **Consenti** colonna.

    5.  Nelle **nomi utente o gruppo**, fare clic su **server RAS e IAS**, fare clic su **Rimuovi**.

6.  Fare clic su **OK** per salvare il modello di certificato Server NPS.

7.  Chiudere la console modelli di certificato.

8.  Nel riquadro di spostamento dello snap-in Autorità di certificazione, fare doppio clic su **modelli di certificato**, fare clic su **New** e quindi fare clic su **modello di certificato da emettere**.

9.  Fare clic su **autenticazione Server NPS**, fare clic su **OK**.

10. Chiudere lo snap-in Autorità di certificazione.

## <a name="enroll-and-validate-the-user-certificate"></a>Registrare e convalidare il certificato utente

Perché si sta usando criteri di gruppo per registrare automaticamente i certificati utente, è necessario aggiornare solo i criteri e Windows 10 verrà automaticamente registrato per l'account utente per il certificato corretto. È quindi possibile convalidare il certificato nella console di certificati.

**Procedura:**

1.  Accedi a un computer client aggiunti al dominio come membro del **gli utenti VPN** gruppo.

2.  Premere il tasto Windows + R, digitare **gpupdate /force**, quindi premere INVIO.

3.  Nel menu Start, digitare **Certmgr. msc**, quindi premere INVIO.

4.  Nello snap-in certificati, sotto **personali**, fare clic su **certificati**. I certificati vengono visualizzati nel riquadro dei dettagli.

5.  Fare doppio clic il certificato con il nome utente di dominio corrente e quindi fare clic su **aperto**.

6.  Nel **generali** scheda, verificare che la data è elencato in **valido da** corrisponde alla data corrente. In caso contrario, si sia stato scelto il certificato non corretto.

7.  Fare clic su **OK**e chiudere lo snap-in certificati.

## <a name="enroll-and-validate-the-server-certificates"></a>Registrare e convalidare i certificati del server

A differenza del certificato utente, è necessario registrare manualmente il certificato del server VPN. Dopo che è stato registrato, convalidarla utilizzando lo stesso processo usato per il certificato utente. Ad esempio il certificato utente, il server NPS registra automaticamente il certificato di autenticazione, in modo che tutto è necessario eseguire è convalidarlo.

>[!NOTE] 
>Si potrebbe essere necessario riavviare il server VPN e NPS per consentire loro di aggiornare l'appartenenza al gruppo prima che si possono completare questi passaggi.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Registrare e convalidare il certificato del server VPN

1.  Nel menu di avvio del server VPN, digitare **certlm. msc**, quindi premere INVIO.

2.  Fare doppio clic su **personali**, fare clic su **tutte le attività** e quindi fare clic su **Richiedi nuovo certificato** per avviare la procedura guidata registrazione certificato.

3.  Nella pagina prima di iniziare, fare clic su **successivo**.

4.  Nella pagina Seleziona criteri di registrazione certificato, fare clic su **successivo**.

5.  Nella pagina richiesta certificati, scegliere la casella di controllo accanto al server VPN per selezionarlo.

6.  Sotto la casella di controllo del server VPN, fare clic su **sono necessarie ulteriori informazioni** per aprire la finestra di dialogo proprietà del certificato e completare i passaggi seguenti:

    1.  Fare clic sul **Subject** scheda, fare clic su **nome comune** sotto **nome soggetto**nella **tipo**.

    2.  Sotto **nome soggetto**, nel **valore**, immettere il nome del client del dominio esterno usato per connettersi alla VPN, ad esempio, vpn.contoso.com e fare clic su **Add**.

    3.  Sotto **relativa al nome alternativo**, nel **tipo**, fare clic su **DNS**.

    4.  Sotto **relativa al nome alternativo**, nel **valore**, immettere tutti i nomi di server, i client usano per connettersi alla VPN, ad esempio, vpn.contoso.com, vpn, 132.64.86.2.

    5.  Fare clic su **Add** dopo l'immissione di ogni nome.

    6.  Al termine, fai clic su **OK**.

7.  Fai clic su **Registra**.

8.  Scegliere **Fine**.

9.  Nello snap-in certificati, sotto **personali**, fare clic su **certificati**.<p>I certificati elencati vengono visualizzati nel riquadro dei dettagli.

10. Fare doppio clic il certificato del server VPN assegnare un nome e quindi fare clic su **aperto**.

11. Nel **generali** scheda, verificare che la data è elencato in **valido da** corrisponde alla data corrente. In caso contrario, si sia stato scelto il certificato non corretto.

12. Nel **informazioni dettagliate** scheda, fare clic su **Enhanced Key Usage**e verificare che **Mediatore IKE sicurezza IP** e **autenticazione Server** visualizzare nell'elenco.

13. Fare clic su **OK** per chiudere il certificato.

14. Chiudere lo snap-in certificati.

### <a name="validate-the-nps-server-certificate"></a>Convalidare il certificato del server dei criteri di rete

1.  Riavviare il server dei criteri di rete.

2.  Nel menu di avvio del server dei criteri di rete, digitare **certlm. msc**, quindi premere INVIO.

3.  Nello snap-in certificati, sotto **personali**, fare clic su **certificati**.<p>I certificati elencati vengono visualizzati nel riquadro dei dettagli.

4.  Fare clic sul certificato che dispone del server dei criteri di rete di assegnare un nome e quindi fare clic su **aperto**.

5.  Nel **generali** scheda, verificare che la data è elencato in **valido da** corrisponde alla data corrente. In caso contrario, si sia stato scelto il certificato non corretto.

6.  Fare clic su **OK** per chiudere il certificato.

7.  Chiudere lo snap-in certificati.

## <a name="next-step"></a>Passaggio successivo
[Passaggio 3. Configurare il Server di accesso remoto per AlwaysOn VPN](vpn-deploy-ras.md): In questo passaggio si configura accesso remoto VPN per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione client VPN autorizzati.







---
