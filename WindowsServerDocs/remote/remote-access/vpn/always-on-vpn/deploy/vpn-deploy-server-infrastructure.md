---
title: Configurare l'infrastruttura server
description: In questo passaggio si installano e configurano i componenti lato server necessari per supportare la VPN. I componenti lato server includono la configurazione dell'infrastruttura a chiave pubblica per distribuire i certificati utilizzati dagli utenti, dal server VPN e dal server dei criteri di rete.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 260d5c6273d877386dc1cd8833b2f226533127c3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871300"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Passaggio 2. Configurare l'infrastruttura server

>Si applica a Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente** Passaggio 1. Pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)
- [**Prossimo** Passaggio 3. Configurare il server di accesso remoto per VPN Always On](vpn-deploy-ras.md)

In questo passaggio verranno installati e configurati i componenti lato server necessari per supportare la VPN. I componenti lato server includono la configurazione dell'infrastruttura a chiave pubblica per distribuire i certificati utilizzati dagli utenti, dal server VPN e dal server dei criteri di rete.  È inoltre possibile configurare RRAS per supportare le connessioni IKEv2 e il server NPS per eseguire l'autorizzazione per le connessioni VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurare la registrazione automatica del certificato in Criteri di gruppo
In questa procedura vengono configurati Criteri di gruppo sul controller di dominio in modo che i membri del dominio richiedano automaticamente i certificati utente e computer. Questa operazione consente agli utenti VPN di richiedere e recuperare automaticamente i certificati utente che autenticano le connessioni VPN. Analogamente, questo criterio consente ai server NPS di richiedere automaticamente i certificati di autenticazione server. 

Registrare manualmente i certificati nei server VPN.

