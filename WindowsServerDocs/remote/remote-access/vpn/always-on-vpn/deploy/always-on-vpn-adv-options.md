---
title: Funzionalità avanzate di VPN Always On
description: Oltre lo scenario di distribuzione disponibile in questa distribuzione, è possibile aggiungere altre funzionalità VPN avanzate per migliorare la sicurezza e la disponibilità della connessione VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 5f43d64dc7642ef67da03fec989909bc4f2f14ae
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/19/2019
ms.locfileid: "67263034"
---
# <a name="advanced-features-of-always-on-vpn"></a>Funzionalità avanzate di VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Apprendere le tecnologie VPN Always On](../always-on-vpn-technology-overview.md)
- [**prossimo:** Iniziare a pianificare la distribuzione VPN Always On](always-on-vpn-deploy-planning.md)

Oltre gli scenari di distribuzione forniti, è possibile aggiungere altre funzionalità VPN avanzate per migliorare la sicurezza e la disponibilità della connessione VPN. Ad esempio, tali componenti possono garantire che i client che si connette sia integro prima di consentire una connessione.

## <a name="high-availability"></a>Disponibilità elevata

Di seguito sono le opzioni aggiuntive per la disponibilità elevata.

|Opzione  |Descrizione  |
|---------|---------|
|Bilanciamento del carico e resilienza del server     |Negli ambienti che richiedono elevata disponibilità o il supporto numerose richieste, è possibile aumentare le prestazioni e la resilienza di accesso remoto con bilanciamento del carico tra più server che eseguono Server dei criteri di rete (NPS) e abilitazione di connessioni Remote Cluster di server di accesso.<p>Documenti correlati:<ul><li>[Bilanciamento del carico Server Proxy dei criteri di rete](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Distribuire Accesso remoto in un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resilienza del sito geografica     |Per la georilevazione basato su IP, è possibile utilizzare Global Traffic Manager con il servizio DNS in Windows Server 2016. Per il bilanciamento del carico geografico più affidabile, è possibile usare soluzioni Global Server il bilanciamento del carico, ad esempio Gestione traffico di Microsoft Azure.<p>Documenti correlati:<ul><li>[Panoramica di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gestione traffico di Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Autenticazione avanzata

Di seguito sono le opzioni aggiuntive per l'autenticazione.

|Opzione  |Descrizione  |
|---------|---------|
|Windows Hello for Business     |In Windows 10, su PC e dispositivi mobili Windows Hello for Business sostituisce le password con l'autenticazione avanzata a due fattori. Questa autenticazione è costituito da un nuovo tipo di credenziali dell'utente che sono associata a un dispositivo e Usa un elemento biometrico o numero di identificazione personale (PIN).<p>Il client VPN di Windows 10 è compatibile con Windows Hello for Business. Dopo che l'utente accede con un movimento, la connessione VPN Usa di Windows Hello per il certificato di Business per l'autenticazione basata su certificato.<p>Documenti correlati:<ul><li>[Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Case Study tecnico: [Abilitazione dell'accesso remoto con Windows Hello for Business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure Multifactor Authentication (MFA)     |Azure MFA ha cloud e le versioni che è possibile integrare con il meccanismo di autenticazione di Windows VPN locali.<p>Per altre informazioni sul funzionamento di questo meccanismo, vedere [integrare l'autenticazione con il Server Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Funzionalità avanzate VPN

Di seguito sono le opzioni aggiuntive per funzionalità avanzate.

|Opzione  |Descrizione  |
|---------|---------|
|Filtro del traffico     |Se è necessario applicare i client VPN quali applicazioni possono accedere, è possibile abilitare i filtri traffico VPN.<p>Per altre informazioni, vedere [funzionalità di sicurezza VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN attivata dall'App     |È possibile configurare i profili VPN per connettersi automaticamente alcune applicazioni o i tipi di applicazioni di avvio.<p>Per altre informazioni su questa e altre opzioni di attivazione, vedere [opzioni di attivazione automatica del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accesso condizionale di VPN   |Conformità di dispositivi e l'accesso condizionale può richiedere i dispositivi gestiti per rispettare gli standard prima di potersi connettere alla VPN. Una delle funzionalità avanzate per l'accesso condizionale VPN consente di limitare le connessioni VPN solo a quelli in cui il certificato di autenticazione client contiene 'Degli AAD l'accesso condizionale OID di ' 1.3.6.1.4.1.311.87'.<p>Per limitare le connessioni VPN, è necessario:<ol><li>Nel server NPS, aprire il **Server dei criteri di rete** snap-in.</li><li>Espandere **politiche** > **i criteri di rete**.</li><li>Fare doppio clic il **le connessioni di rete privata virtuale (VPN)** criteri di rete e selezionare **proprietà**.</li><li>Selezionare il **impostazioni** scheda.</li><li>Selezionare **fornitore specifico** e selezionare **Add**.</li><li>Selezionare il **Allowed-certificato-OID** opzione e quindi selezionare **Add**.</li><li>L'OID di accesso condizionale di AAD di incollare **1.3.6.1.4.1.311.87** come valore dell'attributo, quindi selezionare **OK** due volte.</li><li>Selezionare **Close** e quindi **applicare**.<p>A questo punto quando i client VPN tentano di connettersi utilizzando qualsiasi certificato diverso dal certificato cloud breve durata, la connessione avrà esito negativo.</li></ol>Per altre informazioni sull'accesso condizionale, vedere [VPN e l'accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Blocco dei client VPN che usano i certificati revocati
  
Dopo aver installato gli aggiornamenti, il server RRAS è possibile applicare la revoca dei certificati per le connessioni VPN che usano IKEv2 e i certificati del computer per l'autenticazione, ad esempio dispositivo tunnel VPN Always on. Ciò significa che per questo tipo VPN, il server RRAS può negare le connessioni VPN per i client che tentano di usare un certificato revocato.

**Disponibilità**

La tabella seguente elenca le date di rilascio approssimativo delle correzioni per ogni versione di Windows.

|Versione del sistema operativo |Data di rilascio * |
|---------|---------|
|Windows Server, versione 1903  |Q2, 2019  |
|Windows Server 2019<br />Windows Server, versione 1809  |(DOMANDA 3), 2019  |
|Windows Server, versione 1803  |(DOMANDA 3), 2019  |
|Windows Server, versione 1709  |(DOMANDA 3), 2019  |
|Windows Server 2016, versione 1607  |Q2, 2019  |
  
\* Tutte le date di rilascio sono elencate in trimestri di calendario. Le date sono approssimative e possono cambiare senza preavviso.

**Come configurare i prerequisiti** 

1. Installare gli aggiornamenti di Windows man mano che diventano disponibili.
1. Assicurarsi che tutti i client VPN e i certificati del server RRAS che consente di abbiano voci di punti di distribuzione, e che il server RRAS può raggiungere i rispettivi CRL.
1. Nel server RRAS, usare il **Set-VpnAuthProtocol** cmdlet di PowerShell per configurare il **RootCertificateNameToAccept** parametro.<br /><br />
   L'esempio seguente elenca i comandi per eseguire questa operazione. Nell'esempio riportato **CN = autorità di certificazione radice Contoso** rappresenta il nome distinto dell'autorità di certificazione radice. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Come configurare il server RRAS per imporre la revoca dei certificati per le connessioni VPN che si basano sui certificati del computer IKEv2**

1. In una finestra del prompt dei comandi eseguire il comando seguente: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Riavviare il **Routing e accesso remoto** servizio.
  
Per disabilitare la revoca dei certificati per le connessioni VPN, impostare **CertAuthFlags scrittura=2** o rimuovere le **CertAuthFlags** valore e quindi riavviare il **Routing e accesso remoto**service. 

**Come revocare un certificato client VPN per una connessione VPN basata su un certificato computer IKEv2**
1. Revocare il certificato client VPN all'autorità di certificazione.
1. Pubblicare un nuovo CRL all'autorità di certificazione.
1. Nel server RRAS, aprire una finestra del prompt dei comandi amministrativa e quindi eseguire i comandi seguenti:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Come verificare che la revoca di certificato per le connessioni VPN basata su certificato computer IKEv2 funzioni**  
>[!Note]  
> Prima di usare questa procedura, assicurarsi di abilitare il registro eventi operativi CAPI2.
1. Seguire i passaggi precedenti per revocare un certificato client VPN.
1. Provare a connettersi alla VPN tramite un client con il certificato revocato. Il server RRAS deve rifiutare la connessione e visualizzare un messaggio, ad esempio "credenziali di autenticazione IKE sono inaccettabili".
1. Nel server RRAS, aprire il Visualizzatore eventi e spostarsi **applicazioni e i log di servizi/Microsoft/Windows/CAPI2**. 
1. Cerca un evento che contiene le informazioni seguenti:
   * Nome registro: **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID evento: **41** 
   * L'evento contiene il testo seguente: **subject = "*FQDN di Client*"** (*FQDN di Client* rappresenta il nome di dominio completo del client che ha il revocati certificato). 

   Il **<Result>** campo dei dati dell'evento deve includere **il certificato viene revocato**. Ad esempio, vedere i seguenti estratti da un evento:
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

### <a name="trusted-platform-module-tpm-key-attestation"></a>Attestazione chiave di Trusted Platform Module (TPM)

Un certificato utente con una chiave attestata TPM garantisce maggiore sicurezza, il backup esportabilità, anti-hammering, isolamento e delle chiavi fornite dal TPM.

Per altre informazioni sull'attestazione chiave TPM in Windows 10, vedere [attestazione chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Passaggio successivo

[Iniziare a pianificare la distribuzione VPN Always On](always-on-vpn-deploy-planning.md): Prima di installare il ruolo server Accesso remoto al computer in cui che si prevede di utilizzare come server VPN, eseguire le attività seguenti. Dopo la corretta pianificazione, è possibile distribuire VPN Always On e, facoltativamente, configurare l'accesso condizionale per la connettività VPN con Azure AD.  

## <a name="related-topics"></a>Argomenti correlati
- [Bilanciamento del carico Server Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): Remote Authentication Dial-In utente client RADIUS (Service), che sono server di accesso di rete come server di rete privata virtuale (VPN) e punti di accesso wireless, creare richieste di connessione e inviarli al server RADIUS, ad esempio criteri di rete. In alcuni casi, un server dei criteri di rete potrà ricevere un numero eccessivo di richieste di connessione in una sola volta, conseguente riduzione delle prestazioni o da un overload.

- [Panoramica di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): In questo argomento offre una panoramica di gestione traffico di Azure, che consente di controllare la distribuzione del traffico utente per gli endpoint di servizio. Gestione traffico Usa il sistema DNS (Domain Name) per indirizzare le richieste client all'endpoint più appropriato in base a un metodo di routing del traffico e all'integrità degli endpoint. 

- [Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): In questo argomento fornisce i prerequisiti, ad esempio le distribuzioni solo cloud e ibride.  Questo argomento elenca anche domande frequenti su Windows Hello for Business.

- [Case study tecnico: Abilitazione dell'accesso remoto con Windows Hello for Business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): In questo case study tecnico viene illustrato come Microsoft implementa accesso remoto con Windows Hello for Business.  Windows Hello for Business è una chiave privata/pubblica o un approccio di autenticazione basata su certificati per le organizzazioni e i consumer che vanno oltre le password. Questo tipo di autenticazione si basa su credenziali a coppie di chiavi che possono sostituire le password e resistente alle violazioni, furti e phishing. 

- [Integrare l'autenticazione RADIUS con il Server Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Questo argomento descrive come aggiungere e configurare l'autenticazione client RADIUS con il Server Azure multi-Factor Authentication. RADIUS è un protocollo standard per accettare le richieste di autenticazione ed elaborarle. Il Server Azure multi-Factor Authentication può fungere da server RADIUS. 

- [Funzionalità di sicurezza VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): In questo argomento fornisce si VPN linee guida sulla sicurezza per LockDown VPN, l'integrazione di Windows Information Protection (WIP) con VPN e i filtri traffico. 

- [Opzioni di attivazione automatica del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): In questo argomento fornisce opzioni di attivazione automatica del profilo VPN, ad esempio trigger dell'app, in base al nome di un trigger e Always On.

- [VPN e l'accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): In questo argomento fornisce una panoramica della piattaforma accesso condizionale basato sul cloud, per fornire un'opzione di conformità del dispositivo per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

- [Attestazione chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): In questo argomento fornisce una panoramica del modulo TPM (Trusted Platform) e i passaggi per distribuire l'attestazione chiave TPM. È anche possibile trovare la risoluzione dei problemi di informazioni e procedure per risolvere i problemi.
