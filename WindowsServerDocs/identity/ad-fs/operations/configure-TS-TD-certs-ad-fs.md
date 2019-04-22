---
title: Ottenere e configurare la firma di Token e certificati di decrittografia Token per AD FS
description: Questo documento descrive come ottenere e configurare i servizi terminal e TD dei certificati per AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820212"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Ottenere e configurare Servizi terminal e i certificati TD per AD FS

Questo argomento descrive le attività e procedure che è possibile eseguire per assicurarsi che la firma di token di AD FS e i certificati di decrittografia token siano aggiornati.

I certificati di firma di token sono standard X509 i certificati usati per firmare in modo sicuro tutti i token rilasciati dal server federativo. Certificati di decrittografia token vengono standard X509 i certificati usati per decrittografare i token in ingresso. Inoltre vengono pubblicati nei metadati federativi.

Per altre informazioni vedere [requisiti dei certificati](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinare se ADFS rinnova automaticamente i certificati
Per impostazione predefinita, ADFS è configurato per generare la firma di token e certificati di decrittografia token automaticamente, sia in fase di configurazione iniziale e quando i certificati hanno quasi raggiunto la data di scadenza.

È possibile eseguire il comando Windows PowerShell seguente: `Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La proprietà AutoCertificateRollover descrive se ADFS è configurato per rinnovare automaticamente certificati di decrittografia token e firma dei token.

Se AutoCertificateRollover è impostata su TRUE, i certificati di AD FS verranno rinnovati e configurati automaticamente in AD FS. Dopo aver configurato il nuovo certificato, per evitare un'interruzione del servizio, è necessario assicurarsi che ogni partner di federazione (rappresentato nella farm AD FS trust della relying party o trust di provider di attestazioni) viene aggiornato con il nuovo certificato.
    
Se AD FS non è configurato per rinnovare la firma di token e token di decrittografia di certificati automaticamente (se AutoCertificateRollover è impostato su False), AD FS verrà automaticamente generare o iniziare a usare nuova firma di token o certificati di decrittografia token. È necessario eseguire manualmente queste attività.
    
Se ADFS è configurato per rinnovare automaticamente certificati di decrittografia token e firma dei token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando venga rinnovati:

CertificateGenerationThreshold descrive il numero di giorni prima che la data del certificato dopo non verrà generato un nuovo certificato.

CertificatePromotionThreshold determina il numero di giorni dopo il nuovo certificato viene generato che verrà promosso per essere il certificato primario (in altre parole, AD FS verrà iniziare a usarlo per firmare i token emessi e decrittografare i token dai provider di identità).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Se ADFS è configurato per rinnovare automaticamente certificati di decrittografia token e firma dei token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando venga rinnovati:

 - **CertificateGenerationThreshold** descrive il numero di giorni prima che la data del certificato dopo non verrà generato un nuovo certificato.
 - **CertificatePromotionThreshold** determina il numero di giorni dopo il nuovo certificato viene generato che verrà promosso per essere il certificato primario (in altre parole, AD FS verrà iniziare a usarlo per firmare i token emessi e decrittografare i token dall'identità provider).

## <a name="determine-when-the-current-certificates-expire"></a>Stabilire quando scadono i certificati correnti
È possibile utilizzare la procedura seguente per identificare i certificati di decrittografia token e firma di token primario e per stabilire quando scadono i certificati correnti.

È possibile eseguire il comando Windows PowerShell seguente: `Get-AdfsCertificate –CertificateType token-signing` (o `Get-AdfsCertificate –CertificateType token-decrypting `). In alternativa, è possibile esaminare i certificati correnti in di MMC: Servizio -> certificati.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Il certificato per il quale il **IsPrimary** è impostato su **True** è il certificato che AD FS è attualmente in uso.

La data visualizzata per il **non dopo** data mediante il quale deve essere configurato un nuovo token primario di firma o la decrittografia di certificato.

Per garantire la continuità del servizio, tutti i partner di federazione (rappresentati nella farm AD FS trust della relying party o trust di provider di attestazioni) devono usare la nuova firma di token e certificati di decrittografia token prima della scadenza di questa API. È consigliabile iniziare la pianificazione del processo almeno 60 giorni di anticipo.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generazione di un nuovo certificato autofirmato manualmente prima della fine del periodo di tolleranza
È possibile utilizzare la procedura seguente per generare un nuovo certificato autofirmato manualmente prima della fine del periodo di tolleranza.

1. Assicurarsi che si è connessi al server ADFS primario.
2. Aprire Windows PowerShell ed eseguire il comando seguente: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Facoltativamente, è possibile verificare i certificati di firma correnti in AD FS. A tale scopo, eseguire il comando seguente: `Get-ADFSCertificate –CertificateType token-signing`. Esaminare l'output del comando per visualizzare le date non dopo degli eventuali certificati elencati.
4. Per generare un nuovo certificato, eseguire il comando seguente per rinnovare e aggiornare i certificati nel server AD FS: `Update-ADFSCertificate –CertificateType token-signing`.
5. Verificare l'aggiornamento eseguendo di nuovo il comando seguente: `Get-ADFSCertificate –CertificateType token-signing`
6. Dovrebbero essere elencati due certificati, uno dei quali ha un **non dopo** data di circa un anno nel futuro e per il quale il **IsPrimary** valore **False**.

>[!IMPORTANT]
>Per evitare un'interruzione del servizio, aggiornare le informazioni sul certificato in Azure AD eseguendo i passaggi descritti in dettaglio nell'aggiornamento AD Azure con un certificato di firma di token valido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se non si usa certificati autofirmati...
Se non si utilizza il valore predefinito generato automaticamente, la firma di token autofirmati e certificati di decrittografia token, è necessario rinnovare e configurare questi certificati manualmente.

In primo luogo, è necessario ottenere un nuovo certificato dall'autorità di certificazione e importarlo nell'archivio certificati personali del computer locale in ogni server federativo. Per istruzioni, vedere la [importare un certificato](https://technet.microsoft.com/library/cc754489.aspx) articolo.

È quindi necessario configurare il certificato come la firma token di AD FS secondario o un certificato di decrittografia. (È configurarlo come certificato secondario per consentire tempo sufficiente per utilizzare il nuovo certificato prima di promuoverlo a certificato primario partner federati).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Per configurare un nuovo certificato come certificato secondario
1. Aprire PowerShell ed eseguire il comando seguente: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Una volta è stato importato il certificato. Aprire il **gestione di AD FS** console.
3. Espandere **assistenza** e quindi selezionare **certificati**.
4. Nel riquadro azioni, fare clic su **Add Token-Signing Certificate**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selezionare il nuovo certificato dall'elenco dei certificati visualizzati e quindi fare clic su OK.
6.  Aprire PowerShell ed eseguire il comando seguente: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Verificare che il nuovo certificato abbia una chiave privata associata e che l'account del servizio AD FS sia concesse autorizzazioni di lettura per la chiave privata. Questa verifica in ogni server federativo. A tale scopo, nello snap-in certificati, fare doppio clic su nuovo certificato, fare clic su tutte le attività e quindi fare clic su Gestisci chiavi Private.

Dopo aver autorizzato tempo sufficiente per i partner della federazione utilizzare il nuovo certificato (estraggono i metadati di federazione o è inviare loro la chiave pubblica del nuovo certificato), è necessario alzare di livello il certificato da secondario a certificato primario.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Per alzare di livello il nuovo certificato da secondario a primario

1. Aprire il **gestione di AD FS** console.
2. Espandere **assistenza** e quindi selezionare **certificati**.
3. Fare clic sul certificato di firma di token secondario.
4. Nel **azioni** riquadro, fare clic su **impostata come primaria**. Fare clic su Sì alla richiesta di conferma.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>L'aggiornamento di partner di federazione

### <a name="partners-who-can-consume-federation-metadata"></a>Partner che possono utilizzare i metadati di federazione
Se si hanno rinnovato e configura una nuova firma di token o un certificato di decrittografia di token, è necessario assicurarsi che tutti i partner di federazione (organizzazione o account dell'organizzazione partner risorse che sono rappresentati in AD FS dal relying party relazioni di trust e provider di attestazioni) sono prelevati i nuovi certificati.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partner che non possono utilizzare i metadati della federazione
Se i partner della federazione non è possibile utilizzare i metadati di federazione, è necessario manualmente inviare la chiave pubblica del certificato di firma di token o la decrittografia di token di nuovo. Inviare la nuova chiave pubblica del certificato (con estensione p7b se si vuole includere l'intera catena o file con estensione cer) per tutta l'organizzazione di risorse o account partner dell'organizzazione (rappresentato dall'attendibilità della relying party e provider di attestazioni in AD FS). Hanno i partner di implementazione delle modifiche sul lato di considerare attendibili i nuovi certificati.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Alzare di livello al database primario (se AutoCertificateRollover è False)
Se **AutoCertificateRollover** è impostata su **False**, ADFS non verrà generato automaticamente o iniziare a usare nuove di firma di token o certificati di decrittografia token. È necessario eseguire manualmente queste attività.
Dopo aver concesso un periodo di tempo sufficiente per tutti i partner della federazione per utilizzare il nuovo certificato secondario, alzare di livello questo certificato da secondario a primario (nello snap-in MMC, fare clic sulla firma di token secondario del certificato e nel riquadro azioni, fare clic su **Impostato come primario**.)

## <a name="updating-azure-ad"></a>L'aggiornamento di Azure AD
ADFS offre accesso single sign-on ai servizi cloud Microsoft come Office 365 per l'autenticazione degli utenti tramite le proprie credenziali Active Directory Domain Services.  Per altre informazioni sull'utilizzo dei certificati, vedere [rinnovare i certificati di federazione per Office 365 e Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).