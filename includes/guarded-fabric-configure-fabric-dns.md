Esistono diversi modi per configurare la risoluzione dei nomi per il dominio dell'infrastruttura. Un modo semplice è configurare una zona server d'inoltro condizionale DNS per l'infrastruttura. Per configurare quest'area, eseguire i comandi seguenti in una console di Windows PowerShell con privilegi elevata in un server di infrastruttura DNS. Sostituire i nomi e indirizzi nella sintassi riportata di seguito come necessario per l'ambiente Windows PowerShell. Aggiungere il server master per i nodi aggiuntivi del servizio HGS.

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    