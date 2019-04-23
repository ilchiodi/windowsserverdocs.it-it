Il servizio sorveglianza Host deve essere installato in una foresta di Active Directory separata.
Assicurarsi che la macchina HGS **non** aggiunto a un dominio prima di avviare e accedere come redatte computer locale.

Eseguire i comandi seguenti per installare il servizio sorveglianza Host e configurare il dominio.
La password specificati qui verrà applicate solo alla password modalità ripristino servizi Directory per Active Directory. verrà *non* modificare la password di accesso dell'account amministratore.
È possibile fornire qualsiasi nome di dominio preferito per - HgsDomainName.

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
