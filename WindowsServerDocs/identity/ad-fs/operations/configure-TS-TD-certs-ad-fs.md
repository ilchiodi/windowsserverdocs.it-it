---
title: Ottenere e configurare i certificati per la firma di token e la decrittografia di token per AD FS
description: Questo documento descrive come ottenere e configurare i certificati di Servizi terminal e TD per AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c544f956983357bdcaaaf2e5ee5b4ab5f80abb6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407485"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Ottenere e configurare i certificati TS e TD per AD FS

In questo argomento vengono descritte le attività e le procedure che è possibile eseguire per assicurarsi che i certificati di firma e di decrittografia dei token AD FS siano aggiornati.

I certificati per la firma di token sono certificati X509 standard usati per firmare in modo sicuro tutti i token che il server federativo rilascia. I certificati di decrittografia dei token sono certificati X509 standard usati per decrittografare i token in ingresso. Sono pubblicate anche in metadati federativi.

Per ulteriori informazioni, vedere [requisiti dei certificati](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinare se AD FS rinnova automaticamente i certificati
Per impostazione predefinita, AD FS è configurato in modo da generare automaticamente i certificati di firma del token e di decrittografia dei token, sia al momento della configurazione iniziale che alla data di scadenza dei certificati.

È possibile eseguire il comando di Windows PowerShell seguente `Get-AdfsProperties`:.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La proprietà AutoCertificateRollover descrive se AD FS è configurato per rinnovare automaticamente i certificati di firma del token e di decrittografia dei token.

Se AutoCertificateRollover è impostato su TRUE, i certificati AD FS verranno rinnovati e configurati in AD FS automaticamente. Una volta configurato il nuovo certificato, per evitare un'interruzione del servizio, è necessario assicurarsi che ogni partner federativo (rappresentato nella farm AD FS da relying party trust o attendibilità del provider di attestazioni) venga aggiornato con il nuovo certificato.
    
Se AD FS non è configurato per rinnovare automaticamente la firma di token e i certificati di decrittografia dei token (se AutoCertificateRollover è impostato su false), AD FS non genererà o inizierà automaticamente a usare nuovi certificati per la firma di token o per la decrittografia di token. Queste attività dovranno essere eseguite manualmente.
    
Se AD FS è configurato per rinnovare automaticamente la firma di token e i certificati di decrittografia dei token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando verranno rinnovati:

CertificateGenerationThreshold descrive il numero di giorni prima della data di scadenza del certificato che verrà generato un nuovo certificato.

CertificatePromotionThreshold determina il numero di giorni dopo la generazione del nuovo certificato che verrà promosso come certificato primario (in altre parole, AD FS inizierà a usarlo per firmare i token che rilascia e decrittografare i token dai provider di identità).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Se AD FS è configurato per rinnovare automaticamente la firma di token e i certificati di decrittografia dei token (AutoCertificateRollover è impostato su TRUE), è possibile determinare quando verranno rinnovati:

 - **CertificateGenerationThreshold** descrive il numero di giorni prima della data di scadenza del certificato che verrà generato un nuovo certificato.
 - **CertificatePromotionThreshold** determina il numero di giorni dopo la generazione del nuovo certificato che verrà promosso come certificato primario (in altre parole, ad FS inizierà a usarlo per firmare i token che rilascia e decrittografare i token dall'identità provider).

## <a name="determine-when-the-current-certificates-expire"></a>Determinare la scadenza dei certificati correnti
È possibile utilizzare la procedura seguente per identificare i certificati di firma del token primario e di decrittografia dei token e per determinare quando i certificati correnti scadono.

È possibile eseguire il comando di Windows PowerShell seguente `Get-AdfsCertificate –CertificateType token-signing` : ( `Get-AdfsCertificate –CertificateType token-decrypting `o). In alternativa, è possibile esaminare i certificati correnti in MMC: Certificati del servizio >.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

Il certificato per il quale il valore di **deprimary** è impostato su **true** è il certificato attualmente utilizzato da ad FS.

La data indicata per **non dopo** è la data in cui è necessario configurare un nuovo certificato primario per la firma o la decrittografia del token.

Per garantire la continuità del servizio, tutti i partner federativi, rappresentati nella farm AD FS da relying party trust o attendibilità del provider di attestazioni, devono utilizzare i nuovi certificati per la firma di token e la decrittografia di token prima di questa scadenza. È consigliabile iniziare a pianificare questo processo almeno 60 giorni prima.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generazione manuale di un nuovo certificato autofirmato prima della fine del periodo di tolleranza
È possibile utilizzare la procedura seguente per generare manualmente un nuovo certificato autofirmato prima della fine del periodo di tolleranza.

1. Assicurarsi di essere connessi al server AD FS primario.
2. Aprire Windows PowerShell ed eseguire il comando seguente:`Add-PSSnapin "microsoft.adfs.powershell"`
3. Facoltativamente, è possibile controllare i certificati di firma correnti in AD FS. A tale scopo, eseguire il comando seguente: `Get-ADFSCertificate –CertificateType token-signing`. Esaminare l'output del comando per visualizzare le date non successive di tutti i certificati elencati.
4. Per generare un nuovo certificato, eseguire il comando seguente per rinnovare e aggiornare i certificati nel server AD FS: `Update-ADFSCertificate –CertificateType token-signing`.
5. Verificare l'aggiornamento eseguendo di nuovo il comando seguente:`Get-ADFSCertificate –CertificateType token-signing`
6. Sono ora elencati due certificati, uno dei quali ha una data **non successiva** di circa un anno nel futuro e per **cui il valore di base** è **false**.

>[!IMPORTANT]
>Per evitare un'interruzione del servizio, aggiornare le informazioni sul certificato in Azure AD eseguendo i passaggi descritti nella procedura per aggiornare Azure AD con un certificato per la firma di token valido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se non si usano certificati autofirmati...
Se non si utilizzano i certificati di decrittografia token autofirmati e i certificati di decrittografia token autofirmati, è necessario rinnovarli e configurarli manualmente.

Per prima cosa, è necessario ottenere un nuovo certificato dall'autorità di certificazione e importarlo nell'archivio certificati personale del computer locale in ogni server federativo. Per istruzioni, vedere l'articolo relativo all' [importazione di un certificato](https://technet.microsoft.com/library/cc754489.aspx) .

È quindi necessario configurare questo certificato come certificato di firma o di decrittografia del token di AD FS secondario. È possibile configurarlo come certificato secondario per consentire ai partner federativi il tempo sufficiente per l'utilizzo del nuovo certificato prima di innalzarlo di livello al certificato primario.

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Per configurare un nuovo certificato come certificato secondario
1. Aprire PowerShell ed eseguire le operazioni seguenti:`Set-ADFSProperties -AutoCertificateRollover $false`
2. Dopo aver importato il certificato. Aprire la console di **gestione di ad FS** .
3. Espandere **servizio** , quindi selezionare **certificati**.
4. Nel riquadro azioni fare clic su **Aggiungi certificato**per la firma di token.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selezionare il nuovo certificato dall'elenco dei certificati visualizzati, quindi fare clic su OK.
6.  Aprire PowerShell ed eseguire le operazioni seguenti:`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Verificare che al nuovo certificato sia associata una chiave privata e che all'account del servizio AD FS siano concesse le autorizzazioni di lettura per la chiave privata. Verificare questa operazione in ogni server federativo. A tale scopo, nello snap-in certificati fare clic con il pulsante destro del mouse sul nuovo certificato, scegliere tutte le attività, quindi fare clic su Gestisci chiavi private.

Dopo avere concesso tempo sufficiente ai partner federativi per l'utilizzo del nuovo certificato, ovvero il pull dei metadati federativi o l'invio della chiave pubblica del nuovo certificato, è necessario alzare di livello il certificato secondario al certificato primario.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Per innalzare di livello il nuovo certificato da secondario a primario

1. Aprire la console di **gestione di ad FS** .
2. Espandere **servizio** , quindi selezionare **certificati**.
3. Fare clic sul certificato secondario per la firma di token.
4. Nel riquadro **azioni** fare clic su **Imposta come primario**. Fare clic su Sì alla richiesta di conferma.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Aggiornamento di partner federativi

### <a name="partners-who-can-consume-federation-metadata"></a>Partner che possono utilizzare i metadati federativi
Se è stato rinnovato e configurato un nuovo certificato per la firma di token o la decrittografia di token, è necessario assicurarsi che tutti i partner federativi (organizzazione di risorse o partner dell'organizzazione account che sono rappresentati nella AD FS da relying party trust e i trust del provider di attestazioni) hanno selezionato i nuovi certificati.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Partner che non possono utilizzare i metadati federativi
Se i partner federativi non possono utilizzare i metadati federativi, è necessario inviare manualmente la chiave pubblica del nuovo certificato di firma del token/decrittografia di token. Inviare la nuova chiave pubblica del certificato (file con estensione cer o. p7b se si desidera includere l'intera catena) a tutti i partner dell'organizzazione delle risorse o dell'organizzazione account (rappresentati nel AD FS da relying party trust e trust del provider di attestazioni). Chiedere ai partner di implementare modifiche sul lato per considerare attendibili i nuovi certificati.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Alza di livello a primario (se AutoCertificateRollover è false)
Se **AutoCertificateRollover** è impostato su **false**, ad FS non genererà o inizierà automaticamente a usare nuovi certificati per la firma di token o per la decrittografia di token. Queste attività dovranno essere eseguite manualmente.
Dopo aver consentito a un periodo di tempo sufficiente per l'utilizzo del nuovo certificato secondario da parte di tutti i partner federativi, innalzare di livello il certificato secondario a primario (nello snap-in MMC fare clic sul certificato secondario per la firma di token e nel riquadro azioni fare clic su **Imposta come primaria**.)

## <a name="updating-azure-ad"></a>Aggiornamento di Azure AD
AD FS fornisce Single Sign-On l'accesso ai servizi cloud Microsoft, ad esempio Office 365, mediante l'autenticazione degli utenti tramite le proprie credenziali di servizi di dominio Active Directory esistenti.  Per ulteriori informazioni sull'utilizzo dei certificati [, vedere rinnovo dei certificati federativi per Office 365 e Azure ad](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).