---
title: Ottenere e configurare la firma di Token e i certificati di decrittografia di Token per AD FS
description: Questo documento viene descritto come ottenere e configurare i servizi terminal e TD certificati per AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Ottenere e configurare Servizi terminal e i certificati TD per AD FS

Questo argomento descrive le attività e procedure che è possibile eseguire per assicurarsi che la firma di token AD FS e i certificati di decrittografia di token siano aggiornati.

Token di firma dei certificati sono standard X509 i certificati utilizzati per accedere in modo sicuro tutti i token che il server federativo rilascia. I certificati di decrittografia di token sono standard X509 i certificati utilizzati per decrittografare i token in ingresso. Vengono inoltre pubblicati nei metadati federativi.

Per ulteriori informazioni vedere [requisiti dei certificati](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinare se ADFS rinnova automaticamente i certificati
Per impostazione predefinita, ADFS è configurato per generare la firma di token e i certificati di decrittografia di token, automaticamente in fase di configurazione iniziale e quando i certificati si stanno avvicinando la data di scadenza.

È possibile eseguire il comando di Windows PowerShell seguente:`Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La proprietà AutoCertificateRollover descrive se ADFS è configurato per rinnovare la firma di token e il token di decrittografia di certificati automaticamente.

Se AutoCertificateRollover è impostato su TRUE, i certificati AD FS verranno rinnovati e configurati automaticamente in ADFS. Dopo aver configurato il nuovo certificato, per evitare un'interruzione del servizio, è necessario assicurarsi che ogni partner federativo (rappresentato da provider di attestazioni o trust della relying party nella farm AD FS) sia aggiornato con il nuovo certificato.
    
Se ADFS non è configurato per rinnovare la firma di token e il token decrittografia di certificati (automaticamente se AutoCertificateRollover è impostato su False), AD FS verrà automaticamente generare o iniziare a utilizzare la firma di token nuovo o certificati di decrittografia di token. È necessario eseguire manualmente queste attività.
    
Se ADFS è configurato per rinnovare la firma di token e certificati di decrittografia automaticamente il token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando verrà da rinnovare:

CertificateGenerationThreshold descrive il numero di giorni prima che la data del certificato non dopo verrà generato un nuovo certificato.

CertificatePromotionThreshold determina il numero di giorni dopo il nuovo certificato viene generato che verrà promosso per il certificato primario (in altre parole, AD FS verrà avviata usano per firmare i token che possono essere emessi e decrittografare i token di provider di identità).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Se ADFS è configurato per rinnovare la firma di token e certificati di decrittografia automaticamente il token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando verrà da rinnovare:

 - **CertificateGenerationThreshold** descrive il numero di giorni prima che la data del certificato non dopo verrà generato un nuovo certificato.
 - **CertificatePromotionThreshold** determina il numero di giorni dopo il nuovo certificato viene generato che verrà promosso per il certificato primario (in altre parole, AD FS verrà iniziare a usarlo per firmare i token che possono essere emessi e decrittografare i token di identità provider).

## <a name="determine-when-the-current-certificates-expire"></a>Determinare la scadono dei certificati correnti
È possibile utilizzare la procedura seguente per identificare i certificati di decrittografia di token e di firma di token primario e per determinare quando scadono i certificati correnti.

È possibile eseguire il comando di Windows PowerShell seguente: `Get-AdfsCertificate –CertificateType token-signing` (o `Get-AdfsCertificate –CertificateType token-decrypting `). Oppure è possibile esaminare i certificati correnti in MMC: servizio -> certificati.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Il certificato per il quale il **IsPrimary** valore è impostato su **True** è il certificato che ADFS è attualmente in uso.

La data indicata per la **non dopo** è la data da cui deve essere configurato un nuovo token primario firma o la decrittografia di certificato.

Per garantire la continuità del servizio, tutti i partner federativo (rappresentati da provider di attestazioni o trust della relying party nella farm AD FS) è necessario utilizzare il nuovo firma di token e i certificati di decrittografia di token prima la scadenza. È consigliabile iniziare questo processo almeno 60 giorni in anticipo.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generazione di un nuovo certificato autofirmato manualmente prima della fine del periodo di prova
È possibile utilizzare i passaggi seguenti per generare un nuovo certificato autofirmato manualmente prima della fine del periodo di prova.

1. Assicurarsi che si è connessi al server ADFS primario.
2. Aprire Windows PowerShell ed eseguire il comando seguente: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Facoltativamente, è possibile verificare i certificati di firma correnti in ADFS. A tale scopo, eseguire il comando seguente:`Get-ADFSCertificate –CertificateType token-signing`. Esaminare l'output del comando per visualizzare le date non dopo di eventuali certificati elencati.
4. Per generare un nuovo certificato, eseguire il comando seguente per il rinnovo e aggiornare i certificati nel server AD FS:`Update-ADFSCertificate –CertificateType token-signing`.
5. Verificare che l'aggiornamento eseguendo di nuovo il comando seguente: `Get-ADFSCertificate –CertificateType token-signing`
6. Due certificati dovrebbero essere elencati ora, uno dei quali ha un **non dopo** data di circa un anno in futuro e per il quale il **IsPrimary** valore è **False**.

>[!IMPORTANT]
>Per evitare un'interruzione del servizio, aggiornare le informazioni sul certificato in Azure AD eseguendo i passaggi in modalità di aggiornamento Azure AD con un certificato di firma di token valido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se non si usi i certificati autofirmati...
Se non si utilizza il valore predefinito generato automaticamente la firma di token autofirmato certificati e la decrittografia di token, è necessario rinnovare e configurare questi certificati manualmente.

Prima di tutto, è necessario ottenere un nuovo certificato da un'autorità di certificazione e importarlo nell'archivio certificati personali del computer locale in ogni server federativo. Per istruzioni, vedere il [importare un certificato](https://technet.microsoft.com/library/cc754489.aspx) articolo.

È quindi necessario configurare questo certificato come la firma token di ADFS secondario o un certificato di decrittografia. (Viene configurato come un certificato per consentire ai partner federati un tempo sufficiente per utilizzare il nuovo certificato prima di promuovere a certificato primario secondario).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Per configurare un nuovo certificato come certificato secondario
1. Aprire PowerShell ed eseguire le operazioni seguenti: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Dopo avere importato il certificato. Aprire il **gestione di ADFS** console.
3. Espandere **servizio** e quindi seleziona **certificati**.
4. Nel riquadro azioni, fare clic su **certificato di firma del Token aggiungere**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selezionare il nuovo certificato dall'elenco dei certificati visualizzati e quindi fare clic su OK.
6.  Aprire PowerShell ed eseguire le operazioni seguenti: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Verificare che il nuovo certificato ha una chiave privata associata e che l'account del servizio ADFS è concesse autorizzazioni di lettura per la chiave privata. Eseguire questa verifica, in ogni server federativo. A tale scopo, nello snap-in certificati, fare doppio clic su nuovo certificato, fare clic su tutte le attività e quindi fare clic su Gestisci chiavi Private.

Una volta che hai consentito a un tempo sufficiente per i partner di federazione di utilizzare il nuovo certificato (eseguano il pull dei metadati di federazione o si inviarli la chiave pubblica del certificato di nuovo), è necessario promuovere il certificato secondario a certificato primario.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Per promuovere il nuovo certificato da secondario a primario

1. Aprire il **gestione di ADFS** console.
2. Espandere **servizio** e quindi seleziona **certificati**.
3. Scegliere il certificato di firma di token secondario.
4. Nel **azioni** riquadro, fare clic su **imposta come principale**. Al prompt di conferma, fare clic su Sì.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Partner federativo aggiornamento

### <a name="partners-who-can-consume-federation-metadata"></a>Partner che possono utilizzare i metadati della federazione
Se si hanno rinnovato e configurare un nuovo token di firma o un certificato di decrittografia di token, è necessario assicurarsi che tutti i partner federativo (organizzazione o l'account dell'organizzazione partner risorse che sono rappresentati in ADFS relying party trust e provider di attestazioni) sono selezionati i nuovi certificati.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partner che non possono utilizzare i metadati della federazione
Se il partner federativo non può utilizzare i metadati di federazione, è necessario inviare manualmente li la chiave pubblica del certificato di firma di token / decrittografia di token di nuovo. Invia la nuova chiave pubblica certificato (.cer file o p7b se si desidera includere l'intera catena) a tutti i partner risorse dell'organizzazione o l'account dell'organizzazione (rappresentato in ADFS da relying party trust e provider di attestazioni trust). Disporre i partner di implementare le modifiche nel loro lato considerare attendibili i nuovi certificati.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Alzare di livello a primario (se AutoCertificateRollover è False)
Se **AutoCertificateRollover** è impostato su **False**, ADFS non verrà generato automaticamente o start con nuovi token di firma o i certificati di decrittografia di token. È necessario eseguire manualmente queste attività.
Dopo avere concesso un periodo di tempo sufficiente per tutti i partner federativo per l'utilizzo del nuovo certificato secondario, alzare di livello questo certificato secondario a primario (nello snap-in MMC, fare clic su firma token secondario del certificato e nel riquadro azioni, fare clic su **Impostare come primario**.)

## <a name="updating-azure-ad"></a>L'aggiornamento di Azure AD
ADFS offre l'accesso single sign-on a servizi cloud Microsoft, ad esempio Office 365 per l'autenticazione degli utenti tramite le proprie credenziali di dominio Active Directory esistenti.  Per ulteriori informazioni sull'utilizzo dei certificati, vedere [rinnovare i certificati di federazione per Office 365 e Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs).