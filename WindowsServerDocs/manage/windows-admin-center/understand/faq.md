---
title: Domande frequenti su Windows Admin Center
description: Ottenere risposte su Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 5c306dd181d4db400e6ab5bab919399fdebca9f3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811671"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Domande frequenti su Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center anteprima

Di seguito sono riportate le risposte alle domande più frequenti su Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center?

Windows Admin Center è una piattaforma GUI semplice e basata su browser e un set di strumenti per gli amministratori IT che consente di gestire Windows Server e Windows 10. È l'evoluzione dei familiari strumenti di amministrazione preinstallati, ad esempio Server Manager e Microsoft Management Console (MMC) in un'esperienza modernizzata, semplificata, integrata e sicura.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>Posso utilizzare Windows Admin Center in ambienti di produzione?

Sì. Windows Admin Center è disponibile a livello globale e pronto per ampie distribuzioni di produzione e utilizzo. La funzionalità della piattaforma corrente e strumenti di base di soddisfano i criteri di rilascio standard di Microsoft e l'indicatore di qualità per l'usabilità, affidabilità, prestazioni, accessibilità, sicurezza e l'adozione.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto costa utilizzare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows. Puoi utilizzare Windows Admin Center (disponibile come download distinto) con le licenze valide di Windows Server o Windows 10 senza alcun costo aggiuntivo: viene concesso in licenza nel quadro di condizioni di licenza integrative di Windows.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>Quali versioni di Windows Server posso gestire con Windows Admin Center?

Windows Admin Center è ottimizzato per Windows Server 2019 abilitare i temi principali nella versione di Windows Server 2019: hybrid cloud in particolare gli scenari e la gestione dell'infrastruttura iperconvergente. Sebbene Windows Admin Center funziona meglio con Windows Server 2019, supporta la gestione di un'ampia gamma di versioni che i clienti usano già: Windows Server 2012 e versioni successive sono completamente supportati. È inoltre disponibile una funzionalità limitata per la gestione di Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center sostituisce completamente tutti i tradizionali strumenti di amministrazione remota del server e preinstallati?

No. Sebbene Windows Admin Center possa gestire molti scenari comuni, non sostituisce completamente tutti gli strumenti di Microsoft Management Console (MMC) tradizionali. Per un approfondimento su quali strumenti sono inclusi in Windows Admin Center, altre informazioni, vedere [la gestione dei server](../use/manage-servers.md) nella documentazione. Windows Admin Center include le seguenti funzionalità chiave nella sua soluzione Server Manager:

* Visualizzazione e utilizzo delle risorse
* Gestione dei certificati
* Gestione di dispositivi
* Visualizzatore eventi
* Esplora file
* Gestione firewall
* La gestione delle App installate
* Configurazione di utenti e gruppi locali
* Impostazioni di rete
* Visualizzazione/completamento di processi e creazione di dump di processo
* Modifica del Registro di sistema
* La gestione delle attività pianificate
* Gestione dei servizi Windows
* Attivazione/disattivazione ruoli e funzionalità
* Gestione delle macchine virtuali Hyper-V e commutatori virtuale
* Gestione dell'archiviazione
* Gestione di Replica di archiviazione
* Gestione degli aggiornamenti di Windows
* Console di PowerShell
* Connessione Desktop remoto

Windows Admin Center fornisce inoltre queste soluzioni:

* Gestione computer – è disponibile un sottoinsieme delle funzionalità Server Manager per la gestione di PC client Windows 10
* Gestione cluster di failover – fornisce il supporto per la gestione continuativa dei cluster di failover e le risorse del cluster
* Gestione di cluster iperconvergenti-offre un'esperienza completamente nuova personalizzata per Spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio.

Windows Admin Center integra e non sostituisce Strumenti di amministrazione remota del server dal momento che ruoli come Active Directory, DHCP, DNS, IIS non hanno ancora funzionalità di gestione equivalenti visibili in Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>Windows Admin Center può essere utilizzato per gestire il server Microsoft Hyper-V gratuito?

Sì. Windows Admin Center può essere utilizzato per gestire Microsoft Hyper-V Server 2016 e Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>Posso distribuire Windows Admin Center su un computer Windows 10?