>[!TIP]
>Per i computer non appartenenti a un dominio, vedere [configurazione della CA per i computer non appartenenti](#ca-configuration-for-non-domain-joined-computers)a un dominio. Poiché il server RRAS non è aggiunto a un dominio, non è possibile usare la registrazione automatica per registrare il certificato del gateway VPN.  Utilizzare pertanto una procedura di richiesta di certificato offline.

1. In un controller di dominio aprire Gestione Criteri di gruppo.

2. Nel riquadro di spostamento fare clic con il pulsante destro del mouse sul dominio (ad esempio, corp.contoso.com), quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.

3. Nella finestra di dialogo nuovo oggetto Criteri di gruppo immettere i **criteri di registrazione automatica**, quindi selezionare **OK**.

4. Nel riquadro di spostamento fare clic con il pulsante destro del mouse su **criterio di registrazione automatica**, quindi scegliere **modifica**.

5. Nella Editor Gestione Criteri di gruppo completare la procedura seguente per configurare la registrazione automatica del certificato del computer:

    1. Nel riquadro di spostamento passare a **configurazione** > computer >  > **criteri impostazioni di Windows**impostazioni di**sicurezza**Criterichiave > pubblica.

    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **client Servizi certificati-registrazione automatica**, quindi selezionare **Proprietà**.

    3. Nella finestra di dialogo client Servizi certificati-proprietà registrazione automatica, in **modello di configurazione**, selezionare **abilitato**.

    4. Selezionare **Rinnova i certificati scaduti, aggiorna quelli in sospeso e rimuovi i certificati revocati** e **Aggiorna i certificati che utilizzano modelli di certificato**.

    5. Scegliere **OK**.

6. Nella Editor Gestione Criteri di gruppo completare la procedura seguente per configurare la registrazione automatica dei certificati utente:

    1. Nel riquadro di spostamento passare a **configurazione** > utente >  > **criteri impostazioni di Windows**impostazioni di**sicurezza**Criterichiave > pubblica.

    2. Nel riquadro dei dettagli fai clic con il pulsante destro del mouse su **Client Servizi certificati - registrazione automatica** e seleziona **Proprietà**.

    3. Nella finestra di dialogo client Servizi certificati-proprietà registrazione automatica, in **modello di configurazione**, selezionare **abilitato**.

    4. Selezionare **Rinnova i certificati scaduti, aggiorna quelli in sospeso e rimuovi i certificati revocati** e **Aggiorna i certificati che utilizzano modelli di certificato**.

    5. Scegliere **OK**.

    6. Chiudi l'Editor Gestione Criteri di gruppo.

7. Chiudere Gestione Criteri di gruppo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configurazione CA per computer non appartenenti a un dominio

Poiché il server RRAS non è aggiunto a un dominio, non è possibile usare la registrazione automatica per registrare il certificato del gateway VPN.  Utilizzare pertanto una procedura di richiesta di certificato offline.

1. Nel server RRAS generare un file denominato **VPNGateway. inf** in base alla richiesta di esempio di criteri di certificato fornita nell'appendice a (sezione 0) e personalizzare le voci seguenti:

   - Nella sezione [NewRequest] sostituire vpn.contoso.com usato per il nome del soggetto con il nome di dominio completo dell'endpoint VPN [_Customer_] scelto.

   - Nella sezione [Extensions] sostituire vpn.contoso.com usato per il nome soggetto alternativo con il nome di dominio completo dell'endpoint VPN [_Customer_] scelto.

2. Salva o copia il file **VPNGateway. inf** in un percorso selezionato.

3. Da un prompt dei comandi con privilegi elevati passare alla cartella che contiene il file **VPNGateway. inf** e digitare:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copiare il file di output **VPNGateway. req** appena creato in un server dell'autorità di certificazione o con Privileged Access workstation (Paw).

5. Salvare o copiare il file **VPNGateway. req** in un percorso scelto nel server Autorità di certificazione o nella workstation con privilegi di accesso con privilegi (Paw).

6. Da un prompt dei comandi con privilegi elevati passare alla cartella che contiene il file VPNGateway. req creato nel passaggio precedente e digitare:

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Se richiesto dalla finestra elenco autorità di certificazione, selezionare la CA globale (Enterprise) per il servizio della richiesta di certificato.

8. Copiare il file di output **VPNGateway. cer** appena creato nel server RRAS. 

9. Salvare o copiare il file **VPNGateway. cer** in un percorso scelto nel server RRAS.

10. Da un prompt dei comandi con privilegi elevati passare alla cartella che contiene il file VPNGateway. cer creato nel passaggio precedente e digitare:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Eseguire lo snap-in MMC certificati come descritto [qui](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selezionando l'opzione **account computer** .

12. Verificare che esista un certificato valido per il server RRAS con le proprietà seguenti:

    - **Scopi desiderati:** Autenticazione server, intermedio IKE sicurezza IP 

    - **Modello di certificato:** [_Cliente_] Server VPN

#### <a name="example-vpngatewayinf-script"></a>Esempio: Script VPNGateway. inf

Qui è possibile visualizzare uno script di esempio di un criterio di richiesta di certificato usato per richiedere un certificato del gateway VPN usando un processo fuori banda.

>[!TIP]
>È possibile trovare una copia dello script VPNGateway. inf nel kit IP offerta VPN nella cartella criteri di richiesta certificato. Aggiornare solo ' subject ' è\_continue\_' con valori specifici del cliente.

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

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Creare i gruppi di utenti VPN, server VPN e server dei criteri di rete

In questa procedura è possibile aggiungere un nuovo gruppo di Active Directory (AD) contenente gli utenti autorizzati a usare la VPN per connettersi alla rete dell'organizzazione.

Questo gruppo svolge due finalità:

- Definisce gli utenti autorizzati a eseguire la registrazione automatica per i certificati utente necessari per la VPN.

- Definisce gli utenti che il server dei criteri di rete autorizza ad accedere alla VPN.

Se si desidera revocare l'accesso VPN di un utente tramite un gruppo personalizzato, è possibile rimuovere tale utente dal gruppo.

È inoltre possibile aggiungere un gruppo contenente i server VPN e un altro gruppo contenente server dei criteri di rete. Questi gruppi vengono usati per limitare le richieste di certificati ai relativi membri.

>[!NOTE]
>È consigliabile che i server VPN che risiedono nel DMA/perimetro non siano aggiunti a un dominio. Tuttavia, se si preferisce che i server VPN siano aggiunti a un dominio per una migliore gestibilità (criteri di gruppo, agente di backup/monitoraggio, nessun utente locale da gestire e così via), aggiungere un gruppo di Active Directory al modello di certificato del server VPN.

### <a name="configure-the-vpn-users-group"></a>Configurare il gruppo utenti VPN

1. In un controller di dominio aprire Active Directory utenti e computer.

2. Fare clic con il pulsante destro del mouse su un contenitore o un'unità organizzativa, scegliere **nuovo**, quindi selezionare **gruppo**.

3. In **nome gruppo**immettere **utenti VPN**e quindi fare clic su **OK**.

4. Fare clic con il pulsante destro del mouse su **utenti VPN** e scegliere **Proprietà**.

5. Nella scheda **membri** della finestra di dialogo Proprietà utenti VPN selezionare **Aggiungi**.

6. Nella finestra di dialogo Seleziona utenti aggiungere tutti gli utenti che necessitano dell'accesso VPN e selezionare **OK**.

7. Chiudere Utenti e computer di Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurare i server VPN e i gruppi di server dei criteri di rete

1. In un controller di dominio aprire Active Directory utenti e computer.

2. Fare clic con il pulsante destro del mouse su un contenitore o un'unità organizzativa, scegliere **nuovo**, quindi selezionare **gruppo**.

3. In **nome gruppo**immettere **server VPN**e quindi fare clic su **OK**.

4. Fare clic con il pulsante destro del mouse su **server VPN** e scegliere **Proprietà**.

5. Nella scheda **membri** della finestra di dialogo Proprietà server VPN selezionare **Aggiungi**.

6. Selezionare **tipi di oggetto**, selezionare la casella di controllo **computer** , quindi fare clic su **OK**.

7. In **immettere i nomi degli oggetti da selezionare**immettere i nomi dei server VPN e quindi fare clic su **OK**.

8. Selezionare **OK** per chiudere la finestra di dialogo Proprietà server VPN.

9. Ripetere i passaggi precedenti per il gruppo server NPS.

10. Chiudere Utenti e computer di Active Directory.

## <a name="create-the-user-authentication-template"></a>Creare il modello di autenticazione utente

In questa procedura viene configurato un modello di autenticazione client-server personalizzato. Questo modello è necessario perché si desidera migliorare la sicurezza complessiva del certificato selezionando livelli di compatibilità aggiornati e scegliendo il provider di crittografia della piattaforma Microsoft. Questa Ultima modifica consente di utilizzare il TPM nei computer client per proteggere il certificato. Per una panoramica del TPM, vedere [Panoramica della tecnologia Trusted Platform Module](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

>[!IMPORTANT] 
>Il provider di crittografia della piattaforma Microsoft "richiede un chip TPM, nel caso in cui si esegua una macchina virtuale e si ottenga l'errore seguente: "Impossibile trovare un CSP valido nel computer locale" quando si tenta di registrare manualmente il certificato necessario per verificare il "provider di archiviazione chiavi del software Microsoft" e impostarlo secondo nell'ordine dopo "provider crittografia piattaforma Microsoft" nella scheda crittografia del certificato. Proprietà.

**Procedura**

1. Nella CA aprire autorità di certificazione.

2. Nel riquadro di spostamento fare clic con il pulsante destro del mouse su **modelli di certificato** e scegliere **Gestisci**.

3. Nella console modelli di certificato, fare clic con il pulsante destro del mouse su **utente** e scegliere **Duplica modello**.

   >[!WARNING]
   >Non selezionare **applica** o **OK** in qualsiasi momento prima del passaggio 10.  Se si selezionano questi pulsanti prima di immettere tutti i parametri, molte scelte diventano fisse e non sono più modificabili. Ad esempio, nella scheda **crittografia** , se il _provider di archiviazione di crittografia legacy_ viene visualizzato nel campo categoria provider, diventa disabilitato, impedendo eventuali ulteriori modifiche. L'unica alternativa consiste nell'eliminare il modello e ricrearlo.  

4. Nella finestra di dialogo Proprietà nuovo modello della scheda **generale** completare i passaggi seguenti:

   1. In **nome visualizzato modello**digitare **autenticazione utente VPN**.

   2. Deselezionare la casella **di controllo pubblica certificato in Active Directory** .

5. Nella scheda **sicurezza** completare i passaggi seguenti:

   1. Selezionare **Aggiungi**.

   2. Nella finestra di dialogo Seleziona utenti, computer, account di servizio o gruppi immettere **utenti VPN**, quindi fare clic su **OK**.

   3. In **nome gruppo o utente**selezionare **utenti VPN**.

   4. In **autorizzazioni per utenti VPN**selezionare le caselle di controllo **registrazione** e **registrazione** automatica nella colonna **Consenti** .

      >[!TIP]
      >Assicurarsi di lasciare selezionata la casella di controllo Leggi. In altre parole, sono necessarie le autorizzazioni di lettura per la registrazione. 

   5. In **nomi**utenti e gruppi selezionare **Domain Users**, quindi selezionare **Rimuovi**.

6. Nella scheda **compatibilità** completare i passaggi seguenti:

   1. In **autorità di certificazione**selezionare **Windows Server 2012 R2**.

   2. Nella finestra di dialogo **modifiche risultanti** selezionare **OK**.

   3. In **destinatario certificato**selezionare **Windows 8.1/Windows Server 2012 R2**.

   4. Nella finestra di dialogo **modifiche risultanti** selezionare **OK**.

7. Nella scheda **Gestione richiesta** deselezionare la casella **di controllo Consenti esportazione della chiave privata** .

8. Nella scheda **crittografia** completare i passaggi seguenti:

   1. In **categoria provider**selezionare **provider di archiviazione chiavi**.

   2. Le **richieste SELECT devono usare uno dei seguenti provider**.

   3. Selezionare la casella di controllo **provider di crittografia piattaforma Microsoft** .

9. Nella scheda **nome soggetto** , se non si dispone di un indirizzo di posta elettronica elencato per tutti gli account utente, deselezionare le caselle **di controllo Includi nome posta elettronica in nome soggetto** e **nome posta elettronica** .

10. Selezionare **OK** per salvare il modello di certificato di autenticazione utente VPN.

11. Chiudere la console Modelli di certificato.

12. Nel riquadro di spostamento dello snap-in autorità di certificazione, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo** e quindi **modello di certificato da emettere**.

13. Selezionare **autenticazione utente VPN**e quindi fare clic su **OK**.

14. Chiudere lo snap-in autorità di certificazione.

## <a name="create-the-vpn-server-authentication-template"></a>Creare il modello di autenticazione server VPN

In questa procedura è possibile configurare un nuovo modello di autenticazione server per il server VPN. L'aggiunta del criterio di applicazione intermedio IKE IPsec (IP Security) consente al server di filtrare i certificati se è disponibile più di un certificato con l'utilizzo chiavi avanzato di autenticazione server.

>[!IMPORTANT]
>Poiché i client VPN accedono a questo server dalla rete Internet pubblica, l'oggetto e i nomi alternativi sono diversi dal nome del server interno. Di conseguenza, non è possibile registrare automaticamente il certificato nei server VPN.

**Prerequisiti**

Server VPN aggiunti a un dominio

**Procedura**

1. Nella CA aprire autorità di certificazione.

2. Nel riquadro di spostamento fare clic con il pulsante destro del mouse su **modelli di certificato** e scegliere **Gestisci**.

3. Nella console modelli di certificato, fare clic con il pulsante destro del mouse su **server RAS e IAS** e scegliere **Duplica modello**.

4. Nella finestra di dialogo Proprietà nuovo modello della scheda **generale** , in **nome visualizzato modello**, immettere un nome descrittivo per il server VPN, ad esempio **autenticazione server VPN** o **server RADIUS**.

5. Nella scheda **estensioni** completare i passaggi seguenti:

    1. Selezionare **criteri di applicazione**, quindi selezionare **modifica**.

    2. Nella finestra di dialogo **modifica estensione criteri di applicazione** selezionare **Aggiungi**.

    3. Nella finestra di dialogo **Aggiungi criteri di applicazione** selezionare **intermediario di sicurezza IP IKE**, quindi selezionare **OK**.
   
        L'aggiunta di un livello intermedio IKE di protezione IP all'EKU è utile negli scenari in cui esistono più certificati di autenticazione server nel server VPN. Quando è presente la protezione IP IKE intermedio, IPSec utilizza solo il certificato con entrambe le opzioni EKU. Senza questo, l'autenticazione IKEv2 potrebbe non riuscire con l'errore 13801: Le credenziali di autenticazione IKE sono inaccettabili.

    4. Selezionare **OK** per tornare alla finestra di dialogo **Proprietà nuovo modello** .

6. Nella scheda **sicurezza** completare i passaggi seguenti:

    1. Selezionare **Aggiungi**.

    2. Nella finestra di dialogo **Seleziona utenti, computer, account servizio o gruppi** immettere **server VPN**, quindi fare clic su **OK**.

    3. In **nomi utenti e gruppi**selezionare **server VPN**.

    4. In **autorizzazioni per server VPN**selezionare la casella di controllo **registra** nella colonna **Consenti** .

    5. In **nomi utenti e gruppi**selezionare **server RAS e IAS**, quindi selezionare **Rimuovi**.

7. Nella scheda **nome soggetto** completare i passaggi seguenti:

    1. Selezionare **specificare nella richiesta**.

    2. Nella finestra di dialogo avviso **modelli di certificato** fare clic su **OK**.

8. Opzionale Se si configura l'accesso condizionale per la connettività VPN, selezionare la scheda **Gestione richieste** , quindi selezionare **Consenti esportazione della chiave privata**.

9. Selezionare **OK** per salvare il modello di certificato del server VPN.

10. Chiudere la console Modelli di certificato.

11. Nel riquadro di spostamento dello snap-in autorità di certificazione, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo** , quindi fare clic su **modello di certificato da emettere**.

12. Riavviare i servizi di autorità di certificazione. (*)

13. Nel riquadro di spostamento dello snap-in autorità di certificazione, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo** e quindi **modello di certificato da emettere**.

14. Selezionare il nome scelto nel passaggio 4 precedente e fare clic su **OK**.

15. Chiudere lo snap-in autorità di certificazione.

* **È possibile arrestare/avviare il servizio CA eseguendo il comando seguente in CMD:**

```
Net Stop "certsvc"
Net Start "certsvc"
```

## <a name="create-the-nps-server-authentication-template"></a>Creare il modello di autenticazione server dei criteri di server

Il terzo e ultimo modello di certificato da creare è il modello di autenticazione server NPS. Il modello di autenticazione server dei criteri di gruppo è una copia semplice del modello server RAS e IAS protetto per il gruppo di server NPS creato in precedenza in questa sezione.

Questo certificato verrà configurato per la registrazione automatica.

**Procedura**

1. Nella CA aprire autorità di certificazione.

2. Nel riquadro di spostamento fare clic con il pulsante destro del mouse su **modelli di certificato** e scegliere **Gestisci**.

3. Nella console modelli di certificato, fare clic con il pulsante destro del mouse su **server RAS e IAS**e scegliere **Duplica modello**.

4. Nella finestra di dialogo Proprietà nuovo modello della scheda **generale** , in **nome visualizzato modello**, digitare **autenticazione server NPS**.

5. Nella scheda **sicurezza** completare i passaggi seguenti:

    1. Selezionare **Aggiungi**.

    2. Nella finestra di dialogo **Seleziona utenti, computer, account servizio o gruppi** immettere server dei **criteri**di gruppo, quindi selezionare **OK**.

    3. In **nomi utenti e gruppi**selezionare **server NPS**.

    4. In **autorizzazioni per server NPS**selezionare le caselle di controllo **registrazione** e **registrazione** automatica nella colonna **Consenti** .

    5. In **nomi utenti e gruppi**selezionare **server RAS e IAS**, quindi selezionare **Rimuovi**.

6. Fare clic su **OK** per salvare il modello di certificato del server NPS.

7. Chiudere la console Modelli di certificato.

8. Nel riquadro di spostamento dello snap-in autorità di certificazione, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo** e quindi **modello di certificato da emettere**.

9. Selezionare **autenticazione server dei criteri di server**e fare clic su **OK**.

10. Chiudere lo snap-in autorità di certificazione.

## <a name="enroll-and-validate-the-user-certificate"></a>Registrare e convalidare il certificato utente

Poiché si sta usando Criteri di gruppo per registrare automaticamente i certificati utente, è necessario aggiornare solo il criterio e Windows 10 registrerà automaticamente l'account utente per il certificato corretto. È quindi possibile convalidare il certificato nella console dei certificati.

**Procedura**

1. Accedere a un computer client aggiunto al dominio come membro del gruppo **utenti VPN** .

2. Premere il tasto Windows + R, digitare **gpupdate/force**e premere INVIO.

3. Nel menu Start digitare **certmgr. msc**e premere INVIO.

4. In **personale**selezionare **certificati**nello snap-in certificati. I certificati vengono visualizzati nel riquadro dei dettagli.

5. Fare clic con il pulsante destro del mouse sul certificato con il nome utente di dominio corrente, quindi scegliere **Apri**.

6. Nella scheda **generale** verificare che la data indicata in **validità da** sia data odierna. In caso contrario, è possibile che sia stato selezionato il certificato errato.

7. Selezionare **OK**e chiudere lo snap-in certificati.

## <a name="enroll-and-validate-the-server-certificates"></a>Registrare e convalidare i certificati del server

Diversamente dal certificato utente, è necessario registrare manualmente il certificato del server VPN. Dopo averla registrata, convalidarla usando lo stesso processo usato per il certificato utente. Analogamente al certificato utente, il server NPS registra automaticamente il certificato di autenticazione, quindi è sufficiente convalidarlo.

>[!NOTE]
>Potrebbe essere necessario riavviare i server VPN e server dei criteri di rete per consentire l'aggiornamento delle appartenenze ai gruppi prima di completare questi passaggi.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Registrare e convalidare il certificato del server VPN

1. Nel menu Start del server VPN digitare **certlm. msc**e premere INVIO.

2. Fare clic con il pulsante destro del mouse su **personale**, selezionare **tutte le attività** e quindi selezionare **Richiedi nuovo certificato** per avviare la registrazione guidata certificati.

3. Nella pagina prima di iniziare selezionare **Avanti**.

4. Nella pagina Seleziona criteri di registrazione certificato selezionare **Avanti**.

5. Nella pagina Richiedi certificati selezionare la casella di controllo accanto al server VPN per selezionarla.

6. Nella casella di controllo server VPN selezionare **altre informazioni sono necessarie** per aprire la finestra di dialogo Proprietà certificato e completare i passaggi seguenti:

    1. Selezionare la scheda **oggetto** , selezionare **nome comune** sotto **nome soggetto**, in **tipo**.

    2. In **valore** **nome soggetto**immettere il nome dei client del dominio esterno usati per connettersi alla VPN, ad esempio VPN.contoso.com, quindi selezionare **Aggiungi**.

    3. In **nome alternativo**, in **tipo**, selezionare **DNS**.

    4. In **nome alternativo**, in **valore**, immettere tutti i nomi di server utilizzati dai client per connettersi alla vpn, ad esempio VPN.contoso.com, VPN, 132.64.86.2.

    5. Selezionare **Aggiungi** dopo aver immesso ogni nome.

    6. Al termine, selezionare **OK** .

7. Selezionare **registra**.

8. Selezionare **Fine**.

9. In **personale**selezionare **certificati**nello snap-in certificati.
    
    I certificati elencati vengono visualizzati nel riquadro dei dettagli.

10. Fare clic con il pulsante destro del mouse sul certificato con il nome del server VPN e quindi scegliere **Apri**.

11. Nella scheda **generale** verificare che la data indicata in **validità da** sia data odierna. In caso contrario, è possibile che sia stato selezionato il certificato non corretto.

12. Nella scheda **Dettagli** selezionare **utilizzo chiavi avanzato**e verificare che l' **autenticazione del server** e **intermedio IKE di sicurezza IP** sia visualizzata nell'elenco.

13. Selezionare **OK** per chiudere il certificato.

14. Chiudere lo snap-in certificati.

### <a name="validate-the-nps-server-certificate"></a>Convalidare il certificato del server NPS

1. Riavviare il server NPS.

2. Nel menu Start del server dei criteri di NPS digitare **certlm. msc**e premere INVIO.

3. In **personale**selezionare **certificati**nello snap-in certificati.

    I certificati elencati vengono visualizzati nel riquadro dei dettagli.

4. Fare clic con il pulsante destro del mouse sul certificato con il nome del server NPS, quindi scegliere **Apri**.

5. Nella scheda **generale** verificare che la data indicata in **validità da** sia data odierna. In caso contrario, è possibile che sia stato selezionato il certificato non corretto.

6. Selezionare **OK** per chiudere il certificato.

7. Chiudere lo snap-in certificati.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 3. Configurare il server di accesso remoto per la](vpn-deploy-ras.md)VPN always on: In questo passaggio viene configurata la VPN di accesso remoto per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione dei client VPN autorizzati.
