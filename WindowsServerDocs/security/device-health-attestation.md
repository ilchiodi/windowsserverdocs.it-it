---
title: "Attestazione dell'integrità del dispositivo"
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: d304ee3456f8db1e5b202c1d9221d1374a5251be
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="device-health-attestation"></a>Attestazione dell'integrità del dispositivo

>Si applica a: Windows Server 2016

Introdotto in Windows 10 versione 1507, dispositivo (attestazione dell'integrità) include quanto segue:

-   Si integra con il framework di gestione di dispositivi mobili (MDM) di Windows 10 in allineamento con [standard Open Mobile Alliance (OMA)](http://openmobilealliance.org/).

-   Supporta i dispositivi con un modulo TPM (Trusted Platform) eseguito il provisioning in un formato firmware o discreto.

-   Consente alle aziende di aumentare il livello di protezione della propria organizzazione all'hardware monitorata e garantita protezione, con una quantità minima o alcun impatto sui costi di esercizio.

A partire da Windows Server 2016, è ora possibile eseguire il servizio DHA come un ruolo del server all'interno dell'organizzazione. Utilizzare questo argomento per informazioni su come installare e configurare il ruolo del server attestazione dell'integrità del dispositivo.

## <a name="overview"></a>Panoramica

È possibile usare DHA per valutare l'integrità dei dispositivi per:
  
-   Dispositivi Windows 10 e Windows 10 Mobile che supportano TPM 1.2 o 2.0.  
-   Dispositivi locali gestiti con Active Directory con accesso a Internet, i dispositivi gestiti con Active Directory senza accesso a Internet, dispositivi gestiti da Azure Active Directory o di una distribuzione ibrida con Active Directory e Azure Active Directory.


### <a name="dha-service"></a>Servizio DHA

Il servizio DHA convalida i registri TPM e PCR per un dispositivo e quindi rilascia un report DHA. Microsoft offre il servizio DHA in tre modi:

- **Servizio DHA cloud** servizio DHA gestito da Microsoft che è disponibile, geo-con carico bilanciato e ottimizzato per l'accesso da diverse aree del mondo.

- **DHA locale servizio** un nuovo ruolo del server introdotto in Windows Server 2016. È disponibile gratuitamente per i clienti che hanno una licenza di Windows Server 2016.

- **Servizio cloud di Azure DHA** host virtuale in Microsoft Azure. A tale scopo, è necessario un host virtuale e licenze per il servizio locale DHA.

Il servizio DHA si integra con soluzioni MDM e offre quanto segue: 

-   Combinare le informazioni ricevute dai dispositivi (tramite i canali di comunicazione gestione dispositivo esistente) con il report DHA
-   Prendere una decisione di protezione più sicuro e attendibile, in base a hardware dati controllati e protetti

Ecco un esempio che mostra come è possibile usare DHA per aumentare il livello di protezione di protezione per le risorse dell'organizzazione.

1. Creare un criterio che controlla il configurazione seguente avvio e gli attributi:
  - Avvio protetto
  - BitLocker
  - ANTIMALWARE AD ESECUZIONE ANTICIPATA
2. La soluzione MDM applica questo criterio e attiva un'azione correttiva in base ai dati del report DHA.  Ad esempio, può verificare quanto segue:
  - Avvio protetto è stato abilitato, il dispositivo ha caricato codice attendibile sia autentico e il caricatore di avvio di Windows non è stato manomesso.
  - Trusted che avvio ha verificato correttamente la firma digitale del kernel di Windows e i componenti che sono stati caricati durante l'avvio al dispositivo.
  - Avvio con misurazioni creato un itinerario di controllo protetto da TPM che può essere verificato in modalità remota.
  - Disattiva BitLocker è stato abilitato e che ha protetto i dati quando il dispositivo è stato attivato.
  - Antimalware ad esecuzione anticipata è stata attivata nelle prime fasi di avvio e sta monitorando il runtime.
  
#### <a name="dha-cloud-service"></a>Servizio DHA cloud

Il servizio DHA cloud offre i vantaggi seguenti:

-   Esaminare i registri di avvio dispositivo TCG e PCR che riceve da un dispositivo registrato con una soluzione MDM. 
-   Crea un resistenti alle manomissioni e manomissioni evidente report (report DHA) che descrive come l'avvio al dispositivo in base ai dati raccolti e protetti da chip TPM del dispositivo. 
-   Invia il report DHA al server MDM che ha richiesto il report in un canale di comunicazione protetto.

#### <a name="dha-on-premises-service"></a>Servizio locale DHA

Servizio DHA locale offrono tutte le funzionalità offerte dal servizio DHA cloud.  Consente inoltre ai clienti di:

 - Ottimizzare le prestazioni eseguendo il servizio DHA nel proprio data center
 - Assicurarsi che il report DHA non lasci la rete

#### <a name="dha-azure-cloud-service"></a>Servizio cloud di Azure DHA

Questo servizio offre la stessa funzionalità come il servizio DHA locale, ad eccezione del fatto che il servizio cloud di Azure DHA eseguito come host virtuale in Microsoft Azure.

### <a name="dha-validation-modes"></a>Modalità di convalida DHA

È possibile impostare un servizio DHA locale per l'esecuzione in modalità di convalida EKCert o AIKCert. Quando il servizio DHA genera un report, indica se è stato rilasciato in modalità di convalida AIKCert o EKCert. Modalità di convalida EKCert e AIKCert offrono la stessa garanzia di sicurezza, purché la catena di EKCert del trust viene aggiornata.

#### <a name="ekcert-validation-mode"></a>Modalità di convalida EKCert

Modalità di convalida EKCert è ottimizzata per i dispositivi nelle organizzazioni che non sono connessi a Internet. Dispositivi che si connettono a un servizio DHA in esecuzione in modalità di convalida EKCert si **non** hanno accesso diretto a Internet.

Quando DHA è in esecuzione in modalità di convalida EKCert, si basa su una catena a gestione aziendale di trust che deve essere periodicamente aggiornata (circa 5 - 10 volte all'anno). 

Microsoft pubblica pacchetti aggregati di radici attendibili e CA intermedie per i produttori di TPM approvati (quando diventano disponibili) in un archivio accessibile pubblicamente nell'archivio cab. È necessario scaricare il feed, convalidarne l'integrità e installarlo sul server di attestazione dell'integrità del dispositivo.

An example archive is [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

#### <a name="aikcert-validation-mode"></a>Modalità di convalida AIKCert

Modalità di convalida AIKCert è ottimizzata per ambienti operativi che hanno accesso a Internet. Dispositivi che si connettono a un servizio DHA in esecuzione in modalità di convalida AIKCert devono avere accesso diretto a Internet e sono in grado di ottenere un certificato AIK da Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Installare e configurare il servizio DHA in Windows Server 2016

Utilizzare le sezioni seguenti per il servizio DHA installato e configurato in Windows Server 2016.

### <a name="prerequisites"></a>Prerequisiti

Per configurare e verificare servizio DHA locale, è necessario:

- Un server che esegue Windows Server 2016.
- Creare i dispositivi client Windows 10 (almeno) con un TPM (1.2 o 2.0) che si trova in stato clear/ready che esegue il più recente di Windows Insider.
- Decidere se prevede di eseguire in modalità di convalida EKCert o AIKCert.
- I certificati seguenti:
  - **Certificato SSL DHA** un certificato x. 509 SSL concatenato a una radice aziendale attendibile con una chiave privata esportabile. Questo certificato consente di proteggere DHA le comunicazioni di dati in transito tra server a server (servizio DHA e server MDM) e server per le comunicazioni client (servizio DHA e un dispositivo Windows 10).
  - **Certificato di firma DHA** un certificato x. 509 concatenato a una radice aziendale attendibile con una chiave privata esportabile. Il servizio DHA Usa questo certificato per la firma digitale. 
  - **Certificato di crittografia DHA** un certificato x. 509 concatenato a una radice aziendale attendibile con una chiave privata esportabile. Inoltre, il servizio DHA Usa questo certificato per la crittografia. 


### <a name="install-windows-server-2016"></a>Installare Windows Server 2016

Installare Windows Server 2016 usando il metodo di installazione preferito, ad esempio servizi di distribuzione Windows, o in esecuzione il programma di installazione dal supporto di avvio, un'unità USB o il file system locale. Se questa è la prima volta che si sta configurando il servizio DHA locale, è necessario installare Windows Server 2016 usando il **esperienza Desktop** opzione di installazione.

### <a name="add-the-device-health-attestation-server-role"></a>Aggiungere il ruolo del server attestazione dell'integrità del dispositivo

È possibile installare il ruolo del server attestazione dell'integrità del dispositivo e le relative dipendenze usando Server Manager. 

Dopo aver installato Windows Server 2016, il dispositivo viene riavviato e apre Server Manager. Se non viene avviato automaticamente Server manager, fare clic su **Start**, quindi fare clic su **Server Manager**.

1.  Fare clic su **Aggiungi ruoli e funzionalità**.
2.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.
3.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su **Avanti**.
4.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, selezionare il server e quindi fare clic su **Avanti**.
5.  Nel **Selezione ruoli server** pagina, selezionare il **attestazione dell'integrità del dispositivo** casella di controllo.
6.  Fare clic su **Aggiungi funzionalità** per installare altri necessari servizi ruolo e funzionalità.
7.  Fare clic su **Avanti**.
8.  Nel **selezionare le funzionalità** pagina, fare clic su **Avanti**.
9.  Nel **ruolo Server Web (IIS)** pagina, fare clic su **Avanti**.
10. Nel **Selezione servizi ruolo** pagina, fare clic su **Avanti**.
11. Nel **servizio attestazione dell'integrità del dispositivo** pagina, fare clic su **Avanti**.
12. Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.
13. Al termine dell'installazione, fare clic su **Chiudi**.

### <a name="install-the-signing-and-encryption-certificates"></a>Installare i certificati di firma e crittografia

Utilizzando il seguente script di Windows PowerShell per installare i certificati di firma e crittografia. Per ulteriori informazioni sull'identificazione personale, vedere [come: recuperare l'identificazione personale del certificato](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Installare il pacchetto di certificati radici TPM attendibili

Per installare il pacchetto di certificati radici TPM attendibili, è necessario estrarlo, rimuovere eventuali catene attendibili che non sono ritenute attendibili dall'organizzazione e quindi eseguire setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Scaricare il pacchetto di certificati radici TPM attendibili

Before you install the certificate package, you can download the latest list of trusted TPM roots from [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

> **Importante:** prima di installare il pacchetto, verificare che sia firmato digitalmente da Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Estrarre il pacchetto di certificati attendibili
Estrarre il pacchetto di certificati attendibili eseguendo i comandi seguenti.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Rimuovere le catene di certificati per i fornitori TPM che sono *non* attendibili dall'organizzazione (facoltativo)

Eliminare le cartelle per eventuali catene di certificati fornitori TPM non considerati attendibili dall'organizzazione.

> **Nota:** in modalità di certificazione AIK, la cartella Microsoft è necessaria per convalidare i certificati AIK rilasciati Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Installare il pacchetto di certificati attendibili
Installare il pacchetto di certificati attendibili eseguendo lo script di installazione dal file CAB.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurare il servizio di attestazione dell'integrità del dispositivo

È possibile utilizzare Windows PowerShell per configurare il servizio locale DHA.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurare i criteri della catena di certificati

Configurare i criteri della catena di certificati eseguendo lo script di Windows PowerShell seguente.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Comandi di gestione DHA

Ecco alcuni esempi di Windows PowerShell che consentono di gestire il servizio DHA.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configurare il servizio DHA per la prima volta

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Rimuovere la configurazione del servizio DHA

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>Ottenere il certificato di firma attivo

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>Impostare il certificato di firma attivo

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **Nota:** questo certificato deve essere distribuito nel server che esegue il servizio DHA **LocalMachine\My** archivio certificati. Quando il certificato di firma attivo è impostato, il certificato di firma attivo esistente viene spostato nell'elenco dei certificati di firma non attivi.

### <a name="list-the-inactive-signing-certificates"></a>Elenco di certificati di firma non attivi
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Rimuovere eventuali certificati di firma non attivi
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Nota:** solo *uno* certificato non attivo (di qualsiasi tipo) può esistere nel servizio in qualsiasi momento. I certificati devono essere rimossi dall'elenco dei certificati non attivi quando non sono più necessari.

### <a name="get-the-active-encryption-certificate"></a>Ottenere il certificato di crittografia attivo

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Impostare il certificato di crittografia attivo

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

Il certificato deve essere distribuito nel dispositivo nel **LocalMachine\My** archivio certificati. 

Quando il certificato di crittografia attivo è impostato, il certificato di crittografia attivo esistente viene spostato nell'elenco dei certificati di crittografia non attivi.

### <a name="list-the-inactive-encryption-certificates"></a>Elencare i certificati di crittografia non attivi

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Rimuovere eventuali certificati di crittografia non attivi

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>Ottenere la configurazione di X509ChainPolicy 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>Modificare la configurazione di X509ChainPolicy

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>Report del servizio DHA

Di seguito sono un elenco di messaggi che vengono segnalati dal servizio DHA alla soluzione MDM: 

- **200** HTTP OK. Il certificato viene restituito.
- **400** richiesta non valida. Formato di richiesta non valido, certificato di integrità non valido, firma del certificato non corrispondono, Blob di attestazione dell'integrità non valido o Blob di stato di integrità non valido. La risposta contiene anche un messaggio, come descritto nello schema di risposta, con un codice di errore e un messaggio di errore che può essere utilizzato per la diagnostica.
- **500** errore interno del server. Questa situazione può verificarsi se sono presenti problemi che impediscono l'emissione di certificati del servizio.
- **503** limitazione vengono rifiutate richieste per impedire il sovraccarico del server. 
