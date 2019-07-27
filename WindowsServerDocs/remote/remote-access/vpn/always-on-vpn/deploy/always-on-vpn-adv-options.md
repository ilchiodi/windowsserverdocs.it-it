---
title: Funzionalità avanzate di VPN Always On
description: Oltre allo scenario di distribuzione fornito in questa distribuzione, è possibile aggiungere altre funzionalità VPN avanzate per migliorare la sicurezza e la disponibilità della connessione VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/19
ms.author: pashort, v-tea
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: ae3c088122a0100f94b4d9bca41078d901487237
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590406"
---
# <a name="advanced-features-of-always-on-vpn"></a>Funzionalità avanzate di Always On VPN

>Si applica a Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente** Scopri la tecnologia VPN Always On](../always-on-vpn-technology-overview.md)
- [**Prossimo** Iniziare a pianificare la distribuzione di Always On VPN](always-on-vpn-deploy-planning.md)

Oltre agli scenari di distribuzione disponibili, è possibile aggiungere altre funzionalità VPN avanzate per migliorare la sicurezza e la disponibilità della connessione VPN. Ad esempio, il server VPN può usare queste funzionalità per verificare che il client connesso sia integro prima di consentire una connessione.

## <a name="high-availability"></a>Disponibilità elevata

Di seguito sono riportate altre opzioni per la disponibilità elevata.

|Opzione  |Descrizione  |
|---------|---------|
|Resilienza del server e bilanciamento del carico     |Negli ambienti che richiedono disponibilità elevata o che supportano un numero elevato di richieste, è possibile aumentare le prestazioni e la resilienza dell'accesso remoto usando il bilanciamento del carico tra più server che eseguono Server dei criteri di rete (NPS) e abilitando Clustering del server di accesso remoto.<p>Documenti correlati:<ul><li>[Bilanciamento del carico del server proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Distribuire Accesso remoto in un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resilienza del sito geografico     |Per la georilevazione basata su IP, è possibile usare gestione traffico globale con DNS in Windows Server 2016. Per il bilanciamento del carico geografico più affidabile, è possibile usare soluzioni globali di bilanciamento del carico del server, ad esempio Gestione traffico di Microsoft Azure.<p>Documenti correlati:<ul><li>[Panoramica di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gestione traffico di Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Autenticazione avanzata

Di seguito sono riportate altre opzioni per l'autenticazione.

