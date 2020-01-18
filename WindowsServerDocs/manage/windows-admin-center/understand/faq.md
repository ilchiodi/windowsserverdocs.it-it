---
title: Domande frequenti su Windows Admin Center
description: Ottenere risposte su Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 12/02/2019
ms.prod: windows-server
ms.openlocfilehash: 4ce42420430e9a12dd6123ec18c9ded25abc97bb
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949957"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Domande frequenti su Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Di seguito sono riportate le risposte alle domande più frequenti su Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center?

Windows Admin Center è una piattaforma GUI semplice basata su browser e un set di strumenti per amministratori IT che permette di gestire Windows Server e Windows 10. È l'evoluzione di familiari strumenti di amministrazione preinstallati, ad esempio Server Manager e Microsoft Management Console (MMC), in un'esperienza più moderna, semplice, integrata e sicura.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>Posso usare Windows Admin Center in ambienti di produzione?

Sì. Windows Admin Center è disponibile a livello globale e pronto per distribuzioni di produzione e utilizzo su vasta scala. Le funzionalità e gli strumenti di base della piattaforma corrente soddisfano i criteri di versione standard di Microsoft e il livello di qualità per usabilità, affidabilità, prestazioni, accessibilità, sicurezza e adozione.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto costa usare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows. Puoi usare Windows Admin Center (disponibile come download separato) con licenze valide di Windows Server o Windows 10 senza alcun costo aggiuntivo: viene concesso in licenza nel quadro di condizioni di licenza integrative di Windows.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>Quali versioni di Windows Server posso gestire con Windows Admin Center?

Windows Admin Center è ottimizzato per Windows Server 2019 per abilitare i temi chiave nella versione di Windows Server 2019: in particolare, scenari di cloud ibrido e gestione dell'infrastruttura iperconvergente. Sebbene Windows Admin Center funzioni in modo ottimale con Windows Server 2019, supporta la gestione di varie versioni che i clienti già usano: Windows Server 2012 e versioni successive sono completamente supportati. È inoltre disponibile una funzionalità limitata per la gestione di Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center sostituisce completamente tutti i tradizionali strumenti preinstallati e gli strumenti di amministrazione remota del server?

No. Sebbene Windows Admin Center possa gestire molti scenari comuni, non sostituisce completamente tutti gli strumenti di Microsoft Management Console (MMC) tradizionali. Per un'analisi dettagliata degli strumenti inclusi in Windows Admin Center, leggi altre informazioni sulla [gestione dei server](../use/manage-servers.md) nella documentazione. Windows Admin Center include le seguenti funzionalità chiave nella soluzione Server Manager:

* Visualizzazione e uso delle risorse
* Gestione dei certificati
* Gestione dei dispositivi
* Visualizzatore eventi
* Esplora file
* Gestione firewall
* Gestione delle app installate
* Configurazione di utenti e gruppi locali
* Impostazioni di rete
* Visualizzazione/completamento di processi e creazione di dump di processo
* Modifica del Registro di sistema
* Gestione delle attività pianificate
* Gestione dei servizi Windows
* Attivazione/disattivazione di ruoli e funzionalità
* Gestione di macchine virtuali e commutatori virtuali Hyper-V
* Gestione dell'archiviazione
* Gestione della replica di archiviazione
* Gestione degli aggiornamenti di Windows
* Console di PowerShell
* Connessione Desktop remoto

Windows Admin Center offre inoltre queste soluzioni:

* Gestione computer: fornisce un sottoinsieme delle funzionalità Server Manager per la gestione di PC client Windows 10.
* Gestione cluster di failover: fornisce il supporto per la gestione continuativa dei cluster di failover e le risorse dei cluster.
* Gestione di cluster iperconvergenti: offre un'esperienza completamente nuova personalizzata per Spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio.

Windows Admin Center integra e non sostituisce Strumenti di amministrazione remota del server dal momento che ruoli come Active Directory, DHCP, DNS, IIS non hanno ancora funzionalità di gestione equivalenti visibili in Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>Windows Admin Center può essere usato per gestire il server Microsoft Hyper-V gratuito?

Sì. Windows Admin Center può essere usato per gestire Microsoft Hyper-V Server 2016 e Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>Posso distribuire Windows Admin Center su un computer Windows 10?

