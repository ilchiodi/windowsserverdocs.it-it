---
title: Gestire i certificati per Software Defined Networking
description: È possibile usare questo argomento per informazioni su come gestire i certificati per le comunicazioni del controller di rete verso nord e sud quando si distribuisce la rete SDN (Software Defined Networking) in Windows Server 2016 datacenter.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 3225b3f5065e49521411b35fa3781338086b4e59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854354"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gestire i certificati per Software Defined Networking

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come gestire i certificati per le comunicazioni del controller di rete verso nord e sud quando si distribuisce software defined networking \(SDN\) in Windows Server 2016 datacenter e si usa System Center Virtual Machine Manager \(SCVMM\) come client di gestione SDN.

>[!NOTE]
>Per informazioni generali sul controller di rete, vedere [controller di rete](../technologies/network-controller/Network-Controller.md).

Se non si utilizza Kerberos per proteggere la comunicazione del controller di rete, è possibile utilizzare i certificati X. 509 per l'autenticazione, l'autorizzazione e la crittografia.

SDN in Windows Server 2016 datacenter supporta sia la firma\-automatica che l'autorità di certificazione \(CA\)certificati X. 509 firmati. In questo argomento vengono fornite istruzioni dettagliate per la creazione di questi certificati e per la relativa applicazione ai canali di comunicazione verso il controller di rete in direzione nord con i client di gestione e le comunicazioni in direzione sud con i dispositivi di rete, ad esempio il software Load Balancer \(\)SLB.
.
Quando si utilizza l'autenticazione basata su\-certificati, è necessario registrare un certificato nei nodi del controller di rete utilizzato nei modi seguenti.

1. Crittografia della comunicazione in direzione nord con Secure Sockets Layer \(\) SSL tra i nodi del controller di rete e i client di gestione, ad esempio System Center Virtual Machine Manager.
2. Autenticazione tra i nodi del controller di rete e i dispositivi e i servizi a sud, ad esempio gli host Hyper-V e i servizi di bilanciamento del carico software \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Creazione e registrazione di un certificato X. 509

È possibile creare e registrare un certificato auto\-firmato o un certificato emesso da un'autorità di certificazione.

>[!NOTE]
>Quando si usa SCVMM per distribuire il controller di rete, è necessario specificare il certificato X. 509 usato per crittografare le comunicazioni in direzione nord durante la configurazione del modello di servizio di controller di rete.

La configurazione del certificato deve includere i valori seguenti.

- Il valore della casella di testo **RestEndPoint** deve essere il nome di dominio completo del controller di rete \(FQDN\) o indirizzo IP. 
- Il valore **RestEndPoint** deve corrispondere al nome del soggetto \(nome comune,\) CN del certificato X. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Creazione di un certificato X. 509 con firma automatica\-

È possibile creare un certificato X. 509 autofirmato ed esportarlo con la chiave privata \(protetti con una password\) attenendosi alla seguente procedura per un singolo nodo di\-e per più distribuzioni di\-node del controller di rete.

Quando si creano certificati auto\-firmati, è possibile usare le linee guida seguenti.

- È possibile usare l'indirizzo IP dell'endpoint REST del controller di rete per il parametro DnsName, ma questa operazione non è consigliata perché richiede che i nodi del controller di rete si trovino tutti all'interno di una singola subnet di gestione \(ad esempio in un singolo rack\)
- Per le distribuzioni NC a più nodi, il nome DNS specificato diventerà il nome di dominio completo del cluster di controller di rete \(host DNS vengono creati automaticamente record A.\) 
- Per le distribuzioni del controller di rete a nodo singolo, il nome DNS può essere il nome host del controller di rete seguito dal nome di dominio completo.

#### <a name="multiple-node"></a>Più nodi

È possibile usare il comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) di Windows PowerShell per creare un certificato auto\-firmato.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nodo singolo

