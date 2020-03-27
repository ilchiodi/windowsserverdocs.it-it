---
title: Migrazione da DirectAccess a Always On VPN
description: La migrazione da DirectAccess a Always On VPN richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le condizioni di Race che derivano dall'esecuzione di passaggi di migrazione non in ordine.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 06/07/2018
ms.openlocfilehash: 9f78edf0e48dc914b09a5e6f2d054e0fafba62e3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309308"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Eseguire la migrazione a VPN Always On e rimuovere le autorizzazioni di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

&#171;[ **Precedente:** pianificare DirectAccess per Always on migrazione VPN](da-always-on-migration-planning.md)<br>

La migrazione da DirectAccess a Always On VPN richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le condizioni di Race che derivano dall'esecuzione di passaggi di migrazione non in ordine. A un livello elevato, il processo di migrazione è costituito da questi quattro passaggi principali:

1.  **Distribuire un'infrastruttura VPN affiancata.** Dopo aver determinato le fasi di migrazione e le funzionalità che si desidera includere nella distribuzione, l'infrastruttura VPN viene distribuita side-by-side con l'infrastruttura DirectAccess esistente.  

2.  **Distribuire i certificati e la configurazione ai client.**  Quando l'infrastruttura VPN è pronta, è possibile creare e pubblicare i certificati necessari per il client. Quando i client hanno ricevuto i certificati, distribuire lo script di configurazione VPN_Profile. ps1. In alternativa, è possibile usare Intune per configurare il client VPN. Usare Microsoft endpoint Configuration Manager o Microsoft Intune per monitorare le distribuzioni di configurazione VPN riuscite.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Prima di avviare il processo di migrazione da DirectAccess a Always On VPN, assicurarsi che sia stata pianificata la migrazione.  Se la migrazione non è stata pianificata, vedere [pianificare il DirectAccess per Always on migrazione VPN](da-always-on-migration-planning.md).

>[!TIP] 
>Questa sezione non è una guida dettagliata alla distribuzione per Always On VPN, ma è destinata a completare [Always on distribuzione VPN per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) e fornire indicazioni sulla distribuzione specifiche per la migrazione.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Distribuire un'infrastruttura VPN side-by-side

Si distribuisce l'infrastruttura VPN side-by-side con l'infrastruttura DirectAccess esistente.  Per informazioni dettagliate, vedere [Always on distribuzione VPN per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) per installare e configurare l'infrastruttura VPN always on. 

La distribuzione side-by-Side è costituita dalle attività di alto livello seguenti:

1.  Creare i gruppi di utenti VPN, server VPN e server dei criteri di rete.

2.  Creare e pubblicare i modelli di certificato necessari.

3.  Registrare i certificati del server.

4.  Installare e configurare il servizio di accesso remoto per la VPN Always On.

5.  Installare e configurare NPS.

6.  Configurare le regole del firewall e DNS per la VPN Always On.

Nell'immagine seguente viene fornito un riferimento visivo per le modifiche dell'infrastruttura durante la migrazione della VPN da DirectAccess a Always On.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Distribuire i certificati e lo script di configurazione VPN ai client

Anche se la maggior parte della configurazione del client VPN nella sezione [Deploy always on VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) sono necessari due passaggi aggiuntivi per completare la migrazione da directaccess a always on VPN. 

È necessario assicurarsi che il **VPN_Profile. ps1** venga eseguito _dopo_ l'emissione del certificato, in modo che il client VPN non tenti di connettersi senza di esso. A tale scopo, è necessario eseguire uno script che aggiunge solo gli utenti che hanno registrato il certificato al gruppo pronto per la distribuzione VPN, che è possibile usare per distribuire la configurazione VPN Always On.

>[!NOTE] 
>Microsoft consiglia di testare questo processo prima di eseguirlo in qualsiasi anello di migrazione utente.