Sì. Windows Admin Center può essere installato in Windows 10 (versione 1709 o successive), in esecuzione in modalità desktop.  Windows Admin Center può anche essere installato in un server che esegue Windows Server 2016 o versioni successive in modalità gateway e quindi è possibile accedervi mediante un Web browser da un computer Windows 10. [Altre informazioni sulle opzioni di installazione](../plan/installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>Ho saputo che Windows Admin Center usa PowerShell dietro le quinte. Posso visualizzare gli script effettivi usati?

Sì. La [funzionalità di visualizzazione degli script](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center) è stata aggiunta in Windows Admin Center Preview 1806 e ora è inclusa nel canale di disponibilità a livello generale.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Si prevede che in futuro Windows Admin Center possa gestire Windows Server 2008 R2 o versioni precedenti?

Windows Admin Center supporta ora funzionalità **limitate** per la gestione di Windows Server 2008 R2. Windows Admin Center si basa su tecnologie di piattaforma e funzionalità di PowerShell non presenti in Windows Server 2008 R2 e versioni precedenti, rendendo impossibile il supporto completo. Si avvicina la fine del supporto per Windows Server 2008/2008 R2 a gennaio 2020, pertanto Microsoft consiglia ai clienti di [passare ad Azure o effettuare l'aggiornamento alla versione di Windows Server più recente](https://www.microsoft.com/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>Si prevede che Windows Admin Center possa gestire in futuro connessioni Linux?

Questa possibilità è in fase di studio in virtù delle richieste dei clienti, ma attualmente non è disponibile alcun piano stabilito e il supporto potrebbe consistere solo in una connessione alla console su SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Quali Web browser sono supportati da Windows Admin Center?

Le versioni più recenti di Microsoft Edge (Windows 10, versione 1709 o successive), Google Chrome e [Microsoft Edge Insider](https://microsoftedgeinsider.com) sono testate e supportate in Windows 10. [Visualizza i problemi specifici dei browser](../support/known-issues.md#browser-specific-issues). Altri Web browser moderni o altre piattaforme non fanno parte attualmente della nostra matrice per i test e pertanto non sono *ufficialmente* supportati.

## <a name="how-does-windows-admin-center-handle-security"></a>In che modo Windows Admin Center gestisce la sicurezza?

Il traffico dal browser al gateway di Windows Admin Center usa il protocollo HTTPS. Il traffico dal gateway ai server gestiti è PowerShell e WMI standard su WinRM. Supportiamo LAPS (Soluzione password dell'amministratore locale), la delega vincolata basata sulle risorse, il controllo dell'accesso gateway con AD o Azure AD e il controllo degli accessi in base al ruolo per la gestione dei server di destinazione.

## <a name="does-windows-admin-center-use-credssp"></a>Windows Admin Center usa CredSSP?

Sì, in alcuni casi Windows Admin Center richiede CredSSP. È necessario per passare le credenziali di autenticazione a computer esterni al server specifico di destinazione della gestione. Se ad esempio gestisci le macchine virtuali sul **server B**, ma vuoi archiviare i file vhdx per tali macchine virtuali in una condivisione file ospitata dal **server C**, Windows Admin Center deve usare CredSSP per eseguire l'autenticazione nel **server C** e accedere alla condivisione file.

Windows Admin Center gestisce la configurazione di CredSSP automaticamente dopo aver richiesto l'autorizzazione dell'utente. Prima di configurare CredSSP, Windows Admin Center controllerà che nel sistema siano disponibili [aggiornamenti](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018) recenti di CredSSP. Quando è abilitato CredSSP, è presente una notifica utente nella panoramica del server e un'opzione per disabilitare il protocollo.

![CredSSP nella panoramica del server](../media/CredSSP-overview.png)

CredSSP è attualmente usato nelle aree seguenti:

- Se nello strumento di macchine virtuali si usa l'archiviazione SMB disaggregata (esempio precedente)
- Se nelle soluzioni di gestione di cluster di failover o iperconvergenti si usa lo strumento per gli aggiornamenti, che esegue l'[aggiornamento compatibile con cluster](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## <a name="are-there-any-cloud-dependencies"></a>Esistono eventuali dipendenze cloud?

Windows Admin Center non richiede l'accesso a Internet e non richiede Microsoft Azure. Windows Admin Center gestisce le istanze di Windows e Windows Server ovunque: in sistemi fisici o in macchine virtuali su qualsiasi hypervisor oppure in esecuzione in un cloud. Anche se l'integrazione con vari servizi di Azure verrà aggiunta nel tempo, si tratterà di funzionalità a valore aggiunto facoltative e non di un requisito per usare Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>Esistono altre dipendenze o prerequisiti?

Windows Admin Center può essere installato in Windows 10 Fall Anniversary Update (1709) o versioni successive oppure Windows Server 2016 o versioni successive. Per gestire Windows Server 2008 R2, 2012 o 2012 R2, è necessario installare Windows Management Framework 5.1 in tali server. Non vi sono altre dipendenze. IIS, gli agenti e SQL Server non sono obbligatori.

## <a name="what-about-extensibility-and-3rd-party-support"></a>Quali sono i dettagli su estendibilità e supporto tecnico di terze parti?

Windows Admin Center dispone di un SDK in modo che chiunque possa scrivere le proprie estensioni. In quanto piattaforma, la crescita dell'ecosistema e l'abilitazione dell'estendibilità del partner sono state priorità chiave fin dall'inizio. [Altre informazioni su Windows Admin Center SDK](../extend/extensibility-overview.md).

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Posso gestire l'infrastruttura iperconvergente con Windows Admin Center?

Sì. Windows Admin Center supporta la gestione di cluster iperconvergenti che eseguono Windows Server 2016 o Windows Server 2019. La soluzione di gestione dei cluster iperconvergenti in Windows Admin Center era in precedenza disponibile in anteprima, ma è ora **disponibile a livello generale**, con alcune nuove funzionalità in anteprima. Per altre informazioni, [leggi altri dettagli sulla gestione dell'infrastruttura iperconvergente](../use/manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center richiede System Center?

No. Windows Admin Center è un'integrazione di System Center, ma System Center non è obbligatorio. [Altre informazioni su Windows Admin Center e System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center può sostituire System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center e SCVMM sono complementari. Windows Admin Center è progettato per sostituire i tradizionali snap-in di Microsoft Management Console (MMC) e l'esperienza di amministrazione server.  Windows Admin Center non è progettato per sostituire gli aspetti di monitoraggio di SCVMM. [Altre informazioni su Windows Admin Center e System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>Che cos'è Windows Admin Center Preview? Quale versione è più adatta per me?

Esistono due versioni di Windows Admin Center disponibili per il download:

### <a name="windows-admin-center"></a>Windows Admin Center

* Questa versione è adatta per gli amministratori IT che non sono in grado di eseguire aggiornamenti frequenti o che vogliono più tempo di convalida per le versioni che usano nell'ambiente di produzione. La versione corrente disponibile a livello generale è Windows Admin Center 1910.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Per ottenere la versione più recente, [scaricala qui](https://aka.ms/WACDownload).

### <a name="windows-admin-center-preview"></a>Windows Admin Center Preview

* Questa è la versione adatta per gli amministratori IT che vogliono ottenere le funzionalità più recenti e importanti a cadenza regolare. Il nostro obiettivo è fornire successive versioni di aggiornamento circa ogni mese. La piattaforma di base è sempre pronta per la produzione e la licenza fornisce diritti di utilizzo di produzione. Tuttavia, verranno continuamente introdotti nuovi strumenti e funzionalità chiaramente contrassegnati come PREVIEW e adatti per la valutazione e i test.
* Per ottenere l'ultima versione Insider Preview, i partecipanti al Programma Insider registrati possono scaricare Windows Admin Center Preview direttamente dalla [pagina di download Windows Server Insider Preview](https://www.microsoft.com/software-download/windowsinsiderpreviewserver), sotto l'elenco a discesa relativo ai download aggiuntivi. Se non hai ancora effettuato la registrazione come partecipante al programma Insider, vedi [Introduzione a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) nel portale dei partecipanti al Programma Windows Insider per le aziende.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>Perché è stato scelto "Windows Admin Center" come nome finale per "Project Honolulu"?

Windows Admin Center è il nome ufficiale del prodotto per "Project Honolulu" e rafforza la visione di un'esperienza integrata per gli amministratori IT su un'ampia gamma di scenari amministrativi e di gestione di base. Evidenza inoltre come l'attenzione per il cliente sulle esigenze utente degli amministratori IT sia centrale per definire la modalità di investimento e l'offerta.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>Dove posso trovare altre informazioni su Windows Admin Center o maggiori dettagli sugli argomenti illustrati sopra?

La [pagina di avvio](https://aka.ms/WindowsAdminCenter) è il punto di partenza migliore e include collegamenti a contenuto di documentazione recentemente suddiviso per categorie, percorsi di download, istruzioni per fornire feedback, informazioni di riferimento e altre risorse.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>Che cos'è la cronologia delle versioni di Windows Admin Center?

[Visualizza la cronologia delle versioni qui.](../support/release-history.md)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Sto avendo un problema con Windows Admin Center. Dove posso ottenere assistenza?

Vedi la [Guida alla risoluzione dei problemi](../use/troubleshooting.md) e l'elenco dei [problemi noti](../use/known-issues.md).