È possibile usare il comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) di Windows PowerShell per creare un certificato auto\-firmato.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Creazione di una CA\-certificato X. 509 firmato

Per creare un certificato utilizzando un'autorità di certificazione, è necessario avere già distribuito un'infrastruttura a chiave pubblica \(PKI\) con servizi certificati Active Directory \(AD CS\). 

>[!NOTE]
>È possibile utilizzare CA o strumenti di terze parti, ad esempio OpenSSL, per creare un certificato da utilizzare con il controller di rete. Tuttavia, le istruzioni riportate in questo argomento sono specifiche di Servizi certificati Active Directory. Per informazioni sull'utilizzo di una CA o di uno strumento di terze parti, vedere la documentazione relativa al software in uso.

La creazione di un certificato con una CA include i passaggi seguenti.

1. L'utente o il dominio dell'organizzazione o l'amministratore della sicurezza configura il modello di certificato
2. L'utente o l'amministratore del controller di rete dell'organizzazione o l'amministratore SCVMM richiede un nuovo certificato dalla CA.

#### <a name="certificate-configuration-requirements"></a>Requisiti di configurazione dei certificati

Quando si configura un modello di certificato nel passaggio successivo, verificare che il modello configurato includa gli elementi necessari seguenti.

1. Il nome del soggetto del certificato deve essere il nome di dominio completo dell'host Hyper-V
2. Il certificato deve trovarsi nell'archivio personale del computer locale (My – Cert: \ LocalMachine\My)
3. Il certificato deve disporre di criteri di applicazione di autenticazione server (EKU: 1.3.6.1.5.5.7.3.1) e autenticazione client (EKU: 1.3.6.1.5.5.7.3.2).

>[!NOTE]
>Se l'archivio certificati personale \(My – Cert: \ LocalMachine\My\) nell'host Hyper\-V ha più di un certificato X. 509 con nome soggetto (CN) come nome di dominio completo dell'host \(FQDN\), verificare che il certificato che verrà usato da SDN disponga di una proprietà di utilizzo chiavi avanzata personalizzata con l'OID 1.3.6.1.4.1.311.95.1.1.1. In caso contrario, la comunicazione tra il controller di rete e l'host potrebbe non funzionare.

#### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato
  
>[!NOTE]
>Prima di eseguire questa procedura, è necessario esaminare i requisiti del certificato e i modelli di certificato disponibili nella console modelli di certificato. È possibile modificare un modello esistente o creare un duplicato di un modello esistente e quindi modificare la copia del modello. È consigliabile creare una copia di un modello esistente.

1. Nel server in cui è installato Servizi certificati Active Directory, nella Server Manager fare clic su **strumenti**e quindi su **autorità di certificazione**. Viene visualizzata l'autorità di certificazione Microsoft Management Console \(MMC\). 
2. In MMC fare doppio clic sul nome della CA, fare clic con il pulsante destro del mouse su **modelli di certificato**, quindi scegliere **Gestisci**.
3. Verrà visualizzata la console modelli di certificato. Tutti i modelli di certificato vengono visualizzati nel riquadro dei dettagli.
4. Nel riquadro dei dettagli fare clic sul modello che si desidera duplicare.
5.  Fare clic sul menu **azione** , quindi fare clic su **Duplica modello**. Verrà visualizzata la finestra di dialogo **Proprietà** modello.
6.  Nella finestra di dialogo **Proprietà** modello, nella scheda **nome soggetto** , fare clic su **specificare nella richiesta**. \(questa impostazione è obbligatoria per i certificati SSL del controller di rete.\)
7.  Nella finestra di dialogo **Proprietà** modello, nella scheda **Gestione richiesta** , verificare che sia selezionata l'opzione **Consenti esportazione della chiave privata** . Assicurarsi inoltre che sia selezionata la **firma e** lo scopo della crittografia.
8.  Nella finestra di dialogo **Proprietà** modello, nella scheda **estensioni** Selezionare **utilizzo chiave**, quindi fare clic su **modifica**.
9.  In **firma**verificare che sia selezionata l'opzione **firma digitale** .
10.  Nella finestra di dialogo **Proprietà** modello, nella scheda **estensioni** selezionare criteri di **applicazione**, quindi fare clic su **modifica**.
11.  In **criteri di applicazione**assicurarsi che l' **autenticazione client** e **l'autenticazione server** siano elencate.
12.  Salvare la copia del modello di certificato con un nome univoco, ad esempio il **modello del controller di rete**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Per richiedere un certificato dalla CA