1.  **Creare e pubblicare il certificato VPN e abilitare la registrazione automatica Criteri di gruppo oggetto (GPO).** Per le distribuzioni VPN Windows 10 tradizionali basate su certificati, viene emesso un certificato per il dispositivo o l'utente in modo che possa autenticare la connessione. Quando il nuovo certificato di autenticazione viene creato e pubblicato per la registrazione automatica, è necessario creare e distribuire un oggetto Criteri di gruppo con l'impostazione registrazione automatica configurata per il gruppo utenti VPN. Per i passaggi necessari per configurare i certificati e la registrazione automatica, vedere [configurare l'infrastruttura server](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Aggiungere utenti al gruppo utenti VPN.** Aggiungere gli utenti di cui si esegue la migrazione al gruppo utenti VPN. Questi utenti rimangono nel gruppo di sicurezza dopo la migrazione, in modo che possano ricevere eventuali aggiornamenti dei certificati in futuro. Continuare ad aggiungere utenti a questo gruppo fino a quando non si sposta ogni utente da DirectAccess a Always On VPN. 

3.  **Identificare gli utenti che hanno ricevuto un certificato di autenticazione VPN.** Si sta eseguendo la migrazione da DirectAccess, quindi è necessario aggiungere un metodo per identificare quando un client ha ricevuto il certificato richiesto ed è pronto per ricevere le informazioni di configurazione VPN. Eseguire lo script **GetUsersWithCert. ps1** per aggiungere a un gruppo di sicurezza specifico di servizi di dominio Active Directory gli utenti che hanno attualmente emesso certificati non revocati provenienti dal nome del modello specificato. Ad esempio, dopo aver eseguito lo script **GetUsersWithCert. ps1** , qualsiasi utente che ha emesso un certificato valido dal modello di certificato di autenticazione VPN viene aggiunto al gruppo pronto per la distribuzione di VPN.

    >[!NOTE] 
    >Se non si dispone di un metodo per identificare quando un client ha ricevuto il certificato richiesto, è possibile distribuire la configurazione VPN prima che il certificato venga rilasciato all'utente, causando un errore della connessione VPN. Per evitare questa situazione, eseguire lo script **GetUsersWithCert. ps1** nell'autorità di certificazione o in base a una pianificazione per sincronizzare gli utenti che hanno ricevuto il certificato per il gruppo di distribuzione VPN pronto. Si userà quindi il gruppo di sicurezza per la distribuzione della configurazione VPN in Microsoft endpoint Configuration Manager o Intune, che garantisce che il client gestito non riceva la configurazione VPN prima di aver ricevuto il certificato.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert. ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. Distribuire la configurazione VPN Always On. Quando vengono emessi i certificati di autenticazione VPN e si esegue lo script **GetUsersWithCert. ps1** , gli utenti vengono aggiunti al gruppo di sicurezza pronto per la distribuzione VPN.


| Se si usa...  | Necessità di ripristino |
| ---- | ---- |
| Gestione configurazione | Creare una raccolta di utenti in base all'appartenenza del gruppo di sicurezza.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png).|
| Intune | È sufficiente indirizzare il gruppo di sicurezza direttamente dopo la sincronizzazione. |
|
    
Ogni volta che si esegue lo script di configurazione **GetUsersWithCert. ps1** , è necessario eseguire anche una regola di individuazione di servizi di dominio Active Directory per aggiornare l'appartenenza al gruppo di sicurezza in Configuration Manager. Inoltre, assicurarsi che l'aggiornamento delle appartenenze per la raccolta di distribuzione si verifichi spesso abbastanza (allineato con lo script e la regola di individuazione).

Per altre informazioni sull'uso di Configuration Manager o Intune per distribuire Always On VPN ai client Windows, vedere [Always on distribuzione VPN per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Assicurarsi tuttavia di incorporare queste attività specifiche della migrazione.

>[!NOTE] 
>L'integrazione di queste attività specifiche della migrazione è una differenza fondamentale tra una semplice distribuzione VPN Always On e la migrazione da DirectAccess a Always On VPN. Assicurarsi di definire correttamente la raccolta per fare riferimento al gruppo di sicurezza anziché usare il metodo nella Guida alla distribuzione.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Rimuovere i dispositivi dal gruppo di sicurezza DirectAccess

Quando gli utenti ricevono il certificato di autenticazione e lo script di configurazione **VPN_Profile. ps1** , vengono visualizzate le distribuzioni di script di configurazione VPN corrispondenti in Configuration Manager o Intune. Dopo ogni distribuzione, rimuovere il dispositivo dell'utente dal gruppo di sicurezza DirectAccess in modo da poter successivamente rimuovere DirectAccess. Intune e Configuration Manager contengono informazioni sull'assegnazione dei dispositivi utente che consentono di determinare il dispositivo di ogni utente.

>[!NOTE] 
>Se si applicano oggetti Criteri di gruppo DirectAccess tramite unità organizzative anziché gruppi di computer, spostare l'oggetto computer dell'utente all'esterno dell'unità organizzativa.

## <a name="decommission-the-directaccess-infrastructure"></a>Rimuovere le autorizzazioni dell'infrastruttura DirectAccess

Al termine della migrazione di tutti i client DirectAccess a Always On VPN, è possibile rimuovere le autorizzazioni dell'infrastruttura DirectAccess e rimuovere le impostazioni DirectAccess da Criteri di gruppo. Microsoft consiglia di eseguire i passaggi seguenti per rimuovere correttamente DirectAccess dall'ambiente:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Pulire il DNS.** Assicurarsi di rimuovere tutti i record dal server DNS interno e dal server DNS pubblico correlati a DirectAccess, ad esempio DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Rimuovere i certificati DirectAccess da Active Directory Servizi certificati.** Se sono stati usati certificati computer per l'implementazione di DirectAccess, rimuovere i modelli pubblicati dalla cartella modelli di certificato nella console autorità di certificazione.

## <a name="next-step"></a>Passaggio successivo
La migrazione da DirectAccess a Always On VPN è stata eseguita. 

---