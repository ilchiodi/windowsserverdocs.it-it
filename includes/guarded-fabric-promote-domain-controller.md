1.  Eseguire [Install-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver) aggiunta al dominio e innalzare di livello il nodo a un controller di dominio.

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  Quando si riavvia il server, accedere con un account di amministratore di dominio.

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->