È possibile utilizzare lo snap-in certificati per richiedere i certificati. È possibile richiedere qualsiasi tipo di certificato preconfigurato e reso disponibile da un amministratore della CA che elabora la richiesta di certificato.

**Gli utenti** o gli **amministratori** locali sono l'appartenenza al gruppo minima richiesta per completare questa procedura.

1. Aprire lo snap-in certificati per un computer.
2. Nell'albero della console fare clic su **certificati \(\)del computer locale** . Selezionare l'archivio certificati **personale** .
3. Scegliere * * tutte le attività dal menu **azione** <strong>e quindi fare clic su * * Richiedi nuovo certificato</strong> per avviare la procedura guidata di registrazione del certificato. Fare clic su **Avanti**.
4. Selezionare il criterio di registrazione del certificato **di amministratore** e fare clic su **Avanti**.
5. Selezionare il **Active Directory** \(dei criteri di registrazione in base al modello di CA configurato nella sezione precedente\).
6. Espandere la sezione **Dettagli** e configurare gli elementi seguenti.
   1. Assicurarsi che l' **utilizzo delle chiavi** includa sia la <strong>firma digitale * * che la * * crittografia delle chiavi</strong>.
   2. Verificare che i **criteri di applicazione** includano sia **l'autenticazione Server** \(1.3.6.1.5.5.7.3.1\) che **l'autenticazione client** \(1.3.6.1.5.5.7.3.2\).
7. Fare clic su **Proprietà**.
8. Nella scheda **oggetto** , in **nome soggetto**, in **tipo**, selezionare **nome comune**. In valore specificare **endpoint REST del controller di rete**.
9. Fare clic su **Applica** e quindi su **OK**.
10. Fai clic su **Registra**.

In MMC certificati fare clic sull'archivio personale per visualizzare il certificato registrato dalla CA.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Esportazione e copia del certificato nella libreria SCVMM

Dopo aver creato un certificato autofirmato o\-una CA\-firmata, è necessario esportare il certificato con la chiave privata \(nel formato pfx\) e senza la chiave privata \(nel formato base-64. cer\) dallo snap-in certificati. 

È quindi necessario copiare i due file esportati nelle cartelle **serverCertificate.CR** e **NCCertificate.CR** specificate al momento dell'importazione del modello di servizio NC.

1. Aprire lo snap-in certificati (certlm. msc) e individuare il certificato nell'archivio certificati personali per il computer locale.
2. A destra\-fare clic sul certificato, scegliere **tutte le attività**e quindi fare clic su **Esporta**. Verrà visualizzata l'Esportazione guidata certificati. Fare clic su **Avanti**.
3. Selezionare l'opzione **Sì**, Esporta la chiave privata e fare clic su **Avanti**.
4. Scegliere **scambio informazioni personali-PKCS #12 (. PFX)** e accettare l'impostazione predefinita per **includere tutti i certificati nel percorso di certificazione** , se possibile.
5. Assegnare gli utenti o i gruppi e una password per il certificato da esportare, fare clic su **Avanti**.
6. Nella pagina file da esportare individuare il percorso in cui si desidera inserire il file esportato e assegnargli un nome.
7. In modo analogo, esportare il certificato in. Formato CER. Nota: per esportare in. Formato CER, deselezionare l'opzione Sì, Esporta la chiave privata.
8. Copiare il. PFX nella cartella ServerCertificate.cr.
9. Copiare il. File CER nella cartella NCCertificate.cr.

