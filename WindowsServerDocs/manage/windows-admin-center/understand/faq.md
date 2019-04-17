---
title: Domande frequenti su Windows Admin Center
description: Ottenere risposte su Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 04/12/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 2f1591a32147e3c11ba635f1a11d36b7f38a3470
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296743"
---
# Domande frequenti su Windows Admin Center

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

Di seguito sono riportate le risposte alle domande più frequenti su Windows Admin Center.

## Che cos'è Windows Admin Center?

Windows Admin Center è una piattaforma GUI semplice e basata su browser e un set di strumenti per gli amministratori IT che consente di gestire Windows Server e Windows 10. È l'evoluzione dei familiari strumenti di amministrazione preinstallati, ad esempio Server Manager e Microsoft Management Console (MMC) in un'esperienza modernizzata, semplificata, integrata e sicura.

## Posso utilizzare Windows Admin Center in ambienti di produzione?

Sì. Windows Admin Center è disponibile a livello globale e pronto per ampie distribuzioni di produzione e utilizzo. La funzionalità della piattaforma corrente e gli strumenti principali soddisfano i criteri di rilascio standard di Microsoft e il livello di qualità per usabilità, affidabilità, prestazioni, accessibilità, sicurezza e adozione.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## Quanto costa utilizzare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows. Puoi utilizzare Windows Admin Center (disponibile come download distinto) con le licenze valide di Windows Server o Windows 10 senza alcun costo aggiuntivo: viene concesso in licenza nel quadro di condizioni di licenza integrative di Windows.

## Quali versioni di Windows Server posso gestire con Windows Admin Center?

Windows Admin Center è ottimizzato per Windows Server 2019 abilitare il rilascio di Windows Server 2019 i temi chiave: cloud ibrido scenari e gestione dell'infrastruttura iperconvergente in particolare. Sebbene Windows Admin Center funzioni in modo ottimale con Windows Server 2019, supporta la gestione di varie versioni che i clienti già utilizzano: Windows Server 2012 e versioni successiva sono completamente supportati. Esiste anche la funzionalità limitate per la gestione di Windows Server 2008 R2.

## Windows Admin Center sostituisce completamente tutti i tradizionali strumenti di amministrazione remota del server e preinstallati?

No. Sebbene Windows Admin Center possa gestire molti scenari comuni, non sostituisce completamente tutti gli strumenti di Microsoft Management Console (MMC) tradizionali. Per un'analisi dettagliata in quali strumenti sono incluse in Windows Admin Center, Leggi altre informazioni sulla [gestione dei server](..\use\manage-servers.md) nella nostra documentazione. Windows Admin Center include le seguenti funzionalità chiave nella sua soluzione Server Manager:

* Visualizzazione e utilizzo delle risorse
* Gestione dei certificati
* Gestione di dispositivi
* Visualizzatore eventi
* Esplora file
* Gestione dei firewall
* Gestire le app installate
* Configurazione di utenti e gruppi locali
* Impostazioni di rete
* Visualizzazione/completamento di processi e creazione di dump di processo
* Modifica del Registro di sistema
* La gestione di attività pianificate
* Gestione dei servizi Windows
* Attivazione/disattivazione ruoli e funzionalità
* Gestione delle macchine virtuali Hyper-V e commutatori virtuale
* Gestione dell'archiviazione
* La gestione di Replica di archiviazione
* Gestione degli aggiornamenti di Windows
* Console di PowerShell
* Connessione Desktop remoto

Windows Admin Center fornisce inoltre queste soluzioni:

* Gestione computer – è disponibile un sottoinsieme delle funzionalità Server Manager per la gestione di PC client Windows 10
* Gestione cluster di failover – fornisce il supporto per la gestione continuativa dei cluster di failover e le risorse del cluster
* Gestione di cluster iperconvergenti-offre un'esperienza completamente nuova personalizzata per Spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio.

Windows Admin Center integra e non sostituisce Strumenti di amministrazione remota del server dal momento che ruoli come Active Directory, DHCP, DNS, IIS non hanno ancora funzionalità di gestione equivalenti visibili in Windows Admin Center.

## Windows Admin Center può essere utilizzato per gestire il server Microsoft Hyper-V gratuito?

Sì. Windows Admin Center può essere utilizzato per gestire Microsoft Hyper-V Server 2016 e Microsoft Hyper-V Server 2012 R2.

## Posso distribuire Windows Admin Center su un computer Windows 10?

