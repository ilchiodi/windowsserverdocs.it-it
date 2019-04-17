---
title: Gestire i certificati per Software Defined Networking
description: È possibile utilizzare questo argomento per informazioni su come gestire i certificati per Northbound Controller di rete e le comunicazioni Southbound quando si distribuiscono reti SDN (Software Defined) in Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gestire i certificati per Software Defined Networking

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come gestire i certificati per Northbound Controller di rete e le comunicazioni Southbound quando si distribuisce Software Defined Networking \(SDN\) in Windows Server 2016 Datacenter e si utilizza System Center Virtual Machine Manager \(SCVMM\) come client di gestione di SDN.

>[!NOTE]
>Per informazioni introduttive sui Controller di rete, vedere [Controller di rete](../technologies/network-controller/Network-Controller.md).

Se non si utilizza Kerberos per proteggere la comunicazione di Controller di rete, è possibile utilizzare i certificati x. 509 per l'autenticazione, autorizzazione e crittografia.

In Windows Server 2016 Datacenter SDN supporta sia firmato e autorità di certificazione e \ (CA\)-firma i certificati x. 509. Questo argomento fornisce istruzioni dettagliate per la creazione di questi certificati e li applica per proteggere i Controller di rete Northbound canali di comunicazione con i client di gestione e le comunicazioni Southbound con dispositivi di rete, ad esempio \(SLB\) il bilanciamento del carico Software.
.
Quando si utilizza l'autenticazione basata su certificate\, è necessario registrare un certificato nei nodi di Controller di rete che viene utilizzato nei modi seguenti.

1. La crittografia delle comunicazioni Northbound con Secure Sockets Layer \(SSL\) tra i nodi di Controller di rete e i client di gestione, ad esempio System Center Virtual Machine Manager.
2. Autenticazione tra i nodi di Controller di rete e dispositivi Southbound e servizi, ad esempio host Hyper-V e \(SLBs\) bilanciamenti del carico Software.

## <a name="creating-and-enrolling-an-x509-certificate"></a>Creazione e registrazione di un certificato x. 509

È possibile creare e registrare un certificato firmato e o un certificato emesso da un'autorità di certificazione.

>[!NOTE]
>Quando si utilizza SCVMM distribuire Controller di rete, è necessario specificare il certificato x. 509 che viene utilizzato per crittografare le comunicazioni Northbound durante la configurazione del modello di servizio Controller di rete.

La configurazione del certificato deve includere i valori seguenti.

- Il valore per il **RestEndPoint** casella di testo deve essere il \(FQDN\) rete Controller nome di dominio completo o l'indirizzo IP. 
- Il **RestEndPoint** valore deve corrispondere al nome soggetto \ (nome comune, CN\) del certificato x. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Creazione di un certificato di firma e x. 509

È possibile creare un certificato x. 509 autofirmato ed esportarlo con la chiave privata \ (protetto con un password\) attenendosi alla procedura seguente per le distribuzioni apartment nodo e nodi riepilogo di Controller di rete.

Quando si creano i certificati firmati e, è possibile utilizzare le seguenti linee guida.

- È possibile utilizzare l'indirizzo IP dell'Endpoint REST Controller di rete per il parametro - DnsName, ma non è consigliabile perché richiede che i nodi di Controller di rete si trovano tutti in una subnet di gestione singolo \ (ad esempio su un singolo rack\)
- Per le distribuzioni a più nodi contesto dei nomi, il nome DNS specificato diventa il nome FQDN del Cluster di Controller di rete \ (vengono creati automaticamente i record Host DNS. \) 
- Per le distribuzioni di Controller di rete singolo nodo, il nome DNS può essere nome host del Controller di rete seguito dal nome di dominio completo.

#### <a name="multiple-node"></a>Più nodi

