---
title: Perché le workstation con accesso con privilegi possono contribuire alla sicurezza dell'organizzazione
description: In che modo le workstation con accesso con privilegi possono migliorare il comportamento relativo alla sicurezza dell'organizzazione
ms.prod: windows-server
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 86d7b2ff99debbecec930693fb93dc965fefc59e
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517596"
---
# <a name="privileged-access-workstations"></a>Workstation con accesso con privilegi

>Si applica a: Windows Server

Le workstation PAW (Privileged Access Workstation, workstation amministrativa con privilegi) dispongono di un sistema operativo dedicato per le attività sensibili che devono essere protette dagli attacchi provenienti da Internet e dai vettori di minacce di qualsiasi tipo. La possibilità di separare le attività e gli account sensibili dalle workstation e dai dispositivi usati ogni giorno garantisce una protezione estremamente avanzata dagli attacchi di phishing, dalle vulnerabilità delle applicazioni e del sistema operativo, dai diversi tipi di attacchi di impersonificazione e dagli attacchi finalizzati al furto di credenziali, ad esempio la registrazione delle pressioni dei tasti e gli attacchi [Pass-the-Hash](https://aka.ms/pth) e Pass-The-Ticket.

## <a name="what-is-a-privileged-access-workstation"></a>Che cos'è una workstation con accesso con privilegi (PAW)?

In termini semplici, una workstation PAW è una workstation con protezione avanzata e bloccata, progettata per garantire la sicurezza degli account e delle attività sensibili.  Le workstation PAW sono consigliate per l'amministrazione dei sistemi di gestione delle identità, dei servizi cloud e dell'infrastruttura cloud privata, nonché delle funzioni aziendali sensibili.

> [!NOTE]
> L'architettura PAW non richiede un mapping 1:1 degli account alle workstation, anche se questa è una configurazione comune. Questa architettura crea un ambiente workstation attendibile che può essere usato da uno o più account.

Per garantire la massima sicurezza, le workstation PAW devono sempre eseguire il sistema operativo più aggiornato e sicuro disponibile: Microsoft consiglia vivamente Windows 10 Enterprise, che include diverse funzionalità di sicurezza aggiuntive non disponibili in altre edizioni (in particolare, [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Le organizzazioni senza accesso a Windows 10 Enterprise possono usare Windows 10 Pro, che include molte delle tecnologie critiche fondamentali per PAW, tra cui Avvio sicuro, BitLocker e Desktop remoto.  I clienti Education possono usare Windows 10 Education.  Nelle workstation PAW non si deve usare Windows 10 Home.
>
> Per una matrice di confronto delle diverse edizioni di Windows 10, leggere [ questo articolo](https://www.microsoft.com/WindowsForBusiness/Compare).

I controlli di sicurezza PAW sono incentrati sulla mitigazione dei rischi di compromissione ad alto impatto e ad alta probabilità. Tra questi sono inclusi gli attacchi all'ambiente e i rischi che possono ridurre l'efficacia dei controlli PAW nel tempo:

* **Attacchi da Internet**: la maggior parte degli attacchi deriva direttamente o indirettamente da origini Internet e usa Internet per sottrarre dati e assumere comando e controllo. Isolare la workstation PAW dai siti Internet non controllati è un elemento fondamentale per garantire che la workstation non venga compromessa.
* **Rischi legati all'usabilità** se una workstation PAW è troppo difficile da usare per le attività quotidiane, gli amministratori sono portati a facilitare il lavoro creando soluzioni alternative. Spesso le soluzioni alternative espongono la workstation e gli account amministrativi a rischi di sicurezza, quindi è fondamentale coinvolgere gli utenti PAW, consentendo loro di attenuare i problemi di usabilità in modo protetto. Questo obiettivo può essere raggiunto ascoltando il feedback degli utenti, installando gli strumenti e gli script necessari allo svolgimento del lavoro e assicurandosi che tutto il personale amministrativo sia consapevole dei motivi per cui è necessario usare una workstation PAW, delle caratteristiche di una workstation PAW e di come usarla in modo corretto ed efficiente.
* **Rischi legati all'ambiente**: poiché all'interno dell'ambiente molti altri computer e account sono esposti direttamente o indirettamente a rischi originati da Internet, una workstation PAW deve essere protetta dagli attacchi di risorse compromesse presenti all'interno dell'ambiente di produzione. Ciò comporta la necessità di limitare al minimo l'uso di strumenti e account di gestione che hanno accesso alle workstation PAW per proteggere e monitorare queste workstation specializzate.
* **Manomissione della Supply Chain**: non è possibile rimuovere tutti i possibili rischi di manomissione nella Supply Chain per hardware e software, ma alcune azioni chiave possono ridurre il numero di vettori critici di attacchi immediatamente disponibili per gli utenti malintenzionati. Tali azioni includono la convalida dell'integrità di tutti i supporti di installazione ([principio di origine pulita](https://aka.ms/cleansource)) e l'uso di un fornitore attendibile e affidabile per hardware e software.
* **Attacchi fisici**: le workstation PAW sono fisicamente mobili e possono essere usate all'esterno di strutture protette. Devono quindi essere protette dagli attacchi basati sull'accesso fisico non autorizzato al computer.

> [!NOTE]
> Una workstation PAW non protegge l'ambiente da un antagonista che abbia già ottenuto l'accesso amministrativo a una foresta Active Directory.
> Poiché molte implementazioni esistenti di Active Directory Domain Services hanno funzionato per anni esposte al rischio di furto di credenziali, le organizzazioni devono presupporre che si siano verificate violazioni e considerare la possibilità che si siano verificate compromissioni non rilevate di credenziali amministratore di dominio o dell'organizzazione. Un'organizzazione che sospetti la compromissione del dominio deve prendere in considerazione l'uso di servizi professionali di risposta agli eventi imprevisti.
>
> Per altre informazioni sulla risposta e indicazioni per il ripristino, vedere le sezioni "Respond to suspicious activity" (Risposta a un'attività sospetta) e "Recover from a breach" (Ripristino da una violazione) di [Mitigating Pass-the-Hash and Other Credential Theft v2](https://aka.ms/pth) (Limitazione della tecnica Pass-the-Hash e furto di altre credenziali versione 2).
>
> Per altre informazioni, visitare la pagina [Microsoft's Incident Response and Recovery services](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx) (Servizi Microsoft di risposta agli eventi imprevisti e ripristino).

### <a name="paw-hardware-profiles"></a>Profili hardware PAW

Anche i membri del personale amministrativo sono utenti standard. Oltre a una workstation PAW, hanno bisogno di una workstation utente standard per controllare i messaggi di posta elettronica, esplorare il Web e accedere alle applicazioni line-of-business aziendali.  Garantire che gli amministratori siano costantemente produttivi e protetti è essenziale per il successo di qualsiasi distribuzione PAW.  Una soluzione sicura che limiti notevolmente la produttività verrà abbandonata dagli utenti a favore di una soluzione che migliori la produttività, anche a costo della mancanza di sicurezza.

Per bilanciare l'esigenza di sicurezza con l'esigenza di produttività, Microsoft consiglia di usare uno di questi profili hardware PAW:

* **Hardware dedicato**: dispositivi dedicati separati per le attività utente e per le attività amministrative.
* **Uso simultaneo**: singolo dispositivo che può eseguire attività utente e attività amministrative contemporaneamente, sfruttando i vantaggi della virtualizzazione del sistema operativo o della presentazione.

Le organizzazioni possono usare un solo profilo o entrambi. Non ci sono problemi di interoperabilità tra i profili hardware e le organizzazioni hanno la possibilità di far corrispondere in modo flessibile il profilo hardware alle esigenze e alla situazione di ciascun amministratore.

> [!NOTE]
> È fondamentale che, in tutti questi scenari, ai membri del personale amministrativo vengano rilasciati account utente standard separati dagli account designati come amministrativi. Gli account amministrativi devono essere usati solo per il sistema operativo amministrativo PAW.

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

Con Hyper-V per Windows 10, una macchina virtuale guest (che esegue anch'essa Windows 10) può offrire un'esperienza utente avanzata che include audio, video e applicazioni di comunicazione via Internet, ad esempio Skype for Business.

In questa configurazione le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite all'interno della macchina virtuale con il sistema operativo utente, che dispone di un'immagine Windows 10 aziendale regolare e non è soggetta alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito all'interno del sistema operativo di amministrazione.

Per la configurazione di quest'ultimo, seguire le istruzioni presenti in questo materiale sussidiario per l'host PAW, aggiungere le funzionalità di Hyper-V client, creare una VM utente e installare all'interno di questa un'immagine aziendale Windows 10.

Per altre informazioni su questa funzionalità, leggere l'articolo [Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) (Hyper-V client). Si noti che il sistema operativo nelle macchine virtuali guest deve avere una licenza basata sulle [regole per l'assegnazione di licenze ai prodotti Microsoft](https://www.microsoft.com/Licensing/product-licensing/products.aspx), descritte anche [qui](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf).

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>Uso simultaneo: aggiunta di RemoteApp, di RDP o di un'infrastruttura VDI

In questo scenario di uso simultaneo un unico PC viene usato sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo. Con questa configurazione il sistema operativo utente dei diversi PC viene distribuito e gestito in modo centralizzato, nel cloud o nel centro dati, ma non è disponibile se il PC è disconnesso.

![Figura che illustra l'uso simultaneo di un unico PC sia per le attività di amministrazione che per le attività quotidiane, come la posta elettronica, la modifica di documenti e le attività di sviluppo](../media/privileged-access-workstations/PAWFig11.JPG)

L'hardware fisico esegue un solo sistema operativo PAW locale per le attività amministrative e contatta un servizio Desktop remoto Microsoft o di terze parti per le applicazioni utente, ad esempio per la posta elettronica, la modifica di documenti e le applicazioni line-of-business.

In questa configurazione le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite all'interno dei sistemi operativi remoti, che non sono soggetti alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito all'interno del sistema operativo di amministrazione.

Per questa configurazione, seguire le istruzioni presenti in questo materiale sussidiario per l'host PAW, consentire la connettività di rete al servizio Desktop remoto e aggiungere collegamenti al desktop dell'utente PAW per consentire l'accesso alle applicazioni. I servizi Desktop remoto possono essere ospitati in molti modi, ad esempio tramite:

* Un servizio Desktop remoto o VDI esistente (locale o nel cloud)
* Un nuovo servizio installato in locale o nel cloud
* Azure RemoteApp con modelli di Office 365 preconfigurati o immagini di installazione personalizzate

Per altre informazioni su Azure RemoteApp, visitare [questa pagina](https://www.remoteapp.windowsazure.com).

## <a name="how-microsoft-is-using-administrative-workstations"></a>In che modo Microsoft usa le workstation amministrative

Microsoft usa l'approccio architettonico PAW sia internamente nei propri sistemi che con i propri clienti. Microsoft usa le workstation amministrative internamente per diverse funzioni, ad esempio l'amministrazione della propria infrastruttura IT, lo sviluppo e le operazioni della propria infrastruttura cloud, nonché per altre risorse di valore elevato.

Questo materiale sussidiario si basa direttamente sull'architettura di riferimento PAW (Privileged Access Workstation) distribuita dal team Microsoft dedicato ai servizi professionali riguardanti la sicurezza informatica per proteggere i clienti da attacchi alla sicurezza. Le workstation amministrative sono anche un elemento chiave dello strumento più avanzato di protezione delle attività di amministrazione del dominio: ESAE (Enhanced Security Administrative Environment, ambiente amministrativo di sicurezza avanzata), l'architettura di riferimento per le foreste amministrative.

Per altri dettagli sulla foresta amministrativa ESAE, vedere la sezione *Approccio per la progettazione della foresta amministrativa ESAE* in [Materiale di riferimento per la protezione dell'accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).

## <a name="architecture-overview"></a>Panoramica dell'architettura

Il diagramma seguente illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile), creato tramite la gestione separata di account e workstation amministrativi dedicati.

![Diagramma che illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile), creato tramite la gestione separata di account e workstation amministrativi dedicati](../media/privileged-access-workstations/PAWFig1.JPG)

Questo approccio architettonico, basato sulle misure di sicurezza disponibili nelle funzionalità [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx) di Windows 10, per gli account e le attività sensibili si spinge oltre tali misure .

Questa metodologia è appropriata per gli account con accesso alle risorse di valore elevato:

* **Privilegi amministrativi**: le workstation PAW offrono maggiore sicurezza per le attività e i ruoli amministrativi ad alto impatto del reparto IT. Questa architettura può essere applicata all'amministrazione di molti tipi di sistemi, ad esempio domini e foreste di Active Directory, tenant di Microsoft Azure Active Directory e di Office 365, reti PCN (Process Control Network), sistemi di supervisione e di acquisizione dati (SCADA, Supervisory Control and Data Acquisition), Bancomat e dispositivi POS.
* **Information worker a contatto con informazioni a elevata sensibilità**: l'approccio applicato con le workstation PAW può essere usato anche per proteggere le attività del personale e degli information worker che gestiscono informazioni a elevata sensibilità, ad esempio le attività che precedono l'annuncio di fusioni e acquisizioni o il rilascio di rendiconti finanziari, la presenza dell'organizzazione sui social media, le comunicazioni di livello dirigenziale, i segreti commerciali non ancora brevettati, le ricerche tenute riservate e altri dati sensibili o di proprietà. Questo materiale sussidiario non illustra in modo approfondito la configurazione degli scenari relativi agli information worker né include questo scenario nelle istruzioni tecniche.

    > [!NOTE]
    > Il reparto IT Microsoft usa workstation PAW, definite internamente come "workstation amministrative protette" o SAW (Secure Admin Workstation) per gestire l'accesso protetto ai sistemi a valore elevato all'interno di Microsoft. Più avanti, nella sezione "Come Microsoft usa le workstation amministrative", questo materiale sussidiario descrive in maggiore dettaglio l'uso di workstation PAW presso Microsoft. Per informazioni più dettagliate su questo approccio agli ambienti con risorse di valore elevato, fare riferimento all'articolo [Protecting high-value assets with secure admin workstations](https://msdn.microsoft.com/library/mt186538.aspx) (Protezione delle risorse di valore elevato con workstation amministrative protette).

Questo documento illustra i motivi per cui questa procedura è consigliata per la protezione degli account con privilegi a impatto elevato, l'aspetto delle soluzioni PAW per la protezione dei privilegi amministrativi e il metodo per distribuire rapidamente una soluzione PAW per l'amministrazione di servizi di dominio e cloud.

Questo documento offre indicazioni dettagliate per l'implementazione di diverse configurazioni PAW e include istruzioni di implementazione dettagliate che consentono di iniziare a proteggere account comuni a impatto elevato:

* [**Fase 1: distribuzione immediata per gli amministratori di Active Directory**](#phase-1-immediate-deployment-for-active-directory-administrators) consente di implementare rapidamente una workstation PAW in grado di proteggere i ruoli locali relativi all'amministrazione di domini e foreste
* [**Fase 2: estensione di PAW a tutti gli amministratori**](#phase-2-extend-paw-to-all-administrators) consente di abilitare la protezione per gli amministratori di servizi cloud quali Office 365 e Azure, server e applicazioni aziendali e workstation
* [**Fase 3: sicurezza avanzata delle workstation PAW**](#phase-3-extend-and-enhance-protection) descrive altre misure di protezione e contiene altre considerazioni sulla sicurezza delle workstation PAW

### <a name="why-dedicated-workstations"></a>Perché workstation dedicate?

L'universo attuale delle minacce alle organizzazioni pullula di sofisticati tipi di phishing e di altri attacchi via Internet che creano un rischio costante di compromissione della sicurezza per gli account e le workstation esposti a Internet.

Questo universo di minacce obbliga le organizzazioni ad adottare un atteggiamento di "presunzione della violazione" nella progettazione delle misure di protezione destinate a risorse di elevato valore, ad esempio account amministrativi e risorse aziendali sensibili. Queste risorse di elevato valore devono essere protette sia dalle minacce provenienti direttamente da Internet che dagli attacchi predisposti da workstation, server e dispositivi dell'ambiente.

![Figura che illustra il rischio a cui sono esposte le risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili](../media/privileged-access-workstations/PAWFig2.JPG)

Questa figura illustra il rischio a cui sono esposte le risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili.

Se ha il controllo del sistema operativo, un utente malintenzionato ha a disposizione molti modi per accedere illecitamente a tutte le attività della workstation e per rappresentare l'account legittimo. Per ottenere questo livello di accesso è possibile usare una serie di tecniche di attacco note e sconosciute. L'aumento costante del numero e della complessità degli attacchi informatici ha reso necessario estendere il concetto di separazione fino al punto di applicarlo completamente ai sistemi operativi client per gli account sensibili. Per altre informazioni su questi tipi di attacchi e per ottenere white paper informativi, video e altro ancora, visitare il [sito Web Pass-The-Hash](https://www.microsoft.com/pth).

L'approccio PAW rappresenta l'estensione della procedura consigliata ormai consolidata di usare account amministrativi e account utente separati per il personale amministrativo. Con questa procedura consigliata vengono assegnati agli utenti account amministrativi individuali completamente separati dall'account utente standard. La workstation PAW sviluppa ulteriormente la pratica di separazione degli account usando una workstation attendibile per gli account sensibili.

Questo materiale sussidiario su PAW ha lo scopo di consentire l'implementazione di questa funzionalità per la protezione di account di valore elevato, ad esempio gli account degli amministratori IT con privilegi elevati e gli account aziendali a elevata sensibilità. Il materiale sussidiario consente di:

* Limitare l'esposizione delle credenziali ai soli host attendibili
* Fornire agli amministratori una workstation con un livello di sicurezza elevato con cui eseguire facilmente attività amministrative.

Limitare gli account sensibili all'uso esclusivo di workstation PAW con protezione avanzata consente di proteggere in modo semplice questi account, consentendone l'elevata usabilità da parte degli amministratori e rendendone molto difficile la compromissione da parte degli avversari.

### <a name="alternate-approaches"></a>Approcci alternativi

Questa sezione contiene informazioni sul confronto tra la sicurezza offerta dagli approcci alternativi e la workstation PAW e sulla corretta integrazione di questi approcci all'interno di un'architettura PAW. Se implementati individualmente, tutti questi approcci comportano rischi significativi, ma in alcuni scenari possono aggiungere valore a un'architettura PAW.

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard e Windows Hello for Business

Introdotto con Windows 10, [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) usa la sicurezza basata su hardware e virtualizzazione per prevenire gli attacchi più comuni di furto di credenziali, ad esempio Pass-the-Hash, proteggendo le credenziali derivate. La chiave privata per le credenziali usata da [Windows Hello for Business](https://aka.ms/passport) può essere protetta anche tramite hardware TPM (Trusted Platform Module).

Queste sono soluzioni di prevenzione avanzate che però lasciano le workstation ancora vulnerabili ad alcuni attacchi, anche se le credenziali sono protette tramite Credential Guard o Windows Hello for Business. Gli attacchi in questione riguardano l'abuso di privilegi e l'uso di credenziali direttamente da un dispositivo compromesso, il riutilizzo di credenziali rubate prima dell'abilitazione di Credential Guard e l'uso improprio di strumenti di gestione per lo sfruttamento di una configurazione vulnerabile delle applicazioni delle workstation.

Le indicazioni su PAW in questa sezione riguardano l'uso di molte di queste tecnologie per gli account e le attività a elevata sensibilità.

#### <a name="administrative-vm"></a>VM amministrative

Una macchina virtuale (VM, Virtual Machine) amministrativa è un sistema operativo dedicato solo alle attività amministrative e ospitato in un desktop utente standard. Questo approccio è simile all'architettura PAW poiché offre un sistema operativo dedicato per le attività amministrative, ma ha un grave difetto: la sicurezza di una VM amministrativa dipende dal desktop utente standard.

Il diagramma che segue illustra in che modo gli utenti malintenzionati possono seguire la catena di controllo fino all'oggetto obiettivo dell'attacco con una VM amministrativa in una workstation utente. Illustra anche la difficoltà di creare un percorso nella configurazione inversa.

L'architettura PAW non consente l'hosting di una VM amministrativa in una workstation utente, ma una VM utente con un'immagine aziendale standard può essere ospitata in una workstation PAW amministrativa. In questo modo, il personale potrà usare un unico PC per tutte le responsabilità.

![Diagramma dell'architettura PAW](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>Workstation PAW basate su VM schermate

Una variante sicura del modello di VM amministrativa consiste nell'uso di [macchine virtuali schermate](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) per ospitare una o più VM amministrative insieme a una macchina virtuale utente.
Le VM schermate sono progettate per eseguire carichi di lavoro protetti in un ambiente in cui codice o utenti potenzialmente non attendibili possono essere eseguiti sul desktop utente standard del computer fisico.
Una VM schermata dispone di un modulo TPM virtuale che consente di crittografare i dati inattivi e diversi controlli amministrativi, come l'accesso alla console di base, PowerShell Direct e la possibilità di eseguire il debug della macchina virtuale, sono disabilitati per isolare ulteriormente la VM dal desktop utente standard e da altre VM.
Le chiavi per una VM schermata sono archiviate in un server di gestione delle chiavi attendibili, che richiede al dispositivo fisico di attestare la propria identità e integrità prima di rilasciare una chiave per l'avvio della VM.
Ciò garantisce che le VM schermate possano essere avviate solo nei dispositivi previsti e che tali dispositivi eseguano configurazioni software note e attendibili.

Poiché le VM schermate sono isolate l'una dall'altra e dal desktop utente standard, è accettabile eseguire più VM PAW schermate in un singolo host, anche quando le VM di amministrazione gestiscono livelli diversi.

Per altre informazioni, vedere la sezione [Distribuire le workstation PAW usando un'infrastruttura sorvegliata](#deploy-paws-using-a-guarded-fabric).

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
> Per altre informazioni sul modello a livelli, vedere [https://aka.ms/tiermodel](https://aka.ms/tiermodel). Per altre informazioni sui gruppi di livello 0, vedere Equivalenza di livello 0 in [Materiale di riferimento per la protezione dell'accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md).

Per altre informazioni sulla distribuzione di Privileged Access Management (PAM) di Microsoft Identity Manager (MIM), vedere [https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy).

## <a name="paw-scenarios"></a>Scenari PAW

Questa sezione contiene indicazioni sugli scenari a cui è applicabile questo materiale sussidiario su PAW. Per tutti gli scenari gli amministratori devono essere addestrati a usare solo workstation PAW per il supporto a sistemi remoti. Per favorire un uso corretto e sicuro e consentire il miglioramento dell'esperienza PAW, è anche necessario incoraggiare tutti gli utenti PAW a fornire feedback, che dovrà essere esaminato attentamente e integrato nel programma PAW.

Per tutti gli scenari è possibile usare nelle fasi successive misure di protezione avanzata aggiuntive e profili hardware diversi rispetto a quelli citati in questo materiale sussidiario per soddisfare i requisiti di usabilità o di sicurezza dei ruoli.

> [!NOTE]
> Questo materiale sussidiario fa esplicitamente distinzione tra la richiesta di accesso a servizi specifici su Internet, ad esempio i portali amministrativi Azure e Office 365, e la rete Internet "aperta" in cui si trovano tutti gli host e i servizi.

Per altre informazioni sulle designazioni di livello, vedere la [pagina sul modello a livelli](https://aka.ms/tiermodel).

|**Scenari**|**Usare PAW?**|**Considerazioni su ambito e sicurezza**|
|---------|--------|---------------------|
|Amministratori di Active Directory, livello 0|Sì|Una workstation PAW creata secondo le indicazioni per la fase 1 è sufficiente per questo ruolo.<br /><br />-   È possibile aggiungere una foresta amministrativa per garantire la protezione più avanzata a questo scenario. Per altre informazioni sulla foresta amministrativa ESAE, vedere [Approccio per la progettazione della foresta amministrativa ESAE](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)<br />-   Una workstation PAW può essere usata per gestire più domini o più foreste.<br />-   Se i controller di dominio sono ospitati in un'infrastruttura distribuita come servizio (IaaS, Infrastructure as a Service) o in una soluzione di virtualizzazione locale, è necessario assegnare maggiore priorità all'implementazione di workstation PAW per gli amministratori di tali soluzioni.|
|Amministratori di servizi IaaS e PaaS di Azure, livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW almeno per l'amministratore globale e per l'amministratore della fatturazione delle sottoscrizioni. È necessario usare workstation PAW anche per gli amministratori delegati di server critici o sensibili.<br />-   Devono essere usate workstation PAW per la gestione del sistema operativo e delle applicazioni che forniscono la sincronizzazione della directory e la federazione delle identità per i servizi cloud, ad esempio [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect/) e Active Directory Federation Services (AD FS).<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi cloud autorizzati in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.<br />-   Sulla workstation deve essere configurato il software Windows Defender Exploit Guard. **Nota:**     una sottoscrizione è considerata di livello 0 per una foresta se nella sottoscrizione sono presenti controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se in Azure non sono ospitati server di livello 0.|
|Amministratore di tenant di Office 365 <br />- Livello 1|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW almeno per l'amministratore della fatturazione delle sottoscrizioni, per l'amministratore globale, per l'amministratore di Exchange, per l'amministratore di SharePoint e per ruoli di amministratore di gestione degli utenti. È anche consigliabile prendere seriamente in considerazione l'uso di workstation PAW per gli amministratori delegati di dati estremamente critici o sensibili.<br />-   Sulla workstation deve essere configurato il software Windows Defender Exploit Guard.<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.|
|Altro amministratore di servizi cloud IaaS o PaaS<br />- Livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW per tutti i ruoli con diritti amministrativi su VM ospitate nel cloud, tra cui la possibilità di installare agenti, esportare file dal disco rigido o accedere alle risorse di archiviazione in cui sono archiviati dischi rigidi con sistemi operativi, dati sensibili o dati aziendali critici.<br />-   Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft in base alle indicazioni per la fase 2. Nessun accesso a Internet "aperta" deve essere consentito da workstation PAW.<br />-   Sulla workstation deve essere configurato il software Windows Defender Exploit Guard. **Nota:** una sottoscrizione è di livello 0 per una foresta se nella sottoscrizione sono presenti controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se in Azure non sono ospitati server di livello 0.|
|Amministratori di virtualizzazioni<br />- Livello 0 o 1 (vedere Considerazioni su ambito e sicurezza)|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Devono essere usate workstation PAW per tutti i ruoli con diritti amministrativi su VM, tra cui la possibilità di installare agenti, esportare file da dischi rigidi virtuali o accedere alle risorse di archiviazione in cui sono archiviati dischi rigidi con informazioni di sistemi operativi guest, dati sensibili o dati aziendali critici. **Nota:** un sistema di virtualizzazione e i relativi amministratori sono considerati di livello 0 per una foresta se nella sottoscrizione sono presenti controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se nel sistema di virtualizzazione non sono ospitati server di livello 0.|
|Amministratori della manutenzione dei server<br />- Livello 1|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Una workstation PAW deve essere usata dagli amministratori che aggiornano server e app aziendali in Windows Server, Linux e altri sistemi operativi, oltre ad applicare patch e risolvere i problemi di tali server e app.<br />-   Può essere necessario aggiungere strumenti di gestione dedicati alle workstation PAW per consentire lo svolgimento di tali attività di amministrazione su scala più ampia.|
|Amministratori di workstation utente <br />- Livello 2|Sì|Una workstation PAW creata secondo le indicazioni relative alla fase 2 è sufficiente per i ruoli con diritti amministrativi sui dispositivi degli utenti finali, ad esempio i ruoli di help desk e di supporto deskside.<br /><br />-   Può essere necessario installare applicazioni aggiuntive nelle workstation PAW per consentire la gestione di ticket e altre funzioni di supporto.<br />-   Sulla workstation deve essere configurato il software Windows Defender Exploit Guard.<br />    Può essere necessario aggiungere strumenti di gestione dedicati alle workstation PAW per consentire lo svolgimento di tali attività di amministrazione su scala più ampia.|
|Amministratore di SQL, SharePoint o line-of-business (LOB)<br />- Livello 1|Sì|Una workstation PAW creata secondo le indicazioni per la fase 2 è sufficiente per questo ruolo.<br /><br />-   Può essere necessario installare strumenti di gestione aggiuntivi nelle workstation PAW installato per consentire agli amministratori di gestire le applicazioni senza la necessità di connettersi ai server tramite Desktop remoto.|
|Utenti che gestiscono la presenza nei social media|Parziale|Una workstation PAW creata secondo le indicazioni specificate per la fase 2 può essere usata come punto di partenza per garantire la sicurezza a questo ruolo.<br /><br />-   Proteggere e gestire gli account per i social media tramite Azure Active Directory (AAD) per condividere e proteggere tali account e per tenere traccia dell'accesso a questi.<br />    Per altre informazioni su questa funzionalità, leggere [questo post di blog](https://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-   Le restrizioni di rete in uscita devono consentire la connettività a questi servizi. Ciò può avvenire consentendo connessioni a Internet "aperta" (con un rischio per la sicurezza molto più alto e l'annullamento di molte garanzie PAW) o consentendo il servizio solo tramite indirizzi DNS specifici (alternativa difficile da realizzare).|
|Utenti standard|No|È possibile eseguire molti passaggi di protezione avanzata per gli utenti standard. L'architettura PAW, tuttavia, è progettata per isolare gli account impedendo l'accesso a Internet "aperta", di cui la maggior parte degli utenti ha bisogno per lo svolgimento delle attività lavorative.|
|VDI/chiosco multimediale guest|No|È possibile eseguire molti passaggi di protezione avanzata per gli utenti guest di sistemi chiosco multimediale. L'architettura PAW, tuttavia, è progettata per garantire una maggiore sicurezza per gli account a sensibilità elevata, non una maggiore sicurezza per gli account a sensibilità inferiore.|
|Utente VIP (dirigenti, ricercatori e così via)|Parziale|Una workstation PAW creata secondo le indicazioni specificate per la fase 2 può essere usata come punto di partenza per garantire la sicurezza per questi ruoli.<br /><br />-   Questo scenario è simile a quello dei desktop utente standard, ma in genere il profilo dell'applicazione è più piccolo, più semplice e ben noto. Questo scenario richiede in genere l'individuazione e la protezione dei dati, dei servizi e delle applicazioni sensibili, indipendentemente dalla loro installazione nei computer desktop.<br />-   Questi ruoli in genere richiedono un livello di sicurezza elevato e un livello di usabilità molto elevato. Ciò richiede modifiche di progettazione per soddisfare le preferenze dell'utente.|
|Sistemi di controllo industriale (ad esempio SCADA, PCN e DCS)|Parziale|Una workstation PAW creata secondo le indicazioni specificate per la fase 2 può essere usata come punto di partenza per garantire la sicurezza a questi ruoli. Infatti la maggior parte delle console dei sistemi di controllo industriale (inclusi standard comuni come SCADA e PCN) non hanno bisogno di accedere a Internet "aperta" né di controllare la posta elettronica.<br /><br />-   Le applicazioni usate per il controllo di macchinari fisici devono essere integrate, testate per verificarne la compatibilità e protette in modo appropriato.|
|Sistema operativo incorporato|No|È possibile eseguire molti passaggi di protezione avanzata da PAW per i sistemi operativi incorporati. In questo scenario, tuttavia, è necessario sviluppare una soluzione personalizzata per la protezione avanzata.|

> [!NOTE]
> **Combinazione di scenari** Alcuni membri del personale possono avere responsabilità amministrative per più scenari.
> In questi casi, il principio fondamentale da tenere presente è quello di seguire sempre le regole del modello a livelli. Per altre informazioni, vedere la pagina sul modello a livelli.

> [!NOTE]
> **Ridimensionamento del programma PAW** Man mano che il programma PAW viene ridimensionato per comprendere un numero maggiore di amministratori e di ruoli, è necessario continuare a mantenere la conformità agli standard di sicurezza e usabilità. Questo può richiedere l'aggiornamento delle strutture di supporto IT o la creazione di nuove strutture di questo tipo per risolvere problemi specifici di PAW, ad esempio il processo di caricamento PAW, la gestione degli incidenti e della configurazione e la raccolta di feedback relativo a problemi di usabilità.  Ad esempio, l'organizzazione potrebbe decidere di abilitare nuovi scenari per consentire agli amministratori di lavorare da casa. A tale scopo sarebbe necessario passare da workstation PAW desktop a workstation PAW portatili, un passaggio che richiederebbe considerazioni di sicurezza aggiuntive.  Un altro esempio comune consiste nella creazione o nell'aggiornamento di corsi di formazione per nuovi amministratori con l'aggiunta di contenuti relativi all'uso appropriato di una workstation PAW, all'importanza di questa architettura e alle sue caratteristiche.  Per altre considerazioni da tenere presenti in caso di ridimensionamento del programma PAW, vedere la fase 2 delle istruzioni.

Questo materiale sussidiario contiene istruzioni dettagliate per la configurazione PAW per gli scenari sopra citati. Se sono necessari altri scenari, è possibile personalizzare le istruzioni riportate in questo materiale o incaricare un'organizzazione di servizi professionali come Microsoft del supporto.

Per altre informazioni sull'attivazione dei servizi Microsoft per la progettazione di un'architettura PAW su misura per l'ambiente in uso, contattare il rappresentante Microsoft o visitare [questa pagina](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

## <a name="paw-phased-implementation"></a>Implementazione di PAW in più fasi

Poiché PAW deve fornire un'origine protetta e attendibile per l'amministrazione, è essenziale che il processo di creazione sia sicuro e attendibile.  Questa sezione fornisce istruzioni dettagliate che consentono di creare il proprio ambiente PAW tramite principi e concetti generali molto simili a quelli applicati dal reparto IT e dalle organizzazioni di gestione dei servizi e di cloud engineering Microsoft.

Le istruzioni sono suddivise in tre fasi che si concentrano sulla rapida realizzazione delle misure di prevenzione più critiche e quindi sulla crescita e sulla diffusione dell'uso di PAW all'interno dell'azienda.

* [Fase 1: distribuzione immediata per gli amministratori di Active Directory](#phase-1-immediate-deployment-for-active-directory-administrators)
* [Fase 2: estensione di PAW a tutti gli amministratori](#phase-2-extend-paw-to-all-administrators)
* [Fase 3: sicurezza avanzata delle workstation PAW](#phase-3-extend-and-enhance-protection)

È importante notare che le fasi devono essere sempre eseguite nell'ordine in cui sono presentate, anche se vengono pianificate e implementate all'interno di uno stesso progetto complessivo.

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>Fase 1: distribuzione immediata per gli amministratori di Active Directory

Scopo: rendere disponibile rapidamente un'architettura PAW in grado di proteggere i ruoli di amministrazione del dominio e della foresta locali.

Ambito: amministratori di livello 0, tra cui Enterprise Admins e Domain Admins (per tutti i domini) e amministratori di altri sistemi di gestione delle identità autorevoli.

La fase 1 è dedicata principalmente agli amministratori che gestiscono il dominio di Active Directory. Si tratta di ruoli di fondamentale importanza e rappresentano spesso l'obiettivo di utenti malintenzionati. Questi sistemi di gestione delle identità funzionano in modo efficace per la protezione di questi amministratori, indipendentemente dal fatto che i controller di dominio di Active Directory siano ospitati in centri dati locali, in un'infrastruttura distribuita come servizio (IaaS, Infrastructure as a Service) di Azure o in un'infrastruttura IaaS di un altro provider.

Durante questa fase viene creata la struttura dell'unità organizzativa (OU) Active Directory amministrativa protetta che ospiterà le workstation amministrative con privilegi (PAW). Vengono anche distribuite le workstation PAW stesse.  Questa struttura include anche i Criteri di gruppo e i gruppi necessari per il supporto delle workstation PAW.  La maggior parte della struttura viene creata tramite script di PowerShell disponibili nella [Raccolta TechNet](https://aka.ms/pawmedia).

Gli script creano le OU e i gruppi di sicurezza seguenti:

* Unità organizzative (OU)
   * Sei nuove OU di primo livello:  Admin (Amministrazione), Groups (Gruppi), Tier 1 Servers (Server livello 1), Workstations (Workstation), User Accounts (Account utente) e Computer Quarantine (Quarantena computer).  Ogni OU di primo livello conterrà un certo numero di OU figlio.
* Gruppi
   * Sei nuovi gruppi globali con protezione attivata:  Tier 0 Replication Maintenance (Manutenzione replica livello 0), Tier 1 Server Maintenance (Manutenzione server livello 1), Service Desk Operators (Operatori Service Desk), Workstation Maintenance (Manutenzione workstation), PAW Users (Utenti PAW) e PAW Maintenance (Manutenzione PAW).

Verrà creato anche un certo numero di oggetti Criteri di gruppo: PAW Configuration - Computer (Configurazione PAW- Computer), PAW Configuration - User (Configurazione PAW- Utente), RestrictedAdmin Required - Computer (RestrictedAdmin necessaria - Computer); PAW Outbound Restrictions (Restrizioni in uscita PAW); Restrict Workstation Logon (Limita accesso workstation) e Restrict Server Logon (Limita accesso server).

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

4. **Acquisire e convalidare Windows 10 Enterprise Edition e il software applicativo necessario**. Ottenere il software necessario per la workstation PAW e convalidarlo in base alle indicazioni fornite in [Clean Source for installation media](https://aka.ms/cleansource) (Origine pulita per i supporti di installazione).

   * Windows 10 Enterprise Edition
   * [Strumenti di amministrazione remota del server](https://www.microsoft.com/download/details.aspx?id=45520) per Windows 10
   * [Elementi di base della sicurezza di Windows 10](https://aka.ms/win10baselines)

      > [!NOTE]
      > Microsoft pubblica gli hash MD5 per tutti i sistemi operativi e le applicazioni in MSDN, ma non tutti i fornitori di software offrono una documentazione simile.  In questi casi sono necessarie altre strategie.  Per altre informazioni sulla convalida del software, fare riferimento a [Clean Source principle](https://aka.ms/cleansource) (Principio di origine pulita) per i supporti di installazione.

5. **Assicurarsi che il server WSUS sia disponibile nell'Intranet**. Per scaricare e installare gli aggiornamenti per PAW è necessario un server WSUS all'interno dell'intranet. Il server WSUS deve essere configurato in modo da approvare automaticamente tutti gli aggiornamenti della sicurezza per Windows 10. In alternativa, un membro del personale amministrativo deve avere l'incarico e assumersi la responsabilità di approvare rapidamente gli aggiornamenti software.

   > [!NOTE]
   > Per altre informazioni, vedere la sezione "Automatically Approve Updates for Installation" (Approvare automaticamente gli aggiornamenti per l'installazione) nelle indicazioni [Approving Updates](https://technet.microsoft.com/library/cc708458(v=ws.10).aspx) (Approvazione degli aggiornamenti).

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>Distribuire il framework della OU amministrativa che deve ospitare le workstation PAW

1. Scaricare la libreria di script PAW dalla [Raccolta TechNet](https://aka.ms/PAWmedia)

   > [!NOTE]
   > Scaricare tutti i file e salvarli nella stessa directory e quindi eseguirli nell'ordine specificato di seguito.  Create-PAWGroups dipende dalla struttura della OU creata da Create-PAWOUs e Set-PAWOUDelegation dipende dai gruppi creati da Create-PAWGroups.
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

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>Creare un nuovo oggetto Criteri di gruppo "PAW Configuration - Computer"

In questa sezione si creerà un nuovo oggetto Criteri di gruppo "PAW Configuration - Computer" (Configurazione PAW - Computer) che fornirà protezione specifica per queste workstation PAW e si collegherà tale oggetto all'OU Devices (Dispositivi) di livello 0 ("Devices" in Tier 0\Admin).

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
      * Replicators
   3. **PAW Logon Restrictions (Restrizioni accesso PAW)** : questa impostazione limita gli account che possono accedere all'ambiente PAW. Per configurare questa impostazione, attenersi alla procedura seguente:
      1. Passare a Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Consenti accesso locale.
      2. Selezionare Definisci le impostazioni relative ai criteri e aggiungere "PAW Users" (Utenti PAW) e Administrators. Anche in questo caso, non usare il pulsante Sfoglia per selezionare il gruppo Administrators.
   4. **Blocca traffico di rete in entrata**questa impostazione garantisce l'ambiente PAW non venga raggiunto da alcun traffico di rete in entrata indesiderato. Per configurare questa impostazione, attenersi alla procedura seguente:
      1. Passare a Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Windows Firewall con protezione avanzata\Windows Firewall con protezione avanzata e attenersi alla procedura seguente.
         1. Fare clic con il pulsante destro del mouse su Windows Firewall con protezione avanzata e selezionare **Importa criteri**.
         2. Fare clic su **Sì** per accettare la sovrascrittura dei criteri firewall eventualmente esistenti.
         3. Selezionare PAWFirewall.wfw e quindi selezionare **Apri**.
         4. Fare clic su **OK**.

            > [!NOTE]
            > A questo punto è possibile aggiungere indirizzi o subnet che devono raggiungere l'ambiente PAW con traffico indesiderato, ad esempio software di analisi di sicurezza o di gestione.
            > Le impostazioni nel file WFW abilitano la modalità "Blocca (predefinito)" per tutti i profili del firewall, disattivano l'unione di regole e consentono la registrazione sia dei pacchetti ignorati che di quelli ricevuti. Queste impostazioni bloccano il traffico indesiderato consentendo la comunicazione bidirezionale per le connessioni avviate da PAW, impediscono la creazione di regole firewall locali che sostituiscano le impostazioni degli oggetti Criteri di gruppo da parte degli utenti con accesso amministrativo locale e assicurano la registrazione del traffico da e verso PAW.
            > **L'apertura di questo firewall amplierà la superficie di PAW esposta ad attacchi e aumenterà e rischi per la sicurezza. Prima di aggiungere gli indirizzi, vedere la sezione sulla gestione e sul funzionamento di PAW in questo materiale sussidiario**.

   5. **Configurare Windows Update per WSUS**: attenersi alla procedura seguente per modificare le impostazioni di configurazione di Windows Update per le workstation PAW:
      1. Passare a Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Aggiornamenti di Windows e attenersi alla procedura seguente:
         1. Abilitare il **criterio Configura Aggiornamenti automatici**.
         2. Selezionare l'opzione **4 - Download automatico e pianificazione dell'installazione**.
         3. Modificare l'opzione **Giorno pianificato per l'installazione** in **0 - Ogni giorno** e l'opzione **Orario pianificato per l'installazione** sull'orario preferito dall'organizzazione.
         4. Abilitare l'opzione **Specifica il percorso del servizio di aggiornamento Microsoft nella rete Intranet** e specificare l'URL del server ESAE WSUS in entrambe le opzioni.
   6. Collegare l'oggetto Criteri di gruppo "PAW Configuration - Computer" (Configurazione PAW- Computer) come indicato di seguito:

         |Condizione|Percorso collegamento|
         |-----|---------|
         |PAW Configuration - Computer (Configurazione PAW- Computer) |Admin\Tier 0\Devices|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>Creare un nuovo oggetto Criteri di gruppo "PAW Configuration - User"

In questa sezione si creerà un nuovo oggetto Criteri di gruppo "PAW Configuration - User" (Configurazione PAW - Utente) che fornirà protezione specifica per queste workstation PAW e si collegherà tale oggetto all'OU Accounts (Account) di livello 0 ("Accounts" in Tier 0\Admin).

   > [!NOTE]
   > Non aggiungere queste impostazioni ai criteri di dominio predefiniti

1. **Block internet browsing** (Blocca esplorazione Internet): per evitare l'esplorazione accidentale di Internet, questa impostazione deve riportare un indirizzo proxy di un indirizzo di loopback (127.0.0.1).
   1. Passare a Configurazione utente\Preferenze\Impostazioni di Windows\Registro di sistema. Fare clic con il pulsante destro del mouse su Registro di sistema, scegliere **Nuovo** > **Elemento Registro di sistema** e configurare le impostazioni seguenti:
      1. Azione:  Sostituzione
      2. Hive: HKEY_CURRENT_USER
      3. Percorso chiave:  Software\Microsoft\Windows\CurrentVersion\Impostazioni Internet
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
   2. Passare a Configurazione utente\Preferenze\Impostazioni di Windows\Registro di sistema. Fare clic con il pulsante destro del mouse su Registro di sistema, scegliere **Nuovo** > **Elemento Registro di sistema** e configurare le impostazioni seguenti:

      * Azione: Sostituzione
      * Hive: HKEY_CURRENT_USER
      * Percorso chiave: Software\Microsoft\Windows\CurrentVersion\Impostazioni Internet
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
2. Passare a Configurazione utente\Criteri\Modelli amministrativi\Componenti di Windows\Internet Explorer e abilitare le opzioni seguenti. Queste impostazioni impediscono agli amministratori di sostituire manualmente le impostazioni del server proxy.
   1. Abilitare **Impedisci la modifica delle impostazioni di configurazione automatica**.
   2. Abilitare **Impedisci la modifica delle impostazioni del server proxy**.

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>Impedire agli amministratori l'accesso a host di livello inferiore

In questa sezione verranno configurati Criteri di gruppo per impedire agli account amministrativi con privilegi di accedere a host di livello inferiore.

1. Creare il nuovo oggetto Criteri di gruppo **Restrict Workstation Logon** (Limita accesso workstation): questa impostazione impedisce agli account amministratore di livello 0 e 1 di accedere alle workstation standard.  Questo oggetto Criteri di gruppo deve essere collegato all'OU di primo livello "Workstations" (Workstation) e deve avere le impostazioni seguenti:
   * In Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Nega accesso come processo batch selezionare **Definisci le impostazioni relative ai criteri** e aggiungere i gruppi di livello 0 e livello 1:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > gruppi di livello 0 predefiniti. Per altri dettagli, vedere Equivalenza di livello 0.

         Other Delegated Groups

     > [!NOTE]
     > gruppi personalizzati creati con accesso valido al livello 0. Per altri dettagli, vedere Equivalenza di livello 0.

         Tier 1 Admins

     > [!NOTE]
     > Questo gruppo è stato creato in precedenza durante la fase 1.

   * In Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Nega accesso come servizio selezionare **Definisci le impostazioni relative ai criteri** e aggiungere i gruppi di livello 0 e livello 1:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Nota: gruppi di livello 0 predefiniti. Per altri dettagli, vedere Equivalenza di livello 0.

         Other Delegated Groups

     > [!NOTE]
     > Nota: gruppi personalizzati creati con accesso valido al livello 0. Per altri dettagli, vedere Equivalenza di livello 0.

         Tier 1 Admins

     > [!NOTE]
     > Nota: questo gruppo è stato creato in precedenza durante la fase 1.

2. Creare il nuovo oggetto Criteri di gruppo **Restrict Server Logon** (Limita accesso server): questa impostazione impedisce agli account amministratore di livello 0 di accedere ai server di livello 1.  Questo oggetto Criteri di gruppo deve essere collegato all'OU di primo livello "Tier 1 Servers" (Server livello 1) e deve avere le impostazioni seguenti:
   * In Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Nega accesso come processo batch selezionare **Definisci le impostazioni relative ai criteri** e aggiungere i gruppi di livello 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > gruppi di livello 0 predefiniti. Per altri dettagli, vedere Equivalenza di livello 0.

         Other Delegated Groups

     > [!NOTE]
     > Gruppi personalizzati creati con accesso valido al livello 0. Per altri dettagli, vedere Equivalenza di livello 0.

   * In Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Nega accesso come servizio selezionare **Definisci le impostazioni relative ai criteri** e aggiungere i gruppi di livello 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Gruppi di livello 0 predefiniti. Per altri dettagli, vedere Equivalenza di livello 0.

         Other Delegated Groups

     > [!NOTE]
     > gruppi personalizzati creati con accesso valido al livello 0. Per altri dettagli, vedere Equivalenza di livello 0.

   * In Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\Nega accesso locale selezionare **Definisci le impostazioni relative ai criteri** e aggiungere i gruppi di livello 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Nota: gruppi di livello 0 predefiniti. Per altri dettagli, vedere Equivalenza di livello 0.

         Other Delegated Groups

     > [!NOTE]
     > Nota: gruppi personalizzati creati con accesso valido al livello 0. Per altri dettagli, vedere Equivalenza di livello 0.

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
   > Microsoft consiglia di usare [Local Administrator Password Solution (LAPS)](https://www.microsoft.com/download/details.aspx?id=46899) per gestire la password dell'account Administrator locale per tutte le workstation, incluse le workstation PAW.  Se si usa la soluzione LAPS, assicurarsi di concedere solo al gruppo PAW Maintenance (Manutenzione PAW) il diritto di lettura delle password delle workstation PAW gestite da LAPS.

3. Installare Strumenti di amministrazione remota del server per Windows 10 tramite il supporto di installazione di origine pulita.
4. Configurare Windows Defender Exploit Guard

   > [!NOTE]
   > Indicazioni da seguire per la configurazione

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

10. (Facoltativo) Scaricare e installare il software di accesso remoto necessario. Se per l'amministrazione gli amministratori useranno le workstation PAW in remoto, installare il software di accesso remoto rispettando le indicazioni di sicurezza del fornitore della soluzione di accesso remoto. Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in Clean Source for installation media (Origine pulita per i supporti di installazione).

    > [!NOTE]
    > Valutare con attenzione tutti i rischi impliciti nell'accesso remoto da una workstation PAW.  Un sistema PAW mobile consente molti scenari di una certa importanza, tra cui il lavoro da casa, ma il software di accesso remoto può essere vulnerabile agli attacchi e usato per compromettere una workstation PAW.

11. Per verificare l'integrità del sistema PAW, esaminare quest'ultimo per confermare che siano attivate tutte le impostazioni appropriate. A tale scopo usare la procedura seguente:
    1. Verificare che alla workstation PAW siano applicate solo Criteri di gruppo specifici dell'architettura PAW
       1. Aprire un prompt dei comandi con privilegi elevati e immettere il comando seguente: `Gpresult /scope computer /r`
       2. Esaminare l'elenco risultante e verificare che gli unici Criteri di gruppo presenti siano quelli creati in precedenza.
    2. Verificare che nessun account utente aggiuntivo sia membro di gruppi con privilegi della workstation PAW. A tale scopo, attenersi alla procedura seguente:
       1. Aprire **Modifica utenti e gruppi locali** (lusrmgr.msc), selezionare **Gruppi** e verificare che gli unici membri del gruppo Administrators locale siano l'account Administrator locale e il gruppo di sicurezza globale PAW Maintenance (Manutenzione PAW).

          > [!NOTE]
          > Il gruppo PAW Users (Utenti PAW) non deve essere membro del gruppo Administrators locale.  Gli unici membri devono essere l'account Administrator locale e il gruppo di sicurezza globale PAW Maintenance (Manutenzione PAW). Il gruppo PAW Users (Utenti PAW) non deve essere membro neanche di questo gruppo globale.

       2. Sempre tramite **Modifica utenti e gruppi locali** assicurarsi che i gruppi seguenti non contengano membri: Backup Operators, Cryptographic Operators, Hyper-V Administrators, Network Configuration Operators, Power Users, Remote Desktop Users, Replicators

12. (Facoltativo) Se l'organizzazione usa una soluzione di gestione di eventi e informazioni di sicurezza (SIEM, Security Information and Event Management), verificare che la workstation PAW sia [configurata per l'inoltro di eventi al sistema tramite WEF (Windows Event Forwarding)](https://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) o che sia registrata in altro modo con la soluzione, affinché la soluzione SIEM riceva attivamente eventi e informazioni dalla workstation PAW.  I dettagli di questa operazione dipendono dalla soluzione SIEM in uso.

    > [!NOTE]
    > Se la soluzione SIEM richiede l'esecuzione nella workstation PAW di un agente come account di sistema o amministratore locale, assicurarsi che le soluzioni SIEM vengano gestite con lo stesso livello di attendibilità dei controller di dominio e dei sistemi di gestione delle identità.

13. (Facoltativo) Se hai scelto di distribuire LAPS per gestire la password per l'account Administrator locale nella workstation PAW, verifica che la password sia registrata correttamente.

    * Tramite un account con autorizzazioni per la lettura di password gestite con LAPS aprire **Utenti e computer di Active Directory** (dsa.msc).  Assicurarsi che l'opzione Funzionalità avanzate sia abilitata e fare clic con il pulsante destro del mouse sull'oggetto computer appropriato.  Selezionare la scheda Editor attributi e verificare che il valore per msSVSadmPwd corrisponda a una password valida.

### <a name="phase-2-extend-paw-to-all-administrators"></a>Fase 2: Estensione di PAW a tutti gli amministratori

Ambito: tutti gli utenti con diritti amministrativi su applicazioni e dipendenze cruciali.  Questi devono corrispondere almeno agli amministratori dei server applicazioni, delle soluzioni di monitoraggio dell'integrità operativa e della sicurezza, delle soluzioni di virtualizzazione, dei sistemi di archiviazione e dei dispositivi di rete.

> [!NOTE]
> Le istruzioni relative a questa fase presuppongono che la fase 1 sia stata interamente completata.  Iniziare la fase 2 solo dopo aver completato tutti i passaggi della fase 1.

Dopo aver verificato l'esecuzione di tutti i passaggi, eseguire la procedura seguente per completare la fase 2:

#### <a name="recommended-enable-restrictedadmin-mode"></a>(Scelta consigliata) Abilitare la modalità **RestrictedAdmin**

Abilitare questa funzionalità nei server e nelle workstation esistenti e quindi imporne l'uso. Questa funzionalità richiede che nei server di destinazione sia in esecuzione Windows Server 2008 R2 o versione successiva e che nelle workstation di destinazione sia in esecuzione Windows 7 o versione successiva.

1. Abilitare la modalità **RestrictedAdmin** nei server e nelle workstation seguendo le istruzioni disponibili in questa [pagina](https://aka.ms/rdpra).

   > [!NOTE]
   > Prima di abilitare questa funzionalità per i server con connessione Internet, è necessario valutare il rischio che eventuali antagonisti siano in grado di effettuare l'autenticazione a tali server con l'hash di una password rubata in precedenza.

2. Creare l'oggetto Criteri di gruppo "RestrictedAdmin Required - Computer" (RestrictedAdmin necessaria - Computer). In questa sezione viene creato un oggetto Criteri di gruppo che impone l'uso dell'opzione /RestrictedAdmin per le connessioni Desktop remoto in uscita, per proteggere gli account dal furto di credenziali nei sistemi di destinazione
   * Passare a Configurazione computer\Criteri\Modelli amministrativi\Sistema\Delega di credenziali\Limita delega delle credenziali ai server remoti e impostare l'opzione su **Abilitato**.
3. Collegare **RestrictedAdmin** Required - Computer (RestrictedAdmin necessaria - Computer) ai dispositivi di livello 1 e/o 2 appropriati usando le opzioni dei criteri seguenti:
   * PAW Configuration - Computer (Configurazione PAW- Computer)
      * -> Percorso collegamento: Admin\Tier 0\Devices (esistente)
   * PAW Configuration - User (Configurazione PAW- Utente)
      * -> Percorso collegamento: Admin\Tier 0\Accounts
   * RestrictedAdmin Required - Computer (RestrictedAdmin necessaria - Computer)
      * -> Admin\Tier1\Devices o -> Admin\Tier2\Devices (entrambi facoltativi)

   > [!NOTE]
   > Questo non è necessario per i sistemi di livello 0, che hanno già il controllo completo di tutte le risorse nell'ambiente.

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>Spostare gli oggetti di livello 1 nelle OU appropriate

1. Spostare i gruppi di livello 1 nella OU Admin\Tier 1\Groups. Individuare tutti i gruppi che concedono i diritti amministrativi seguenti e spostarli in questa OU.
   * Amministratore locale in più server
      * Accesso amministrativo a servizi cloud
      * Accesso amministrativo ad applicazioni aziendali
2. Spostare gli account di livello 1 nella OU Admin\Tier 1\Accounts. Spostare ogni account membro di questi gruppi di livello 1 (incluse le appartenenze annidate) in questa OU.
3. Aggiungere i membri appropriati ai gruppi pertinenti
   * **Tier 1 Admins** (Amministratori livello 1): questo gruppo conterrà gli amministratori di livello 1 a cui non è consentito l'accesso agli host di livello 2. Aggiungere tutti i gruppi amministrativi di livello 1 che dispongono di privilegi amministrativi sui server o sui servizi Internet.

      > [!NOTE]
      > Se i compiti del personale amministrativo prevedono la gestione di risorse di più livelli, è necessario creare un account amministrativo separato per ogni livello.

4. Abilitare Credential Guard per ridurre il rischio di furto e riuso di credenziali.  Credential Guard è una nuova funzionalità di Windows 10 che limita l'accesso delle applicazioni alle credenziali, evitando così gli attacchi corrispondenti al furto delle credenziali (inclusi gli attacchi Pass-the-Hash).  Credential Guard è completamente trasparente per l'utente finale e la sua installazione richiede pochissimo tempo e un impegno minimo.  Per altre informazioni su Credential Guard, tra cui la procedura di distribuzione e i requisiti hardware, fare riferimento all'articolo [Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > Per configurare e usare Credential Guard è necessario abilitare Device Guard.  Tuttavia, per usare Credential Guard non è necessario configurare altre protezioni di Device Guard.

5. (Facoltativo) Abilitare la connettività a servizi cloud. Questo passaggio consente la gestione di servizi cloud quali Azure e Office 365 con garanzie di sicurezza appropriate. Questo passaggio è necessario anche per la gestione delle workstation PAW con Microsoft Intune.

   > [!NOTE]
   > Ignorare questo passaggio se non è necessaria la connettività al cloud per l'amministrazione dei servizi cloud o la gestione tramite Intune.

   * Questi passaggi limitano la comunicazione tramite Internet ai soli servizi cloud autorizzati (ma non alla rete Internet "aperta") e aggiungono misure di protezione ai browser e ad altre applicazioni che elaborano contenuto proveniente da Internet. Le workstation PAW per l'amministrazione non devono mai essere usate per attività utente standard come le comunicazioni e le attività produttive tramite Internet.
   * Per consentire la connettività ai servizi PAW attenersi alla procedura seguente:

   1. Configurare la workstation PAW per consentire solo destinazioni Internet autorizzate.  Man mano che si estende la distribuzione PAW per abilitare l'amministrazione cloud, è necessario consentire l'accesso ai servizi autorizzati, escludendo nello stesso tempo l'accesso a Internet "aperta", da cui è più probabile che provengano attacchi contro gli amministratori.

      1. Creare il gruppo **Cloud Services Admins** (Amministratori servizi cloud) e aggiungervi tutti gli account che richiedono l'accesso a servizi cloud in Internet.
      2. Scaricare il file *proxy.pac* per PAW dalla [Raccolta TechNet](https://aka.ms/pawmedia) e pubblicarlo in un sito Web interno.

         > [!NOTE]
         > Sarà necessario aggiornare il file *proxy.pac* dopo il download per assicurarsi che sia aggiornato e completo.  
         > Microsoft pubblica tutti gli URL relativi a Office 365 e ad Azure nel sito di [supporto tecnico per Office](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US). Queste istruzioni presuppongono che per l'amministrazione di Office 365, Azure e di altri servizi cloud si usi Internet Explorer o Microsoft Edge. Microsoft consiglia di configurare restrizioni simili per tutti i browser di terze parti necessari per l'amministrazione. Nelle workstation PAW i Web browser devono essere usati solo per l'amministrazione dei servizi cloud e mai semplicemente per esplorare il Web.
         >
         > Potrebbe essere necessario aggiungere all'elenco altre destinazioni Internet valide corrispondenti ad altri provider IaaS. All'elenco non devono però essere aggiunti siti di produttività, intrattenimento, notizie o ricerca.
         >
         > Potrebbe anche essere necessario modificare il file PAC per aggiungere un indirizzo proxy valido da usare per questi indirizzi.
         >
         > Per una difesa ancora più solida è possibile limitare l'accesso dalla workstation PAW anche tramite proxy Web. Non è consigliabile usare quest'ultimo autonomamente, senza il file PAC, perché per le workstation PAW l'accesso verrebbe limitato solo quando sono connesse alla rete aziendale.

      3. Dopo aver configurato il file *proxy.pac* aggiornare l'oggetto Criteri di gruppo PAW Configuration - User (Configurazione PAW- Utente).
         1. Passare a Configurazione utente\Preferenze\Impostazioni di Windows\Registro di sistema. Fare clic con il pulsante destro del mouse su Registro di sistema, scegliere **Nuovo** > **Elemento Registro di sistema** e configurare le impostazioni seguenti:
            1. Azione: Sostituzione
            2. Hive: HKEY_ CURRENT_USER
            3. Percorso chiave: Software\Microsoft\Windows\CurrentVersion\Impostazioni Internet
            4. Nome valore: AutoConfigUrl

               > [!NOTE]
               > Non selezionare la casella **Predefinito** a sinistra del nome valore.

            5. Tipo di valore: REG_SZ
            6. Dati valore: immettere l'URL completo del file *proxy.pac* includendo http:// e il nome del file, ad esempio http://proxy.fabrikam.com/proxy.pac.  L'URL può anche essere un URL con etichetta singola, ad esempio http://proxy/proxy.pac

               > [!NOTE]
               > Il file PAC può anche essere ospitato in una condivisione file con la sintassi file://server.fabrikan.com/share/proxy.pac ma ciò richiede che sia consentito il protocollo file://. Vedere la sezione "NOTE: File://-based Proxy Scripts Deprecated" (NOTA: script proxy basati su file:\\ obsoleti) del blog [Understanding Web Proxy Configuration](https://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) (Informazioni sulla configurazione di proxy Web) per altri dettagli sulla configurazione del valore del Registro di sistema.

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
   > Assicurarsi di proteggere le nuove applicazioni (inclusi i Web browser) inserendole nelle misure di protezione fornite da Windows Defender Exploit Guard.

   Alcuni esempi di applicazioni e software aggiuntivi sono:

      * Microsoft Azure PowerShell
      * PowerShell di Office 365 (noto anche come Modulo dei Microsoft Online Services)
      * Software di gestione di applicazioni o servizi basato su MMC (Microsoft Management Console)
      * Software di gestione di applicazioni o servizi proprietario (non basato su MMC)

         > [!NOTE]
         > Molte applicazioni vengono ora gestite esclusivamente tramite Web browser, inclusi molti servizi cloud.  Ciò consente di ridurre il numero di applicazioni che devono essere installate in una workstation PAW ma introduce anche il rischio di problemi di interoperabilità del browser.  Potrebbe essere necessario distribuire un Web browser non Microsoft in istanze PAW specifiche per consentire l'amministrazione di servizi particolari.  Se si distribuisce un Web browser aggiuntivo, assicurarsi di seguire tutti i principi di origine pulita e di proteggere il browser in base alle indicazioni di sicurezza del fornitore.

8. (Facoltativo) Scaricare e installare gli agenti di gestione richiesti.

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

    * Come si garantirà la conformità al nuovo processo?  Il modello PAW include una serie di controlli tecnici per evitare l'esposizione delle credenziali con privilegi. Non è tuttavia possibile evitare completamente tutti i casi di esposizione affidandosi esclusivamente ai controlli tecnici.  Ad esempio, sebbene sia possibile impedire a un amministratore di accedere a un desktop utente con credenziali con privilegi, il semplice fatto di tentare l'accesso può esporre le credenziali a malware eventualmente installato in quel desktop utente.  È pertanto essenziale descrivere in dettaglio non solo i vantaggi del modello PAW, ma anche i rischi legati alla mancata conformità.  Questo deve essere completato da controlli e avvisi, per consentire la rilevazione e la soluzione rapida di problemi di esposizione delle credenziali.

### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: estensione e miglioramento della protezione

Ambito: queste protezioni migliorano i sistemi creati nella fase 1, potenziando la protezione di base con funzionalità avanzate, come l'autenticazione a più fattori e le regole per l'accesso di rete.

> [!NOTE]
> Questa fase può essere eseguita in qualsiasi momento dopo il completamento della fase 1.  Non dipende dal completamento della fase 2 e pertanto può essere eseguita prima, contemporaneamente o dopo la fase 2.

Per configurare questa fase, attenersi alla procedura seguente:

1. **Abilitare l'autenticazione a più fattori per gli account con privilegi**.  L'autenticazione a più fattori aumenta il livello di sicurezza degli account, richiedendo all'utente, oltre alle credenziali, di presentare un token fisico.  L'autenticazione a più fattori integra i criteri di autenticazione in modo molto efficace, ma non dipende dai criteri di autenticazione per la distribuzione e, in modo analogo, i criteri di autenticazione non richiedono l'autenticazione a più fattori.  Microsoft consiglia di usare una di queste forme di autenticazione a più fattori:

   * **Smart card**: una smart card è un dispositivo fisico portatile e resistente alle manomissioni che consente una seconda verifica durante il processo di accesso a Windows.  Richiedendo a un utente di presentare una smart card per l'accesso, è possibile ridurre il rischio di furto di credenziali e di riuso di queste da remoto.  Per i dettagli sull'accesso con smart card a Windows, fare riferimento all'articolo [Panoramica delle smart card](https://technet.microsoft.com/library/hh831433.aspx).
   * **Smart card virtuale**:  una smart card virtuale offre gli stessi vantaggi di sicurezza delle smart card fisiche, con il vantaggio aggiuntivo del collegamento ad hardware specifico.  Per i dettagli sulla distribuzione e i requisiti hardware, fare riferimento agli articoli [Virtual Smart Card Overview](https://technet.microsoft.com/library/dn593708.aspx) (Panoramica delle smart card virtuali) e [Get Started with Virtual Smart Cards: Walkthrough Guide](https://technet.microsoft.com/library/dn579260.aspx) (Introduzione alle smart card virtuali: guida dettagliata).
   * **Windows Hello for Business**: Windows Hello for Business permette agli utenti di eseguire l'autenticazione per un account Microsoft, un account di Active Directory, un account di Microsoft Azure Active Directory (Azure AD) o un servizio non Microsoft che supporta l'autenticazione FIDO (Fast ID Online). Dopo una verifica in due passaggi iniziale durante la registrazione di Windows Hello for Business, nel dispositivo dell'utente viene configurata un'istanza di Windows Hello for Business e l'utente imposta un movimento, che può essere Windows Hello o un PIN. Le credenziali di Windows Hello for Business corrispondono a una coppia di chiavi asimmetrica, che può essere generata all'interno di ambienti isolati TPM (Trusted Platform Module).
      Per altre informazioni su Windows Hello for Business, vedere l'articolo [Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).
   * **Azure Multi-Factor Authentication**:  Azure Multi-Factor Authentication (MFA) offre la sicurezza di un secondo fattore di verifica, nonché protezione avanzata tramite monitoraggio e analisi basata su apprendimento automatico.  Azure MFA consente di proteggere non solo gli amministratori di Azure, ma anche molte altre soluzioni, tra cui applicazioni Web, Azure Active Directory e soluzioni locali come Desktop remoto e l'accesso remoto.  Per altre informazioni su Azure Multi-Factor Authentication, fare riferimento all'articolo [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication).

2. **Applicazioni attendibili dell'elenco di quelle consentite che usano Controllo di applicazioni di Windows Defender e/o AppLocker**.  Limitando la possibilità di eseguire codice non attendibile o non firmato in una workstation PAW, è possibile ridurre ancora di più la probabilità di attività di utenti malintenzionati e di compromissione.  Windows include due opzioni principali per il controllo delle applicazioni:

   * **AppLocker**:  AppLocker consente agli amministratori di mantenere il controllo sulle applicazioni che possono essere eseguite in un sistema specifico.  AppLocker può essere controllato in modo centralizzato tramite Criteri di gruppo e applicato a utenti o gruppi specifici (per applicazioni destinate a utenti di workstation PAW).  Per altre informazioni su AppLocker, fare riferimento all'articolo [Panoramica tecnica di AppLocker](https://technet.microsoft.com/library/hh831440.aspx) in TechNet.
   * **Controllo di applicazioni di Windows Defender**: la nuova funzionalità Controllo di applicazioni di Windows Defender offre il controllo avanzato basato su hardware delle applicazioni. A differenza di AppLocker, il controllo non può essere ignorato nel dispositivo interessato.  Come AppLocker, è possibile controllare Controllo di applicazioni di Windows Defender tramite Criteri di gruppo e destinarlo a utenti specifici.  Per altre informazioni sulla limitazione dell'utilizzo delle applicazioni con Controllo di applicazioni di Windows Defender, vedere [Guida alla distribuzione di Controllo di applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).

3. **Usare il gruppo Protected Users, criteri di autenticazione e silo di autenticazione per proteggere maggiormente gli account con privilegi**.  I membri di Protected Users sono soggetti a criteri di sicurezza aggiuntivi che proteggono le credenziali archiviate all'interno dell'agente LSA (Local Security Agent, agente di sicurezza locale) e riducono notevolmente il rischio di furto di credenziali e il riuso di queste.  I criteri e i silo di autenticazione controllano il modo in cui gli utenti con privilegi possono accedere alle risorse nel dominio.  Tutte insieme queste protezioni sono in grado di rafforzare notevolmente la sicurezza degli account degli utenti con privilegi.  Per altri dettagli su queste funzionalità, fare riferimento all'articolo Web [How to Configure Protected Accounts](https://technet.microsoft.com/library/dn518179.aspx) (Come configurare gli account protetti).

   > [!NOTE]
   > Queste protezioni hanno lo scopo di completare, non di sostituire, le misure di sicurezza esistenti della fase 1.  Gli amministratori devono comunque usare account separati per l'amministrazione e per l'uso generale.

## <a name="managing-and-updating"></a>Gestione e aggiornamento

Le workstation PAW devono avere funzionalità antimalware e gli aggiornamenti software devono essere applicati rapidamente per mantenere l'integrità delle workstation stesse.

Con le workstation PAW è possibile usare anche le funzioni di gestione di configurazioni aggiuntive e della sicurezza e di monitoraggio operativo, ma l'integrazione di queste funzioni deve essere considerato con attenzione, perché ogni funzionalità di gestione introduce anche il rischio di compromissione della workstation PAW attraverso lo strumento corrispondente. L'opportunità di introdurre funzionalità di gestione avanzata dipende da diversi fattori, tra cui:

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
|Gestione delle workstation PAW con gli strumenti di gestione già disponibili|-   Rischio significativo di compromissione delle workstation PAW, a meno che l'infrastruttura di gestione esistente non venga elevata al livello di sicurezza delle workstation stesse **Nota:**     Microsoft in genere sconsiglia questo approccio, a meno che l'organizzazione non abbia un motivo specifico per usarlo. In base all'esperienza, di solito il costo dovuto all'elevazione di tutti questi strumenti (e delle corrispondenti dipendenze di sicurezza) al livello di sicurezza delle workstation PAW è molto alto.<br />-   La maggior parte di questi strumenti garantisce visibilità e controllo della configurazione, della distribuzione software e degli aggiornamenti della sicurezza|
|Security Scanning o strumenti di monitoraggio che richiedono accesso amministratore|Qualsiasi strumento che consenta di installare un agente o che richieda un account con accesso amministrativo locale.<br /><br />-   Necessità di portare il controllo della sicurezza dello strumento al livello delle workstation PAW.<br />-   Eventuale necessità di abbassare il livello di sicurezza delle workstation PAW per consentire il funzionamento dello strumento (apertura di porte, installazione di Java o altro middleware e così via), con la necessità di prendere una decisione di compromesso sulla sicurezza|
|Soluzione SIEM (Security information and event management)|<ul><li>Se la soluzione SIEM è senza agenti<br /><br /><ul><li>Possibilità di accedere agli eventi nelle workstation PAW senza accesso amministrativo usando un account del gruppo **Lettori registri eventi**</li><li>Necessità di aprire porte di rete per consentire il traffico in ingresso dai server SIEM</li></ul></li><li>Se SIEM richiede un agente, vedere la riga **Security Scanning o strumenti di monitoraggio che richiedono accesso amministratore**.</li></ul>|
|WEF (Windows Event Forwarding)|- Metodo senza agente per l'inoltro di eventi di sicurezza dalle workstation PAW a un agente di raccolta o a una soluzione SIEM<br />-   Possibilità di accedere agli eventi nelle workstation PAW senza accesso amministrativo<br />-   Nessuna necessità di aprire porte di rete per consentire il traffico in ingresso dai server SIEM|

## <a name="operating-paws"></a>Uso delle workstation PAW

La soluzione PAW deve essere usata secondo gli standard in [Operational Standards](https://aka.ms/securitystandards) (Standard operativi) basati sul principio di origine pulita.

## <a name="deploy-paws-using-a-guarded-fabric"></a>Distribuire le workstation PAW usando un'infrastruttura sorvegliata

È possibile usare un'[infrastruttura sorvegliata](https://aka.ms/shieldedvms) per eseguire carichi di lavoro di PAW in una macchina virtuale schermata su un portatile o un jump server.
L'adozione di questo approccio richiede un'altra infrastruttura e passaggi operativi aggiuntivi, ma può semplificare la ridistribuzione delle immagini PAW a intervalli regolari e consente di consolidare più workstation PAW a livelli (o classificazioni) differenti in macchine virtuali in esecuzione side-by-side su un singolo dispositivo.
Per una spiegazione completa della topologia e delle promesse di sicurezza dell'infrastruttura sorvegliata, vedere la relativa [documentazione](https://aka.ms/shieldedvms).

### <a name="changes-to-the-paw-gpos"></a>Modifiche agli oggetti Criteri di gruppo di PAW

Quando si usano le workstation PAW basate su VM schermate, è necessario modificare le [impostazioni degli oggetti Criteri di gruppo](#create-paw-configuration---computer-group-policy-object-gpo) definite in precedenza in modo da supportare l'uso delle macchine virtuali.

1. Creare una nuova OU per gli host PAW fisici. Le workstation PAW fisiche e virtuali hanno requisiti di sicurezza diversi e devono essere separate di conseguenza in Active Directory.
2. L'oggetto Criteri di gruppo del computer PAW deve essere collegato alle OU PAW fisiche e virtuali.
3. Creare un nuovo oggetto Criteri di gruppo per il computer PAW fisico per aggiungere gli utenti di PAW al gruppo Hyper-V Administrators. Questa operazione è necessaria per consentire agli amministratori di connettersi alle VM di amministrazione e di attivarle o disattivarle quando necessario. È importante che l'utente che accede al computer PAW fisico non disponga dei diritti di amministratore, dell'accesso a Internet o della possibilità di copiare dati di macchine virtuali dannosi da condivisioni di rete o dispositivi di archiviazione esterni in tale computer.
4. Creare un nuovo oggetto Criteri di gruppo per le VM di amministrazione per aggiungere utenti di PAW al gruppo Remote Desktop Users. Ciò consentirà agli utenti di usare le sessioni della console avanzata di Hyper-V, che offrono un'esperienza utente migliore e abilitano il pass-through tramite smart card alla VM.

### <a name="set-up-the-host-guardian-service"></a>Configurare il servizio Sorveglianza host

Il servizio Sorveglianza host (HGS, Host Guardian Service) è responsabile dell'attestazione dell'identità e dell'integrità di un dispositivo PAW fisico.
Solo i computer noti al servizio HGS che eseguono un [criterio di integrità del codice](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) attendibile possono avviare VM schermate.
Ciò consente di proteggere le VM schermate, che eseguono carichi di lavoro attendibili per gestire le risorse a più livelli, da minacce all'ambiente desktop dell'utente.

Poiché il servizio HGS è responsabile della determinazione dei dispositivi che possono eseguire VM PAW, viene considerato una risorsa di livello 0.
Deve essere distribuito insieme ad altre risorse di livello 0 e protetto da tentativi di accesso fisico e logico non autorizzati.
HGS è un ruolo del cluster che semplifica la scalabilità orizzontale per distribuzioni di qualsiasi dimensione.
La regola generale consiste nel pianificare 1 server HGS per ogni 1000 dispositivi, con un minimo di 3 nodi.

1. Per installare il primo server HGS, iniziare con l'articolo [Installare HGS in una foresta Bastion esistente](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md) e aggiungere HGS al dominio di livello 0.

2. Quindi, [creare certificati per HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md) usando l'autorità di certificazione dell'organizzazione.
Tutti gli utenti che possiedono i certificati di crittografia e firma HGS possono decrittografare una VM schermata. Di conseguenza, se si ha accesso a un modulo di protezione hardware per proteggere le chiavi private, è consigliabile generare questi certificati usando tale modulo.
Per maggiore sicurezza, selezionare una dimensione della chiave maggiore o pari a 4096 bit.

3. Seguire infine la procedura per [inizializzare il server HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md) in **modalità TPM**.
L'inizializzazione consente di configurare i servizi Web di attestazione e protezione delle chiavi usati dalle workstation PAW.
Il servizio HGS deve essere [configurato con un certificato TLS](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md) per proteggere queste comunicazioni e solo la porta 443 deve essere aperta al servizio HGS da reti non attendibili.

4. Seguire i passaggi per [aggiungere nodi](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md) per il secondo nodo, il terzo e per altri nodi HGS.

5. Se il server HGS esegue Windows Server 2019 o versione successiva, è possibile abilitare una funzionalità facoltativa per memorizzare nella cache le chiavi per le VM schermate in PAW, in modo che possano essere usate offline. Le chiavi sono sealed nella configurazione di sicurezza corrente del sistema per impedire a un utente di usare chiavi memorizzate nella cache in un altro computer o nello stesso computer in uno stato non protetto. Questa soluzione può essere utile se gli utenti di PAW non hanno possibilità di accedere a Internet, ma devono comunque essere in grado di accedere alle macchine virtuali PAW. Per usare questa funzionalità, eseguire il comando seguente su qualsiasi server HGS:

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>Configurare il dispositivo PAW fisico

Per impostazione predefinita, il dispositivo PAW fisico viene considerato non attendibile nella soluzione con infrastruttura sorvegliata.
Può dimostrare di essere affidabile durante il processo di attestazione, dopo il quale può ottenere le chiavi necessarie per avviare una VM di amministrazione schermata.
Il dispositivo deve essere in grado di eseguire Hyper-V e avere l'avvio protetto e un modulo TPM 2.0 abilitato per soddisfare i [prerequisiti dell'host sorvegliato](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md).
La versione minima del sistema operativo per il supporto di tutte le funzionalità di PAW è **Windows 10 versione 1803**.

Il dispositivo PAW fisico deve essere configurato come qualsiasi altro dispositivo, tranne nel caso in cui gli utenti di PAW debbano essere amministratori di Hyper-V per poter attivare la VM di amministrazione e connettersi a tale VM.
In un ambiente di camera bianca sarà necessario creare una configurazione modello per ogni combinazione hardware/software univoca distribuita come host sorvegliato per le VM di amministrazione.
In ogni configurazione modello completare le attività seguenti:

1. Installare gli aggiornamenti più recenti per Windows, i driver e il firmware nel computer, nonché gli agenti di monitoraggio o gestione di terze parti.
2. [Acquisire le informazioni di base necessarie](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md), tra cui l'identificatore univoco del modulo TPM (chiave di verifica dell'autenticità), le misurazioni di avvio (log TCG) e i criteri di integrità del codice per il computer.
3. Copiare questi artefatti in un server HGS ed eseguire i comandi di attestazione di HGS nell'articolo precedente per registrare l'host. Se tutti gli host usano lo stesso criterio di integrità del codice e/o la stessa configurazione hardware, è sufficiente registrare il log dei criteri di integrità del codice/TCG una sola volta.

### <a name="create-the-signed-template-disk"></a>Creare il disco modello firmato

Le VM schermate vengono create usando dischi modello firmati.
La firma viene verificata in fase di distribuzione per determinare l'integrità e l'autenticità del disco prima di rilasciare i segreti, ad esempio la password dell'amministratore nella VM.

Per creare un disco modello firmato, seguire i passaggi della fase 1 per la distribuzione in una normale macchina virtuale di seconda generazione.
Il computer diventerà l'immagine modello per una VM di amministrazione.
È possibile creare più dischi modello per avere strumenti specializzati disponibili in contesti diversi.

Quando la VM è configurata nel modo desiderato, eseguire `C:\Windows\System32\sysprep\sysprep.exe` e scegliere di **generalizzare** il disco. Al termine della generalizzazione, **arrestare** il sistema operativo.

Eseguire infine la [Creazione guidata disco modello](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md) nel file VHDX dalla VM per installare i componenti di BitLocker e generare la firma del disco.

#### <a name="create-the-shielding-data-file"></a>Creare il file di dati di schermatura

Il disco modello generalizzato è associato a un file di dati di schermatura che contiene i segreti necessari per effettuare il provisioning di una VM schermata.
Il file di dati di schermatura include:
   - Un elenco di agenti di sorveglianza che definiscono le infrastrutture in cui è consentita l'esecuzione della VM. Ogni cluster HGS è un agente di sorveglianza per i dispositivi PAW che protegge.
   - Un elenco di firme del disco attendibili per la distribuzione. I file di dati di schermatura rilasciano i propri segreti solo alle VM create con supporti di origine autorizzati.
   - Un criterio di sicurezza che determina se devono essere applicate altre misure per proteggere la VM dall'host e se è consentito lo spostamento della VM in un altro computer. Le VM di amministrazione PAW devono essere sempre completamente schermate.
   - Il file di specializzazione unattend.xml, che consente a Windows di completare automaticamente l'installazione e include segreti come la password dell'amministratore locale.
   - File aggiuntivi, ad esempio i certificati RDP o VPN.

Per la procedura da seguire per creare un file di dati di schermatura, vedere l'[articolo relativo al file di dati di schermatura](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md).

Le chiavi del proprietario per le VM schermate sono estremamente riservate e devono essere conservate in un modulo di protezione hardware o archiviate offline in una posizione sicura.
Queste chiavi possono essere usate in uno scenario di emergenza per l'avvio di una VM schermata senza la presenza di HGS.

È fortemente consigliabile che i dati di schermatura per le VM di amministrazione PAW includano l'impostazione per bloccare una VM nel primo host fisico in cui viene avviata.
In questo modo si impedisce il trasferimento di una VM di amministrazione da un dispositivo PAW a un altro nello stesso ambiente.
Per usare questa funzionalità, creare il file di dati di schermatura con PowerShell e includere il parametro **BindToHostTpm**:

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>Distribuire una VM di amministrazione

Quando il disco modello e il file di dati di schermatura sono pronti, è possibile distribuire una VM di amministrazione in qualsiasi workstation PAW registrata con HGS.

1. Copiare il disco modello (con estensione vhdx) e il file di dati di schermatura (con estensione pdk) in un dispositivo PAW attendibile.
2. Seguire le istruzioni per [distribuire una nuova VM schermata usando PowerShell](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md).
3. Completare i passaggi rimanenti della fase 1 del processo di distribuzione per proteggere il sistema operativo della VM e configurarlo per il ruolo desiderato, in base alle esigenze.


## <a name="related-topics"></a>Argomenti correlati

[Coinvolgimento dei servizi Microsoft per la sicurezza informatica](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Taste of Premier: How to Mitigate Pass-the-Hash and Other Forms of Credential Theft](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft) (Taste of Premier: come prevenire Pass-the-Hash e altre forme di furto delle credenziali)

[Analisi avanzata delle minacce Microsoft](https://aka.ms/ata)

[Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)

[Panoramica di Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)

[Protezione delle risorse di valore elevato con workstation amministrative protette](https://msdn.microsoft.com/library/mt186538.aspx)

[Modalità utente isolato in Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Processi e funzionalità della modalità utente isolato in Windows 10 con Logan Gabriel (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Altre informazioni sui processi e sulle funzionalità nella modalità utente isolato di Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Prevenzione del furto di credenziali usando la modalità utente isolato di Windows 10 (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Abilitazione della convalida KDC ristretta in Kerberos per Windows](https://www.microsoft.com/download/details.aspx?id=6382)

[Novità dell'autenticazione Kerberos per Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Guida dettagliata alla funzionalità di verifica dell'autenticazione per Servizi di dominio Active Directory in Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Trusted Platform Module](C:/sd/docs/p_ent_keep_secure/p_ent_keep_secure/trusted_platform_module_technology_overview.xml)
