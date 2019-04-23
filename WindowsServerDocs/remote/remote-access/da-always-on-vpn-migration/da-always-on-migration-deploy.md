---
title: Migrazione da DirectAccess a VPN Always On
description: Migrazione da DirectAccess a VPN Always On richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le race condition che derivano dall'esecuzione di passaggi della migrazione non in ordine.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854582"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Eseguire la migrazione a VPN Always On e rimuovere le autorizzazioni di DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

&#171;[ **Precedente:** Pianificare DirectAccess migrazione da VPN Always On](da-always-on-migration-planning.md)<br>

Migrazione da DirectAccess a VPN Always On richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le race condition che derivano dall'esecuzione di passaggi della migrazione non in ordine. A livello generale, il processo di migrazione è costituito da quattro passaggi principali:

1.  **Distribuire un'infrastruttura VPN side-by-side.** Dopo aver determinato le fasi di migrazione e le funzionalità da includere nella distribuzione, verrà distribuita l'infrastruttura VPN affiancato con l'infrastruttura DirectAccess esistente.  

2.  **Distribuire i certificati e la configurazione per i client.**  Quando l'infrastruttura VPN è pronto, creare e pubblicare i certificati necessari al client. Quando i client hanno ricevuto i certificati, come distribuire lo script di configurazione VPN_Profile.ps1. In alternativa, è possibile usare Intune per configurare il client VPN. Usare Microsoft System Center Configuration Manager o Microsoft Intune per verificare la presenza di distribuzioni completate di configurazione VPN.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Prima di iniziare il processo di migrazione da DirectAccess a VPN Always On, assicurarsi di che aver pianificato la migrazione.  Se non è stata pianificata la migrazione, vedere [pianificare DirectAccess migrazione da VPN Always On](da-always-on-migration-planning.md).

>[!TIP] 
>In questa sezione non è una Guida dettagliata alla distribuzione per VPN Always On ma piuttosto intende complemento [Always On VPN distribuzione per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) e fornire indicazioni di distribuzione specifiche per la migrazione.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Distribuire un'infrastruttura VPN side-by-side

Si distribuisce l'infrastruttura VPN affiancato con l'infrastruttura DirectAccess esistente.  Per istruzioni dettagliate, vedere [Always On VPN distribuzione per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) per installare e configurare l'infrastruttura VPN Always On. 

Distribuzione side-by-side include le attività di alto livello seguenti:

1.  Creare i gruppi di utenti di VPN, server VPN e i server NPS.

2.  Creare e pubblicare i modelli di certificato necessari.

3.  Registrare i certificati del server.

4.  Installare e configurare il servizio di accesso remoto per VPN Always On.

5.  Installare e configurare criteri di rete.

6.  Configurare DNS e regole del firewall per VPN Always On.

La figura seguente fornisce un riferimento visivo per l'infrastruttura cambia in tutto il DirectAccess-a-Always On migrazione VPN.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Distribuire i certificati e script di configurazione VPN per i client

Sebbene la maggior parte della configurazione del client VPN nel [distribuire VPN Always On](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) sezione, altri due passaggi sono necessari per completare correttamente la migrazione da DirectAccess a VPN Always On. 

È necessario assicurarsi che il **VPN_Profile.ps1** proviene _dopo_ è stato rilasciato il certificato in modo che il client VPN non tenta di connettersi senza di essa. A tale scopo, si esegue uno script che consente di aggiungere solo gli utenti che hanno registrato il certificato al gruppo di VPN distribuzione pronto, si utilizza per distribuire la configurazione VPN Always On.

>[!NOTE] 
>Microsoft consiglia di testare questo processo prima di procedere in uno qualsiasi degli anelli di migrazione utente.