Sì, Windows Admin Center può essere installato in Windows 10 (versione 1709 o successiva), in esecuzione in modalità desktop.  Windows Admin Center può anche essere installato in un server con Windows Server 2016 o versione successiva in modalità gateway e quindi si accede tramite un web browser da un computer Windows 10. [Ulteriori informazioni sulle opzioni di installazione](../plan/installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>Ho sentito dire che Windows Admin Center viene usato PowerShell dietro le quinte, è possibile visualizzare gli script effettivi usato?

Sì! il [Showscript funzionalità](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center) è stata aggiunta in Windows Admin Center anteprima 1806 ed è ora incluso nel canale a livello generale.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Si prevede che in futuro Windows Admin Center possa gestire Windows Server 2008 R2 o versioni precedenti?

Supporta ora Windows Admin Center **limitato** funzionalità per gestire Windows Server 2008 R2. Windows Admin Center si basa sulle tecnologie di piattaforma e le funzionalità di PowerShell escluse da Windows Server 2008 R2 e versioni precedenti, rendendo impossibile il supporto completo. Windows Server 2008 o 2008 R2 si stanno avvicinando fine del supporto in gennaio 2020 pertanto si consiglia ai clienti [lo spostamento in Azure o l'aggiornamento alla versione più recente di Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>Si prevede che Windows Admin Center possa gestire in futuro le connessioni Linux?

Stiamo analizzando a causa di richieste dei clienti, ma attualmente non è prevista bloccato per offrire e supporto può essere costituito solo da una connessione alla console tramite SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Quali Web browser sono supportati da Windows Admin Center?

Le versioni più recenti di Microsoft Edge (Windows 10, versione 1709 o versioni successive) e Google Chrome sono testate e supportate in Windows 10. [Visualizzazione specifiche del browser problemi noti](../support/known-issues.md#browser-specific-issues). Altri browser moderni web o altre piattaforme non fanno attualmente parte del nostro matrice di test e non sono pertanto *ufficialmente* supportato.

## <a name="how-does-windows-admin-center-handle-security"></a>Come gestisce la sicurezza Windows Admin Center?

Il traffico dal browser al gateway di Windows Admin Center utilizza il protocollo HTTPS. Il traffico dal gateway ai server gestiti è PowerShell e WMI standard su WinRM. È supportata la LAPS (Soluzione password dell'amministratore locale), la delega vincolata basata sulle risorse, il controllo dell'accesso gateway con AD o Azure AD e il controllo dell'accesso basato sui ruoli per la gestione dei server di destinazione.

## <a name="does-windows-admin-center-use-credssp"></a>Windows Admin Center Usa CredSSP?

Sì, in alcuni casi Windows Admin Center richiede CredSSP. Ciò è necessario passare le credenziali di autenticazione per computer oltre a specifici server di destinazione per la gestione. Ad esempio, se si siano gestendo le macchine virtuali sul **server B**, ma si desidera archiviare i file vhdx per tali macchine virtuali in una condivisione file ospitata da **server C**, Windows Admin Center deve usare CredSSP per eseguire l'autenticazione con **server C** per accedere alla condivisione file.

Windows Admin Center gestisce la configurazione di CredSSP automaticamente dopo la richiesta di consenso da parte dell'utente. Prima di configurare CredSSP, Windows Admin Center controllerà per assicurarsi che il sistema abbia il recente CredSSP [aggiornamenti](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Anche se CredSSP è stato abilitato, esisterà una notifica utente visuale nella panoramica del Server e un'opzione per disabilitarla,

![CredSSP nella panoramica di server](../media/CredSSP-overview.png)

CredSSP è attualmente in uso nelle aree seguenti:

- Usando disaggregata archiviazione SMB nello strumento di macchine virtuali (l'esempio precedente).
- Usando gli aggiornamenti degli strumenti nel cluster Iperconvergente o Failover soluzioni di gestione, che consente di eseguire [aggiornamento compatibile con Cluster](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## <a name="are-there-any-cloud-dependencies"></a>Esistono eventuali dipendenze cloud?

Windows Admin Center non richiede l'accesso a internet e non richiede Microsoft Azure. Windows Admin Center gestisce le istanze di Windows e Windows Server ovunque: nei sistemi fisici o in macchine virtuali su qualsiasi hypervisor o in esecuzione in qualsiasi cloud. Sebbene l'integrazione con vari servizi di Azure verrà aggiunta nel tempo, si tratterà di funzionalità a valore aggiunto facoltative e non di un requisito per utilizzare Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>Esistono altre dipendenze o prerequisiti?

Windows Admin Center può essere installato su Windows 10 Fall Anniversary Update (1709) o versione successiva o Windows Server 2016 o versione successiva. Per gestire Windows Server 2008 R2, 2012 o 2012 R2, è obbligatorio installare Windows Management Framework 5.1 su tali server. Non vi sono altre dipendenze. IIS non è obbligatorio, gli agenti non sono obbligatori, SQL Server non è obbligatorio.

## <a name="what-about-extensibility-and-3rd-party-support"></a>Quali sono i dettagli su estendibilità e supporto tecnico di terze parti?

Windows Admin Center dispone di un SDK disponibili in modo che chiunque può scrivere le proprie estensioni. In quanto piattaforma, la crescita dell'ecosistema e l'abilitazione all'estendibilità del partner è stata una priorità chiave fin dall'inizio. [Ulteriori informazioni su Windows Admin Center SDK](../extend/extensibility-overview.md).

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Posso gestire l'infrastruttura iperconvergente con Windows Admin Center?

Sì. Windows Admin Center supporta la gestione dei cluster iperconvergente che esegue Windows Server 2016 o Windows Server 2019. La soluzione di gestione di cluster iperconvergente in Windows Admin Center in precedenza era disponibile in anteprima, ma è ora **disponibile a livello generale**, con alcune nuove funzionalità in anteprima. Per ulteriori informazioni, [leggi altri dettagli sulla gestione dell'infrastruttura convergente](../use/manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center richiede System Center?

No. Windows Admin Center è un'integrazione di System Center, ma System Center non è obbligatorio. [Altre informazioni su Windows Admin Center e System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center può sostituire System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center e SCVMM sono complementari; Windows Admin Center è progettato per sostituire i tradizionali snap in Microsoft Management Console (MMC) e l'esperienza di amministrazione server.  Windows Admin Center non è progettato per sostituire gli aspetti di monitoraggio di SCVMM. [Altre informazioni su Windows Admin Center e System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>Che cos'è Anteprima di Windows Admin Center, quale versione è più adatta per me?

Esistono due versioni di Windows Admin Center disponibili per il download:

### <a name="windows-admin-center"></a>Windows Admin Center

* Per gli amministratori IT che non sono in grado di aggiornare frequentemente o che desiderano più tempo di convalida per le versioni che utilizzano nell'ambiente di produzione, questa è la versione adatta. La release (GA) disponibile a livello generale corrente è Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Per ottenere la versione più recente [scaricare qui](https://aka.ms/WACDownload).

### <a name="windows-admin-center-preview"></a>Anteprima di Windows Admin Center

> [!NOTE]
> La versione corrente di disponibilità generale (Windows Admin Center 1904) contiene tutte le funzionalità di anteprima precedente.
> Restituisce l'Insider Preview nei prossimi mesi.

* Per gli amministratori IT che desiderano ottenere le funzionalità più recenti e importanti a cadenza regolare, questa è la versione più adatta. Il nostro obiettivo consiste nel fornire l'aggiornamento successivo rilascia ogni mese scorso. La piattaforma di base continua a essere pronta per la produzione e la licenza fornisce diritti di utilizzo di produzione. Tuttavia, verranno continuamente introdotti nuovi strumenti e funzionalità chiaramente contrassegnati come ANTEPRIMA e adatti per la valutazione e i test.
* Per ottenere la versione Insider Preview più recente, Insider registrati possono scaricare Windows Admin Center anteprima direttamente dai [pagina di download di Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sotto l'elenco a discesa download aggiuntivi. Se non hai ancora effettuato la registrazione come partecipante al programma Insider, vedi [Introduzione a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) nel portale dei partecipanti al Programma Windows Insider per le aziende.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>Perché è stato scelto "Windows Admin Center" come nome finale per "Project Honolulu"?

Windows Admin Center è il nome ufficiale del prodotto per "Project Honolulu" e rafforza la visione di un'esperienza integrata per gli amministratori IT su un'ampia gamma di scenari di gestione e amministrativi di base. Evidenza inoltre in che modo l'attenzione dei nostri clienti sulle esigenze dell'utente amministratore IT sia centrale per definire la modalità di investimento e l'offerta.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>Dove posso trovare altre informazioni su Windows Admin Center o maggiori dettagli sugli argomenti illustrati sopra?

La [pagina di avvio](https://aka.ms/WindowsAdminCenter) è il punto di partenza migliore e include i collegamenti a nuovo contenuto di documentazione suddiviso per categoria, percorso di download, istruzioni per fornire feedback, informazioni di riferimento e altre risorse.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>Che cos'è la cronologia delle versioni di Windows Admin Center?

[Consente di visualizzare la cronologia delle versioni.](../overview.md#release-history)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Sto avendo un problema con Windows Admin Center, dove posso ottenere assistenza?

Vedi la [Guida alla risoluzione dei problemi](../use/troubleshooting.md) e l'elenco di [problemi noti](../use/known-issues.md).