È possibile utilizzare il [New-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando Windows PowerShell per creare un certificato firmato e.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Singolo nodo

È possibile utilizzare il [New-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando Windows PowerShell per creare un certificato firmato e.

**Sintassi**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Esempio di utilizzo**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Creazione di un certificato firmato CA\ x. 509

Per creare un certificato con un'autorità di certificazione, è necessario aver già distribuito un \(PKI\) infrastruttura a chiave pubblica con Servizi certificati Active Directory \(AD CS\). 

>[!NOTE]
>È possibile utilizzare le autorità di certificazione di terze parti o strumenti, ad esempio openssl, per creare un certificato da utilizzare con il Controller di rete, ma le istruzioni in questo argomento sono specifiche di servizi certificati Active Directory. Per informazioni su come utilizzare una CA di terze parti o altro strumento, vedere la documentazione per il software in uso.

Creazione di un certificato con un'autorità di certificazione include i passaggi seguenti.

1. Utente o dell'organizzazione dominio o sicurezza amministratore configura il modello di certificato
2. Utente o dell'organizzazione amministratore Controller di rete o SCVMM amministratore richiede un nuovo certificato dall'autorità di certificazione.

#### <a name="certificate-configuration-requirements"></a>Requisiti di configurazione dei certificati

Durante la configurazione di un modello di certificato nel passaggio successivo, assicurarsi che il modello che è configurare include i seguenti elementi necessari.

1. Il nome soggetto del certificato deve essere il nome di dominio completo dell'host Hyper-V
2. Il certificato deve trovarsi nell'archivio personale sul computer locale (My-cert: \localmachine\my)
3. Il certificato deve avere sia l'autenticazione Server (EKU: 1.3.6.1.5.5.7.3.1) e l'autenticazione Client (EKU: 1.3.6.1.5.5.7.3.2) i criteri di applicazione.

>[!NOTE]
>Se il personale \ (My-cert: \localmachine\my\) certificato store nell'host Hyper\-V è x. 509 più di un certificato con nome soggetto (CN) \(FQDN\) nome di dominio completo dell'host, assicurarsi che il certificato che verrà utilizzato da SDN ha una proprietà di utilizzo chiavi avanzato personalizzata aggiuntiva con l'OID 1.3.6.1.4.1.311.95.1.1.1. In caso contrario, la comunicazione tra i Controller di rete e l'host potrebbe non funzionare.

#### <a name="to-configure-the-certificate-template"></a>Per configurare il modello di certificato
  
>[!NOTE]
>Prima di eseguire questa procedura, è necessario esaminare i requisiti del certificato e modelli di certificato disponibili nella console di modelli di certificato. È possibile modificare un modello esistente o creare un duplicato di un modello esistente e quindi modificare la copia del modello. È consigliabile creare una copia di un modello esistente.

1. Nel server in cui è installato Servizi certificati Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **autorità di certificazione**. Apre la Console autorità di certificazione Microsoft \(MMC\). 
2. In MMC, fare doppio clic sul nome della CA, fare doppio clic su **modelli di certificato**, quindi fare clic su **Gestisci**.
3. Apre la console modelli di certificato. Nel riquadro dei dettagli verranno visualizzati tutti i modelli di certificato.
4. Nel riquadro dei dettagli, fare clic sul modello che si desidera duplicare.
5.  Fare clic su di **azione** menu, quindi fare clic su **Duplica modello**. Il modello **proprietà** apre la finestra di dialogo.
6.  Nel modello **proprietà** della finestra di dialogo di **nome soggetto** scheda, fare clic su **Inserisci nella richiesta**. \ (Questa impostazione è necessaria per i certificati SSL di Controller di rete. \)
7.  Nel modello **proprietà** della finestra di dialogo di **Gestione richiesta** scheda, verificare che **Rendi la chiave privata esportabile** sia selezionata. Assicurarsi inoltre che il **firma e crittografia** scopo selezionato.
8.  Nel modello **proprietà** della finestra di dialogo di **estensioni** selezionare **utilizzo chiave**e quindi fare clic su **modifica**.
9.  In **firma**, assicurarsi che **firma digitale** sia selezionata.
10.  Nel modello **proprietà** della finestra di dialogo di **estensioni** selezionare **criteri di applicazione**e quindi fare clic su **modifica**.
11.  In **criteri di applicazione**, assicurarsi che **l'autenticazione Client** e **autenticazione Server** sono elencati.
12.  Salvare la copia del modello di certificato con un nome univoco, ad esempio **modello di Controller di rete**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Per richiedere un certificato dall'autorità di certificazione

È possibile utilizzare lo snap-in certificati per richiedere certificati. È possibile richiedere qualsiasi tipo di certificato che è stato preconfigurato e reso disponibili dall'amministratore dell'autorità di certificazione che elabora la richiesta di certificato.

**Gli utenti** locale o **amministratori** l'appartenenza al gruppo minimo necessario per completare questa procedura.

1. Aprire lo snap-in certificati per un computer.
2. Nell'albero della console, fare clic su **certificati \(Local Computer\)**. Selezionare il **personali** archivio certificati.
3. Nel **azione** dal menu * * tutte le attività * *, quindi fare clic su **Richiedi nuovo certificato** per avviare la registrazione guidata certificato. Fare clic su **Avanti**.
4. Selezionare il **configurata dall'amministratore** criteri di registrazione certificato e fare clic su **Avanti**.
5. Selezionare il **criteri di registrazione di Active Directory** \ (in base al modello di autorità di certificazione che è stato configurato in section\ il precedente).
6. Espandere il **dettagli** sezione e configurare gli elementi seguenti.
    1. Assicurarsi che **utilizzo chiave** include sia * * firma digitale * * e **crittografia chiave**.
    2. Assicurarsi che **criteri di applicazione** include sia **autenticazione Server** \(1.3.6.1.5.5.7.3.1\) e **l'autenticazione Client** \(1.3.6.1.5.5.7.3.2\).
7. Fare clic su **proprietà**.
8. Nel **soggetto** scheda **nome soggetto**, in **tipo**selezionare **nome comune**. Valore, specificare **Endpoint REST Controller di rete**.
9. Fare clic su **applica**, quindi fare clic su **OK**.
10. Fare clic su **registrare**.

In MMC certificati, fare clic sull'archivio personale per visualizzare il certificato che è stato registrato dall'autorità di certificazione.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Esportazione e la copia del certificato per la libreria SCVMM

Dopo aver creato un certificato firmato CA\ o firmato e, è necessario esportare il certificato con la chiave privata \(in.pfx format\) e senza la chiave privata \ (in Base 64. cer Format) nello snap-in certificati. 

È quindi necessario copiare i file esportati due per il **ServerCertificate.cr** e **NCCertificate.cr** cartelle specificate al momento quando importato il modello di servizio del contesto dei nomi.

1. Aprire lo snap-in certificati (certlm.msc) e individuare il certificato nell'archivio certificati personali del computer locale.
2. Fare clic con il certificato, fare clic su **tutte le attività**, quindi fare clic su **esportare**. Verrà visualizzata la finestra di esportazione guidata certificati. Fare clic su **Avanti**.
3. Selezionare **Sì**esportare l'opzione di chiave privata, fare clic su **Avanti**.
4. Scegliere **scambio informazioni personali - PKCS #12 (. PFX)** e accettare il valore predefinito per **includono tutti i certificati nel percorso certificazione** se possibile.
5. Assegnare/gruppi di utenti e una password per il certificato che si desidera esportare, fare clic su **Avanti**.
6. Nel File di esportazione di pagina, selezionare il percorso in cui si desidera collocare il file esportato e assegnargli un nome.
7. Analogamente, esportare il certificato in. Formato CER. Nota: Per esportare in. Formato CER, deselezionare l'opzione Sì, Esporta la possibilità di chiave privata.
8. Copia il. PFX nella cartella ServerCertificate.cr.
9. Copia il. File con estensione CER nella cartella NCCertificate.cr.

Al termine, aggiornare queste cartelle nella libreria SCVMM e assicurarsi di disporre di questi certificati copiati. Continuare con la configurazione del modello di servizio Controller di rete e la distribuzione.

## <a name="authenticating-southbound-devices-and-services"></a>L'autenticazione di servizi e dispositivi Southbound 

La comunicazione con gli host e i dispositivi SLB MUX Controller di rete utilizza i certificati per l'autenticazione. La comunicazione con gli host è tramite il protocollo OVSDB durante la comunicazione con i dispositivi SLB MUX è tramite il protocollo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicazione tra Host di Hyper-V con Controller di rete

Per la comunicazione con gli host Hyper-V tramite OVSDB, Controller di rete deve presentare un certificato nei computer host. Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel Controller di rete e lo usa per la comunicazione southbound con gli host.

Questo è il motivo per cui il certificato SSL deve avere l'EKU di autenticazione Client configurato. Questo certificato è configurato sul resto "server" risorse \ (host Hyper-V sono rappresentati nel Controller di rete come un resource\ Server) e può essere visualizzato eseguendo il comando di Windows PowerShell **Get-NetworkControllerServer**.

Ecco un esempio del server di risorse REST parziale.

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

Per l'autenticazione reciproca, l'host di Hyper-V deve disporre anche un certificato per comunicare con il Controller di rete. 

È possibile registrare il certificato da un'autorità di certificazione di \(CA\). Se non viene trovato un certificato di CA basata sul computer host, SCVMM crea un certificato autofirmato e si esegue il provisioning del computer host.

Controller di rete e i certificati di host Hyper-V devono essere considerato attendibile dai tra loro. Certificato radice del certificato host Hyper-V deve essere presente nell'archivio di rete Controller radice autorità di certificazione attendibili per il Computer locale e viceversa. 

Quando usi i certificati firmati e, SCVMM garantisce che i certificati necessari siano presenti nell'archivio Autorità di certificazione radice attendibili per il Computer locale. 

Se si utilizza i certificati della CA in base per gli host Hyper-V, è necessario assicurarsi che il certificato radice CA sia presente nell'archivio Autorità di certificazione radice attendibili del Controller di rete per il Computer locale.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software comunicazione MUX bilanciamento di carico con Controller di rete

Il Controller di rete e \(MUX\) Multiplexor del servizio di bilanciamento carico di Software comunicare tramite il protocollo WCF, utilizzando i certificati per l'autenticazione.

Per impostazione predefinita, SCVMM preleva il certificato SSL configurato nel Controller di rete e lo usa per la comunicazione southbound con i dispositivi Mux. Questo certificato viene configurato sul "NetworkControllerLoadBalancerMux" resto delle risorse e può essere visualizzato eseguendo il cmdlet Powershell **Get-NetworkControllerLoadBalancerMux**.

Esempio di \(partial\) risorsa REST MUX:

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

Per l'autenticazione reciproca, è necessario anche un certificato nei dispositivi SLB MUX. Questo certificato viene configurato automaticamente da SCVMM durante la distribuzione di bilanciamento del carico software tramite SCVMM.

>[!IMPORTANT]
>Nell'host e i nodi SLB, è fondamentale che l'archivio certificati Autorità di certificazione radice attendibili non include alcun certificato dove "Rilasciato a" non è uguale a quello "Rilasciato da". In questo caso, la comunicazione tra i Controller di rete e il dispositivo southbound ha esito negativo.

Controller di rete e i certificati SLB MUX devono essere attendibile reciprocamente \ (certificato radice del certificato MUX SLB deve essere presente nel computer del Controller di rete, autorità di certificazione radice attendibili archiviare e morsa versa\). Quando usi i certificati firmati e, SCVMM garantisce che i certificati necessari siano presenti nel in Autorità di certificazione radice attendibili archiviare per il Computer locale.



