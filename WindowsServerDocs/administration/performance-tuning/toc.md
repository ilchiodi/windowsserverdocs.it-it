# [Linee guida di ottimizzazione delle prestazioni per Windows Server 2016](index.md)
## [Ottimizzazione dell'hardware del server](hardware/index.md)
### [Considerazioni sulle prestazioni](hardware/index.md)
### [Considerazioni sul risparmio di energia](hardware/power.md)
#### [Risparmio energia e ottimizzazione delle prestazioni](hardware/power/power-performance-tuning.md)
#### [Ottimizzazione di Risparmio energia del processore](hardware/power/processor-power-management-tuning.md)
#### [Parametri della combinazione per il risparmio di energia Bilanciato](hardware/power/recommended-balanced-plan-parameters.md)
## Ottimizzazione del ruolo del server
### [Server Active Directory](role/active-directory-server/index.md)
#### [Pianificazione della capacità per Active Directory Domain Services](role/active-directory-server/capacity-planning-for-active-directory-domain-services.md)
#### [Considerazioni sulla definizione di sito](role/active-directory-server/site-definition-considerations.md)
#### [Considerazioni relative ai requisiti hardware](role/active-directory-server/hardware-considerations.md)
#### [Considerazioni sull'utilizzo della memoria](role/active-directory-server/memory-usage-considerations.md)
#### [Considerazioni relative a LDAP](role/active-directory-server/ldap-considerations.md)
#### [Risolvere i problemi delle prestazioni di Active Directory Domain Services](role/active-directory-server/troubleshoot.md)
### [File server](role/file-server/index.md)
#### [File server SMB](role/file-server/smb-file-server.md)
#### [File server NFS](role/file-server/nfs-file-server.md)
### [Server Hyper-V](role/hyper-v-server/index.md)
#### [Terminologia](role/hyper-v-server/terminology.md)
#### [Architettura](role/hyper-v-server/architecture.md)
#### [Configurazione](role/hyper-v-server/configuration.md)
#### [Prestazioni del processore](role/hyper-v-server/processor-performance.md)
#### [Prestazioni della memoria](role/hyper-v-server/memory-performance.md)
#### [Prestazioni di I/O dell'archiviazione](role/hyper-v-server/storage-io-performance.md)
#### [Prestazioni di I/O della rete](role/hyper-v-server/network-io-performance.md)
#### [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](role/hyper-v-server/detecting-virtualized-environment-bottlenecks.md)
#### [Considerazioni sulle macchine virtuali Linux](role/hyper-v-server/linux-virtual-machine-considerations.md)
### [Contenitori di Windows Server](role/windows-server-container/index.md)
### [Servizi Desktop remoto](role/remote-desktop/session-hosts.md)
#### [Host sessione](role/remote-desktop/session-hosts.md)
#### [Host di virtualizzazione](role/remote-desktop/virtualization-hosts.md)
#### [Gateway](role/remote-desktop/gateways.md)
### [Server Web](role/web-server/index.md)
#### [Ottimizzazione di IIS 10.0](role/web-server/tuning-iis-10.md)
#### [HTTP 1.1/2](role/web-server/http-performance.md)
## Ottimizzazione del sottosistema del server
### [Ottimizzazione della cache e della memoria](subsystem/cache-memory-management/index.md)
#### [Risolvere i problemi di prestazioni per la gestione di cache e memoria](subsystem/cache-memory-management/troubleshoot.md)
#### [Miglioramenti di gestione della memoria e della cache](subsystem/cache-memory-management/improvements-in-windows-server.md)
### [Ottimizzazione del sottosistema di rete](../../networking/technologies/network-subsystem/net-sub-performance-top.md)
#### [Scegliere una scheda di rete](../../networking/technologies/network-subsystem/net-sub-choose-nic.md)
#### [Configurare l'ordine delle interfacce di rete](../../networking/technologies/network-subsystem/net-sub-interface-metric.md)
#### [Ottimizzazione delle schede di rete](../../networking/technologies/network-subsystem/net-sub-performance-tuning-nics.md)
#### [Contatori delle prestazioni di rete](../../networking/technologies/network-subsystem/net-sub-performance-counters.md)
#### [Strumenti per le prestazioni per i carichi di lavoro di rete](../../networking/technologies/network-subsystem/net-sub-performance-tools.md)
#### [Gruppo NIC per le prestazioni di rete](../../networking/technologies/nic-teaming/NIC-Teaming.md)
#### [Gruppo NIC nelle macchine virtuali](../../networking/technologies/nic-teaming/nict-vms.md)
#### [Gruppo NIC e reti locali virtuali (VLAN)](../../networking/technologies/nic-teaming/nict-and-vlans.md)
#### [Uso e gestione dell'indirizzo MAC del gruppo NIC](../../networking/technologies/nic-teaming/NIC-Teaming-MAC-address-Use-and-Management.md)
#### [Creare un nuovo gruppo NIC in una macchina virtuale o un computer host](../../networking/technologies/nic-teaming/create-a-New-NIC-Team-on-a-Host-computer-or-VM.md)
#### [Creare un nuovo gruppo NIC](../../networking/technologies/nic-teaming/create-a-New-NIC-Team.md)
#### [Creare un nuovo gruppo NIC in una macchina virtuale](../../networking/technologies/nic-teaming/create-a-New-NIC-Team-in-a-VM.md)
#### [Risoluzione dei problemi del gruppo NIC](../../networking/technologies/nic-teaming/Troubleshooting-NIC-Teaming.md)
### [Ottimizzazione di SDN (Software Defined Networking)](subsystem/software-defined-networking/index.md)
#### [Prestazioni del gateway HNV](subsystem/software-defined-networking/hnv-gateway-performance.md)
#### [Prestazioni del gateway SLB](subsystem/software-defined-networking/slb-gateway-performance.md)
### Ottimizzazione del sottosistema di archiviazione
#### [Spazi di archiviazione diretta](subsystem/storage-spaces-direct/index.md)
#### [Domande frequenti sulla replica di archiviazione](../../storage/storage-replica/storage-replica-frequently-asked-questions.md)
#### [Impostazioni avanzate della deduplicazione dati](../../storage/data-deduplication/advanced-settings.md)
## [Ottimizzazione di PowerShell](powershell/index.md)
### [Considerazioni sulla creazione di script](powershell/script-authoring-considerations.md)
### [Considerazioni sulla creazione di moduli](powershell/module-authoring-considerations.md)
## [Risorse aggiuntive di ottimizzazione](additional-resources.md)