Sì, Windows Admin Center può essere installato in Windows 10 (versione 1709 o successiva), in esecuzione in modalità desktop.  Windows Admin Center può anche essere installato su un server con Windows Server 2016 o versione successiva in modalità gateway e quindi accessibile mediante un web browser da un computer Windows 10. [Ulteriori informazioni sulle opzioni di installazione](..\plan\installation-options.md).

## Ho sentito che Windows Admin Center Usa PowerShell dietro le quinte, è possibile vedere gli script effettivi che usa?

Sì! la [funzionalità Showscript](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center) sono state aggiunte in Windows Admin Center Preview 1806 ed è ora incluso al canale GA.

## Si prevede che in futuro Windows Admin Center possa gestire Windows Server 2008 R2 o versioni precedenti?

Windows Admin Center supporta ora funzionalità **limitate** per gestire Windows Server 2008 R2. Windows Admin Center si basa sulle tecnologie di piattaforma e le funzionalità di PowerShell escluse da Windows Server 2008 R2 e versioni precedenti, rendendo impossibile il supporto completo. Windows Server 2008/2008 R2 si avvicina la fine del supporto gennaio 2020, pertanto Microsoft consiglia ai clienti [passare ad Azure o eseguire l'aggiornamento all'ultima versione di Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## Si prevede che Windows Admin Center possa gestire in futuro le connessioni Linux?

Stiamo esaminando a causa di richieste dei clienti, ma non esiste attualmente alcun piano stabilito e il supporto potrebbe consistere solo di una connessione alla console su SSH.

## Quali Web browser sono supportati da Windows Admin Center?

Le versioni più recenti di Microsoft Edge (Windows 10, versione 1709 o versioni successive) e Google Chrome sono testate e supportate in Windows 10. [Problemi noti di visualizzazione del browser specifici](..\support\known-issues.md#browser-specific-issues). Altri web browser moderni o altre piattaforme non fanno parte della nostra matrice per i test e sono pertanto non *ufficialmente* supportati.

## Come gestisce la sicurezza Windows Admin Center?

Il traffico dal browser al gateway di Windows Admin Center utilizza il protocollo HTTPS. Il traffico dal gateway ai server gestiti è PowerShell e WMI standard su WinRM. È supportata la LAPS (Soluzione password dell'amministratore locale), la delega vincolata basata sulle risorse, il controllo dell'accesso gateway con AD o Azure AD e il controllo dell'accesso basato sui ruoli per la gestione dei server di destinazione.

## Windows Admin Center utilizza CredSSP?

Sì, in alcuni casi Windows Admin Center richiede CredSSP. Ciò è necessario passare le credenziali per l'autenticazione in computer oltre il server specifico di destinazione per la gestione. Ad esempio, se si siano gestendo le macchine virtuali nel **server B**, ma vuoi archiviare i file vhdx per le macchine virtuali in una condivisione file **server C**ospitata, Windows Admin Center deve usare CredSSP per l'autenticazione **server C** per accedere al condivisione file.

Windows Admin Center gestisce la configurazione di CredSSP automaticamente dopo la richiesta di consenso da te. Prima di configurare CredSSP, Windows Admin Center verificherà per assicurarti che il sistema dispone di CredSSP recenti [aggiornamenti](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Mentre CredSSP è abilitato, sarà presente una notifica sulla panoramica di Server e un'opzione per disabilitarla-

![CredSSP nella panoramica di server](../media/CredSSP-overview.png)

CredSSP è attualmente usato nelle aree seguenti:

- Usando disaggregati archiviazione SMB nello strumento di macchine virtuali (l'esempio precedente).
- Con gli aggiornamenti dello strumento nel cluster Iperconvergente o Failover soluzioni di gestione, che esegue [L'aggiornamento compatibile con Cluster](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## Esistono eventuali dipendenze cloud?

Windows Admin Center non richiede l'accesso a internet e non richiede Microsoft Azure. Windows Admin Center gestisce le istanze di Windows e Windows Server ovunque: nei sistemi fisici o in macchine virtuali su qualsiasi hypervisor o in esecuzione in qualsiasi cloud. Sebbene l'integrazione con vari servizi di Azure verrà aggiunta nel tempo, si tratterà di funzionalità a valore aggiunto facoltative e non di un requisito per utilizzare Windows Admin Center.

## Esistono altre dipendenze o prerequisiti?

Windows Admin Center può essere installato su Windows 10 Fall Anniversary Update (1709) o versione successiva o Windows Server 2016 o versione successiva. Per gestire Windows Server 2008 R2, 2012 o 2012 R2, è obbligatorio installare Windows Management Framework 5.1 su tali server. Non vi sono altre dipendenze. IIS non è obbligatorio, gli agenti non sono obbligatori, SQL Server non è obbligatorio.

## Quali sono i dettagli su estendibilità e supporto tecnico di terze parti?

Windows Admin Center ha un SDK disponibile in modo che tutti gli utenti possono scrivere i propri estensione. In quanto piattaforma, la crescita dell'ecosistema e l'abilitazione all'estendibilità del partner è stata una priorità chiave fin dall'inizio. [Ulteriori informazioni su Windows Admin Center SDK](..\extend\extensibility-overview.md).

## Posso gestire l'infrastruttura iperconvergente con Windows Admin Center?

Sì. Windows Admin Center supporta la gestione di cluster iperconvergenti che eseguono Windows Server 2016 o Windows Server 2019. La soluzione di gestione di cluster iperconvergenti in Windows Admin Center è stato in precedenza nell'anteprima ma ora è **disponibile a livello generale**, con alcune nuove funzionalità in anteprima. Per ulteriori informazioni, [leggi altri dettagli sulla gestione dell'infrastruttura convergente](..\use\manage-hyper-converged.md).

## Windows Admin Center richiede System Center?

No. Windows Admin Center è un'integrazione di System Center, ma System Center non è obbligatorio. [Leggi altre informazioni sul Windows Admin Center e System Center](related-management.md#system-center).

## Windows Admin Center può sostituire System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center e SCVMM sono complementari; Windows Admin Center è progettato per sostituire i tradizionali snap in Microsoft Management Console (MMC) e l'esperienza di amministrazione server.  Windows Admin Center non è progettato per sostituire gli aspetti di monitoraggio di SCVMM. [Leggi altre informazioni sul Windows Admin Center e System Center](related-management.md#system-center).

## Che cos'è Anteprima di Windows Admin Center, quale versione è più adatta per me?

Esistono due versioni di Windows Admin Center disponibili per il download:

### Windows Admin Center

* Per gli amministratori IT che non sono in grado di aggiornare frequentemente o che desiderano più tempo di convalida per le versioni che utilizzano nell'ambiente di produzione, questa è la versione adatta. La versione (GA) in genere disponibile corrente è Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Per ottenere la versione più recente, [il download qui](https://aka.ms/WACDownload).

### Anteprima di Windows Admin Center

>[!NOTE]
>La versione GA corrente (Windows Admin Center 1904) contiene tutte le funzionalità di anteprima precedente.
>Insider Preview restituirà nei prossimi mesi.

* Per gli amministratori IT che desiderano ottenere le funzionalità più recenti e importanti a cadenza regolare, questa è la versione più adatta. Il nostro scopo è di fornire successive versioni di aggiornamenti ogni mese o quasi. La piattaforma di base continua a essere pronta per la produzione e la licenza fornisce diritti di utilizzo di produzione. Tuttavia, verranno continuamente introdotti nuovi strumenti e funzionalità chiaramente contrassegnati come ANTEPRIMA e adatti per la valutazione e i test.
* Per ottenere la versione più recente di Insider Preview, partecipanti al programma Insider registrati possono scaricare Windows Admin Center Preview direttamente dalla [Pagina di download di Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sotto l'elenco a discesa download aggiuntivi. Se non hai ancora effettuato la registrazione come partecipante al programma Insider, vedi [Introduzione a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) nel portale dei partecipanti al Programma Windows Insider per le aziende.

## Perché è stato scelto "Windows Admin Center" come nome finale per "Project Honolulu"?

Windows Admin Center è il nome ufficiale del prodotto per "Project Honolulu" e rafforza la visione di un'esperienza integrata per gli amministratori IT su un'ampia gamma di scenari di gestione e amministrativi di base. Evidenza inoltre in che modo l'attenzione dei nostri clienti sulle esigenze dell'utente amministratore IT sia centrale per definire la modalità di investimento e l'offerta.

## Dove posso trovare altre informazioni su Windows Admin Center o maggiori dettagli sugli argomenti illustrati sopra?

La [pagina di avvio](https://aka.ms/WindowsAdminCenter) è il punto di partenza migliore e include i collegamenti a nuovo contenuto di documentazione suddiviso per categoria, percorso di download, istruzioni per fornire feedback, informazioni di riferimento e altre risorse.

## Che cos'è la cronologia delle versioni di Windows Admin Center?

[Visualizzare la cronologia delle versioni.](..\overview.md#release-history)

## Sto avendo un problema con Windows Admin Center, dove posso ottenere assistenza?

Vedi la [Guida alla risoluzione dei problemi](..\use\troubleshooting.md) e l'elenco di [problemi noti](..\use\known-issues.md).
