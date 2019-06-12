---
title: Attestazione dell'integrità dei dispositivi
H1: nd
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
ms.openlocfilehash: 7c2d7113847cc44f18c5234502b58becde1dcb9f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446516"
---
# <a name="device-health-attestation"></a>Attestazione dell'integrità dei dispositivi

>Si applica a: Windows Server 2016

Introdotto nella versione 1507 di Windows 10, il servizio Attestazione dell'integrità del dispositivo (DHA, Device Health Attestation) include quanto segue:

-   Si integra con il framework di gestione dei dispositivi mobili (MDM) di Windows 10 ed è allineato agli [standard OMA (Open Mobile Alliance)](http://openmobilealliance.org/).

-   Supporta i dispositivi con modulo TPM (Trusted Platform Module) di cui è stato eseguito il provisioning in formato firmware o discreto.

-   Consente alle aziende di aumentare il livello di sicurezza della propria organizzazione grazie a una protezione monitorata e garantita dell'hardware, con un impatto minimo o addirittura pari a zero sui costi di esercizio.

A partire da Windows Server 2016, è ora possibile eseguire il servizio DHA come un ruolo del server all'interno dell'organizzazione. Usare questo argomento per apprendere come installare e configurare il ruolo del server Attestazione dell'integrità del dispositivo.

## <a name="overview"></a>Panoramica

È possibile usare DHA per valutare l'integrità dei dispositivi per:
  
-   Dispositivi Windows 10 e Windows 10 Mobile che supportano TPM 1.2 o 2.0.  
-   Dispositivi locali gestiti con Active Directory con accesso a Internet, dispositivi gestiti con Active Directory senza accesso a Internet, dispositivi gestiti da Azure Active Directory o una distribuzione ibrida con Active Directory e Azure Active Directory.


### <a name="dha-service"></a>Servizio DHA

Il servizio DHA convalida i registri TPM e PCR per un dispositivo e quindi genera un report DHA. Microsoft offre il servizio DHA in tre modi:

- **Servizio DHA cloud** Servizio DHA gestito da Microsoft che è gratuito, con bilanciamento del carico tra più sedi e ottimizzato per l'accesso da diverse aree del mondo.

- **Servizio DHA locale** Nuovo ruolo del server introdotto in Windows Server 2016. È disponibile gratuitamente per i clienti che hanno una licenza di Windows Server 2016.

- **Servizio DHA cloud di Azure** Host virtuale in Microsoft Azure. A tale scopo, sono necessari un host virtuale e le licenze per il servizio DHA locale.

Il servizio DHA si integra con le soluzioni MDM e offre quanto segue: 

-   Combinazione delle informazioni ricevute dai dispositivi, attraverso i canali di comunicazione esistenti nella gestione dei dispositivi, con il report DHA
-   Possibilità di prendere decisioni più efficaci e attendibili riguardo alla sicurezza, in base a dati controllati e protetti dall'hardware

Di seguito è riportato un esempio che illustra come è possibile usare DHA per aumentare il livello di protezione di protezione delle risorse dell'organizzazione.

1. Creare un criterio che controlla la configurazione e gli attributi di avvio riportati di seguito:
   - Avvio protetto
   - BitLocker
   - ELAM
2. La soluzione MDM applica questo criterio e attiva un'azione correttiva in base ai dati del report DHA.  Ad esempio, può verificare quanto segue:
   - L'avvio protetto è stato abilitato, il dispositivo ha caricato codice attendibile e autenticato e il caricatore d'avvio di Windows non è stato manomesso.
   - L'avvio sicuro ha verificato correttamente la firma digitale del kernel di Windows e i componenti che sono stati caricati durante l'avvio al dispositivo.
   - L'avvio con misurazioni ha creato un itinerario di controllo protetto da TPM che può essere verificato in modalità remota.
   - BitLocker è stato attivato e ha protetto i dati quando il dispositivo è stato spento.
   - La funzionalità ELAM è stata attivata nelle prime fasi di avvio e sta monitorando il runtime.
  
#### <a name="dha-cloud-service"></a>Servizio DHA cloud

Il servizio DHA basato su cloud offre i seguenti vantaggi:

-   Esamina i registri di avvio dispositivo TCG e PCR che riceve da un dispositivo registrato con una soluzione MDM. 
-   Crea un report DHA che offre resistenza alle e visibilità delle manomissioni e descrive il modo in cui il dispositivo è stato avviato in base ai dati raccolti e protetti da un chip TPM del dispositivo. 
-   Invia il report DHA al server MDM che ha richiesto il report in un canale di comunicazione protetto.

#### <a name="dha-on-premises-service"></a>Servizio DHA locale

Il servizio DHA locale offrono tutte le funzionalità offerte dal servizio DHA nel cloud.  Consente inoltre ai clienti di:

 - Ottimizzare le prestazioni eseguendo il servizio DHA nel proprio data center
 - Assicurarsi che il report DHA non lasci la rete

#### <a name="dha-azure-cloud-service"></a>Servizio DHA cloud di Azure

Questo servizio offre la stessa funzionalità del servizio DHA locale, ad eccezione del fatto che il servizio DHA nel cloud di Azure viene eseguito come host virtuale in Microsoft Azure.

### <a name="dha-validation-modes"></a>Modalità di convalida DHA

È possibile impostare il servizio DHA locale per l'esecuzione in modalità di convalida EKCert o AIKCert. Quando il servizio DHA genera un report, indica se è stato rilasciato in modalità di convalida AIKCert o EKCert. Le modalità di convalida EKCert e AIKCert offrono la stessa garanzia di sicurezza, purché la catena di certificati EKCert sia sempre aggiornata.

#### <a name="ekcert-validation-mode"></a>Modalità di convalida EKCert

La modalità di convalida EKCert è ottimizzata per i dispositivi nelle organizzazioni che non sono connesse a Internet. I dispositivi che si connettono a un servizio DHA in esecuzione in modalità di convalida EKCert **non** hanno accesso diretto a Internet.

Quando DHA è in esecuzione in modalità di convalida EKCert, si basa su una catena a gestione aziendale che deve essere periodicamente aggiornata (circa 5 - 10 volte all'anno). 

Microsoft pubblica pacchetti aggregati di radici attendibili e CA intermedie per i produttori di TPM approvati (quando diventano disponibili) in un archivio accessibile pubblicamente nell'archivio CAB. È necessario scaricare il feed, convalidarne l'integrità e installarlo nel server che esegue il servizio DHA.

È un esempio di archivio [ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

#### <a name="aikcert-validation-mode"></a>Modalità di convalida AIKCert

La modalità di convalida AIKCert è ottimizzata per ambienti operativi che hanno accesso a Internet. I dispositivi che si connettono a un servizio DHA eseguito in modalità di convalida AIKCert devono avere accesso diretto a Internet e sono in grado di ottenere un certificato AIK da Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Installare e configurare il servizio DHA in Windows Server 2016

Usare le sezioni seguenti per installare e configurare il servizio DHA in Windows Server 2016.

### <a name="prerequisites"></a>Prerequisiti

Per configurare e verificare un servizio DHA locale, è necessario quanto segue:

- Un server con Windows Server 2016.
- Uno o più dispositivi client Windows 10 con un modulo TPM (1.2 o 2.0) in stato clear/ready che esegue la build più recente di Windows Insider.
- Decidere se si vuole eseguire il servizio in modalità di convalida EKCert o AIKCert.
- I certificati seguenti:
  - **Certificato SSL DHA** Certificato x. 509 SSL concatenato a una radice aziendale attendibile con una chiave privata esportabile. Questo certificato consente di proteggere le comunicazioni di dati DHA in transito tra server e server (servizio DHA e server MDM) e tra server e client (servizio DHA e un dispositivo Windows 10).
  - **Certificato di firma DHA** Certificato x. 509 concatenato a una radice aziendale attendibile con una chiave privata esportabile. Il servizio DHA usa questo certificato per la firma digitale. 
  - **Certificato di crittografia DHA** Certificato x. 509 concatenato a una radice aziendale attendibile con una chiave privata esportabile. Il servizio DHA usa questo certificato anche per la crittografia. 


### <a name="install-windows-server-2016"></a>Installare Windows Server 2016

Installare Windows Server 2016 usando il metodo di installazione preferito, ad esempio Servizi di distribuzione Windows, o eseguendo il programma di installazione da un supporto di avvio, un'unità USB o il file system locale. Se è la prima volta che si configura il servizio DHA locale, è necessario installare Windows Server 2016 usando l'opzione di installazione **Esperienza desktop**.

### <a name="add-the-device-health-attestation-server-role"></a>Aggiungere il ruolo del server Attestazione dell'integrità del dispositivo

È possibile installare il ruolo del server Attestazione dell'integrità del dispositivo e le relative dipendenze usando Server Manager. 

Dopo aver installato Windows Server 2016, il dispositivo viene riavviato e apre Server Manager. Se Server Manager non si avvia automaticamente, fare clic su **Start** e quindi su **Server Manager**.

1.  Fare clic su **Aggiungi ruoli e funzionalità**.
2.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.
3.  Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.
4.  Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, selezionare il server e quindi fare clic su **Avanti**.
5.  Nella pagina **Selezione ruoli server** selezionare la casella di controllo **Attestazione dell'integrità del dispositivo**.
6.  Fare clic su **Aggiungi funzionalità** per installare altri servizi e funzionalità per il ruolo.
7.  Fare clic su **Avanti**.
8.  Nella pagina **Selezione funzionalità** fare clic sul pulsante **Avanti**.
9.  Nella pagina **Ruolo Server Web (IIS)** fare clic su **Avanti**.
10. Nella pagina **Selezione servizi ruolo** fare clic su **Avanti**.
11. Nella pagina **Servizio di attestazione dell'integrità del dispositivo** fare clic su **Avanti**.
12. Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.
13. Al termine dell'installazione fare clic su **Chiudi**.

### <a name="install-the-signing-and-encryption-certificates"></a>Installare i certificati di firma e crittografia

Usare il seguente script di Windows PowerShell per installare i certificati di firma e crittografia. Per altre informazioni sull'identificazione personale, vedere [come: Recuperare l'identificazione personale di un certificato](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Installare il pacchetto di certificati per radici TPM attendibili

Per installare il pacchetto di certificati per radici TPM attendibili, è necessario estrarlo, rimuovere eventuali catene attendibili che non sono ritenute attendibili dall'organizzazione e quindi eseguire setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Scaricare il pacchetto di certificati per radici TPM attendibili

Prima di installare il pacchetto di certificati, è possibile scaricare l'elenco più recente di radici TPM attendibili da [ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

> **Importante:** Prima di installare il pacchetto, verificare che sia firmato digitalmente da Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Estrarre il pacchetto di certificati attendibili
Per estrarre il pacchetto di certificati attendibili eseguire i comandi seguenti.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Rimuovere le catene di certificati per i fornitori TPM che *non* sono ritenuti attendibili dall'organizzazione (facoltativo)

Eliminare le cartelle per eventuali catene di certificati dei fornitori TPM non considerati attendibili dall'organizzazione.

> **Nota:** Se utilizza la modalità di certificazione AIK, la cartella Microsoft è necessario per convalidare i certificati AIK rilasciati Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Installare il pacchetto di certificati attendibili
Per installare il pacchetto di certificati attendibili eseguire lo script di installazione dal file CAB.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurare il servizio Attestazione dell'integrità del dispositivo

È possibile usare Windows PowerShell per configurare il servizio DHA locale.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurare i criteri della catena di certificati

Per configurare i criteri della catena di certificati eseguire il seguente script di Windows PowerShell.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Comandi di gestione DHA

Di seguito sono riportati alcuni esempi di Windows PowerShell che consentono di gestire il servizio DHA.

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

> **Nota:** Questo certificato deve essere distribuito nel server in esecuzione il servizio DHA nel **LocalMachine\My** archivio certificati. Quando il certificato di firma attivo è impostato, il certificato di firma già esistente viene spostato nell'elenco dei certificati di firma non attivi.

### <a name="list-the-inactive-signing-certificates"></a>Indicare i certificati di firma non attivi
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Rimuovere eventuali certificati di firma non attivi
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Nota:** Solo *uno* certificati inattivi, di qualsiasi tipo, potrebbero esistere nel servizio in qualsiasi momento. I certificati devono essere rimossi dall'elenco dei certificati non attivi quando non sono più necessari.

### <a name="get-the-active-encryption-certificate"></a>Ottenere il certificato di crittografia attivo

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Impostare il certificato di crittografia attivo

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

Il certificato deve essere distribuito nel dispositivo nell'archivio certificati **LocalMachine\My**. 

Quando il certificato di crittografia attivo è impostato, il certificato di crittografia già esistente viene spostato nell'elenco dei certificati di crittografia non attivi.

### <a name="list-the-inactive-encryption-certificates"></a>Indicare i certificati di crittografia non attivi

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

Di seguito è riportato un elenco di messaggi che vengono inviati dal servizio DHA alla soluzione MDM: 

- **200** HTTP OK. Il certificato viene restituito.
- **400** Richiesta non valida. Formato della richiesta non valido, certificato di integrità non valido, firma del certificato non corrispondente, BLOB di attestazione di integrità non valido o BLOB di stato integrità non valido. La risposta contiene anche un messaggio, come descritto nello schema di risposta, con un codice di errore e un messaggio di errore che possono essere usati per la diagnostica.
- **500** Errore interno del server. Questa situazione può verificarsi se sono presenti problemi che impediscono l'emissione dei certificati da parte del servizio.
- **503** A causa di una limitazione vengono rifiutate richieste per impedire il sovraccarico del server. 
