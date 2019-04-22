1. Dispone un amministratore di servizi di directory aggiungere l'account computer per il nuovo nodo al gruppo di sicurezza che contiene tutti i server HGS corrispondente con autorizzazione per consentire a tali server usare l'account gMSA HGS.

2. Riavviare il nuovo nodo per ottenere un nuovo ticket Kerberos che include l'appartenenza del computer nel gruppo di sicurezza. Al completamento del riavvio, accedere con un'identità di dominio appartenente al gruppo administrators locale nel computer.

3. Installare l'account del servizio gestito del gruppo HGS nel nodo.

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
