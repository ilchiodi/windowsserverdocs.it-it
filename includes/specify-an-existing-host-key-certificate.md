In alternativa, è possibile specificare un'identificazione personale se si desidera usare il proprio certificato. Ciò può essere utile se vuoi condividere un certificato tra più computer, oppure utilizzare un certificato associato a un modulo TPM o un modulo HSM. Ecco un esempio di creazione di un certificato associato a TPM (che impedisce l'utilizzo della chiave privata rubato e usato in un altro computer e richiede solo un TPM 1.2):

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->