Al termine, aggiornare le cartelle nella libreria SCVMM e verificare che siano stati copiati questi certificati. Continuare con la configurazione e la distribuzione del modello di servizio del controller di rete.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticazione di dispositivi e servizi a sud 

La comunicazione del controller di rete con gli host e i dispositivi SLB MUX usa i certificati per l'autenticazione. La comunicazione con gli host è tramite il protocollo OVSDB, mentre la comunicazione con i dispositivi SLB MUX è sul protocollo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicazione dell'host Hyper-V con il controller di rete

Per la comunicazione con gli host Hyper-V tramite OVSDB, il controller di rete deve presentare un certificato ai computer host. Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel controller di rete e lo usa per la comunicazione a sud con gli host.

Questo è il motivo per cui il certificato SSL deve avere l'EKU di autenticazione client configurato. Questo certificato è configurato nella risorsa REST "Server" \(gli host Hyper-V sono rappresentati nel controller di rete come risorsa server\)e possono essere visualizzati eseguendo il comando di Windows PowerShell **Get-NetworkControllerServer**.

Di seguito è riportato un esempio parziale della risorsa REST del server.

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Per l'autenticazione reciproca, l'host Hyper-V deve disporre anche di un certificato per comunicare con il controller di rete. 

È possibile registrare il certificato da un'autorità di certificazione \(CA\). Se nel computer host non viene trovato un certificato basato su un'autorità di certificazione, SCVMM crea un certificato autofirmato e ne effettua il provisioning nel computer host.

Il controller di rete e i certificati host Hyper-V devono essere considerati attendibili gli uni dagli altri. Il certificato radice del certificato host Hyper-V deve essere presente nell'archivio Autorità di certificazione radice attendibili del controller di rete per il computer locale e viceversa. 

Quando si usano i certificati auto\-firmati, SCVMM garantisce che i certificati necessari siano presenti nell'archivio Autorità di certificazione radice attendibili per il computer locale. 

Se si usano certificati basati su CA per gli host Hyper-V, è necessario assicurarsi che il certificato radice CA sia presente nell'archivio Autorità di certificazione radice attendibili del controller di rete per il computer locale.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Comunicazione del software Load Balancer MUX con il controller di rete

Il software Load Balancer multiplexer \(MUX\) e il controller di rete comunicano tramite il protocollo WCF, utilizzando i certificati per l'autenticazione.

Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel controller di rete e lo usa per la comunicazione a sud con i dispositivi MUX. Questo certificato è configurato per la risorsa REST "NetworkControllerLoadBalancerMux" e può essere visualizzato eseguendo il cmdlet di PowerShell **Get-NetworkControllerLoadBalancerMux**.

Esempio di risorsa REST MUX \(\)parziale:

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Per l'autenticazione reciproca, è necessario avere anche un certificato nei dispositivi MUX di SLB. Questo certificato viene configurato automaticamente da SCVMM quando si distribuisce il servizio di bilanciamento del carico software tramite SCVMM.

>[!IMPORTANT]
>Nei nodi host e SLB è fondamentale che l'archivio certificati delle autorità di certificazione radice attendibili non includa alcun certificato in cui "rilasciato a" non è uguale a "rilasciato da". In tal caso, la comunicazione tra il controller di rete e il dispositivo sud ha esito negativo.

Il controller di rete e i certificati MUX di SLB devono essere considerati attendibili tra loro \(il certificato radice del certificato MUX SLB deve essere presente nell'archivio Autorità di certificazione radice attendibili del computer del controller di rete e viceversa\). Quando si usano i certificati auto\-firmati, SCVMM garantisce che i certificati necessari siano presenti nella nell'archivio Autorità di certificazione radice attendibili per il computer locale.