1.  **Creare e pubblicare il certificato VPN e abilitare la registrazione automatica oggetto Criteri di gruppo (GPO).** Per le distribuzioni VPN di Windows 10 tradizionali, basata su certificato, viene emesso un certificato per il dispositivo o utente in modo che sia in grado di autenticare la connessione. Quando il nuovo certificato di autenticazione viene creato e pubblicato per la registrazione automatica, è necessario creare e distribuire un oggetto Criteri di gruppo con l'impostazione di autoregistrazione configurata per il gruppo di utenti VPN. Per i passaggi configurare certificati e la registrazione automatica, vedere [configurare l'infrastruttura server](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Aggiungere utenti al gruppo di utenti VPN.** Aggiungere qualsiasi agli utenti di eseguire la migrazione al gruppo di utenti VPN. Gli utenti rimangono in tale gruppo di sicurezza dopo la loro migrazione in modo che ricevano gli aggiornamenti del certificato in futuro. Continuare ad aggiungere utenti a questo gruppo, fino a quando non si sono spostati tutti gli utenti da DirectAccess VPN Always On. 

3.  **Identificare gli utenti che hanno ricevuto un certificato di autenticazione VPN.** Si esegue la migrazione da DirectAccess, pertanto è necessario aggiungere un metodo per identificare quando un client ha ricevuto il certificato richiesto ed è pronto a ricevere le informazioni di configurazione VPN. Eseguire la **GetUsersWithCert.ps1** script per aggiungere gli utenti che attualmente vengono emessi certificati nonrevoked proveniente dal nome del modello specificato a un gruppo di sicurezza di Active Directory Domain Services specificato. Ad esempio, dopo aver eseguito il **GetUsersWithCert.ps1** script, tutti gli utenti di un certificato valido dal certificato di autenticazione VPN modello viene aggiunto al gruppo di VPN distribuzione pronto.

    >[!NOTE] 
    >Se non è un metodo per identificare quando un client ha ricevuto il certificato richiesto, è possibile distribuire la configurazione della VPN, prima è stato rilasciato il certificato all'utente, causando l'errore di connessione VPN. Per evitare questa situazione, eseguire la **GetUsersWithCert.ps1** script nell'autorità di certificazione o a una pianificazione per sincronizzare gli utenti che hanno ricevuto il certificato al gruppo di VPN distribuzione pronto. Si utilizzerà quindi tale gruppo di sicurezza per la distribuzione di configurazione VPN in System Center Configuration Manager o Intune, che assicura che il client gestito non ricevono la configurazione VPN prima di aver ricevuto il certificato di destinazione.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
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

4. Distribuire la configurazione VPN Always On. Come la connessione VPN vengono emessi i certificati di autenticazione e si esegue la **GetUsersWithCert.ps1** script, gli utenti vengono aggiunti al gruppo di sicurezza VPN distribuzione pronto.


| Se si usa...  | Quindi... |
| ---- | ---- |
| System Center Configuration Manager | Creare una raccolta di utenti in base all'appartenenza del gruppo di sicurezza.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)\!|
| Intune | È sufficiente direttamente come destinazione il gruppo di sicurezza di una volta che viene sincronizzato. |
|
    
Ogni volta che si esegue la **GetUsersWithCert.ps1** script di configurazione, è necessario eseguire anche una regola di individuazione Active Directory Domain Services per aggiornare l'appartenenza al gruppo di sicurezza in System Center Configuration Manager. Inoltre, assicurarsi che l'aggiornamento dell'appartenenza per la raccolta di distribuzione si verifica in genere sufficientemente (allineato con la regola di individuazione e script).

Per altre informazioni sull'uso di System Center Configuration Manager o Intune per distribuire VPN Always On per i client Windows, vedere [Always On VPN distribuzione per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Assicurarsi, tuttavia, per incorporare queste attività specifiche per la migrazione.

>[!NOTE] 
>L'inserimento di queste attività specifiche per la migrazione è una differenza fondamentale tra una semplice distribuzione VPN Always On e la migrazione da DirectAccess a VPN Always On. Assicurarsi di definire in modo adeguato la raccolta per il gruppo di sicurezza anziché usare il metodo nella Guida alla distribuzione di destinazione.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Rimuovere i dispositivi dal gruppo di sicurezza di DirectAccess

Come gli utenti ricevono il certificato di autenticazione e il **VPN_Profile.ps1** script di configurazione, vedere corrispondente ha esito positivo distribuzioni di script di configurazione di VPN in System Center Configuration Manager o Intune. Dopo ogni distribuzione, rimuovere il dispositivo dell'utente dal gruppo di sicurezza di DirectAccess in modo che in un secondo momento è possibile rimuovere DirectAccess. Sia Intune che System Center Configuration Manager contiene le informazioni di assegnazione di utente dispositivo per poter stabilire ogni dispositivo dell'utente.

>[!NOTE] 
>Se si stanno applicando oggetti Criteri di gruppo DirectAccess a unità organizzative (OU) anziché i gruppi di computer, Sposta oggetto computer dell'utente all'esterno dell'unità Organizzativa.

## <a name="decommission-the-directaccess-infrastructure"></a>Rimuovere le autorizzazioni dell'infrastruttura DirectAccess

Dopo avere completato la migrazione di tutti i client DirectAccess a VPN Always On, è possibile rimuovere le autorizzazioni dell'infrastruttura DirectAccess e rimuovere le impostazioni di DirectAccess da criteri di gruppo. Microsoft consiglia di eseguire la procedura seguente per rimuovere correttamente DirectAccess dall'ambiente in uso:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Pulire DNS.** Assicurarsi di rimuovere tutti i record dal server DNS interni e server DNS pubblico relativi a DirectAccess, ad esempio, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Rimuovere eventuali certificati DirectAccess da Servizi certificati Active Directory.** Se si usa certificati computer per l'implementazione di DirectAccess, rimuovere i modelli pubblicati dalla cartella dei modelli di certificato nella console Autorità di certificazione.

## <a name="next-step"></a>Passaggio successivo
Aver completato la migrazione da DirectAccess a VPN Always On. 

---