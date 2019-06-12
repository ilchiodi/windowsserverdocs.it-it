---
title: Gestire i certificati per Software Defined Networking
description: È possibile utilizzare questo argomento per informazioni su come gestire i certificati per Northbound di Controller di rete e comunicazioni Southbound quando si distribuiscono reti SDN (Software Defined) in Windows Server 2016 Datacenter.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: d29a98e24b475c38fee61972bf9efbd5a2528974
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446266"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gestire i certificati per Software Defined Networking

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come gestire i certificati per le comunicazioni di rete Controller Northbound e Southbound durante la distribuzione di Software Defined Networking \(SDN\) in Windows Server 2016 Datacenter e si utilizza sistema Allineare al centro di Virtual Machine Manager \(SCVMM\) come il client di gestione SDN.

>[!NOTE]
>Per informazioni introduttive sui Controller di rete, vedere [Controller di rete](../technologies/network-controller/Network-Controller.md).

Se non si usa Kerberos per proteggere la comunicazione di Controller di rete, è possibile usare certificati X.509 per l'autenticazione, autorizzazione e crittografia.

SDN in Windows Server 2016 Datacenter supporta entrambi self\-firmato e autorità di certificazione \(autorità di certificazione\)-certificati X.509 autofirmati. Questo argomento fornisce istruzioni dettagliate per la creazione di questi certificati e li applica per proteggere i canali di comunicazione di Northbound di Controller di rete con i client di gestione e comunicazione di Southbound con dispositivi di rete, ad esempio il Software Load Balancer \(SLB\).
.
Quando si utilizzano certificati\-basato su autenticazione, è necessario registrare un certificato sui nodi di Controller di rete che viene utilizzato nei modi seguenti.

1. Crittografare la comunicazione di Northbound con Secure Sockets Layer \(SSL\) tra i nodi di Controller di rete e i client di gestione, ad esempio System Center Virtual Machine Manager.
2. L'autenticazione tra i nodi di Controller di rete e servizi, ad esempio host Hyper-V e servizi di bilanciamento del carico Software e dispositivi Southbound \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Creazione e la registrazione di un certificato X.509

È possibile creare e registrare entrambi un self\-firmato certificato o un certificato emesso da un'autorità di certificazione.

>[!NOTE]
>Quando si usa SCVMM per distribuire Controller di rete, è necessario specificare il certificato X.509 utilizzato per crittografare le comunicazioni Northbound durante la configurazione del modello del servizio di Controller di rete.

La configurazione del certificato deve includere i valori seguenti.

- Il valore per il **RestEndPoint** casella di testo deve essere la rete Controller Fully Qualified Domain Name \(FQDN\) o l'indirizzo IP. 
- Il **RestEndPoint** valore deve corrispondere al nome soggetto \(Common Name, CN\) del certificato X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Creazione automatica\-certificato X.509 firmato

È possibile creare un certificato X.509 autofirmato ed esportarlo con la chiave privata \(protetto con una password\) seguendo questa procedura per singola\-nodo e più\-le distribuzioni di nodi di Controller di rete .

Quando si crea self\-firmato certificati, è possibile usare le linee guida seguenti.

- È possibile usare l'indirizzo IP dell'Endpoint REST Controller di rete per il parametro - DnsName, ma questa operazione è sconsigliata perché richiede che i nodi di Controller di rete si trovano tutte all'interno di una subnet di gestione singolo \(ad esempio, in un singolo rack\)
- Per le distribuzioni di nodi NC di più, il nome DNS specificato diventa il nome FQDN del Cluster di Controller di rete \(Host DNS i record vengono creati automaticamente.\) 
- Per distribuzioni a nodo singolo Controller di rete, il nome DNS può essere il nome di host del Controller di rete seguito dal nome di dominio completo.

#### <a name="multiple-node"></a>Più nodi

