---
title: Il motivo per cui Privileged Access workstation per la protezione dell'organizzazione
description: Come workstation PAW può aumentare il comportamento di sicurezza dell'organizzazione
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 9ac591d65fb84f3c0a8bbd33ca71c93daf892ced
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280732"
---
# <a name="privileged-access-workstations"></a>Workstation con accesso con privilegi

>Si applica a: Windows Server

Le workstation PAW (Privileged Access Workstation, workstation amministrativa con privilegi) dispongono di un sistema operativo dedicato per le attività sensibili che devono essere protette dagli attacchi provenienti da Internet e dai vettori di minacce di qualsiasi tipo. Separando queste attività e account sensibili da attività quotidiane di usare le workstation e dispositivi offre una protezione estrema contro gli attacchi di phishing, applicazione e le vulnerabilità del sistema operativo, vari attacchi di rappresentazione e attacchi con furto di credenziali, ad esempio di sequenza di tasti Logging [Pass-the-Hash](https://aka.ms/pth)e Pass-The-Ticket.

## <a name="what-is-a-privileged-access-workstation"></a>Che cos'è una Workstation amministrativa con privilegi?

In termini semplici, una workstation PAW è una workstation con protezione avanzata e bloccata, progettata per garantire la sicurezza degli account e delle attività sensibili.  Le workstation PAW sono consigliate per l'amministrazione dei sistemi di gestione delle identità, dei servizi cloud e dell'infrastruttura cloud privata, nonché delle funzioni aziendali sensibili.

> [!NOTE]
> L'architettura PAW non richiede un mapping 1:1 degli account alle workstation, anche se questa è una configurazione comune. Questa architettura crea un ambiente workstation attendibile che può essere usato da uno o più account.

Per garantire la massima sicurezza, workstation Paw deve essere eseguito sempre il sistema operativo disponibile più aggiornato e protetto: Microsoft consiglia vivamente di Windows 10 Enterprise, che include numerose funzionalità di sicurezza aggiuntive non disponibili in altre edizioni (in particolare [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Le organizzazioni senza accesso a Windows 10 Enterprise possono usare Windows 10 Pro, che include molte delle tecnologie critiche fondamentali per PAW, tra cui Avvio sicuro, BitLocker e Desktop remoto.  I clienti Education possono usare Windows 10 Education.  Nelle workstation PAW non si deve usare Windows 10 Home.
>
> Per una matrice di confronto delle diverse edizioni di Windows 10, leggere [ questo articolo](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

I controlli di sicurezza PAW si concentrano sulla prevenzione a impatto elevato e alta probabilità rischi di compromissione. Sono inclusi gli attacchi sull'ambiente e i rischi che possono ridurre l'efficacia dei controlli PAW nel tempo:

* **Attacchi da Internet**: la maggior parte degli attacchi deriva direttamente o indirettamente da origini Internet e usa Internet per sottrarre dati e assumere comando e controllo. Isolare la workstation PAW dai siti Internet non controllati è un elemento fondamentale per garantire che la workstation non venga compromessa.
* **Rischi legati all'usabilità** se una workstation PAW è troppo difficile da usare per le attività quotidiane, gli amministratori sono portati a facilitare il lavoro creando soluzioni alternative. Spesso le soluzioni alternative espongono la workstation e gli account amministrativi a rischi di sicurezza, quindi è fondamentale coinvolgere gli utenti PAW, consentendo loro di attenuare i problemi di usabilità in modo protetto. Questo può essere eseguito ascoltando il feedback degli utenti, installare gli strumenti e gli script necessari per eseguire i processi e garantire tutto il personale amministrativo sono a conoscenza perché devono usare una workstation PAW, quali una workstation PAW è e come usarlo modo corretto ed efficiente.
* **Rischi legati all'ambiente**: poiché all'interno dell'ambiente molti altri computer e account sono esposti direttamente o indirettamente a rischi originati da Internet, una workstation PAW deve essere protetta dagli attacchi di risorse compromesse presenti all'interno dell'ambiente di produzione. È necessario ridurre al minimo l'uso di strumenti di gestione e gli account che dispongono dell'accesso alle workstation Paw per proteggere e monitorare queste workstation specializzate.
* **Manomissione della Supply Chain**: non è possibile rimuovere tutti i possibili rischi di manomissione nella Supply Chain per hardware e software, ma alcune azioni chiave possono ridurre il numero di vettori critici di attacchi immediatamente disponibili per gli utenti malintenzionati. Tali azioni includono la convalida dell'integrità di tutti i supporti di installazione ([principio di origine pulita](https://aka.ms/cleansource)) e l'uso di un fornitore attendibile e affidabile per hardware e software.
* **Attacchi fisici**: le workstation PAW sono fisicamente mobili e possono essere usate all'esterno di strutture protette. Devono quindi essere protette dagli attacchi basati sull'accesso fisico non autorizzato al computer.

> [!NOTE]
> Una workstation PAW non protegge l'ambiente da un antagonista che abbia già ottenuto l'accesso amministrativo a una foresta Active Directory.
> Poiché molte implementazioni esistenti di Active Directory Domain Services hanno funzionato per anni esposte al rischio di furto di credenziali, le organizzazioni devono presupporre che si siano verificate violazioni e considerare la possibilità che si siano verificate compromissioni non rilevate di credenziali amministratore di dominio o dell'organizzazione. Un'organizzazione che sospetti la compromissione del dominio deve prendere in considerazione l'uso di servizi professionali di risposta agli eventi imprevisti.
>
> Per altre informazioni sulla risposta e indicazioni per il ripristino, vedere le sezioni "Respond to suspicious activity" (Risposta a un'attività sospetta) e "Recover from a breach" (Ripristino da una violazione) di [Mitigating Pass-the-Hash and Other Credential Theft v2](https://aka.ms/pth) (Limitazione della tecnica Pass-the-Hash e furto di altre credenziali versione 2).
>
> Per altre informazioni, visitare la pagina [Microsoft's Incident Response and Recovery services](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) (Servizi Microsoft di risposta agli eventi imprevisti e ripristino).

### <a name="paw-hardware-profiles"></a>Profili hardware PAW

Il personale amministrativo sono anche utenti standard, hanno bisogno di una workstation PAW, nonché una workstation utente standard per controllare l'indirizzo di posta elettronica, esplorare il web e accesso aziendale applicazioni line of business.  Garantire che gli amministratori siano costantemente produttivi e protetti è essenziale per il successo di qualsiasi distribuzione PAW.  Una soluzione sicura che limiti notevolmente la produttività verrà abbandonata dagli utenti a favore di una soluzione che migliori la produttività, anche a costo della mancanza di sicurezza.

Per bilanciare l'esigenza di sicurezza con l'esigenza di produttività, Microsoft consiglia di usare uno di questi profili hardware PAW:

* **Hardware dedicato** -dispositivi dedicati separati per le attività utente e le attività amministrative.
* **Uso simultaneo**: singolo dispositivo che può eseguire attività utente e attività amministrative contemporaneamente, sfruttando i vantaggi della virtualizzazione del sistema operativo o della presentazione.

Le organizzazioni possono usare un solo profilo o entrambi. Non ci sono problemi di interoperabilità tra i profili hardware e le organizzazioni hanno la possibilità di far corrispondere in modo flessibile il profilo hardware alle esigenze e alla situazione di ciascun amministratore.

> [!NOTE]
> È fondamentale che, in tutti questi scenari, il personale amministrativo vengono rilasciato un account utente standard che è distinto dall'account designati come amministrativi. Gli account amministrativi devono essere usati solo per il sistema operativo amministrativo PAW.

Questa tabella riepiloga i vantaggi e gli svantaggi di ogni profilo hardware dal punto di vista della facilità di uso operativo, della produttività e della sicurezza.  Entrambi gli approcci hardware offrono sicurezza avanzata per gli account amministrativi contro il furto di credenziali e il riuso.

|**Scenario**|**Vantaggi**|**Svantaggi**|
|--------|---------|-----------|
|Hardware dedicato|-   Segnale forte per la riservatezza delle attività<br />-   Separazione per la massima sicurezza|-   Spazio aggiuntivo occupato sulla scrivania<br />-   Peso aggiuntivo (per il lavoro in remoto)<br />-   Costo dell'hardware|
|Uso simultaneo|-   Costo inferiore dell'hardware<br />-   Uso di un unico dispositivo|-   Possibilità di errori e rischi accidentali per la condivisione di un unica tastiera e di un unico mouse|

Questo materiale sussidiario contiene istruzioni dettagliate per la configurazione PAW per l'approccio corrispondente all'hardware dedicato. Se sono necessari profili hardware per l'uso simultaneo, è possibile personalizzare le istruzioni riportate in questo materiale o incaricare un'organizzazione di servizi professionali come Microsoft del supporto.

#### <a name="dedicated-hardware"></a>Hardware dedicato

In questo scenario, per l'amministrazione viene usata una workstation PAW completamente separata dal PC usato per le attività quotidiane come la posta elettronica, la modifica di documenti e le attività di sviluppo. Tutti gli strumenti e tutte le applicazioni di amministrazione vengono installati nella workstation PAW e tutte le applicazioni di produttività vengono installate nella workstation utente standard. Le istruzioni dettagliate in questo materiale sussidiario si basano su questo profilo hardware.

#### <a name="simultaneous-use---adding-a-local-user-vm"></a>Uso simultaneo: aggiunta di una VM utente locale

In questo scenario di uso simultaneo un unico PC viene usato sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo. In questa configurazione il sistema operativo utente è disponibile quando il PC è disconnesso (per la modifica di documenti e l'uso della posta elettronica memorizzata nella cache locale), ma richiede hardware e processi di supporto di processi in grado di adattarsi a questo stato di disconnessione.

![Diagramma che illustra l'uso simultaneo di un unico PC sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo](../media/privileged-access-workstations/PAWFig10.JPG)

L'hardware fisico esegue in locale due sistemi operativi:

* **Sistema operativo di amministrazione**: l'host fisico esegue Windows 10 nell'host PAW per le attività amministrative
* **Sistema operativo utente**: una macchina virtuale guest Hyper-V con un client Windows 10 esegue un'immagine aziendale

Con Windows 10 Hyper-V, una macchina virtuale guest (anche in esecuzione Windows 10) può avere un'esperienza utente avanzata comprendente audio, video e applicazioni di comunicazione Internet, ad esempio Skype for Business.

In questa configurazione le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite all'interno della macchina virtuale con il sistema operativo utente, che dispone di un'immagine Windows 10 aziendale regolare e non è soggetta alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito all'interno del sistema operativo di amministrazione.

Per la configurazione di quest'ultimo, seguire le istruzioni presenti in questo materiale sussidiario per l'host PAW, aggiungere le funzionalità di Hyper-V client, creare una VM utente e installare all'interno di questa un'immagine aziendale Windows 10.

Per altre informazioni su questa funzionalità, leggere l'articolo [Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) (Hyper-V client). Si noti che il sistema operativo nelle macchine virtuali guest deve avere una licenza basata sulle [regole per l'assegnazione di licenze ai prodotti Microsoft](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), descritte anche [qui](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf).

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>Uso simultaneo: aggiunta di RemoteApp, di RDP o una VDI

In questo scenario di uso simultaneo un unico PC viene usato sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo. Con questa configurazione il sistema operativo utente dei diversi PC viene distribuito e gestito in modo centralizzato, nel cloud o nel centro dati, ma non è disponibile se il PC è disconnesso.

![Figura che illustra l'uso simultaneo di un unico PC sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo](../media/privileged-access-workstations/PAWFig11.JPG)

L'hardware fisico esegue un solo sistema operativo PAW locale per le attività amministrative e contatta un servizio Desktop remoto Microsoft o di terze parti per le applicazioni utente, ad esempio per la posta elettronica, la modifica di documenti e le applicazioni line-of-business.

In questa configurazione le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite all'interno dei sistemi operativi remoti, che non sono soggetti alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito all'interno del sistema operativo di amministrazione.

Per questa configurazione, seguire le istruzioni presenti in questo materiale sussidiario per l'host PAW, consentire la connettività di rete al servizio Desktop remoto e aggiungere collegamenti al desktop dell'utente PAW per consentire l'accesso alle applicazioni. I servizi Desktop remoto possono essere ospitati in molti modi, ad esempio tramite:

* Un servizio Desktop remoto o VDI esistente (locale o nel cloud)
* Un nuovo servizio installato in locale o nel cloud
* Azure RemoteApp con modelli di Office 365 preconfigurati o immagini di installazione personalizzate

Per altre informazioni su Azure RemoteApp, visitare [questa pagina](https://www.remoteapp.windowsazure.com).

## <a name="how-microsoft-is-using-administrative-workstations"></a>Come Microsoft Usa workstation amministrative

Microsoft usa l'approccio architettonico PAW sia internamente nei propri sistemi che con i propri clienti. Microsoft Usa workstation amministrative internamente in diverse funzioni, ad esempio amministrazione dell'infrastruttura di Microsoft IT, lo sviluppo dell'infrastruttura di Microsoft cloud dell'infrastruttura e le operazioni e altre risorse di valore elevato.

Questo materiale sussidiario si basa direttamente sull'architettura di riferimento PAW (Privileged Access Workstation) distribuita dal team Microsoft dedicato ai servizi professionali riguardanti la sicurezza informatica per proteggere i clienti da attacchi alla sicurezza. Le workstation amministrative sono anche un elemento chiave dello strumento più avanzato di protezione delle attività di amministrazione del dominio: ESAE (Enhanced Security Administrative Environment, ambiente amministrativo di sicurezza avanzata), l'architettura di riferimento per le foreste amministrative.

Per altri dettagli sulla foresta amministrativa ESAE, vedere la *approccio di progettazione della foresta amministrativa ESAE* sezione [protezione materiale di riferimento accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).

## <a name="architecture-overview"></a>Panoramica dell'architettura

Il diagramma seguente illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile), creato tramite la gestione separata di account e workstation amministrativi dedicati.

![Diagramma che illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile), creato tramite la gestione separata di account e workstation amministrativi dedicati](../media/privileged-access-workstations/PAWFig1.JPG)

Questo approccio architettonico, basato sulle misure di sicurezza disponibili nelle funzionalità [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx) di Windows 10, per gli account e le attività sensibili si spinge oltre tali misure .

Questa metodologia è appropriata per gli account con accesso alle risorse di valore elevato:

* **Privilegi amministrativi** -workstation Paw offrono una maggiore sicurezza per i ruoli amministrativi IT e delle attività. Questa architettura può essere applicata all'amministrazione di molti tipi di sistemi, ad esempio domini e foreste di Active Directory, tenant di Microsoft Azure Active Directory e di Office 365, reti PCN (Process Control Network), sistemi di supervisione e di acquisizione dati (SCADA, Supervisory Control and Data Acquisition), Bancomat e dispositivi POS.
* **Elevata sensibilità agli Information Worker** -l'approccio usato in una workstation PAW anche garantisce la protezione delle informazioni estremamente riservate ruolo di lavoro attività e personale, ad esempio quelli relativi a pre-annuncio fusione e acquisizione attività, versioni non definitive rendiconti finanziari, presenza sui social media dell'organizzazione, le comunicazioni executive, segreti commerciali non brevettati, research sensibili o altri dati sensibili o proprietari. Questo materiale sussidiario non illustra in modo approfondito la configurazione degli scenari relativi agli information worker né include questo scenario nelle istruzioni tecniche.

    > [!NOTE]
    > Il reparto IT Microsoft usa workstation PAW, definite internamente come "workstation amministrative protette" o SAW (Secure Admin Workstation) per gestire l'accesso protetto ai sistemi a valore elevato all'interno di Microsoft. Più avanti, nella sezione "Come Microsoft usa le workstation amministrative", questo materiale sussidiario descrive in maggiore dettaglio l'uso di workstation PAW presso Microsoft. Per informazioni più dettagliate su questo approccio agli ambienti con risorse di valore elevato, fare riferimento all'articolo [Protecting high-value assets with secure admin workstations](https://msdn.microsoft.com/library/mt186538.aspx) (Protezione delle risorse di valore elevato con workstation amministrative protette).

Questo documento illustra i motivi per cui questa procedura è consigliata per la protezione degli account con privilegi a impatto elevato, l'aspetto delle soluzioni PAW per la protezione dei privilegi amministrativi e il metodo per distribuire rapidamente una soluzione PAW per l'amministrazione di servizi di dominio e cloud.

Questo documento offre indicazioni dettagliate per l'implementazione di diverse configurazioni PAW e include istruzioni di implementazione dettagliate che consentono di iniziare a proteggere account comuni a impatto elevato:

* [**Fase 1: distribuzione immediata per amministratori di Active Directory** ](#phase-1-immediate-deployment-for-active-directory-administrators) fornisce una workstation PAW rapidamente che consente di proteggere in locale i ruoli di amministrazione di domini e foreste
* [**Fase 2: estensione di PAW a tutti gli amministratori** ](#phase-2-extend-paw-to-all-administrators) Ciò abilita la protezione per gli amministratori di servizi cloud quali Office 365 e Azure, server aziendali, le applicazioni aziendali e workstation
* [**Fase 3: protezione PAW avanzata** ](#phase-3-extend-and-enhance-protection) vengono riportati altri tipi di protezione e considerazioni sulla sicurezza delle workstation PAW

### <a name="why-dedicated-workstations"></a>Il motivo per cui le workstation dedicata?

L'universo attuale delle minacce alle organizzazioni pullula di sofisticati tipi di phishing e di altri attacchi via Internet che creano un rischio costante di compromissione della sicurezza per gli account e le workstation esposti a Internet.

Questo universo di minacce obbliga le organizzazioni ad adottare un approccio alla sicurezza "presunzione di violazione" durante la progettazione di funzionalità di protezione per le risorse di valore elevato, ad esempio gli account amministrativi e risorse aziendali sensibili. Queste risorse di elevato valore devono essere protette sia dalle minacce provenienti direttamente da Internet che dagli attacchi predisposti da workstation, server e dispositivi dell'ambiente.

![Figura che illustra il rischio a cui sono esposte le risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili](../media/privileged-access-workstations/PAWFig2.JPG)

Questa figura illustra il rischio a cui sono esposte le risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili.

Se ha il controllo del sistema operativo, un utente malintenzionato ha a disposizione molti modi per accedere illecitamente a tutte le attività della workstation e per rappresentare l'account legittimo. Per ottenere questo livello di accesso è possibile usare una serie di tecniche di attacco note e sconosciute. L'aumento costante del numero e della complessità degli attacchi informatici ha reso necessario estendere il concetto di separazione fino al punto di applicarlo completamente ai sistemi operativi client per gli account sensibili. Per altre informazioni su questi tipi di attacchi e per ottenere white paper informativi, video e altro ancora, visitare il [sito Web Pass-The-Hash](https://www.microsoft.com/pth).

L'approccio PAW rappresenta l'estensione della procedura consigliata ormai consolidata di usare account amministrativi e account utente separati per il personale amministrativo. Con questa procedura consigliata vengono assegnati agli utenti account amministrativi individuali completamente separati dall'account utente standard. La workstation PAW sviluppa ulteriormente la pratica di separazione degli account usando una workstation attendibile per gli account sensibili.

Questo materiale sussidiario su PAW ha lo scopo di consentire l'implementazione di questa funzionalità per la protezione di account di valore elevato, ad esempio gli account degli amministratori IT con privilegi elevati e gli account aziendali a elevata sensibilità. Il materiale sussidiario consente di:

* Limitare l'esposizione delle credenziali ai soli host attendibili
* Fornire agli amministratori una workstation con un livello di sicurezza elevato con cui eseguire facilmente attività amministrative.

Limitare gli account sensibili all'uso esclusivo di workstation PAW con protezione avanzata consente di proteggere in modo semplice questi account, consentendone l'elevata usabilità da parte degli amministratori e rendendone molto difficile la compromissione da parte degli avversari.

### <a name="alternate-approaches"></a>Approcci alternativi

Questa sezione contiene informazioni sul confronto tra la sicurezza offerta dagli approcci alternativi e la workstation PAW e sulla corretta integrazione di questi approcci all'interno di un'architettura PAW. tutti questi approcci comportano rischi significativi una volta implementato in isolamento, ma possono aggiungere valore a un'architettura paw in alcuni scenari.

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard e Windows Hello for Business

Introdotto con Windows 10, [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) usa la sicurezza basata su hardware e virtualizzazione per prevenire gli attacchi più comuni di furto di credenziali, ad esempio Pass-the-Hash, proteggendo le credenziali derivate. La chiave privata per le credenziali utilizzate dal [Windows Hello for Business](https://aka.ms/passport) può essere anche essere protetti dall'hardware di modulo TPM (Trusted Platform).

Si tratta di soluzioni di prevenzione avanzate, ma workstation possono comunque essere vulnerabile ad alcuni attacchi, anche se le credenziali sono protette tramite Credential Guard o Windows Hello for Business. Gli attacchi possono includere abuso di privilegi e l'uso di credenziali direttamente da un dispositivo compromesso, il riutilizzo di credenziali rubate prima dell'abilitazione di Credential Guard e l'abuso di strumenti di gestione e le configurazioni di applicazioni vulnerabili nella workstation.

Le indicazioni su PAW in questa sezione riguardano l'uso di molte di queste tecnologie per gli account e le attività a elevata sensibilità.

#### <a name="administrative-vm"></a>VM amministrative

Una macchina virtuale amministrativa (Admin VM) è un sistema operativo dedicato per le attività amministrative ospitato in un desktop utente standard. Questo approccio è simile all'architettura PAW poiché offre un sistema operativo dedicato per le attività amministrative, ma ha un grave difetto: la sicurezza di una VM amministrativa dipende dal desktop utente standard.

Il diagramma che segue illustra in che modo gli utenti malintenzionati possono seguire la catena di controllo fino all'oggetto obiettivo dell'attacco con una VM amministrativa in una workstation utente. Illustra anche la difficoltà di creare un percorso nella configurazione inversa.

L'architettura PAW non consente l'hosting una VM Admin in una Workstation utente, ma una VM utente con un'immagine aziendale standard può essere ospitata in un'amministrazione PAW per fornire al personale con un unico PC per tutte le responsabilità.

![Diagramma dell'architettura PAW](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>Workstation Paw basate su macchine Virtuali schermate

Una variante del modello amministrativo di macchina virtuale sicura consiste nell'usare [macchine virtuali schermate](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) per ospitare una o più VM admin insieme a un utente della macchina virtuale.
Le macchine virtuali schermate sono progettate per eseguire carichi di lavoro protetti in un ambiente in cui gli utenti potenzialmente non attendibili o il codice potrebbe essere in esecuzione sul desktop utente standard del computer fisico.
Una macchina virtuale schermata include un TPM virtuale che consente di crittografare i propri dati quando sono inattivi, e diversi controlli amministrativi quali l'accesso alla console di base, PowerShell Direct e la possibilità di eseguire il debug della macchina virtuale sono disabilitati per isolare maggiormente la VM dal desktop utente standard e altre macchine virtuali.
Le chiavi per una macchina virtuale schermata vengono archiviate in un server di gestione delle chiavi attendibile, che richiede il dispositivo fisico per attestare l'identità e dell'integrità prima di rilasciare un tasto per avviare la macchina virtuale.
Ciò garantisce che le macchine virtuali schermate possono avviare solo nei dispositivi previsti e che tali dispositivi siano in esecuzione le configurazioni di software conosciuti e attendibili.

Poiché le macchine virtuali schermate sono isolate tra loro e sul desktop utente standard, è accettabile per l'esecuzione di più macchine virtuali schermate di workstation PAW in un singolo host, anche quando tali macchine virtuali admin gestiscono i diversi livelli.

Vedere le [distribuzione di workstation Paw con un'infrastruttura sorvegliata](#deploy-paws-using-a-guarded-fabric) sezione di seguito per altre informazioni.

#### <a name="jump-server"></a>Jump server

Le architetture "jump server" amministrative consentono di configurare un numero ridotto di server console di amministrazione e obbligano il personale a usare tali server per le attività amministrative. In genere queste configurazioni si basano su servizi desktop remoto, su una soluzione di terze parti di virtualizzazione della presentazione o su una tecnologia VDI (Virtual Desktop Infrastructure).

Questo approccio, proposto spesso per attenuare i rischi per l'amministrazione, garantisce effettivamente una certa sicurezza, ma l'approccio jump server da solo è vulnerabile ad alcuni attacchi, perché viola il cosiddetto [principio di "origine pulita"](../securing-privileged-access/securing-privileged-access-reference-material.md#clean-source-principle). Il principio di origine pulita richiede che tutte le dipendenze di sicurezza siano attendibili come l'oggetto protetto.

![Figura che illustra una relazione di controllo semplice](../media/privileged-access-workstations/PAWFig3.JPG)

La figura illustra una relazione di controllo semplice. Qualsiasi soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto. Se un antagonista riesce a prendere il controllo di una dipendenza di sicurezza di un oggetto di destinazione (soggetto) assume il controllo di tale oggetto.

La sessione di amministrazione nel jump server si basa sull'integrità del computer locale che accede al jump server stesso. Se tale computer è una workstation utente soggetta ad attacchi di phishing e ad altri vettori di attacco basati su Internet, anche la sessione di amministrazione è soggetta a tali rischi.

![Figura che illustra in che modo gli utenti malintenzionati possono seguire una catena di controllo stabilita verso l'oggetto di interesse](../media/privileged-access-workstations/PAWFig4.JPG)

La figura sopra riportata illustra in che modo gli utenti malintenzionati possono seguire una catena di controllo stabilita verso l'oggetto di interesse.

Alcuni controlli di sicurezza avanzati, ad esempio l'autenticazione a più fattori, possono aumentare per un utente malintenzionato la difficoltà di impossessarsi della sessione di amministrazione nella workstation dell'utente. Tuttavia nessuna funzionalità di sicurezza può garantire una protezione completa dagli attacchi tecnici (ad esempio l'inserimento di comandi illeciti in una sessione legittima, l'hijack di processi legittimi e così via) se un utente malintenzionato dispone dell'accesso amministrativo al computer di origine.

La configurazione predefinita in questo materiale sussidiario su PAW installa gli strumenti di amministrazione nella workstation PAW, ma è possibile aggiungere anche un'architettura jump server, se necessario.

![Figura che illustra come invertire la relazione di controllo. In questo modo l'accesso alle app utente da una workstation amministrativa non offre all'utente malintenzionato alcun percorso all'oggetto di destinazione](../media/privileged-access-workstations/PAWFig5.JPG)

Questa figura illustra come invertire la relazione di controllo. In questo modo l'accesso alle app utente da una workstation amministrativa non offre all'utente malintenzionato alcun percorso all'oggetto di destinazione. Il jump server utente è ancora esposto a rischi. A tale computer, connesso a Internet, è quindi ancora necessario applicare controlli di protezione appropriati, controlli di rilevamento e processi di risposta.

Questa configurazione richiede che gli amministratori seguano scrupolosamente procedure operative che evitino l'immissione accidentale di credenziali amministrative nella sessione utente del desktop.

![Figura che illustra che l'accesso a un jump server amministrativo da una workstation PAW non aggiunge per l'utente malintenzionato alcun percorso alle risorse amministrative](../media/privileged-access-workstations/PAWFig6.JPG)

Questa figura illustra che l'accesso a un jump server amministrativo da una workstation PAW non aggiunge per l'utente malintenzionato alcun percorso alle risorse amministrative. Un jump server dotato di PAW consente in questo caso di consolidare il numero di percorsi per il monitoraggio delle attività amministrative e la distribuzione di applicazioni e strumenti amministrativi. Ciò rende più complessa la progettazione ma semplifica il monitoraggio della sicurezza e gli aggiornamenti software nei casi in cui l'implementazione PAW usi un numero elevato di account e workstation. Il jump server deve essere creato e configurato secondo standard di sicurezza simili a quelli della workstation PAW.

#### <a name="privilege-management-solutions"></a>Soluzioni di gestione dei privilegi

Le soluzioni di gestione dei privilegi sono applicazioni che consentono l'accesso temporaneo a privilegi discreti o ad account con privilegi su richiesta. Le soluzioni di gestione dei privilegi sono un componente estremamente prezioso di una strategia completa per proteggere l'accesso con privilegi e garantire visibilità e attendibilità, di fondamentale importanza, dell'attività amministrativa.

Per concedere l'accesso, queste soluzioni usano in genere un flusso di lavoro flessibile e molte hanno funzionalità e caratteristiche di sicurezza aggiuntive, ad esempio funzioni di gestione delle password degli account del servizio e integrazione con i jump server amministrativi. Sul mercato sono disponibili molte soluzioni dotate di funzionalità di gestione, tra cui Privileged Access Management (PAM) di Microsoft Identity Manager (MIM).

Per accedere alle soluzioni di gestione dei privilegi è consigliabile usare un sistema PAW. L'accesso a queste soluzioni deve essere concesso solo a workstation PAW. Non è consigliabile usare queste soluzioni in sostituzione di una workstation PAW, perché l'accesso ai privilegi tramite queste soluzioni da un desktop utente potenzialmente compromesso viola il principio di [origine pulita](https://aka.ms/cleansource) come illustrato nel diagramma seguente:

![Diagramma che illustra che Microsoft sconsiglia di usare queste soluzioni in sostituzione di una workstation PAW, perché l'accesso ai privilegi tramite queste soluzioni da un desktop utente potenzialmente compromesso viola il principio di origine pulita](../media/privileged-access-workstations/PAWFig7.JPG)

L'accesso a queste soluzioni tramite una workstation PAW consente di assicurare i vantaggi della sicurezza sia alla workstation stessa che alla soluzione di gestione dei privilegi, come illustrato in questo diagramma:

![Diagramma che illustra come l'accesso a queste soluzioni tramite una workstation PAW consente di assicurare i vantaggi della sicurezza sia alla workstation stessa che alla soluzione di gestione dei privilegi](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Questi sistemi devono essere classificati al livello più alto del privilegio che gestiscono e devono essere protetti a un livello di sicurezza uguale o superiore. Questi sono in genere configurati per gestire soluzioni e attività di livello 0 e devono essere classificati a livello 0.
> Per altre informazioni sul modello a livelli, vedere [ https://aka.ms/tiermodel ](https://aka.ms/tiermodel) per altre informazioni sui gruppi di livello 0, vedere l'equivalenza di livello 0 nella [protezione materiale di riferimento accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md).

Per ulteriori informazioni sulla distribuzione di Microsoft Identity Manager (MIM) privileged access management (PAM), vedere [https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy)

## <a name="paw-scenarios"></a>Scenari PAW

Questa sezione contiene indicazioni sugli scenari a cui è applicabile questo materiale sussidiario su PAW. Per tutti gli scenari gli amministratori devono essere addestrati a usare solo workstation PAW per il supporto a sistemi remoti. Per favorire un uso corretto e sicuro e consentire il miglioramento dell'esperienza PAW, è anche necessario incoraggiare tutti gli utenti PAW a fornire feedback, che dovrà essere esaminato attentamente e integrato nel programma PAW.

Per tutti gli scenari è possibile usare nelle fasi successive misure di protezione avanzata aggiuntive e profili hardware diversi rispetto a quelli citati in questo materiale sussidiario per soddisfare i requisiti di usabilità o di sicurezza dei ruoli.

> [!NOTE]
> Questo materiale sussidiario in modo esplicito viene fatta distinzione tra che richiedono l'accesso a servizi specifici su internet (ad esempio i portali amministrativi Azure e Office 365) e la Internet"aperta" tutti gli host e i servizi.

Per altre informazioni sulle designazioni di livello, vedere la [pagina sul modello a livelli](https://aka.ms/tiermodel).

|**Scenari**|**Usare PAW?**|**Ambito e considerazioni sulla sicurezza**|
|---------|--------|---------------------|
|Amministratori di Active Directory, livello 0|Yes|Una workstation PAW creata secondo le indicazioni per la fase 1 è sufficiente per questo ruolo.<br /><br />-   È possibile aggiungere una foresta amministrativa per garantire la protezione più avanzata a questo scenario. Per altre informazioni sulla foresta amministrativa ESAE, vedere [Approccio per la progettazione della foresta amministrativa ESAE](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)<br />-   Una workstation PAW può essere usata per gestire più domini o più foreste.<br />-Se i controller di dominio sono ospitati in un'infrastruttura come servizio (IaaS) o soluzione di virtualizzazione locale, è necessario specificare le priorità di implementazione di workstation Paw per gli amministratori di tali soluzioni.|
|Amministratori di servizi IaaS e PaaS di Azure, livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW almeno per l'amministratore globale e per l'amministratore della fatturazione delle sottoscrizioni. È necessario usare workstation PAW anche per gli amministratori delegati di server critici o sensibili.<br />-Workstation Paw deve essere utilizzata per la gestione del sistema operativo e le applicazioni che forniscono la sincronizzazione delle Directory e la federazione delle identità per i servizi cloud, ad esempio [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect/) e Active Directory Federation Services (ADFS).<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi cloud autorizzati in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.<br />-Windows Defender Exploit Guard devono essere configurati nella workstation **Nota:**     Una sottoscrizione viene considerata di livello 0 per una foresta se sono presenti controller di dominio o altri host di livello 0 nella sottoscrizione. Una sottoscrizione è di livello 1 se in Azure non sono ospitati server di livello 0.|
|Amministratore di tenant di Office 365 <br />- Livello 1|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW almeno per l'amministratore della fatturazione delle sottoscrizioni, per l'amministratore globale, per l'amministratore di Exchange, per l'amministratore di SharePoint e per ruoli di amministratore di gestione degli utenti. È anche consigliabile prendere seriamente in considerazione l'uso di workstation PAW per gli amministratori delegati di dati estremamente critici o sensibili.<br />-Windows Defender Exploit Guard devono essere configurati nella workstation.<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.|
|Altro amministratore di servizi cloud IaaS o PaaS<br />- Livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW per tutti i ruoli con diritti amministrativi su VM ospitate nel cloud, tra cui la possibilità di installare agenti, esportare file dal disco rigido o accedere alle risorse di archiviazione in cui sono archiviati dischi rigidi con sistemi operativi, dati sensibili o dati aziendali critici.<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.<br />-Windows Defender Exploit Guard devono essere configurati nella workstation. **Nota:** Una sottoscrizione è di livello 0 per una foresta se sono presenti controller di dominio o altri host di livello 0 nella sottoscrizione. Una sottoscrizione è di livello 1 se in Azure non sono ospitati server di livello 0.|
|Amministratori di virtualizzazioni<br />- Livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW per tutti i ruoli con diritti amministrativi su VM, tra cui la possibilità di installare agenti, esportare file da dischi rigidi virtuali o accedere alle risorse di archiviazione in cui sono archiviati dischi rigidi con informazioni di sistemi operativi guest, dati sensibili o dati aziendali critici. **Nota:** Un sistema di virtualizzazione (e i relativi amministratori) sono considerati di livello 0 per una foresta se sono presenti controller di dominio o altri host di livello 0 nella sottoscrizione. Una sottoscrizione è di livello 1 se nel sistema di virtualizzazione non sono ospitati server di livello 0.|
|Amministratori della manutenzione dei server<br />- Livello 1|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Una workstation PAW deve essere usata dagli amministratori che aggiornano server e app aziendali in Windows Server, Linux e altri sistemi operativi, oltre ad applicare patch e risolvere i problemi di tali server e app.<br />-   Può essere necessario aggiungere strumenti di gestione dedicati alle workstation PAW per consentire lo svolgimento di tali attività di amministrazione su scala più ampia.|
|Amministratori di workstation utente <br />- Livello 2|Yes|Una workstation PAW creata secondo le indicazioni relative alla fase 2 è sufficiente per i ruoli con diritti amministrativi sui dispositivi degli utenti finali, ad esempio i ruoli di help desk e di supporto deskside.<br /><br />-   Può essere necessario installare applicazioni aggiuntive nelle workstation PAW per consentire la gestione di ticket e altre funzioni di supporto.<br />-Windows Defender Exploit Guard devono essere configurati nella workstation.<br />    Può essere necessario aggiungere strumenti di gestione dedicati alle workstation PAW per consentire lo svolgimento di tali attività di amministrazione su scala più ampia.|
|Amministratore di SQL, SharePoint o line-of-business (LOB)<br />- Livello 1|Yes|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Può essere necessario installare strumenti di gestione aggiuntivi nelle workstation PAW installato per consentire agli amministratori di gestire le applicazioni senza la necessità di connettersi ai server tramite Desktop remoto.|
|Utenti che gestiscono la presenza nei social media|Parziale|Una workstation PAW creata secondo le indicazioni specificate per la fase 2 può essere usata come punto di partenza per garantire la sicurezza a questo ruolo.<br /><br />-   Proteggere e gestire gli account per i social media tramite Azure Active Directory (AAD) per condividere e proteggere tali account e per tenere traccia dell'accesso a questi.<br />    Per altre informazioni su questa funzionalità, leggere [questo post di blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-   Le restrizioni di rete in uscita devono consentire la connettività a questi servizi. Ciò può avvenire consentendo connessioni a Internet "aperta" (con un rischio per la sicurezza molto più alto e l'annullamento di molte garanzie PAW) o consentendo il servizio solo tramite indirizzi DNS specifici (alternativa difficile da realizzare).|
|Utenti standard|No|È possibile eseguire molti passaggi di protezione avanzata per gli utenti standard. L'architettura PAW, tuttavia, è progettata per isolare gli account impedendo l'accesso a Internet "aperta", di cui la maggior parte degli utenti ha bisogno per lo svolgimento delle attività lavorative.|
|VDI/chiosco multimediale guest|No|È possibile eseguire molti passaggi di protezione avanzata per gli utenti guest di sistemi chiosco multimediale. L'architettura PAW, tuttavia, è progettata per garantire una maggiore sicurezza per gli account a sensibilità elevata, non una maggiore sicurezza per gli account a sensibilità inferiore.|
|Utente VIP (dirigenti, ricercatori e così via)|Parziale|Una workstation PAW creata usando le indicazioni fornite nella fase 2 è utilizzabile come punto di partenza per garantire la sicurezza per questi ruoli.<br /><br />-   Questo scenario è simile a quello dei desktop utente standard, ma in genere il profilo dell'applicazione è più piccolo, più semplice e ben noto. Questo scenario richiede in genere l'individuazione e la protezione dei dati, dei servizi e delle applicazioni sensibili, indipendentemente dalla loro installazione nei computer desktop.<br />-   Questi ruoli in genere richiedono un livello di sicurezza elevato e un livello di usabilità molto elevato. Ciò richiede modifiche di progettazione per soddisfare le preferenze dell'utente.|
|Sistemi di controllo industriale (ad esempio SCADA, PCN e DCS)|Parziale|Una workstation PAW creata secondo le indicazioni specificate per la fase 2 può essere usata come punto di partenza per garantire la sicurezza a questi ruoli. Infatti la maggior parte delle console dei sistemi di controllo industriale (inclusi standard comuni come SCADA e PCN) non hanno bisogno di accedere a Internet "aperta" né di controllare la posta elettronica.<br /><br />-Le applicazioni usate per il controllo di macchinari fisici dovrà essere integrato e testate per garantire la compatibilità e protette in modo appropriato.|
|Sistema operativo incorporato|No|È possibile eseguire molti passaggi di protezione avanzata da PAW per i sistemi operativi incorporati. In questo scenario, tuttavia, è necessario sviluppare una soluzione personalizzata per la protezione avanzata.|

> [!NOTE]
> **Combinazione di scenari** Alcuni membri del personale possono avere responsabilità amministrative per più scenari.
> In questi casi, la regola chiave da ricordare è che le regole di modello di livello devono sempre essere seguite. Per altre informazioni, vedere la pagina sul modello a livelli.

> [!NOTE]
> **Ridimensionamento del programma PAW** Man mano che il programma PAW viene ridimensionato per comprendere un numero maggiore di amministratori e di ruoli, è necessario continuare a mantenere la conformità agli standard di sicurezza e usabilità. Questo può richiedere l'aggiornamento delle strutture di supporto IT o la creazione di nuove strutture di questo tipo per risolvere problemi specifici di PAW, ad esempio il processo di caricamento PAW, la gestione degli incidenti e della configurazione e la raccolta di feedback relativo a problemi di usabilità.  Ad esempio, l'organizzazione potrebbe decidere di abilitare nuovi scenari per consentire agli amministratori di lavorare da casa. A tale scopo sarebbe necessario passare da workstation PAW desktop a workstation PAW portatili, un passaggio che richiederebbe considerazioni di sicurezza aggiuntive.  Un altro esempio comune consiste nella creazione o nell'aggiornamento di corsi di formazione per nuovi amministratori con l'aggiunta di contenuti relativi all'uso appropriato di una workstation PAW, all'importanza di questa architettura e alle sue caratteristiche.  Per altre considerazioni da tenere presenti in caso di ridimensionamento del programma PAW, vedere la fase 2 delle istruzioni.

Questo materiale sussidiario contiene istruzioni dettagliate per la configurazione PAW per gli scenari sopra citati. Se sono necessari altri scenari, è possibile personalizzare le istruzioni riportate in questo materiale o incaricare un'organizzazione di servizi professionali come Microsoft del supporto.

Per altre informazioni sull'attivazione dei servizi Microsoft per la progettazione di un'architettura PAW su misura per l'ambiente in uso, contattare il rappresentante Microsoft o visitare [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

## <a name="paw-phased-implementation"></a>Implementazione dell'approccio PAQ Phased

Poiché PAW deve fornire un'origine protetta e attendibile per l'amministrazione, è essenziale che il processo di creazione sia sicuro e attendibile.  Questa sezione fornisce istruzioni dettagliate che consentono di creare il proprio ambiente PAW tramite principi e concetti generali molto simili a quelli applicati dal reparto IT e dalle organizzazioni di gestione dei servizi e di cloud engineering Microsoft.

Le istruzioni sono suddivise in tre fasi che si concentrano sulla rapida realizzazione delle misure di prevenzione più critiche e quindi sulla crescita e sulla diffusione dell'uso di PAW all'interno dell'azienda.

* [Fase 1: distribuzione immediata per amministratori di Active Directory](#phase-1-immediate-deployment-for-active-directory-administrators)
* [Fase 2: estensione di PAW a tutti gli amministratori](#phase-2-extend-paw-to-all-administrators)
* [Fase 3: protezione PAW avanzata](#phase-3-extend-and-enhance-protection)

È importante notare che le fasi devono essere sempre eseguite nell'ordine in cui sono presentate, anche se vengono pianificate e implementate all'interno di uno stesso progetto complessivo.

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>Fase 1: Distribuzione immediata per amministratori di Active Directory

Scopo: Fornisce una workstation PAW rapidamente che consente di proteggere i ruoli di amministrazione di domini e foreste in locale.

Ambito: Amministratori di livello 0, tra cui Enterprise Admins e Domain Admins (per tutti i domini) gli amministratori di altri sistemi di identità autorevole.

La fase 1 è dedicata principalmente agli amministratori che gestiscono il dominio di Active Directory. Si tratta di ruoli di fondamentale importanza e rappresentano spesso l'obiettivo di utenti malintenzionati. Questi sistemi di gestione delle identità funzionano in modo efficace per la protezione di questi amministratori, indipendentemente dal fatto che i controller di dominio di Active Directory siano ospitati in centri dati locali, in un'infrastruttura distribuita come servizio (IaaS, Infrastructure as a Service) di Azure o in un'infrastruttura IaaS di un altro provider.

Durante questa fase viene creata la struttura dell'unità organizzativa (OU) Active Directory amministrativa protetta che ospiterà le workstation amministrative con privilegi (PAW). Vengono anche distribuite le workstation PAW stesse.  Questa struttura include anche i Criteri di gruppo e i gruppi necessari per il supporto delle workstation PAW.  La maggior parte della struttura viene creata tramite script di PowerShell disponibili nella [Raccolta TechNet](https://aka.ms/pawmedia).

Gli script creano le OU e i gruppi di sicurezza seguenti:

* Unità organizzative (OU)
   * Sei nuove OU di primo livello:  Admin; Gruppi di; Server di livello 1. Workstation; Account utente. e la quarantena Computer.  Ogni OU di primo livello conterrà più unità organizzative figlio.
* Gruppi
   * Sei nuovi globali gruppi con sicurezza abilitata:  Attività di manutenzione replica livello 0 Attività di manutenzione Server livello 1 Operatori del servizio tecnico; Attività di manutenzione workstation Utenti di workstation PAW. PAW Maintenance.

Si creeranno inoltre alcuni oggetti Criteri di gruppo: PAW Configuration - Computer. PAW Configuration - utente. RestrictedAdmin necessaria - Computer. Restrizioni in uscita PAW; Limitare l'accesso a una Workstation; Limitare l'accesso al Server.

La fase 1 prevede i passaggi seguenti:

#### <a name="complete-the-prerequisites"></a>Completare i prerequisiti

1. **Assicurarsi che tutti gli amministratori usino account separati rispettivamente per le attività di amministrazione e quelle di utente finale**, ad esempio posta elettronica, esplorazione di Internet, applicazioni line-of-business e altre attività non amministrative.  L'assegnazione a ciascun membro del personale autorizzato di un account amministrativo separato dall'account utente standard è fondamentale per il modello PAW, dato che solo a determinati account saranno autorizzati ad accedere a PAW.

   > [!NOTE]
   > Ogni amministratore deve usare il proprio account amministratore.  Non condividere un account amministrativo tra più utenti.

2. **Ridurre al minimo il numero di amministratori con privilegi di livello 0**.  Poiché ogni amministratore deve usare una workstation PAW, riducendo il numero di amministratori si riduce il numero di workstation necessarie e i costi associati. Un numero inferiore di amministratori implica anche un'esposizione inferiore dei privilegi e rischi minori. Per gli amministratori che si trovano nella stessa sede è possibile condividere una workstation PAW, mentre per gli amministratori che si trovano in sedi diverse sono necessarie workstation PAW separate.
3. **Acquisire l'hardware da un fornitore affidabile che soddisfi tutti i requisiti tecnici**. Microsoft consiglia di acquisire hardware che soddisfi i requisiti tecnici descritti nell'articolo [Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > L'architettura PAW installata in hardware senza queste caratteristiche può offrire una protezione significativa, ma non ha a disposizione le funzionalità di sicurezza avanzate quali Credential Guard e Device Guard.  Credential Guard e Device Guard non sono necessari per fase 1 della distribuzione, ma sono estremamente consigliabili nel corso della fase 3 (protezione avanzata).
   >
   > Assicurarsi che l'hardware usato per le workstation PAW sia originato da un produttore e da un fornitore le cui procedure di sicurezza siano considerate attendibili dall'organizzazione. Si tratta di un'applicazione del principio di origine pulita per la sicurezza della Supply Chain.
   >
   > Per altre informazioni sull'importanza della sicurezza della Supply Chain, visitare [questo sito](https://www.microsoft.com/security/cybersecurity/).

4. **Acquisire e convalidare il richiesto Windows 10 Enterprise Edition e il software applicativo**. Ottenere il software necessario per la workstation PAW e convalidarlo in base alle indicazioni fornite in [Clean Source for installation media](https://aka.ms/cleansource) (Origine pulita per i supporti di installazione).

   * Windows 10 Enterprise Edition
   * [Strumenti di amministrazione remota del server](https://www.microsoft.com/en-us/download/details.aspx?id=45520) per Windows 10
   * [Baseline di sicurezza di Windows 10](https://aka.ms/win10baselines)

      > [!NOTE]
      > Microsoft pubblica gli hash MD5 per tutti i sistemi operativi e le applicazioni in MSDN, ma non tutti i fornitori di software offrono una documentazione simile.  In questi casi sono necessarie altre strategie.  Per altre informazioni sulla convalida del software, fare riferimento a [Clean Source principle](https://aka.ms/cleansource) (Principio di origine pulita) per i supporti di installazione.

5. **Assicurarsi che il server WSUS sia disponibile nell'Intranet**. Per scaricare e installare gli aggiornamenti per PAW è necessario un server WSUS all'interno dell'intranet. Il server WSUS deve essere configurato in modo da approvare automaticamente tutti gli aggiornamenti della sicurezza per Windows 10. In alternativa, un membro del personale amministrativo deve avere l'incarico e assumersi la responsabilità di approvare rapidamente gli aggiornamenti software.

   > [!NOTE]
   > Per altre informazioni, vedere la sezione "Automatically Approve Updates for Installation" (Approvare automaticamente gli aggiornamenti per l'installazione) nelle indicazioni [Approving Updates](https://technet.microsoft.com/library/cc708458(v=ws.10).aspx) (Approvazione degli aggiornamenti).

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>Distribuire il framework della OU amministrativa che deve ospitare le workstation PAW

1. Scaricare la libreria di script PAW dalla [Raccolta TechNet](https://aka.ms/PAWmedia)

   > [!NOTE]
   > Scaricare tutti i file e salvarli nella stessa directory ed eseguirli nell'ordine specificato di seguito.  Create-PAWGroups dipende dalla struttura della OU creata da Create-PAWOUs e Set-PAWOUDelegation dipende dai gruppi creati da Create-PAWGroups.
   > Non modificare gli script né il file con valori delimitati da virgole (CSV).

2. **Eseguire lo script Create-PAWOUs.ps1**.  Questo script crea la struttura di una nuova unità organizzativa (OU) in Active Directory e blocca l'ereditarietà degli oggetti Criteri di gruppo nelle nuove OU come opportuno.
3. **Eseguire lo script Create-PAWGroups.ps1**.  Questo script crea i nuovi gruppi di sicurezza globale nelle OU appropriate.

   > [!NOTE]
   > Lo script crea i nuovi gruppi di sicurezza ma non li popola automaticamente.

4. **Eseguire lo script Set-PAWOUDelegation.ps1**.  Questo script assegna autorizzazioni ai gruppi appropriati delle nuove OU.

#### <a name="move-tier-0-accounts-to-the-admintier-0accounts-ou"></a>Spostare gli account di livello 0 nella OU Admin\Tier 0\Accounts

Spostare ogni account membro dei gruppi Domain Admin, Enterprise Admins o equivalenti di livello 0, incluse le appartenenze annidate, in questa OU. Se per l'organizzazione a questi gruppi sono stati aggiunti gruppi personalizzati, è necessario spostare questi ultimi nella OU Admin\Tier 0\Groups.

   > [!NOTE]
   > Per altre informazioni sui gruppi di livello 0, vedere "Tier 0 Equivalency" (Equivalenza al livello 0) in [Securing Privileged Access Reference Material](../securing-privileged-access/securing-privileged-access-reference-material.md) (Materiale di riferimento per la protezione dell'accesso con privilegi).

#### <a name="add-the-appropriate-members-to-the-relevant-groups"></a>Aggiungere i membri appropriati ai gruppi pertinenti

1. **PAW Users** (Utenti PAW): aggiungere gli amministratori di livello 0 dei gruppi Domain Admins o Enterprise Admins identificati nel passaggio 1 della fase 1.
2. **PAW Maintenance** (Manutenzione PAW): aggiungere almeno un account da usare per le attività di manutenzione e risoluzione dei problemi dell'ambiente PAW. Gli account di manutenzione PAW verranno usati solo raramente.

   > [!NOTE]
   > Non aggiungere uno stesso account utente o gruppo di utenti sia al gruppo PAW Users (Utenti PAW) che al gruppo PAW Maintenance (Manutenzione PAW).  Il modello di sicurezza PAW si basa in parte sul presupposto che l'account utente PAW disponga di diritti con privilegi sui sistemi gestiti o sull'ambiente PAW stesso, ma non su entrambi.
   >
   > * Questo è importante per la creazione di procedure e prassi amministrative corrette nella fase 1.
   > * Ciò ha un'importanza critica per la fase 2 e le fasi successive, per evitare l'escalation dei privilegi attraverso PAW man mano che le workstation PAW si estendono su più livelli.
   >
   > In teoria, secondo il principio della separazione dei compiti, a nessun membro del personale devono essere assegnati compiti su più livelli. Microsoft tuttavia riconosce che, per la limitatezza del personale disponibile o per altri motivi organizzativi, per molte organizzazioni una separazione totale non è possibile. In questi casi, agli stessi membri del personale possono essere assegnati entrambi i ruoli, ma senza usare lo stesso account per le funzioni corrispondenti.

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>Creare l'oggetto Criteri di gruppo "PAW Configuration – Computer" (GPO)

In questa sezione si creerà un nuovo "PAW Configuration – Computer" oggetto Criteri di gruppo che fornirà protezione specifica per queste workstation Paw e crea un collegamento all'unità Organizzativa di dispositivi di livello 0 ("dispositivi" in Tier 0\Admin).

   > [!NOTE]
   > **Non aggiungere queste impostazioni ai criteri di dominio predefiniti**.  In caso contrario si potrebbero avere conseguenze negative sul funzionamento dell'intero ambiente Active Directory.  Configurare queste impostazioni solo per gli oggetti Criteri di gruppo appena creati descritti qui e applicarle solo alla OU PAW.

1. **PAW Maintenance Access** (Accesso manutenzione PAW): questa impostazione definisce l'appartenenza a gruppi con privilegi specifici per le workstation PAW di un set di utenti specifico. Passare a *Configurazione computer\Preferenze\Impostazioni del Pannello di controllo\Utenti e gruppi locali* e attenersi alla procedura seguente:
   1. Fare clic su **Nuovo** e quindi su **Gruppo locale**
   2. Selezionare l'azione **Aggiorna** e selezionare "Administrators (predefinito)". Non usare il pulsante Sfoglia per selezionare il gruppo di dominio Administrators.
   3. Selezionare le caselle di controllo **Elimina tutti gli utenti membri** ed **Elimina tutti i gruppi membri**
   4. Aggiungere PAW Maintenance (Manutenzione PAW, pawmaint) e Administrator. Anche in questo caso non usare il pulsante Sfoglia per selezionare Administrator.

      > [!NOTE]
      > Non aggiungere il gruppo PAW Users (Utenti PAW) all'elenco appartenenze per il gruppo Administrators locale.  Per assicurarsi che il gruppo PAW Users (Utenti PAW) non modifichi accidentalmente o intenzionalmente le impostazioni di sicurezza dell'ambiente PAW stesso, i membri di questo gruppo non devono essere membri del gruppo Administrators locale.
      >
      > Per altre informazioni sull'uso delle preferenze di Criteri di gruppo per modificare le appartenenze ai gruppi, fare riferimento all'articolo di TechNet [Configurare un elemento Gruppo locale](https://technet.microsoft.com/library/cc732525.aspx).

2. **Restrict Local Group Membership** (Limita appartenenza a gruppi locali): questa impostazione garantisce che la workstation non appartenga mai a gruppi di amministratori locali
   1. Passare a Configurazione computer\Preferenze\Impostazioni del Pannello di controllo\Utenti e gruppi locali e attenersi alla procedura seguente:
      1. Fare clic su **Nuovo** e quindi su **Gruppo locale**
      2. Selezionare l'azione **Aggiorna** e selezionare "Backup Operators (predefinito)". Non usare il pulsante Sfoglia per selezionare il gruppo di dominio Backup Operators.
      3. Selezionare le caselle di controllo **Elimina tutti gli utenti membri** ed **Elimina tutti i gruppi membri**.
      4. Non aggiungere membri al gruppo.  Fare semplicemente clic su **OK**.  Con l'assegnazione di un elenco vuoto i Criteri di gruppo rimuovono automaticamente tutti i membri e assicurano che l'elenco di appartenenze sia vuoto ogni volta che i Criteri di gruppo vengono aggiornati.
   2. Completare i passaggi precedenti per i gruppi aggiuntivi seguenti:
      * Operazioni di crittografia
      * Amministratori Hyper-V
      * Network Configuration Operators
      * Power Users
      * Utenti desktop remoto
      * Replicatori
   3. **Restrizioni accesso PAW** -questa impostazione limita gli account che possono accedere a PAW. Per configurare questa impostazione, attenersi alla procedura seguente:
      1. Passare a Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Consenti accesso locale.
      2. Selezionare Definisci queste impostazioni dei criteri e aggiungere "PAW Users" e gli amministratori (anche in questo caso, non usare il pulsante Sfoglia per selezionare gli amministratori).
   4. **Blocca traffico di rete in entrata**questa impostazione garantisce l'ambiente PAW non venga raggiunto da alcun traffico di rete in entrata indesiderato. Per configurare questa impostazione, attenersi alla procedura seguente:
      1. Passare a Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Windows Firewall con protezione avanzata\Windows Firewall con protezione avanzata e attenersi alla procedura seguente.
         1. Fare clic con il pulsante destro del mouse su Windows Firewall con protezione avanzata e selezionare **Importa criteri**.
         2. Fare clic su **Sì** per accettare la sovrascrittura dei criteri firewall eventualmente esistenti.
         3. Selezionare PAWFirewall.wfw e quindi selezionare **Apri**.
         4. Fare clic su **OK**.

            > [!NOTE]
            > A questo punto è possibile aggiungere indirizzi o subnet che devono raggiungere l'ambiente PAW con traffico indesiderato, ad esempio software di analisi di sicurezza o di gestione.
            > Le impostazioni nel file WFW abilitano la modalità "Blocca (predefinito)" per tutti i profili del firewall, disattivano l'unione di regole e consentono la registrazione sia dei pacchetti ignorati che di quelli ricevuti. Queste impostazioni verranno bloccare traffico indesiderato consentendo la comunicazione bidirezionale per le connessioni avviate da PAW, impedire agli utenti con accesso amministrativo locale di creare regole firewall locali che sostituiscono le impostazioni di GPO e Assicurarsi che il traffico da e verso la workstation PAW sia stato registrato.
            > **L'apertura di questo firewall amplierà la superficie di PAW esposta ad attacchi e aumenterà e rischi per la sicurezza. Prima di aggiungere gli indirizzi, vedere la sezione sulla gestione e sul funzionamento di PAW in questo materiale sussidiario**.

   5. **Configurare Windows Update per WSUS**: attenersi alla procedura seguente per modificare le impostazioni di configurazione di Windows Update per le workstation PAW:
      1. Passare a Computer Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\aggiornamenti e attenersi alla procedura seguente:
         1. Abilitare il **criterio Configura Aggiornamenti automatici**.
         2. Selezionare l'opzione **4 - Download automatico e pianificazione dell'installazione**.
         3. Modificare l'opzione **Giorno pianificato per l'installazione** in **0 - Ogni giorno** e l'opzione **Orario pianificato per l'installazione** sull'orario preferito dall'organizzazione.
         4. Abilitare l'opzione **specificare la posizione del servizio di aggiornamento Microsoft intranet** criteri, e specificare in entrambe le opzioni l'URL del server ESAE WSUS.
   6. Collegamento "PAW Configuration – Computer" oggetto Criteri di gruppo come indicato di seguito:

         |Condizione|Percorso collegamento|
         |-----|---------|
         |PAW Configuration - Computer (Configurazione PAW- Computer) |Admin\Tier 0\Devices|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>Creare l'oggetto Criteri di gruppo "PAW Configuration – User" (GPO)

In questa sezione si creerà un nuovo "PAW Configuration – utente" oggetto Criteri di gruppo che fornirà protezione specifica per queste workstation Paw e un collegamento all'unità organizzativa degli account di livello 0 ("Accounts" in Tier 0\Admin).

   > [!NOTE]
   > Non aggiungere queste impostazioni ai criteri di dominio predefiniti

1. **Block internet browsing** (Blocca esplorazione Internet): per evitare l'esplorazione accidentale di Internet, questa impostazione deve riportare un indirizzo proxy di un indirizzo di loopback (127.0.0.1).
   1. Passare alla configurazione utente windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** > **elemento Registro di sistema** e configurare le impostazioni seguenti:
      1. Azione:  Sostituzione
      2. Hive: HKEY_CURRENT_USER
      3. Percorso della chiave:  Software\microsoft\windows\currentversion\impostazioni Internet
      4. Nome valore: ProxyEnable

         > [!NOTE]
         > Non selezionare la casella Predefinito a sinistra del nome valore.

      5. Tipo di valore: REG_DWORD
      6. Dati valore: 1
         1. Fare clic sulla scheda Comune e selezionare **Rimuovi elemento quando non viene più applicato**.
         2. Nella scheda Comune selezionare **Destinazione a livello di elemento** e fare clic su **Destinazione**.
         3. Fare clic su **Nuovo elemento** e selezionare **Gruppo di sicurezza**.
         4. Selezionare il pulsante "..." e cercare il gruppo PAW Users (Utenti PAW).
         5. Fare clic su **Nuovo elemento** e selezionare **Gruppo di sicurezza**.
         6. Selezionare il pulsante "..." e cercare il gruppo **Cloud Services Admins** (Amministratori servizi cloud).
         7. Fare clic sull'elemento **Cloud Services Admins** (Amministratori servizi cloud) e quindi su **Opzioni elemento**.
         8. Selezionare **Diverso da**.
         9. Fare clic su **OK** nella finestra di destinazione.
      7. Fare clic su **OK** per completare l'impostazione dei Criteri di gruppo ProxyEnable
   2. Passare alla configurazione utente windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** > **elemento Registro di sistema** e configurare le impostazioni seguenti:

      * Azione: Sostituzione
      * Hive: HKEY_CURRENT_USER
      * Percorso della chiave: Software\microsoft\windows\currentversion\impostazioni Internet
         * Nome valore: ProxyServer

            > [!NOTE]
            > Non selezionare la casella Predefinito a sinistra del nome valore.

         * Tipo di valore: REG_SZ
         * Dati valore: 127.0.0.1:80
            1. Fare clic sulla scheda **Comune** e selezionare **Rimuovi elemento quando non viene più applicato**.
            2. Nella scheda **Comune** selezionare **Destinazione a livello di elemento** e fare clic su **Destinazione**.
            3. Fare clic su **Nuovo elemento** e selezionare Gruppo di sicurezza.
            4. Selezionare il pulsante "..." e aggiungere il gruppo PAW Users (Utenti PAW).
            5. Fare clic su **Nuovo elemento** e selezionare Gruppo di sicurezza.
            6. Selezionare il pulsante "..." e cercare il gruppo **Cloud Services Admins** (Amministratori servizi cloud).
            7. Fare clic sull'elemento **Cloud Services Admins** (Amministratori servizi cloud) e quindi su **Opzioni elemento**.
            8. Selezionare **Diverso da**.
            9. Fare clic su **OK** nella finestra di destinazione.

   3. Fare clic su **OK** per completare l'impostazione dei Criteri di gruppo ProxyServer.
2. Passare a utente Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Internet Explorer e abilitare le opzioni seguenti. Queste impostazioni impediscono agli amministratori di sostituire manualmente le impostazioni del server proxy.
   1. Abilitare **Impedisci la modifica delle impostazioni di configurazione automatica**.
   2. Abilitare **Impedisci la modifica delle impostazioni del server proxy**.

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>Limitare gli amministratori di accesso agli host di livello inferiore

In questa sezione verranno configurati Criteri di gruppo per impedire agli account amministrativi con privilegi di accedere a host di livello inferiore.

1. Creare il nuovo oggetto Criteri di gruppo **Restrict Workstation Logon** (Limita accesso workstation): questa impostazione impedisce agli account amministratore di livello 0 e 1 di accedere alle workstation standard.  Questo oggetto Criteri di gruppo deve essere collegato all'unità organizzativa di primo livello "Workstation" e sono disponibili le seguenti impostazioni:
   * Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come processo batch, selezionare **Definisci queste impostazioni dei criteri** e aggiungere i gruppi di livello 1 e il livello 0:     Enterprise Domain Admins Schema Admins DOMAIN\Administrators amministratori Account Operators gruppo Backup Operators stampa operatori Server operatori dominio controller gruppo controller di dominio di sola lettura dei criteri autori proprietari crittografia Oper ators

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

         Tier 1 Admins

         > [!NOTE]
         > This Group was created earlier in Phase 1.

   * Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come servizio, selezionare **Definisci queste impostazioni dei criteri** e aggiungere i gruppi di livello 1 e il livello 0:     Enterprise Domain Admins Schema Admins DOMAIN\Administrators amministratori Account Operators gruppo Backup Operators stampa operatori Server operatori dominio controller gruppo controller di dominio di sola lettura dei criteri autori proprietari crittografia Oper ators

         > [!NOTE]
         > Note: Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Note: Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

         Tier 1 Admins

         > [!NOTE]
         > Note: This Group was created earlier in Phase 1

2. Creare il nuovo **limitare l'accesso al Server** oggetto Criteri di gruppo - questa impostazione limita gli account degli amministratori di livello 0 dall'accesso al server di livello 1.  Questo oggetto Criteri di gruppo deve essere collegato all'unità organizzativa di primo livello "Livello di server 1" e sono disponibili le seguenti impostazioni:
   * Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come processo batch, selezionare **Definisci queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:     Enterprise Domain Admins Schema Admins DOMAIN\Administrators amministratori Account Operators gruppo Backup Operators stampa operatori Server operatori dominio controller gruppo controller di dominio di sola lettura dei criteri autori proprietari crittografia Oper ators

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

   * Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come servizio, selezionare **Definisci queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:     Enterprise Domain Admins Schema Admins DOMAIN\Administrators amministratori Account Operators gruppo Backup Operators stampa operatori Server operatori dominio controller gruppo controller di dominio di sola lettura dei criteri autori proprietari crittografia Oper ators

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

   * In Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso in locale, selezionare **Definisci queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:     Enterprise Domain Admins Schema Admins Account Admins operatori gruppo Backup Operators operatori di stampa operatori Server operatori dominio controller gruppo controller di dominio di sola lettura dei criteri autori proprietari crittografia

         > [!NOTE]
         > Note: Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Note: Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

#### <a name="deploy-your-paws"></a>Distribuire le workstation PAW

   > [!NOTE]
   > Assicurarsi che la workstation PAW sia disconnessa dalla rete durante il processo di build del sistema operativo.

1. Installare Windows 10 usando il supporto di installazione di origine pulita ottenuto in precedenza.

   > [!NOTE]
   > Per automatizzare la distribuzione PAW è possibile usare il toolkit MDT (Microsoft Deployment Toolkit) o un altro sistema di distribuzione automatizzata di immagini, ma è necessario assicurarsi che il processo di build sia attendibile tanto quanto PAW. Gli antagonisti cercano in particolare immagini e sistemi di distribuzione aziendali (inclusi i file ISO, i pacchetti di distribuzione e così via), in quanto meccanismi di salvataggio permanenti. Non devono quindi essere usati sistemi di distribuzione o immagini preesistenti.
   >
   > Se si intende automatizzare la distribuzione PAW è necessario:
   >
   > * Eseguire la build del sistema tramite supporti di installazione convalidati sulla base delle indicazioni riportate in [Clean Source for installation media](https://aka.ms/cleansource) (Origine pulita per i supporti di installazione).
   > * Assicurarsi che il sistema di distribuzione automatizzata sia disconnesso dalla rete durante il processo di build del sistema operativo.

2. Impostare una password complessa univoca per l'account Administrator locale.  Non usare una password usata per un altro account nell'ambiente.

   > [!NOTE]
   > Microsoft consiglia di usare [Local Administrator Password Solution (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) per gestire la password dell'account Administrator locale per tutte le workstation, incluse le workstation PAW.  Se si usa la soluzione LAPS, assicurarsi di concedere solo al gruppo PAW Maintenance (Manutenzione PAW) il diritto di lettura delle password delle workstation PAW gestite da LAPS.

3. Installare Strumenti di amministrazione remota del server per Windows 10 tramite il supporto di installazione di origine pulita.
4. Configura Windows Defender Exploit Guard

   > [!NOTE]
   > Linee guida di configurazione da seguire

5. Connettere la workstation PAW alla rete.  Assicurarsi che la workstation PAW possa connettersi ad almeno un controller di dominio.
6. Usando un account membro del gruppo PAW Maintenance (Manutenzione PAW) eseguire il comando PowerShell seguente dalla workstation PAW appena creata per aggiungerla al dominio nella OU appropriata:

   `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

   Sostituire appropriatamente i riferimenti a *Fabrikam* con il nome di dominio in uso.  Se il nome di dominio si estende su più livelli (ad esempio child.fabrikam.com), per i nomi aggiuntivi usare l'identificatore "DC=", inserendo i nomi aggiuntivi nell'ordine in cui compaiono nel nome di dominio completo del dominio stesso.

   > [!NOTE]
   > Se è stata distribuita una [foresta amministrativa ESAE ](https://aka.ms/esae) (per gli amministratori di livello 0 nella fase 1) o [Privileged Access Management (PAM) di Microsoft Identity Manager (MIM)](https://aka.ms/mimpamdeploy) (per gli amministratori di livello 1 e 2 nella fase 2), è a questo punto necessario aggiungere la workstation PAW al dominio in tale ambiente anziché al dominio di produzione.

7. Applicare tutti gli aggiornamenti di Windows critici e importanti prima di installare qualsiasi altro software, inclusi gli strumenti di amministrazione, gli agenti e così via.
8. Forzare l'applicazione di Criteri di gruppo.
   1. Aprire un prompt dei comandi con privilegi elevati e immettere il comando seguente: `Gpupdate /force /sync`
   2. Riavviare il computer.

9. (Facoltativo) Installare gli strumenti aggiuntivi necessari per gli amministratori di Active Directory. Installare eventuali altri strumenti o script necessari per l'esecuzione delle attività di lavoro. Prima di aggiungere uno strumento a una workstation PAW, valutare il rischio di esposizione delle credenziali a cui tale strumento potrebbe esporre la workstation stessa. Accedere a [questa pagina](https://aka.ms/logontypes) per altre informazioni sulla valutazione del rischio di esposizione delle credenziali degli strumenti di amministrazione e dei metodi di connessione. Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in [Clean Source for installation media](https://aka.ms/cleansource) (Origine pulita per i supporti di installazione).

   > [!NOTE]
   > L'uso di un jump server come posizione centralizzata per questi strumenti può ridurre la complessità, anche se non funge da limite di sicurezza.

10. (Facoltativo) Scaricare e installare il software richiesto l'accesso remoto. Se per l'amministrazione gli amministratori useranno le workstation PAW in remoto, installare il software di accesso remoto rispettando le indicazioni di sicurezza del fornitore della soluzione di accesso remoto. Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in Clean Source for installation media (Origine pulita per i supporti di installazione).

    > [!NOTE]
    > Valutare con attenzione tutti i rischi coinvolti nel consentire l'accesso remoto tramite una workstation PAW.  Un sistema PAW mobile consente molti scenari di una certa importanza, tra cui il lavoro da casa, ma il software di accesso remoto può essere vulnerabile agli attacchi e usato per compromettere una workstation PAW.

11. Per verificare l'integrità del sistema PAW, esaminare quest'ultimo per confermare che siano attivate tutte le impostazioni appropriate. A tale scopo usare la procedura seguente:
    1. Verificare che alla workstation PAW siano applicate solo Criteri di gruppo specifici dell'architettura PAW
       1. Aprire un prompt dei comandi con privilegi elevati e immettere il comando seguente: `Gpresult /scope computer /r`
       2. Esaminare l'elenco risultante e verificare che gli unici Criteri di gruppo presenti siano quelli creati in precedenza.
    2. Verificare che nessun account utente aggiuntivo sia membro di gruppi con privilegi della workstation PAW. A tale scopo, attenersi alla procedura seguente:
       1. Aprire **Modifica utenti e gruppi locali** (lusrmgr.msc), selezionare **Gruppi** e verificare che gli unici membri del gruppo Administrators locale siano l'account Administrator locale e il gruppo di sicurezza globale PAW Maintenance (Manutenzione PAW).

          > [!NOTE]
          > Il gruppo PAW Users (Utenti PAW) non deve essere membro del gruppo Administrators locale.  Gli unici membri devono essere l'account Administrator locale e il gruppo di sicurezza globale PAW Maintenance (Manutenzione PAW). Il gruppo PAW Users (Utenti PAW) non deve essere membro neanche di questo gruppo globale.

       2. Sempre tramite **Modifica utenti e gruppi locali** assicurarsi che i gruppi seguenti non contengano membri: Amministratori di Hyper-V al gruppo Backup Operators operazioni di crittografia di rete configurazione operatori Power Users gruppo utenti Desktop remoto replicatori

12. (Facoltativo) Se l'organizzazione Usa una soluzione di gestione (SIEM) di eventi e le informazioni di sicurezza, assicurarsi che la workstation PAW sia [configurato per inoltrare gli eventi di sistema usando Windows Event Forwarding (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) o in caso contrario, viene registrato con il soluzione in modo che il soluzione SIEM riceva attivamente eventi e informazioni dalla workstation PAW.  I dettagli di questa operazione dipendono dalla soluzione SIEM in uso.

    > [!NOTE]
    > Se la soluzione SIEM richiede l'esecuzione nella workstation PAW di un agente come account di sistema o amministratore locale, assicurarsi che le soluzioni SIEM vengano gestite con lo stesso livello di attendibilità dei controller di dominio e dei sistemi di gestione delle identità.

13. (Facoltativo) Se si sceglie di distribuire LAPS per gestire la password per l'account Administrator locale nella workstation PAW, verificare che la password sia registrata correttamente.

    * Tramite un account con autorizzazioni per la lettura di password gestite con LAPS aprire **Utenti e computer di Active Directory** (dsa.msc).  Assicurarsi che l'opzione Funzionalità avanzate sia abilitata e fare clic con il pulsante destro del mouse sull'oggetto computer appropriato.  Selezionare la scheda Editor attributi e verificare che il valore per msSVSadmPwd corrisponda a una password valida.

### <a name="phase-2-extend-paw-to-all-administrators"></a>Fase 2: Estensione di PAW a tutti gli amministratori

Ambito: Tutti gli utenti con diritti amministrativi su applicazioni mission-critical e le dipendenze.  Questi devono corrispondere almeno agli amministratori dei server applicazioni, delle soluzioni di monitoraggio dell'integrità operativa e della sicurezza, delle soluzioni di virtualizzazione, dei sistemi di archiviazione e dei dispositivi di rete.

> [!NOTE]
> Le istruzioni relative a questa fase presuppongono che la fase 1 sia stata interamente completata.  Iniziare la fase 2 solo dopo avere completato tutti i passaggi nella fase 1.

Dopo aver verificato l'esecuzione di tutti i passaggi, eseguire la procedura seguente per completare la fase 2:

#### <a name="recommended-enable-restrictedadmin-mode"></a>(Scelta consigliata) Abilitare **RestrictedAdmin** modalità

Abilitare questa funzionalità nel server esistenti e le workstation e quindi imporre l'uso di questa funzionalità. Questa funzionalità richiede il server di destinazione sia in esecuzione Windows Server 2008 R2 o versione successiva e nelle workstation in esecuzione Windows 7 o versione successiva di destinazione.

1. Abilitare la modalità **RestrictedAdmin** nei server e nelle workstation seguendo le istruzioni disponibili in questa [pagina](https://aka.ms/rdpra).

   > [!NOTE]
   > Prima di abilitare questa funzionalità per i server con connessione Internet, è necessario valutare il rischio che eventuali antagonisti siano in grado di effettuare l'autenticazione a tali server con l'hash di una password rubata in precedenza.

2. Creare l'oggetto Criteri di gruppo "RestrictedAdmin Required - Computer" (RestrictedAdmin necessaria - Computer). In questa sezione viene creato un oggetto Criteri di gruppo che impone l'uso dell'opzione /RestrictedAdmin per le connessioni Desktop remoto in uscita, per proteggere gli account dal furto di credenziali nei sistemi di destinazione
   * Passare a Configurazione computer\Criteri\Modelli amministrativi\Sistema\Delega di credenziali\Limita delega delle credenziali ai server remoti e impostare l'opzione su **Abilitato**.
3. Collegare **RestrictedAdmin** Required - Computer (RestrictedAdmin necessaria - Computer) ai dispositivi di livello 1 e/o 2 appropriati usando le opzioni dei criteri seguenti:
   * PAW Configuration - Computer (Configurazione PAW- Computer)
      * -> Percorso collegamento: Admin\Tier 0\Devices (esistente)
   * PAW Configuration - User (Configurazione PAW- Utente)
      * -> Percorso collegamento: Nella OU Admin\Tier 0\Accounts
   * RestrictedAdmin Required - Computer (RestrictedAdmin necessaria - Computer)
      * -> Admin\Tier1\Devices o -> Admin\Tier2\Devices (entrambi costituiscono facoltativi)

   > [!NOTE]
   > Questo non è necessario per i sistemi di livello 0, che hanno già il controllo completo di tutte le risorse nell'ambiente.

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>Spostare gli oggetti di livello 1 nelle OU appropriate

1. Spostare i gruppi di livello 1 nella OU Admin\Tier 1\Groups. Individuare tutti i gruppi che concedono i diritti amministrativi seguenti e spostarli in questa OU.
   * Amministratore locale in più server
      * Accesso amministrativo a servizi cloud
      * Accesso amministrativo ad applicazioni aziendali
2. Spostare gli account di livello 1 nella OU Admin\Tier 1\Accounts. Spostare ogni account membro di questi gruppi di livello 1 (incluse le appartenenze annidate) in questa OU.
3. Aggiungere i membri appropriati ai gruppi pertinenti
   * **Tier 1 Admins** (Amministratori livello 1): questo gruppo conterrà gli amministratori di livello 1 a cui non è consentito l'accesso agli host di livello 2. Aggiungere tutti i gruppi amministrativi 1 livello che dispongano di privilegi amministrativi nel server o servizi internet.

      > [!NOTE]
      > Se i compiti del personale amministrativo prevedono la gestione di risorse di più livelli, è necessario creare un account amministrativo separato per ogni livello.

4. Abilitare Credential Guard per ridurre il rischio di furto e riuso di credenziali.  Credential Guard è una nuova funzionalità di Windows 10 che limita l'accesso delle applicazioni alle credenziali, evitando così gli attacchi corrispondenti al furto delle credenziali (inclusi gli attacchi Pass-the-Hash).  Credential Guard è completamente trasparente per l'utente finale e la sua installazione richiede pochissimo tempo e un impegno minimo.  Per altre informazioni su Credential Guard, tra cui la procedura di distribuzione e i requisiti hardware, fare riferimento all'articolo [Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > Per configurare e usare Credential Guard è necessario abilitare Device Guard.  Tuttavia, per usare Credential Guard non è necessario configurare altre protezioni di Device Guard.

5. (Facoltativo) Abilitare la connettività ai servizi Cloud. Questo passaggio consente la gestione di servizi cloud quali Azure e Office 365 con garanzie di sicurezza appropriate. Questo passaggio è necessario anche per la gestione delle workstation PAW con Microsoft Intune.

   > [!NOTE]
   > Ignorare questo passaggio se non è necessaria la connettività al cloud per l'amministrazione dei servizi cloud o la gestione tramite Intune.

   * Questi passaggi limitano la comunicazione tramite Internet ai soli servizi cloud autorizzati (ma non alla rete Internet "aperta") e aggiungono misure di protezione ai browser e ad altre applicazioni che elaborano contenuto proveniente da Internet. Le workstation PAW per l'amministrazione non devono mai essere usate per attività utente standard come le comunicazioni e le attività produttive tramite Internet.
   * Per consentire la connettività ai servizi PAW attenersi alla procedura seguente:

   1. Configurare la workstation PAW per consentire solo destinazioni Internet autorizzate.  Man mano che si estende la distribuzione PAW per abilitare l'amministrazione cloud, è necessario consentire l'accesso ai servizi autorizzati, escludendo nello stesso tempo l'accesso a Internet "aperta", da cui è più probabile che provengano attacchi contro gli amministratori.

      1. Creare **Cloud Services Admins** gruppo e aggiungervi tutti gli account che richiedono l'accesso ai servizi cloud su internet.
      2. Scaricare il file *proxy.pac* per PAW dalla [Raccolta TechNet](https://aka.ms/pawmedia) e pubblicarlo in un sito Web interno.

         > [!NOTE]
         > Sarà necessario aggiornare il file *proxy.pac* dopo il download per assicurarsi che sia aggiornato e completo.  
         > Microsoft pubblica tutti gli URL relativi a Office 365 e ad Azure nel sito di [supporto tecnico per Office](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US). Queste istruzioni presuppongono che per l'amministrazione di Office 365, Azure e di altri servizi cloud si usi Internet Explorer o Microsoft Edge. Microsoft consiglia di configurare restrizioni simili per tutti i browser di terze parti necessari per l'amministrazione. Nelle workstation PAW i Web browser devono essere usati solo per l'amministrazione dei servizi cloud e mai semplicemente per esplorare il Web.
         >
         > Potrebbe essere necessario aggiungere all'elenco altre destinazioni Internet valide corrispondenti ad altri provider IaaS. All'elenco non devono però essere aggiunti siti di produttività, intrattenimento, notizie o ricerca.
         >
         > Potrebbe anche essere necessario modificare il file PAC per aggiungere un indirizzo proxy valido da usare per questi indirizzi.
         >
         > Per una difesa ancora più solida è possibile limitare l'accesso dalla workstation PAW anche tramite proxy Web. Non è consigliabile usare quest'ultimo autonomamente, senza il file PAC, perché per le workstation PAW l'accesso verrebbe limitato solo quando sono connesse alla rete aziendale.

      3. Dopo aver configurato il file *proxy.pac* aggiornare l'oggetto Criteri di gruppo PAW Configuration - User (Configurazione PAW- Utente).
         1. Passare alla configurazione utente windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** > **elemento Registro di sistema** e configurare le impostazioni seguenti:
            1. Azione: Sostituzione
            2. Hive: HKEY_ CURRENT_USER
            3. Percorso della chiave: Software\microsoft\windows\currentversion\impostazioni Internet
            4. Nome valore: AutoConfigUrl

               > [!NOTE]
               > Non selezionare la casella **Predefinito** a sinistra del nome valore.

            5. Tipo di valore: REG_SZ
            6. Dati valore: immettere l'URL completo per il *PAC* del file, tra cui http:// e il nome del file, ad esempio http://proxy.fabrikam.com/proxy.pac.  L'URL può essere anche un URL con etichetta singola, ad esempio, http://proxy/proxy.pac

               > [!NOTE]
               > Il file PAC può anche essere ospitato in una condivisione file con la sintassi file://server.fabrikan.com/share/proxy.pac ma ciò richiede che sia consentito il protocollo file://. Vedere la "Nota: Sezione file://-based Proxy script obsoleto"di questo [Understanding Web Proxy Configuration](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog per altri dettagli sulla configurazione del valore del Registro di sistema.

            7. Fare clic sulla scheda **Comune** e selezionare **Rimuovi elemento quando non viene più applicato**.
            8. Nella scheda **Comune** selezionare **Destinazione a livello di elemento** e fare clic su **Destinazione**.
            9. Fare clic su **Nuovo elemento** e selezionare **Gruppo di sicurezza**.
            10. Selezionare il pulsante "..." e cercare il gruppo **Cloud Services Admins** (Amministratori servizi cloud).
            11. Fare clic su **Nuovo elemento** e selezionare **Gruppo di sicurezza**.
            12. Selezionare il pulsante "..." e cercare il gruppo **PAW Users** (Utenti PAW).
            13. Fare clic sull'elemento**PAW Users** (Utenti PAW) e fare clic su **Opzioni elemento**.
            14. Selezionare **Diverso da**.
            15. Fare clic su **OK** nella finestra di destinazione.
            16. Fare clic su **OK** per completare l'impostazione dei Criteri di gruppo **AutoConfigUrl**.
   2. Applicare le baseline di sicurezza di Windows 10 e l'accesso ai servizi cloud. Collegare le baseline di sicurezza per Windows e per l'accesso ai servizi cloud (se necessario) alle OU corrette tramite la procedura seguente:
      1. Estrarre il contenuto del file ZIP delle baseline di sicurezza di Windows 10.
      2. Creare questi oggetti Criteri di gruppo, [importare le impostazioni dei criteri](https://technet.microsoft.com/library/cc753786.aspx) e [collegarle](https://technet.microsoft.com/library/cc732979.aspx) secondo le indicazioni di questa tabella. Collegare ciascun criterio a ciascun percorso e assicurarsi di seguire l'ordine della tabella (le voci più in basso nella tabella devono essere applicate dopo e con una priorità più alta):

         **Criteri:**

         |||
         |-|-|
         |CM Windows 10 - Domain Security (CM Windows 10 - Sicurezza del dominio)|N/A. Non collegare adesso|
         |SCM Windows 10 TH2 - Computer|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 TH2- BitLocker|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 - Credential Guard|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Internet Explorer - Computer|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |PAW Configuration - Computer (Configurazione PAW- Computer)|Admin\Tier 0\Devices (esistente)|
         ||Admin\Tier 1\Devices (nuovo collegamento)|
         ||Admin\Tier 2\Devices (nuovo collegamento)|
         |RestrictedAdmin Required - Computer (RestrictedAdmin necessaria - Computer)|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 - User (SCM Windows 10 - Utente)|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Internet Explorer - User (SCM Internet Explorer - Utente)|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |PAW Configuration - User (Configurazione PAW- Utente)|Admin\Tier 0\Devices (esistente)|
         ||Admin\Tier 1\Devices (nuovo collegamento)|
         ||Admin\Tier 2\Devices (nuovo collegamento)|

         > [!NOTE]
         > L'oggetto Criteri di gruppo "SCM Windows 10 - Domain Security" (SCM Windows 10 - Sicurezza del dominio) può essere collegato al dominio indipendentemente dall'ambiente PAW, ma influisce sull'intero dominio.

6. (Facoltativo) Installare gli strumenti aggiuntivi necessari per gli amministratori di livello 1. Installare eventuali altri strumenti o script necessari per l'esecuzione delle attività di lavoro. Prima di aggiungere uno strumento a una workstation PAW, valutare il rischio di esposizione delle credenziali a cui tale strumento potrebbe esporre la workstation stessa. Per altre informazioni sulla valutazione del rischio di esposizione delle credenziali degli strumenti di amministrazione e dei metodi di connessione, visitare [questa pagina](https://aka.ms/logontypes). Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in Clean Source for installation media (Origine pulita per i supporti di installazione)
7. Identificare e ottenere in modo sicuro il software e le applicazioni necessarie per l'amministrazione.  Si tratta di un'attività simile alle operazioni eseguite nella fase 1, ma con un ambito più ampio a causa del maggior numero di applicazioni, servizi e sistemi da proteggere.

   > [!NOTE]
   > Assicurarsi di proteggere le nuove applicazioni (inclusi i browser web) scegliendo nelle misure di protezione fornite da Windows Defender Exploit Guard.

   Alcuni esempi di applicazioni e software aggiuntivi sono:

      * Microsoft Azure PowerShell
      * PowerShell di Office 365 (noto anche come Modulo dei Microsoft Online Services)
      * Software di gestione di applicazioni o servizi basato su MMC (Microsoft Management Console)
      * Software di gestione di applicazioni o servizi proprietario (non basato su MMC)

         > [!NOTE]
         > Molte applicazioni vengono ora gestite esclusivamente tramite Web browser, inclusi molti servizi cloud.  Ciò consente di ridurre il numero di applicazioni che devono essere installate in una workstation PAW ma introduce anche il rischio di problemi di interoperabilità del browser.  Potrebbe essere necessario distribuire un Web browser non Microsoft in istanze PAW specifiche per consentire l'amministrazione di servizi particolari.  Se si distribuisce un Web browser aggiuntivo, assicurarsi di seguire tutti i principi di origine pulita e di proteggere il browser in base alle indicazioni di sicurezza del fornitore.

8. (Facoltativo) Scaricare e installare gli agenti di gestione richiesto.

   > [!NOTE]
   > Se si sceglie di installare agenti di gestione aggiuntivi (per la gestione del monitoraggio, della sicurezza, della configurazione e così via), è fondamentale assicurarsi che i sistemi di gestione siano attendibili allo stesso livello dei controller di dominio e dei sistemi di gestione delle identità. Per altre indicazioni, vedere Gestione e aggiornamento delle workstation PAW.

9. Valutare l'infrastruttura per identificare i sistemi che richiedono le misure di protezione della sicurezza aggiuntive offerte dall'ambiente PAW.  Assicurarsi di conoscere esattamente quali sistemi debbano essere protetti.  Porre domande critiche sulle risorse stesse, ad esempio:

   * Dove sono i sistemi di destinazione che devono essere gestiti?  Sono raccolti in un'unica posizione fisica o connessi a una singola subnet ben definita?
   * Quanti sistemi sono presenti?
   * Questi sistemi dipendono altri sistemi (di virtualizzazione, di aviazione e così via) e, in tal caso, come vengono gestiti?  In che modo i sistemi critici sono esposti a queste dipendenze e quali sono i rischi aggiuntivi associati a queste?
   * Fino a che punto sono critici i servizi gestiti e qual è la perdita prevista in caso di compromissione di tali servizi?

      > [!NOTE]
      > Includere i servizi cloud in questa valutazione: gli utenti malintenzionati prendono sempre più di mira le distribuzioni cloud non sicure ed è fondamentale amministrare tali servizi nello stesso modo sicuro impiegato per le applicazioni critiche locali.

        Usare questa valutazione per identificare i sistemi specifici che richiedono una protezione aggiuntiva e quindi estendere il programma PAW agli amministratori di tali sistemi.  Esempi comuni di sistemi che traggono grandi vantaggi dall'amministrazione basata su PAW sono i database SQL Server (locali e SQL Azure), le applicazioni per la gestione delle risorse umane e il software finanziario.

        > [!NOTE]
        > Se una risorsa è gestita tramite un sistema Windows, può essere gestita con una workstation PAW, anche se l'applicazione vera e propria viene eseguita in un sistema operativo diverso da Windows o in una piattaforma cloud non Microsoft.  Ad esempio, il proprietario di un abbonamento Amazon Web Services dovrebbe usare solo una workstation PAW per amministrare tale account.

10. Sviluppare un metodo di richiesta e di distribuzione per la distribuzione di workstation PAW su larga scala nell'organizzazione.  A seconda del numero di workstation PAW che si sceglie di distribuire nella fase 2, potrebbe essere necessario automatizzare il processo.

    * Prendere in considerazione la possibilità di sviluppare un processo formale di richiesta e approvazione utilizzabile da parte degli amministratori per ottenere una workstation PAW.  Questo processo consentirebbe di standardizzare il processo di distribuzione, assicurare l'attendibilità dei dispositivi PAW e facilitare l'identificazione di lacune nella distribuzione PAW.
    * Come affermato in precedenza, questa soluzione di distribuzione deve essere separata dai metodi di automazione esistente (che potrebbero già essere stati compromessi) e seguire i principi descritti per la fase 1.

        > [!NOTE]
        > Tutti i sistemi che gestiscono risorse devono essere gestiti a un livello di attendibilità uguale o superiore.

11. Rivedere e se necessario distribuire profili hardware PAW aggiuntivi.  Il profilo hardware scelto per la distribuzione nella fase 1 potrebbe non essere appropriato per tutti gli amministratori.  Rivedere i profili hardware e, se appropriato, selezionare profili hardware PAW aggiuntivi per soddisfare le esigenze degli amministratori.  Ad esempio, il profilo Hardware dedicato (workstation PAW separate dalle workstation per uso quotidiano) potrebbe non essere adatto a un amministratore che viaggia spesso. In questo caso, per questo amministratore è consigliabile scegliere di distribuire il profilo Uso simultaneo (workstation PAW con VM utente).
12. Prendere in considerazione le esigenze legate alla lingua, alle funzioni operative, alle comunicazioni e alla formazione che accompagnano una distribuzione PAW estesa.   Una modifica così significativa a un modello amministrativo richiede naturalmente un certo grado di gestione del cambiamento ed è essenziale integrare tale aspetto nel progetto di distribuzione stesso.  Si consideri come minimo quanto segue:

    * Come comunicare le modifiche ai dirigenti senior per ottenerne il supporto?  Qualsiasi progetto privo del sostegno dei dirigenti senior è probabilmente destinato al fallimento o, come minimo, a una continua lotta per ottenere i fondi necessari ed essere ampiamente accettato.
    * Come verrà documentato il nuovo processo per gli amministratori?  Queste modifiche devono essere documentate e comunicate non solo agli amministratori esistenti (che dovranno modificare le proprie abitudini e gestire le risorse in modo diverso), ma anche ai nuovi amministratori (quelli promossi all'interno e quelli assunti dall'esterno dell'organizzazione).  È essenziale che la documentazione sia chiara e descriva in modo completo e articolato la portata delle minacce, il ruolo dell'ambiente PAW nella protezione degli amministratori e il modo corretto per l'uso dell'ambiente PAW.

      > [!NOTE]
      > Ciò è particolarmente importante per i ruoli con un turnover elevato; un solo esempio tra i molti possibili è costituito dal personale dell'help desk.

    * Come si garantirà la conformità al nuovo processo?  Mentre il modello PAW include vari controlli tecnici per evitare l'esposizione delle credenziali con privilegi, è impossibile evitare completamente tutti di esposizione affidandosi esclusivamente i controlli tecnici.  Ad esempio, sebbene sia possibile impedire a un amministratore di accedere a un desktop utente con credenziali con privilegi, il semplice fatto di tentare l'accesso può esporre le credenziali a malware eventualmente installato in quel desktop utente.  È pertanto essenziale descrivere in dettaglio non solo i vantaggi del modello PAW, ma anche i rischi legati alla mancata conformità.  Questo deve essere completato da controlli e avvisi, per consentire la rilevazione e la soluzione rapida di problemi di esposizione delle credenziali.

### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: Estendere e migliorare la protezione

Ambito: Queste protezioni migliorano i sistemi creati nella fase 1, potenziando la protezione di base con funzionalità avanzate tra cui multi-factor authentication e le regole di accesso di rete.

> [!NOTE]
> Questa fase può essere eseguita in qualsiasi momento dopo il completamento della fase 1.  Non dipende dal completamento della fase 2 e pertanto può essere eseguita prima, contemporaneamente o dopo la fase 2.

Per configurare questa fase, attenersi alla procedura seguente:

1. **Abilitare multi-factor authentication per gli account con privilegi**.  L'autenticazione a più fattori aumenta il livello di sicurezza degli account, richiedendo all'utente, oltre alle credenziali, di presentare un token fisico.  L'autenticazione a più fattori integra i criteri di autenticazione in modo molto efficace, ma non dipende dai criteri di autenticazione per la distribuzione e, in modo analogo, i criteri di autenticazione non richiedono l'autenticazione a più fattori.  Microsoft consiglia di usare una di queste forme di autenticazione a più fattori:

   * **Smart card**: Una smart card è un dispositivo fisico portatile e resistente alle manomissioni che offre una seconda verifica durante il processo di accesso di Windows.  Richiedendo a un utente di presentare una smart card per l'accesso, è possibile ridurre il rischio di furto di credenziali e di riuso di queste da remoto.  Per i dettagli sull'accesso con smart card a Windows, fare riferimento all'articolo [Panoramica delle smart card](https://technet.microsoft.com/library/hh831433.aspx).
   * **Smart card virtuale**:  Una smart card virtuale offre gli stessi vantaggi di sicurezza come smart card fisiche, con il vantaggio aggiunto del collegamento ad hardware specifico.  Per informazioni dettagliate sulla distribuzione e i requisiti hardware, consultare gli articoli [panoramica delle Smart Card virtuale](https://technet.microsoft.com/library/dn593708.aspx) e [iniziare con Smart card virtuali: Guida allo scenario](https://technet.microsoft.com/library/dn579260.aspx).
   * **Windows Hello for Business**: Windows Hello for Business consente agli utenti di eseguire l'autenticazione a un account Microsoft, un account Active Directory, un account Microsoft Azure Active Directory (Azure AD) o servizio non Microsoft che supporta l'autenticazione Fast ID Online (FIDO). Dopo una verifica iniziale in due passaggi durante Windows Hello per la registrazione aziendale, un Windows Hello for Business è configurato nel dispositivo dell'utente e l'utente imposta un movimento, come Windows Hello o un PIN. Windows Hello per le credenziali aziendali sono una coppia di chiavi asimmetrica, che può essere generata all'interno di ambienti isolati Trusted Platform Module (TPM).
      Per altre informazioni su Windows Hello per la lettura di Business [Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) articolo.
   * **Azure multi-factor authentication**:  Azure multi-factor authentication (MFA) offre la sicurezza di un secondo fattore di verifica, nonché protezione avanzata tramite Monitoraggio e machine learning-basato su analisi.  Azure MFA consente di proteggere non solo gli amministratori di Azure, ma anche molte altre soluzioni, tra cui applicazioni Web, Azure Active Directory e soluzioni locali come Desktop remoto e l'accesso remoto.  Per altre informazioni su Azure Multi-Factor Authentication, fare riferimento all'articolo [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication).

2. **Attendibili dell'elenco di applicazioni con controllo delle applicazioni di Windows Defender e/o AppLocker**.  Limitando la possibilità di eseguire codice non attendibile o non firmato in una workstation PAW, è possibile ridurre ancora di più la probabilità di attività di utenti malintenzionati e di compromissione.  Windows include due opzioni principali per il controllo delle applicazioni:

   * **AppLocker**:  AppLocker consente agli amministratori di controllare quali applicazioni possono essere eseguite in un sistema specifico.  AppLocker può essere controllato in modo centralizzato tramite Criteri di gruppo e applicato a utenti o gruppi specifici (per applicazioni destinate a utenti di workstation PAW).  Per altre informazioni su AppLocker, fare riferimento all'articolo [Panoramica tecnica di AppLocker](https://technet.microsoft.com/library/hh831440.aspx) in TechNet.
   * **Windows Defender Application Control**: la nuova funzionalità di Windows Defender Application Control fornisce il controllo di applicazioni basate su hardware avanzata che, a differenza di AppLocker, non può essere sottoposto a override nel dispositivo interessato.  Come AppLocker, Windows Defender Application Control può essere controllata tramite criteri di gruppo e destinarlo a utenti specifici.  Per altre informazioni sulla limitazione dell'utilizzo delle applicazioni con Windows Defender Application Control, consultare [Guida alla distribuzione di Windows Defender Application controllo](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).

3. **Usare il gruppo Protected Users, criteri di autenticazione e silo di autenticazione per proteggere maggiormente gli account con privilegi**.  I membri di Protected Users sono soggetti a criteri di sicurezza aggiuntivi che proteggono le credenziali archiviate all'interno dell'agente LSA (Local Security Agent, agente di sicurezza locale) e riducono notevolmente il rischio di furto di credenziali e il riuso di queste.  I criteri e i silo di autenticazione controllano il modo in cui gli utenti con privilegi possono accedere alle risorse nel dominio.  Tutte insieme queste protezioni sono in grado di rafforzare notevolmente la sicurezza degli account degli utenti con privilegi.  Per altri dettagli su queste funzionalità, fare riferimento all'articolo Web [How to Configure Protected Accounts](https://technet.microsoft.com/library/dn518179.aspx) (Come configurare gli account protetti).

   > [!NOTE]
   > Queste protezioni hanno lo scopo di completare, non di sostituire, le misure di sicurezza esistenti della fase 1.  Gli amministratori devono comunque usare account separati per l'amministrazione e per l'uso generale.

## <a name="managing-and-updating"></a>Gestione e all'aggiornamento

Le workstation PAW devono avere funzionalità antimalware e gli aggiornamenti software devono essere applicati rapidamente per mantenere l'integrità delle workstation stesse.

Con le workstation PAW è possibile usare anche le funzioni di gestione di configurazioni aggiuntive e della sicurezza e di monitoraggio operativo, ma l'integrazione di queste funzioni deve essere considerato con attenzione, perché ogni funzionalità di gestione introduce anche il rischio di compromissione della workstation PAW attraverso lo strumento corrispondente. Se è opportuno introdurre funzionalità di gestione avanzata dipende da diversi fattori tra cui:

* Lo stato e le procedure di sicurezza delle funzionalità di gestione, incluse le procedure di aggiornamento software per lo strumento, i ruoli amministrativi e gli account presenti in questi ruoli, i sistemi operativi che ospitano lo strumento o da cui questo è gestito e qualsiasi altra dipendenza hardware o software dallo strumento
* La frequenza e la quantità delle distribuzioni e degli aggiornamenti software nelle workstation PAW
* I requisiti per le informazioni dettagliate relative all'inventario e alla configurazione
* I requisiti di monitoraggio della sicurezza
* Gli standard organizzativi e altri fattori specifici dell'organizzazione

Secondo il principio di origine pulita, tutti gli strumenti usati per gestire o monitorare le workstation PAW devono essere attendibili a un livello uguale o superiore rispetto al livello delle workstation PAW stesse. Ciò richiede in genere che tali strumenti siano gestiti da una workstation PAW per garantire l'assenza di dipendenze della sicurezza da workstation con privilegi di livello inferiore.

Questa tabella descrive diversi approcci che è possibile usare per gestire e monitorare le workstation PAW:

|Approccio|Considerazioni|
|------|---------|
|Impostazione predefinita in PAW<br /><br />-   Windows Server Update Services<br />-   Windows Defender|-   Nessun costo aggiuntivo<br />-   Esecuzione delle funzioni di sicurezza di base necessarie<br />-   Istruzioni incluse in questo materiale sussidiario|
|Gestione con [Intune](https://technet.microsoft.com/library/jj676587.aspx)|<ul><li>Visibilità e controllo basati su cloud<br /><br /><ul><li>Distribuzione software</li><li>o   Gestione degli aggiornamenti software</li><li>Gestione dei criteri di Windows Firewall</li><li>Protezione antimalware</li><li>Assistenza remota</li><li>Gestione delle licenze software.</li></ul></li><li>Nessuna necessità di infrastruttura server</li><li>Necessità di eseguire la procedura "Abilitare la connettività a servizi cloud" della fase 2</li><li>Se il computer PAW non è stato aggiunto a un dominio, questo approccio richiede l'applicazione delle baseline SCM alle immagini locali tramite lo strumento disponibile con il download della baseline di sicurezza.</li></ul>|
|Nuove istanze di System Center per la gestione delle workstation PAW|-   Visibilità e controllo della configurazione, della distribuzione software e degli aggiornamenti della sicurezza<br />-   necessità di un'infrastruttura server separata con lo stesso livello di sicurezza delle workstation PAW e competenze di gestione delle risorse umane per il personale con privilegi elevati|
|Gestione delle workstation PAW con gli strumenti di gestione già disponibili|-Rischio significativo di compromissione delle workstation Paw, a meno che l'infrastruttura di gestione esistente viene visualizzata a livello di sicurezza delle workstation Paw **Nota:**     Microsoft in genere sconsiglia questo approccio, a meno che l'organizzazione abbia un motivo specifico per usarlo. Nella nostra esperienza, è in genere un elevato costo di riportare tutti questi strumenti (e le relative dipendenze di sicurezza) fino al livello di protezione della workstation Paw.<br />-   La maggior parte di questi strumenti garantisce visibilità e controllo della configurazione, della distribuzione software e degli aggiornamenti della sicurezza|
|Security Scanning o strumenti di monitoraggio che richiedono accesso amministratore|Qualsiasi strumento che consenta di installare un agente o che richieda un account con accesso amministrativo locale.<br /><br />-   Necessità di portare il controllo della sicurezza dello strumento al livello delle workstation PAW.<br />-   Eventuale necessità di abbassare il livello di sicurezza delle workstation PAW per consentire il funzionamento dello strumento (apertura di porte, installazione di Java o altro middleware e così via), con la necessità di prendere una decisione di compromesso sulla sicurezza|
|Soluzione SIEM (Security information and event management)|<ul><li>Se la soluzione SIEM è senza agenti<br /><br /><ul><li>Possibilità di accedere agli eventi nelle workstation PAW senza accesso amministrativo usando un account del gruppo **Lettori registri eventi**</li><li>Necessità di aprire porte di rete per consentire il traffico in ingresso dai server SIEM</li></ul></li><li>Se SIEM richiede un agente, vedere la riga **Security Scanning o strumenti di monitoraggio che richiedono accesso amministratore**.</li></ul>|
|WEF (Windows Event Forwarding)|- Metodo senza agente per l'inoltro di eventi di sicurezza dalle workstation PAW a un agente di raccolta o a una soluzione SIEM<br />-   Possibilità di accedere agli eventi nelle workstation PAW senza accesso amministrativo<br />-   Nessuna necessità di aprire porte di rete per consentire il traffico in ingresso dai server SIEM|

## <a name="operating-paws"></a>Uso delle workstation PAW

La soluzione PAW deve essere usata secondo gli standard in [Operational Standards](https://aka.ms/securitystandards) (Standard operativi) basati sul principio di origine pulita.

## <a name="deploy-paws-using-a-guarded-fabric"></a>Distribuire workstation Paw con un'infrastruttura protetta

Oggetto [sorvegliato fabric](https://aka.ms/shieldedvms) può essere utilizzato per eseguire carichi di lavoro di workstation PAW in una macchina virtuale schermata in un computer portatile o jump server.
Adottando questo approccio richiede un'infrastruttura aggiuntiva e fasi operative, ma renderla più semplice ridistribuire le immagini PAW a intervalli regolari e consente di consolidare più workstation Paw a più livelli (o le classificazioni) diversi in macchine virtuali in esecuzione side-by-side in un singolo dispositivo.
Per una spiegazione completa delle promesse topologia e la sicurezza dell'infrastruttura sorvegliata, consultare il [sorvegliato documentazione di fabric](https://aka.ms/shieldedvms).

### <a name="changes-to-the-paw-gpos"></a>Modifiche apportate a oggetti Criteri di gruppo PAW

Quando tramite schermate basate su macchine Virtuali delle workstation Paw, il [oggetto Criteri di gruppo impostazioni consigliate](#create-paw-configuration---computer-group-policy-object-gpo) definito sopra devono essere modificate per supportare l'uso di macchine virtuali.

1. Creare una nuova unità Organizzativa per gli host fisici di workstation PAW. Workstation Paw fisiche e virtuali con requisiti di sicurezza diverse e devono essere separate in Active Directory, di conseguenza.
2. Il gruppo di Computer PAW deve essere collegata a entrambe le fisici e virtuali PAW unità organizzative.
3. Creare un nuovo oggetto Criteri di gruppo per le workstation PAW fisica aggiungere gli utenti di workstation PAW al gruppo amministratori Hyper-V. Ciò è necessario per consentire gli amministratori per connettersi all'amministratore che le macchine virtuali e trasformarle attivazione/disattivazione in base alle esigenze. È importante che l'utente che accede a workstation PAW fisica non dispone dei diritti di amministratore, accesso a internet o la possibilità di copiare i dati delle macchine virtuali pericolose da condivisioni di rete o dispositivi di archiviazione esterni a PAW fisico.
4. Creare un nuovo oggetto Criteri di gruppo per l'amministratore di macchine virtuali per aggiungere gli utenti di workstation PAW per il gruppo utenti Desktop remoto. Ciò consentirà agli utenti di usare Hyper-V avanzato sessioni della Console, che offrono un'esperienza utente migliore e Abilita pass-through della smart card per la macchina virtuale.

### <a name="set-up-the-host-guardian-service"></a>Configurare il servizio sorveglianza Host

Il servizio sorveglianza Host è responsabile dell'attestazione per l'identità e l'integrità di un dispositivo fisico di workstation PAW.
Solo le macchine che notoriamente HGS ed esecuzione attendibile [dei criteri di integrità del codice](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) sono consentite per le macchine virtuali schermate di avvio.
Ciò consente di proteggere le macchine virtuali schermate, che eseguono carichi di lavoro attendibili per gestire le risorse a più livelli, dai rischi di ambiente desktop utente.

Poiché HGS è responsabile di determinare quali dispositivi possono eseguire macchine virtuali di PAW, viene considerato una risorsa di livello 0.
Deve essere distribuito insieme ad altre risorse di livello 0 e protetti da accessi non autorizzati fisici e logici.
HGS è un ruolo del cluster, rendendo più semplice per la scalabilità orizzontale per la distribuzione di dimensioni.
La regola generale consiste nel pianificare 1 server HGS per ogni 1.000 dispositivi che si dispone, con un minimo di 3 nodi.

1. Per installare il primo server HGS, iniziare con il [installare HGS - foresta bastione](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md) articolo e includere HGS nel dominio di livello 0.

2. Quindi [creare i certificati per HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md) usando l'autorità di certificazione dell'organizzazione.
Qualsiasi utente che possiede la crittografia del servizio HGS e firma dei certificati in grado di decrittografare una macchina virtuale schermata, pertanto se si ha accesso a un modulo di protezione Hardware per proteggere le chiavi private, è consigliabile generare questi certificati tramite un modulo HSM.
Per una maggiore sicurezza, selezionare una dimensione di chiave maggiore di o uguale a 4096 bit.

3. Infine, seguire la procedura per [inizializzare il server HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md) nelle **modalità TPM**.
L'inizializzazione consente di impostare l'attestazione e i servizi web di protezione della chiave usati da workstation Paw.
Deve essere HGS [configurato con un certificato TLS](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md) per proteggere queste comunicazioni e solo la porta 443 deve essere aperta da reti non attendibili di HGS.

4. Seguire i passaggi necessari per [aggiunge altri nodi](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md) per il secondo e terzo ed eventuali altri nodi HGS.

5. Se il server HGS è in esecuzione Windows Server 2019 o versioni successive, è possibile abilitare una funzionalità facoltativa memorizzare nella cache le chiavi per le macchine virtuali schermate nelle workstation Paw in modo che possa essere usate offline. Le chiavi sono sealed per la configurazione di sicurezza corrente del sistema per impedire l'utilizzo delle chiavi memorizzato nella cache in un altro computer o la stessa macchina in uno stato non protetto. Può trattarsi di una soluzione utile se gli utenti di workstation PAW viaggiano senza accesso a Internet ma devono comunque essere in grado di accedere alle loro macchine virtuali PAW. Per usare questa funzionalità, eseguire il comando seguente in qualsiasi server HGS:

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>Configurare il dispositivo fisico di workstation PAW

Il dispositivo fisico di workstation PAW è considerato non attendibile per impostazione predefinita nella soluzione di infrastruttura sorvegliata.
È possibile dimostrare la propria trustworthy durante il processo di attestazione, dopo il quale è possibile ottenere le chiavi necessarie per avviare una VM amministrativa schermata.
Il dispositivo deve essere in grado di eseguire Hyper-V e avvio protetto è e un TPM 2.0 in grado di soddisfare le [prerequisiti per l'host sorvegliato](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md).
La versione minima del sistema operativo supportano tutte le funzionalità di workstation PAW sia **Windows 10 versione 1803**.

La workstation PAW fisica da impostare come qualsiasi altro, con l'eccezione che gli utenti di workstation PAW dovrà essere amministratori di Hyper-V sia in grado di attivare l'amministratore della macchina virtuale e connettersi a esso.
Pulisci chat nell'ambiente in uso, è necessario creare una configurazione finale per ogni combinazione univoca di hardware e software che come gli host sorvegliati per amministratore macchine virtuali di cui si sta distribuendo.
In ogni configurazione finale, completare le attività seguenti:

1. Installare gli aggiornamenti più recenti per Windows, i driver e firmware del computer, nonché qualsiasi gestione di terze parti o agenti di monitoraggio.
2. [Acquisire le informazioni di base necessari](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md), tra cui l'identificatore TPM univoco (chiave di verifica dell'autenticità), avvio misurazioni (log TCG) e criteri di integrità per la macchina di codice.
3. Copiare questi elementi in un server HGS ed eseguire i comandi di attestazione HGS nell'articolo precedente per registrare l'host. Se tutti gli host utilizzano gli stessi criteri di integrità di codice e/o usano la stessa configurazione hardware, è sufficiente registrare i log di criteri/TCG integrità codice una sola volta.

### <a name="create-the-signed-template-disk"></a>Creare il disco modello firmato

Le macchine virtuali schermate vengono create usando dischi modello firmati.
La firma viene verificata in fase di distribuzione per verificare l'integrità del disco e l'autenticità prima di rilasciare i segreti, ad esempio la password dell'amministratore nella macchina virtuale.

Per creare un disco modello firmato, seguire i passaggi di distribuzione della fase 1 in una normale macchina virtuale di generazione 2.
Questo computer diventerà l'immagine finale per una VM amministrativa.
È possibile creare più di un disco modello per specifico gli strumenti disponibili in contesti diversi.

Quando la macchina virtuale è configurata come lo si desidera, eseguire `C:\Windows\System32\sysprep\sysprep.exe` e si desidera **generalizza** il disco. **Arrestare** il sistema operativo al termine della generalizzazione.

Infine, eseguire la [Creazione guidata disco modello](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md) sul file VHDX dalla macchina virtuale per installare i componenti di BitLocker e generare la firma del disco.

#### <a name="create-the-shielding-data-file"></a>Creare il file di dati di schermatura

Il disco modello generalizzato è associato a un file di dati di schermatura, che contiene i segreti necessari per eseguire il provisioning di una macchina virtuale schermata.
Il file di dati di schermatura include:
   - Elenco dei sorveglianti, che definiscono le infrastrutture in cui la macchina virtuale è consentita per l'esecuzione. Ogni cluster HGS sia un tutore per i dispositivi di workstation PAW che consente di proteggere.
   - Un elenco delle firme dei dischi attendibili per la distribuzione. File di dati di schermatura rilascerà solo i propri segreti in macchine virtuali create usando i supporti di origine autorizzati.
   - Criteri di sicurezza che determina se le protezioni aggiuntive devono essere inserite in grado di proteggere la macchina virtuale dall'host e se la macchina virtuale è consentita spostare in un'altra macchina. Amministratori di workstation PAW macchine virtuali devono sempre essere schermate completamente.
   - Il file di specializzazione Unattend. XML, che consente a Windows completare automaticamente l'installazione include segreti, ad esempio la password dell'amministratore locale.
   - File aggiuntivi, ad esempio i certificati RDP o VPN.

Vedere le [articolo di file di dati di schermatura](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md) per i passaggi su come creare un file di dati di schermatura.

Le chiavi di proprietario per le macchine virtuali schermate sono estremamente sensibili e devono essere mantenute in un modulo HSM o archiviate offline in un luogo sicuro.
Possono essere utilizzati in uno scenario di emergenza break glass per l'avvio di una macchina virtuale schermata senza la presenza di HGS.

È consigliabile che i dati per l'amministratore di workstation PAW di schermatura le macchine virtuali includono l'impostazione per bloccare una macchina virtuale per il primo host fisico in cui è stata avviata.
Ciò impedirà qualcuno lo spostamento di un amministratore della macchina virtuale da una workstation PAW a workstation PAW di un altro nello stesso ambiente.
Per usare questa funzionalità, creare il file di dati di schermatura con PowerShell e includono il **BindToHostTpm** parametro:

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>Distribuire una VM amministrativa

Una volta che il disco modello e i file di dati di schermatura sono pronti, è possibile distribuire una VM amministrativa in qualsiasi workstation PAW è stato registrato con HGS.

1. Copiare il disco modello (con estensione vhdx) e file di dati di schermatura (. pdk) in un dispositivo attendibile di workstation PAW.
2. Seguire le istruzioni per [distribuire una nuova macchina virtuale schermata usando PowerShell](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md)
3. Completare i passaggi rimanenti nella fase 1 del processo di distribuzione per proteggere il sistema operativo della macchina virtuale e configurarlo per il ruolo desiderato, come appropriato.


## <a name="related-topics"></a>Argomenti correlati

[Servizi di coinvolgere Microsoft per la sicurezza informatica](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Gusto di Premier: Come prevenire Pass-the-Hash e altre forme di furto di credenziali](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Threat Analitica](https://aka.ms/ata)

[Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)

[Device Guard Overview](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)

[La protezione delle risorse di valore elevato con secure admin workstation](https://msdn.microsoft.com/library/mt186538.aspx)

[Modalità utente isolato in Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolamento di processi in modalità utente e funzionalità in Windows 10 con Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Più processi di accensione e funzionalità in modalità utente isolato di Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Riduzione del rischio furto delle credenziali tramite il Windows 10 modalità utente isolato (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Abilitando la convalida di tipo Strict KDC in Windows Kerberos](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novità dell'autenticazione Kerberos per Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Verifica del meccanismo di autenticazione per AD DS nella Guida dettagliata di Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Trusted Platform Module](C:/sd/docs/p_ent_keep_secure/p_ent_keep_secure/trusted_platform_module_technology_overview.xml)