|Opzione  |Descrizione  |
|---------|---------|
|Windows Hello for Business     |In Windows 10, Windows Hello for business sostituisce le password fornendo un'autenticazione a due fattori avanzata nei PC e nei dispositivi mobili. Questa autenticazione è costituita da un nuovo tipo di credenziale utente associato a un dispositivo e utilizza un PIN (biometrico o identificazione personale).<p>Il client VPN di Windows 10 è compatibile con Windows Hello for business. Dopo che l'utente ha eseguito l'accesso usando un movimento, la connessione VPN usa il certificato di Windows Hello for business per l'autenticazione basata sui certificati.<p>Documenti correlati:<ul><li>[Windows Hello for business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Case study tecnico: [Abilitazione dell'accesso remoto con Windows Hello for business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Autenticazione a più fattori di Azure     |Azure multi-factor authentication offre versioni cloud e locali che è possibile integrare con il meccanismo di autenticazione VPN di Windows.<p>Per altre informazioni sul funzionamento di questo meccanismo, vedere [integrare l'autenticazione RADIUS con il server Azure Multifactor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Funzionalità VPN avanzate

Di seguito sono riportate le opzioni aggiuntive per le funzionalità avanzate.

|Opzione  |Descrizione  |
|---------|---------|
|Filtro del traffico     |Se è necessario applicare la scelta dei client VPN che possono accedere alle applicazioni, è possibile abilitare i filtri di traffico VPN.<p>Per altre informazioni, vedere [funzionalità di sicurezza VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN attivata dall'app     |È possibile configurare i profili VPN per la connessione automatica quando si avviano determinate applicazioni o tipi di applicazioni.<p>Per altre informazioni su questa e altre opzioni di attivazione, vedere [Opzioni del profilo attivato automaticamente da VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accesso condizionale VPN   |L'accesso condizionale e la conformità dei dispositivi possono richiedere che i dispositivi gestiti soddisfino gli standard prima che possano connettersi alla VPN. Una delle funzionalità avanzate per l'accesso condizionale VPN consente di limitare le connessioni VPN solo a quelle in cui il certificato di autenticazione client contiene l'OID "accesso condizionale AAD" di **1.3.6.1.4.1.311.87**.<p>Per limitare le connessioni VPN, è necessario eseguire le operazioni seguenti:<ol><li>Nel server dei criteri di rete aprire lo snap-in **Server dei criteri di rete** .</li><li>Espandere **criteri** > criteri di**rete**.</li><li>Fare clic con il pulsante destro del mouse su connessioni di rete **privata virtuale (VPN)** e selezionare **Proprietà**.</li><li>Selezionare la scheda **Impostazioni** .</li><li>Selezionare **specifico del fornitore**e quindi selezionare **Aggiungi**.</li><li>Selezionare l'opzione **allowed-certificate-OID** , quindi selezionare **Aggiungi**.</li><li>Incollare l'OID di accesso condizionale di AAD di **1.3.6.1.4.1.311.87** come valore dell'attributo, quindi selezionare **OK** due volte.</li><li>Selezionare **Chiudi**e quindi fare clic su **applica**.<p>Dopo aver eseguito questi passaggi, quando i client VPN tentano di connettersi usando un certificato diverso dal certificato cloud di breve durata, la connessione non riesce.</li></ol>Per altre informazioni sull'accesso condizionale, vedere [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Blocco dei client VPN che usano i certificati revocati
  
Dopo aver installato gli aggiornamenti, il server RRAS può applicare la revoca dei certificati per le VPN che usano IKEv2 e i certificati del computer per l'autenticazione, ad esempio le VPN always-on del tunnel del dispositivo. Ciò significa che, per tali VPN, il server RRAS può negare le connessioni VPN ai client che tentano di utilizzare un certificato revocato.

**Disponibilità**

Nella tabella seguente sono elencate le date di rilascio approssimative delle correzioni per ogni versione di Windows.

|Versione del sistema operativo |Data di rilascio o di rilascio * |
|---------|---------|
|Windows Server, versione 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server, versione 1809  |Q3, 2019  |
|Windows Server, versione 1803  |Q3, 2019  |
|Windows Server, versione 1709  |Q3, 2019  |
|Windows Server 2016, versione 1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |
  
\*Tutte le date di rilascio sono elencate in calendar Quarters. Le date sono approssimative e possono cambiare senza preavviso. Quando viene rilasciato un aggiornamento, un collegamento alla versione sostituisce la data di rilascio.

**Come configurare i prerequisiti** 

1. Installare gli aggiornamenti di Windows non appena diventano disponibili.
1. Assicurarsi che tutti i certificati del server RRAS e del client VPN utilizzati includano voci CDP e che il server RRAS possa raggiungere i rispettivi CRL.
1. Nel server RRAS usare il cmdlet di PowerShell **set-VpnAuthProtocol** per configurare il parametro **RootCertificateNameToAccept** .<br /><br />
   Nell'esempio seguente vengono elencati i comandi a tale scopo. Nell'esempio **CN = Contoso Root Certification Authority** rappresenta il nome distinto dell'autorità di certificazione radice. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Come configurare il server RRAS per applicare la revoca dei certificati per le connessioni VPN basate sui certificati del computer IKEv2**

1. In una finestra del prompt dei comandi eseguire il comando seguente: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Riavviare il servizio **routing e accesso remoto** .
  
Per disabilitare la revoca dei certificati per queste connessioni VPN, impostare **CertAuthFlags = 2** oppure rimuovere il valore **CertAuthFlags** , quindi riavviare il servizio **routing e accesso remoto** . 

**Come revocare un certificato client VPN per una connessione VPN basata su un certificato del computer IKEv2**
1. Revocare il certificato client VPN all'autorità di certificazione.
1. Pubblicare un nuovo CRL dall'autorità di certificazione.
1. Nel server RRAS aprire una finestra del prompt dei comandi di amministrazione, quindi eseguire i comandi seguenti:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Come verificare che la revoca dei certificati per le connessioni VPN basate sui certificati del computer IKEv2 sia funzionante**  
>[!Note]  
> Prima di usare questa procedura, assicurarsi di abilitare il registro eventi operativo di CAPI2.
1. Seguire i passaggi precedenti per revocare un certificato client VPN.
1. Provare a connettersi alla VPN usando un client che dispone del certificato revocato. Il server RRAS deve rifiutare la connessione e visualizzare un messaggio come "le credenziali di autenticazione IKE sono inaccettabili".
1. Nel server RRAS aprire Visualizzatore eventi e passare a **registri applicazioni e servizi/Microsoft/Windows/CAPI2**. 
1. Cercare un evento con le informazioni seguenti:
   * Nome registro: **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID evento: **41** 
   * L'evento contiene il testo seguente: **Subject = "*client FQDN*"** (*FQDN client* rappresenta il nome di dominio completo del client con il certificato revocato). 

   Il **<Result>** campo dei dati dell'evento deve includere **il certificato revocato**. Vedere ad esempio gli estratti seguenti da un evento:
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>Protezione aggiuntiva

### <a name="trusted-platform-module-tpm-key-attestation"></a>Attestazione chiave Trusted Platform Module (TPM)

Un certificato utente che dispone di una chiave con attestazione TPM garantisce una sicurezza più elevata, sottoposta a backup da non esportabilità, anti-martellamento e isolamento delle chiavi fornite dal TPM.

Per ulteriori informazioni sull'attestazione chiave TPM in Windows 10, vedere [attestazione chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Passaggio successivo

[Iniziare a pianificare la distribuzione di always on VPN](always-on-vpn-deploy-planning.md): Prima di installare il ruolo del server accesso remoto nel computer che si intende utilizzare come server VPN, effettuare le operazioni seguenti. Dopo la pianificazione appropriata, è possibile distribuire Always On VPN e, facoltativamente, configurare l'accesso condizionale per la connettività VPN usando Azure AD.  

## <a name="related-topics"></a>Argomenti correlati
- [Bilanciamento del carico del server proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): I client RADIUS (Remote Authentication Dial-in User Service), che sono server di accesso alla rete, ad esempio server VPN (Virtual Private Network) e punti di accesso wireless, creano richieste di connessione e le inviano ai server RADIUS, ad esempio NPS. In alcuni casi, un server NPS può ricevere troppe richieste di connessione contemporaneamente, causando un calo delle prestazioni o un sovraccarico.

- [Panoramica di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Questo argomento fornisce una panoramica di gestione traffico di Azure, che consente di controllare la distribuzione del traffico utente per gli endpoint di servizio. Gestione traffico USA il Domain Name System (DNS) per indirizzare le richieste client all'endpoint più appropriato in base a un metodo di routing del traffico e all'integrità degli endpoint. 

- [Windows Hello for business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Questo argomento fornisce i prerequisiti, ad esempio distribuzioni solo cloud e distribuzioni ibride.  In questo argomento vengono inoltre elencate le domande frequenti su Windows Hello for business.

- [Case study tecnico: Abilitazione dell'accesso remoto con Windows Hello for business](https://msdn.microsoft.com/library/mt728163.aspx)in Windows 10: In questo case study tecnico si apprenderà come Microsoft implementa l'accesso remoto con Windows Hello for business.  Windows Hello for business è un approccio di autenticazione basato su certificato o su chiave privata/pubblica per le organizzazioni e i consumatori che vanno oltre le password. Questa forma di autenticazione si basa sulle credenziali della coppia di chiavi che possono sostituire le password e sono resistenti a violazioni, furti e phishing. 

- [Integrare l'autenticazione RADIUS con il server Azure Multifactor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Questo argomento illustra l'aggiunta e la configurazione di un'autenticazione client RADIUS con il server Azure Multifactor Authentication. RADIUS è un protocollo standard per accettare le richieste di autenticazione ed elaborarle. Il server Azure Multifactor Authentication può fungere da server RADIUS. 

- [Funzionalità di sicurezza VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Questo argomento fornisce le linee guida sulla sicurezza VPN per la VPN di blocco, l'integrazione di Windows Information Protection (WIP) con VPN e i filtri di traffico. 

- [Opzioni del profilo attivato automaticamente da VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Questo argomento fornisce le opzioni del profilo attivato automaticamente da VPN, ad esempio il trigger dell'app, il trigger basato sul nome e Always On.

- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Questo argomento fornisce una panoramica della piattaforma di accesso condizionale basato sul cloud per fornire un'opzione di conformità del dispositivo per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

- [Attestazione chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Questo argomento fornisce una panoramica di Trusted Platform Module (TPM) e i passaggi per distribuire l'attestazione chiave TPM. È anche possibile trovare informazioni sulla risoluzione dei problemi e le procedure per risolvere i problemi.