È possibile usare la [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando Windows PowerShell per creare un self\-certificato autofirmato.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Singolo nodo

È possibile usare la [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando Windows PowerShell per creare un self\-certificato autofirmato.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Creazione di un'autorità di certificazione\-certificato X.509 firmato

Per creare un certificato utilizzando un'autorità di certificazione, è necessario avere già distribuito un'infrastruttura a chiave pubblica \(PKI\) con Servizi certificati Active Directory \(Servizi certificati Active Directory\). 

>[!NOTE]
>È possibile utilizzare le autorità di certificazione di terze parti o strumenti, ad esempio openssl, per creare un certificato per l'uso con Controller di rete, tuttavia, le istruzioni in questo argomento sono specifiche di servizi certificati Active Directory. Per informazioni su come usare un'autorità di certificazione di terze parti o lo strumento, vedere la documentazione per il software in che uso.

Creazione di un certificato con un'autorità di certificazione include i passaggi seguenti.

1. Si o della tua organizzazione dominio o amministratore della sicurezza consente di configurare il modello di certificato
2. Si o della tua organizzazione amministratore Controller di rete o amministratore di SCVMM richiede un nuovo certificato dall'autorità di certificazione.

#### <a name="certificate-configuration-requirements"></a>Requisiti di configurazione certificato

Quando si configura un modello di certificato nel passaggio successivo, assicurarsi che il modello che è configurare include gli elementi necessari seguenti.

1. Il nome soggetto del certificato deve essere il nome FQDN dell'host Hyper-V
2. Il certificato deve trovarsi nell'archivio personale sul computer locale (My-cert: \localmachine\my)
3. Il certificato deve contenere sia l'autenticazione Server (utilizzo chiavi avanzato: 1.3.6.1.5.5.7.3.1) e l'autenticazione Client (utilizzo chiavi avanzato: 1.3.6.1.5.5.7.3.2) i criteri di applicazione.

>[!NOTE]
>Se il personale \(My-cert: \localmachine\my\) archivio certificati di Hyper\-V host dispone di più di un certificato X.509 con nome soggetto (CN) come nome di dominio completo dell'host \(FQDN\), Assicurarsi che il certificato che verrà usato da SDN ha una proprietà aggiuntiva di utilizzo chiavi avanzato personalizzata con l'OID 1.3.6.1.4.1.311.95.1.1.1. In caso contrario, la comunicazione tra i Controller di rete e l'host potrebbe non funzionare.

#### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato
  
>[!NOTE]
>Prima di eseguire questa procedura, è consigliabile esaminare i requisiti del certificato e i modelli di certificato disponibili nella console di modelli di certificato. È possibile modificare un modello esistente o creare un duplicato di un modello esistente e quindi modificare la copia del modello. È consigliabile creare una copia di un modello esistente.

1. Nel server in cui è installato Servizi certificati Active Directory, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **autorità di certificazione**. La Console autorità di certificazione Microsoft \(MMC\) apre. 
2. In MMC fare doppio clic sul nome della CA, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestisci**.
3. Apre la console modelli di certificato. Tutti i modelli di certificato vengono visualizzati nel riquadro dei dettagli.
4. Nel riquadro dei dettagli, scegliere il modello che si desidera duplicare.
5.  Scegliere il **azione** dal menu e quindi fare clic su **Duplica modello**. Il modello **proprietà** verrà visualizzata la finestra di dialogo.
6.  Nel modello **delle proprietà** finestra di dialogo il **nome soggetto** scheda, fare clic su **Inserisci nella richiesta**. \(Questa impostazione è necessaria per i certificati SSL di Controller di rete.\)
7.  Nel modello **delle proprietà** finestra di dialogo il **Gestione richiesta** verificare che l'opzione **Rendi la chiave privata esportabile** sia selezionata. Assicurarsi inoltre che il **firma e crittografia** scopo sia selezionato.
8.  Nel modello **delle proprietà** finestra di dialogo il **estensioni** scheda, seleziona **utilizzo chiavi**e quindi fare clic su **modifica**.
9.  Nelle **firma**, assicurarsi che **firma digitale** sia selezionata.
10.  Nel modello **delle proprietà** finestra di dialogo il **estensioni** scheda, seleziona **i criteri di applicazione**e quindi fare clic su **modifica**.
11.  Nelle **i criteri di applicazione**, assicurarsi che **l'autenticazione del Client** e **autenticazione Server** sono elencati.
12.  Salvare la copia del modello di certificato con un nome univoco, ad esempio **modello di Controller di rete**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Per richiedere un certificato dall'autorità di certificazione

È possibile utilizzare lo snap-in certificati per richiedere certificati. È possibile richiedere qualsiasi tipo di certificato che è stata preconfigurata e resa disponibili dall'amministratore dell'autorità di certificazione che elabora la richiesta di certificato.

**Gli utenti** o locale **Administrators** l'appartenenza al gruppo minimo necessario per completare questa procedura.

1. Aprire lo snap-in certificati per un computer.
2. Nell'albero della console, fare clic su **certificati \(Computer locale\)** . Selezionare il **personali** archivio certificati.
3. Nel **azione** dal menu * * tutte le attività<strong>, quindi fare clic su * * Richiedi nuovo certificato</strong> per avviare la procedura guidata registrazione certificato. Fare clic su **Avanti**.
4. Selezionare il **configurato dall'amministratore** criteri di registrazione certificato e fare clic su **successivo**.
5. Selezionare il **criteri di registrazione Active Directory** \(basato sul modello di autorità di certificazione configurato nella sezione precedente\).
6. Espandere la **dettagli** sezione e configurare gli elementi seguenti.
   1. Assicurarsi che **utilizzo chiavi** include sia <strong>firma digitale * * e * * crittografia chiave</strong>.
   2. Assicurarsi che **i criteri di applicazione** include sia **autenticazione Server** \(1.3.6.1.5.5.7.3.1\) e **l'autenticazione Client** \(1.3.6.1.5.5.7.3.2\).
7. Scegliere **Proprietà**.
8. Nel **Subject** nella scheda **nome soggetto**, in **tipo**, selezionare **nome comune**. In valore, specificare **Endpoint REST del Controller di rete**.
9. Fare clic su **Applica** e quindi su **OK**.
10. Fai clic su **Registra**.

In MMC certificati, fare clic su archivio personale per visualizzare i certificati registrati dalla CA.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Esportazione e la copia del certificato nella libreria SCVMM

Dopo la creazione di un self\-firmati o autorità di certificazione\-firmati certificati, è necessario esportare il certificato con la chiave privata \(in formato pfx\) e senza la chiave privata \(nel formato CER Base64\) dallo snap-in certificati. 

È quindi necessario copiare i due file esportati per la **ServerCertificate.cr** e **NCCertificate.cr** cartelle specificato al momento in cui è stato importato il modello di servizio di controller di rete.

1. Aprire lo snap-in certificati (certlm. msc) e individuare il certificato nell'archivio certificati personale per il computer locale.
2. A destra\-fare clic sul certificato, fare clic su **tutte le attività**, quindi fare clic su **Esporta**. Verrà visualizzata l'Esportazione guidata certificati. Fare clic su **Avanti**.
3. Selezionare **Yes**, l'opzione di chiave privata di esportazione, fare clic su **successivo**.
4. Scegliere **Personal Information Exchange - PKCS #12 (. File PFX)** e accettare l'impostazione predefinita **Includi tutti i certificati nel percorso certificazione** se possibile.
5. Assegnare utenti o i gruppi e una password per il certificato da esportare, fare clic su **successivo**.
6. Nella pagina Esporta il File, passare al percorso in cui si desidera inserire il file esportato e assegnargli un nome.
7. Analogamente, esportare il certificato in. Formato CER. Nota: Per esportare in. Formato CER, deselezionare l'opzione Sì, Esporta l'opzione della chiave privata.
8. Copia il. File PFX nella cartella ServerCertificate.cr.
9. Copia il. File CER nella cartella NCCertificate.cr.

Al termine, aggiornare le cartelle nella libreria SCVMM e assicurarsi di avere questi certificati copiati. Continuare con la configurazione del modello del servizio Controller di rete e la distribuzione.

## <a name="authenticating-southbound-devices-and-services"></a>L'autenticazione di servizi e dispositivi Southbound 

Comunicazione di Controller di rete con gli host e i MUX SLB dispositivi Usa i certificati per l'autenticazione. Comunicazione con l'host è tramite il protocollo OVSDB durante la comunicazione con i dispositivi di MUX SLB venga usato il protocollo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicazione con l'Host Hyper-V con Controller di rete

Per la comunicazione con gli host Hyper-V su OVSDB, Controller di rete deve presentare un certificato nei computer host. Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel Controller di rete e lo usa per la comunicazione di southbound con gli host.

Questo è il motivo per cui il certificato SSL deve avere l'EKU di autenticazione Client configurato. Questo certificato è configurato su "Server" REST resource \(host Hyper-V sono rappresentati nel Controller di rete come una risorsa Server\)e possono essere visualizzati eseguendo il comando di Windows PowerShell  **Get-NetworkControllerServer**.

Ecco un esempio parziale del server di risorsa REST.

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

Per l'autenticazione reciproca, l'host Hyper-V deve avere anche un certificato per comunicare con il Controller di rete. 

È possibile registrare il certificato da un'autorità di certificazione \(CA\). Se un certificato della CA basati su non viene trovato nel computer host, SCVMM crea un certificato autofirmato e viene eseguito il provisioning nel computer host.

Controller di rete e i certificati di host Hyper-V devono essere considerato attendibile dai loro. Certificato radice del certificato host Hyper-V deve essere presente nella rete Controller radice autorità di certificazione attendibili archiviare per il Computer locale e viceversa. 

Quando si usa Auto\-firmato certificati, SCVMM assicura che i certificati necessari siano presenti nell'archivio Autorità di certificazione radice attendibili per il Computer locale. 

Se si utilizzano certificati di autorità di certificazione in base per gli host Hyper-V, è necessario assicurarsi che il certificato radice CA sia presente nell'archivio Autorità di certificazione radice attendibili del Controller di rete per il Computer locale.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software Load Balancer MUX comunicazione con il Controller di rete

Il Multiplexor di servizio di bilanciamento del carico Software \(MUX\) e comunicano tramite il protocollo WCF, Controller di rete usando i certificati per l'autenticazione.

Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel Controller di rete e lo usa per la comunicazione di southbound con i dispositivi Mux. Questo certificato è configurato in "NetworkControllerLoadBalancerMux" REST risorse e possono essere visualizzati eseguendo il cmdlet di Powershell **Get-NetworkControllerLoadBalancerMux**.

Esempio di risorsa REST MUX \(parziale\):

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

Per l'autenticazione reciproca, è necessario disporre anche un certificato nei dispositivi MUX SLB. Questo certificato viene configurato automaticamente da SCVMM durante la distribuzione di bilanciamento del carico software Usa SCVMM.

>[!IMPORTANT]
>Nell'host e i nodi di bilanciamento del carico software, è essenziale che l'archivio certificati Autorità di certificazione radice attendibili non include tutti i certificati in cui "Rilasciato a" non è lo stesso come "Rilasciato da". In questo caso, la comunicazione tra i Controller di rete e il dispositivo southbound ha esito negativo.

Controller di rete e i certificati di MUX SLB devono essere attendibile per loro \(certificato radice del certificato MUX SLB deve essere presente nel computer del Controller di rete di autorità di certificazione radice attendibili archiviare e viceversa\). Quando si usa self\-firmato certificati, SCVMM assicura che i certificati necessari siano presenti nel in Autorità di certificazione radice attendibili archiviare per il Computer locale.



