---
title: Privileged Access workstation
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/09/2016
ms.openlocfilehash: ce64974d771a11ef11257bceef1c3fde1797a7da
ms.sourcegitcommit: 3bf47cf4e25896725137d983d63b8041a53cb9a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2018
---
# <a name="privileged-access-workstations"></a>Privileged Access workstation

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

(Privileged Access workstation) forniscono un sistema operativo dedicato per le attività sensibili protetto dagli attacchi da Internet e vettori di minacce. Separando queste attività sensibili e l'account da ogni giorno Usa le workstation e i dispositivi offre una protezione avanzata da attacchi di phishing, applicazioni e le vulnerabilità del sistema operativo, diversi attacchi di rappresentazione e attacchi contro il furto di credenziali, ad esempio la registrazione di sequenza di tasti, [Pass-the-Hash](https://www.microsoft.com/en-us/download/details.aspx?id=36036), e [Pass-The-Ticket](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf).

## <a name="architecture-overview"></a>Panoramica dell'architettura
Il diagramma seguente illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile) creato tramite la gestione separata di account amministrativa dedicata e workstation.

![Diagramma che illustra un "canale" separato per l'amministrazione (un'attività estremamente sensibile) creato tramite la gestione separata di account amministrativa dedicata e workstation](../media/privileged-access-workstations/PAWFig1.JPG)

Questo approccio architettonico si basa sulle misure di sicurezza disponibili in Windows 10 [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx) funzionalità e si spinge oltre tali misure per gli account sensibili e attività.

Questa metodologia è appropriata per gli account con accesso a risorse di valore elevato:

-   **Privilegi amministrativi** le workstation Paw offrono una maggiore sicurezza per ad alto impatto ruoli amministrativi IT e attività. Questa architettura può essere applicata all'amministrazione di molti tipi di sistemi tra Active Directory domini e foreste, i tenant di Microsoft Azure Active Directory, tenant di Office 365, processo di controllo reti (PCN), sistemi Supervisory Control and acquisizione dati (SCADA), Bancomat (bancomat) e dispositivi punti vendita (PoS).

-   **Elevata sensibilità gli Information Worker** l'approccio utilizzato in una workstation PAW può anche fornire protezione per le attività di lavoro a informazioni riservate e personale, ad esempio quelle che includono attività di unione e acquisizione pre-annuncio, pre-release rendiconti finanziari, presenza nei social media organizzativa, comunicazioni executive, segreti commerciali non brevettati, research sensibili o altri dati sensibili o proprietari. Questa Guida non illustrare la configurazione di questi scenari di lavoro di informazioni approfondite sul né include questo scenario nelle istruzioni tecniche.

    > [!NOTE]
    > IT Microsoft Usa workstation Paw (, definite internamente come "le workstation amministrative protette" o saw) per gestire l'accesso sicuro alle interni sistemi a valore elevato all'interno di Microsoft. Questo materiale sussidiario ulteriori dettagli seguito all'utilizzo di workstation PAW presso Microsoft nella sezione "Come Microsoft Usa le workstation amministrative". Per ulteriori informazioni su questo approccio agli ambienti di asset di valore elevato, fare riferimento all'articolo [la protezione delle risorse di valore elevato con secure admin workstation](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Questo documento viene descritto il motivo per cui questa procedura è consigliata per la protezione di account con privilegiata a impatto elevato, aspetto delle soluzioni PAW per la protezione di privilegi amministrativi e come distribuire rapidamente una soluzione PAW per l'amministrazione di servizi di dominio e cloud.

Questo documento offre indicazioni dettagliate per l'implementazione di diverse configurazioni PAW e include istruzioni dettagliate per consentirti di iniziare a proteggere gli account ad alto impatto comuni:

-   **Fase 1: distribuzione immediata per amministratori di Active Directory** in questo modo rapidamente un'architettura PAW può proteggere in locale, ruoli di amministrazione di domini e foreste

-   **Fase 2: estensione di PAW a tutti gli amministratori** in questo modo di protezione per gli amministratori di servizi cloud quali Office 365 e Azure, server aziendali, le applicazioni aziendali e workstation

-   **Fase 3: protezione PAW avanzata** questo vengono illustrate le protezioni aggiuntive e considerazioni sulla sicurezza delle workstation PAW

### <a name="why-a-dedicated-workstation"></a>Perché una workstation dedicata?
L'ambiente di minaccia corrente per le organizzazioni è pullula di sofisticati tipi di phishing e altri attacchi via internet che crea un rischio costante di compromissione della sicurezza per gli account esposti a internet e workstation.

Questo universo richiede le organizzazioni ad adottare una condizione di sicurezza "presunzione della violazione" quando si progetta protezioni per risorse di valore elevato, ad esempio account amministrativi e risorse aziendali sensibili. Queste risorse di elevato valore devono essere protette sia diretta rischi di internet, nonché gli attacchi predisposti da altri workstation, server e i dispositivi nell'ambiente.

![Figura che illustra il rischio di risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili](../media/privileged-access-workstations/PAWFig2.JPG)

La figura illustra i rischi per risorse gestite se un utente malintenzionato ottiene il controllo di una workstation utente in cui vengono usate credenziali sensibili.

Un utente malintenzionato nel controllo di un sistema operativo è in vari modi in cui illecitamente a ottenere l'accesso a tutte le attività della workstation e rappresentare l'account legittimo. Per ottenere questo livello di accesso, è possibile utilizzare una serie di tecniche di attacco note e sconosciute. L'aumento volume e la complessità degli attacchi informatici ha reso necessario estendere concetto di separazione per separare completamente i sistemi operativi client per gli account sensibili. Per ulteriori informazioni su questi tipi di attacchi, visitare il [sito Web di Pass-The-Hash](https://www.microsoft.com/pth) per white paper informativi, video e altro ancora.

L'approccio PAW è un'estensione della procedura consigliata ormai consolidata di usare gli account utente e amministrativo separato per il personale amministrativo. Questa procedura utilizza un account amministrativo singolarmente assegnato completamente separati dall'account utente standard dell'utente. PAW si basa su tale pratica di separazione di account, fornendo una workstation attendibile per gli account sensibili.

> [!NOTE]
> IT Microsoft Usa workstation Paw (, definite internamente come "le workstation amministrative protette" o saw) per gestire l'accesso sicuro alle interni sistemi a valore elevato all'interno di Microsoft.  Questo materiale sussidiario ulteriori dettagli sull'uso di workstation PAW presso Microsoft nella sezione "Come Microsoft Usa le workstation amministrative"
>
> Per ulteriori informazioni su questo approccio agli ambienti di asset di valore elevato, fare riferimento all'articolo [la protezione delle risorse di valore elevato con secure admin workstation](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Questo materiale sussidiario su PAW deve implementare questa funzionalità per la protezione degli account di valore elevato, ad esempio gli amministratori IT con privilegi elevati e gli account aziendali a elevata sensibilità. Il materiale sussidiario consente di:

-   Limitare l'esposizione delle credenziali ai soli host attendibili

-   Fornire una workstation ad alta sicurezza agli amministratori che consentono di eseguire facilmente attività amministrative.

Limitare gli account sensibili all'uso solo protezione avanzata delle workstation Paw è una semplice protezione per questi account che è molto difficile per un utente malintenzionato di avversari sia elevata utilizzabile per gli amministratori.

#### <a name="alternate-approaches---limitations-considerations-and-integration"></a>Approcci alternativi: limitazioni, considerazioni e integrazione
In questa sezione contiene informazioni su come la sicurezza approcci alternativi Confronta PAW e sulla corretta integrazione questi approcci all'interno di un'architettura PAW. Tutti questi approcci comportano rischi significativi se implementati individualmente, ma possono aggiungere valore a un'implementazione di workstation PAW in alcuni scenari.

**Credential Guard e Microsoft Passport**

Introdotto in Windows 10, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) utilizza hardware e protezione basata su virtualizzazione per prevenire gli attacchi contro il furto delle credenziali comuni, ad esempio Pass-the-Hash, proteggendo le credenziali derivate. La chiave privata per le credenziali utilizzate da [Microsoft Passport](http://aka.ms/passport) può essere protetta anche tramite hardware modulo TPM (Trusted Platform).

Queste sono soluzioni di prevenzione avanzate, ma le workstation possono comunque essere vulnerabili a certi attacchi, anche se le credenziali sono protette tramite Credential Guard o Passport. Attacchi possono includere l'abuso di privilegi e l'uso di credenziali direttamente da un dispositivo compromesso, il riuso di credenziali rubate prima dell'abilitazione di Credential Guard e l'abuso di configurazioni di applicazione di strumenti e weak gestione nella workstation.

Il materiale sussidiario su PAW in questa sezione riguardano l'uso di molte di queste tecnologie per gli account a sensibilità elevata e le attività.

**VM amministrative**

Un'amministratore macchina virtuale (VM) è un sistema operativo dedicato per le attività amministrative ospitato in un desktop utente standard. Questo approccio è simile all'architettura PAW nella fornitura di un sistema operativo dedicato per le attività amministrative, ha un grave difetto: tale VM amministrative è dipendente sul desktop utente standard per la protezione.

Il diagramma seguente illustra la possibilità di utenti malintenzionati di seguire la catena di controllo per l'oggetto di interesse con una VM Admin in una Workstation utente e che è difficile creare un percorso nella configurazione inversa.

L'architettura PAW non consente l'hosting una VM amministrativa in una workstation utente, ma una VM utente con un'immagine aziendale standard può essere ospitata in un host PAW per garantire il personale con un unico PC per tutte le responsabilità.

![Diagramma dell'architettura PAW](../media/privileged-access-workstations/PAWFig9.JPG)

**Jump Server**

Architetture "Jump Server" amministrative impostare un piccolo numero di server console di amministrazione e obbligano il personale a usarle per le attività amministrative. Questo in genere si basa su servizi desktop remoto, una soluzione di virtualizzazione 3rd party presentazione o una tecnologia di Virtual Desktop Infrastructure (VDI).

Questo approccio, proposto spesso per attenuare i rischi per l'amministrazione e fornire alcune delle garanzie di sicurezza, ma l'approccio jump server da solo è vulnerabile ad alcuni attacchi, perché viola il [principio di "origine pulita"](http://aka.ms/cleansource). Il principio di origine pulita richiede tutte le dipendenze di sicurezza siano attendibili come l'oggetto protetto.

![Figura che illustra una relazione di controllo semplice](../media/privileged-access-workstations/PAWFig3.JPG)

La figura illustra una relazione di controllo semplice. Qualsiasi soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto. Se un antagonista può controllare una dipendenza di sicurezza di un oggetto di destinazione (soggetto), è possibile controllare tale oggetto.

La sessione di amministrazione nel jump server si basa sull'integrità del computer locale che vi accedono. Se questo computer è una workstation utente soggetti ad attacchi di phishing e altri vettori di attacco basato su internet, quindi la sessione di amministrazione è soggetta a tali rischi.

![Figura che illustra come gli utenti malintenzionati possono seguire una catena di controllo stabilita verso l'oggetto di interesse](../media/privileged-access-workstations/PAWFig4.JPG)

La figura sopra riportata illustra come gli utenti malintenzionati possono seguire una catena di controllo stabilita verso l'oggetto di interesse.

Mentre alcuni controlli di sicurezza avanzate come autenticazione a più fattori può aumentare il livello di difficoltà di un utente malintenzionato acquisisce la sessione di amministrazione dalla workstation utente, alcuna funzionalità di sicurezza completamente possono di proteggersi da attacchi tecnici quando un utente malintenzionato dispone di diritti amministrativi del computer di origine (ad esempio, inserimento di comandi illeciti in una sessione legittima, l'Hijack di processi legittimi e così via.)

La configurazione predefinita in questo materiale sussidiario su PAW Installa strumenti di amministrazione nella workstation PAW, ma è inoltre possibile aggiungere un'architettura jump server se necessario.

![Figura che illustra come non invertire la relazione di controllo accesso alle App utente da una workstation amministrativa non offre all'utente malintenzionato alcun percorso all'oggetto di destinazione](../media/privileged-access-workstations/PAWFig5.JPG)

Questa figura illustra come non invertire la relazione di controllo accesso alle App utente da una workstation amministrativa non offre all'utente malintenzionato alcun percorso all'oggetto di destinazione. Il jump server utente è ancora esposto a rischi in modo che i controlli di protezione appropriati, controlli detective e processi di risposta ancora devono essere applicati per il computer a internet.

Questa configurazione gli amministratori devono seguire le procedure operative scrupolosamente che evitino l'immissione accidentale le credenziali di amministratore nella sessione utente del desktop.

![Figura che illustra come accedere a un jump server amministrativo da una workstation PAW non aggiunge alcun percorso per l'utente malintenzionato alle risorse amministrative](../media/privileged-access-workstations/PAWFig6.JPG)

Questa figura illustra come accedere a un jump server amministrativo da una workstation PAW non aggiunge alcun percorso per l'utente malintenzionato alle risorse amministrative. Un jump server dotato di PAW consente in questo caso di consolidare il numero di percorsi per il monitoraggio dell'attività di amministrazione e la distribuzione di applicazioni e strumenti amministrativi. Aggiunge una certa complessità di progettazione, ma può semplificare gli aggiornamenti software e di monitoraggio della sicurezza, se un numero elevato di account e workstation venga usato nell'implementazione PAW. Il jump server dovranno essere creato e configurato agli standard di sicurezza simili a quelli della workstation PAW.

**Soluzioni di gestione dei privilegi**

Soluzioni di gestione dei privilegi sono applicazioni che consentono l'accesso temporaneo a privilegi discreti o gli account con privilegi su richiesta. Soluzioni di gestione dei privilegi sono un componente estremamente prezioso di una strategia completa per proteggere l'accesso con privilegiato e fornire visibilità di importanza critica e responsabilità dell'attività amministrativa.

Queste soluzioni usano in genere un flusso di lavoro flessibile per concedere l'accesso e molte hanno funzionalità di sicurezza aggiuntive e funzionalità come la gestione delle password account del servizio e l'integrazione con i jump server amministrativi. Sono disponibili molte soluzioni sul mercato che forniscono funzionalità di gestione, uno dei quali è la gestione degli accessi di Microsoft Identity Manager (MIM) con privilegiata (PAM) di privilegi.

Microsoft consiglia di usare una workstation PAW per soluzioni di gestione accesso con privilegi. Accesso a queste soluzioni deve essere concesso solo a workstation Paw. Microsoft sconsiglia di usare queste soluzioni in sostituzione di una workstation PAW, perché l'accesso ai privilegi tramite queste soluzioni da un desktop utente potenzialmente compromesso viola il [origine pulita](http://aka.ms/cleansource) principio come illustrato nel diagramma seguente:

![Diagramma che illustra come consiglia di non usare queste soluzioni in sostituzione di una workstation PAW, perché l'accesso ai privilegi tramite queste soluzioni da un desktop utente potenzialmente compromesso viola il principio di origine pulita](../media/privileged-access-workstations/PAWFig7.JPG)

Fornire una workstation PAW per l'accesso a queste soluzioni consente di assicurare i vantaggi di sicurezza della workstation PAW sia la soluzione di gestione dei privilegi, come illustrato in questo diagramma:

![Diagramma che illustra come fornire una workstation PAW per l'accesso a queste soluzioni consente di assicurare i vantaggi di sicurezza della workstation PAW sia la soluzione di gestione dei privilegi](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Questi sistemi devono essere classificati al livello più alto del privilegio che gestiscono e da proteggere uguale o superiore a livello di sicurezza. Questi sono in genere configurati per gestire soluzioni di livello 0 e le risorse di livello 0 e devono essere classificati al livello 0.
> Per ulteriori informazioni sul modello a livelli, vedere [http://aka.ms/tiermodel](http://aka.ms/tiermodel) per ulteriori informazioni sui gruppi di livello 0, vedere l'equivalenza di livello 0 in [protezione materiale di riferimento accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md).

Per ulteriori informazioni sulla distribuzione di gestione di accesso di Microsoft Identity Manager (MIM) con privilegi (PAM), vedere [http://aka.ms/mimpamdeploy](http://aka.ms/mimpamdeploy)

#### <a name="how-microsoft-is-using-admin-workstations"></a>Come Microsoft Usa le workstation amministrative
Microsoft Usa l'approccio architettonico PAW sia internamente nei propri sistemi che con i nostri clienti. Microsoft Usa workstation amministrative internamente in un numero di capacità, tra cui amministrazione dell'infrastruttura IT, lo sviluppo dell'infrastruttura cloud Microsoft e le operazioni e altre risorse di valore elevato.

Questo materiale sussidiario si basa direttamente sull'architettura di riferimento Workstation di accesso con privilegi (PAW) distribuita dai nostri team di servizi professionali riguardanti la sicurezza informatica per proteggere i clienti da attacchi alla sicurezza. Le workstation amministrative sono anche un elemento chiave della massima protezione per le attività amministrative di dominio, l'architettura di riferimento foresta amministrativa avanzata sicurezza amministrativa ambiente (ESAE).

Per ulteriori dettagli sulla foresta amministrativa ESAE, vedere [approccio di progettazione di foresta amministrativa ESAE](http://aka.ms/ESAE) sezione [protezione materiale di riferimento accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md).

Per ulteriori informazioni sull'impegno dei servizi Microsoft per la distribuzione di un'architettura PAW o ESAE per l'ambiente, contattare il rappresentante Microsoft o visitare [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="what-is-a-privileged-access-workstation-paw"></a>Che cos'è una Workstation di accesso con privilegi (PAW)?
In termini semplici, una workstation PAW è una protezione avanzata e bloccata workstation progettata per fornire garanzie di sicurezza elevato per le attività e gli account sensibili.  Workstation Paw sono consigliate per l'amministrazione dei sistemi di identità, i servizi cloud e dell'infrastruttura di cloud privato, oltre a funzioni aziendali sensibili.

> [!NOTE]
> L'architettura PAW non richiede un mapping 1:1 degli account alle workstation, anche se questa è una configurazione comune. Questa architettura crea un ambiente workstation attendibile che può essere utilizzato da uno o più account.

Per garantire la massima sicurezza, workstation Paw deve essere sempre eseguito più aggiornato e protetto sistema operativo disponibile: si consiglia di Windows 10 Enterprise, che include una serie di funzionalità di sicurezza aggiuntive non disponibili in altre edizioni (in particolare, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Le organizzazioni senza accesso a Windows 10 Enterprise possono usare Windows 10 Pro, che include molte delle tecnologie critiche fondamentali per Paw, tra cui avvio sicuro, BitLocker e Desktop remoto.  I clienti Education possono utilizzare Windows 10 Education.  Windows 10 Home non da utilizzare per una workstation PAW.
>
> Per una matrice di confronto delle diverse edizioni di Windows 10, Leggi [in questo articolo](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

I controlli di sicurezza in PAW si concentrano sulla prevenzione del massimo impatto e dei rischi più probabili di compromissione. Sono inclusi gli attacchi sull'ambiente e attenuazione dei rischi che possono influire negativamente dei controlli PAW nel tempo:

-   **Attacchi da Internet** -la maggior parte degli attacchi deriva direttamente o indirettamente da origini internet e Usa internet per sottrarre dati e comando e controllo (C2). Isolare la workstation PAW dai siti internet è un elemento fondamentale per garantire che la workstation non venga compromessa.

-   **Il rischio di usabilità** -se una workstation PAW è troppo difficile da usare per le attività quotidiane, gli amministratori sono portati a creare soluzioni alternative per facilitare il lavoro. Spesso le soluzioni alternative espongono gli account a rischi di sicurezza e una workstation amministrativa è fondamentale coinvolgere consentire agli utenti di workstation PAW per ridurre i problemi di usabilità in modo sicuro. Questo obiettivo viene spesso raggiunto ascoltando il feedback degli utenti, l'installazione di strumenti e gli script necessari per eseguire il lavoro e assicurandosi che tutto il personale amministrativo sono a conoscenza perché è necessario usare una workstation PAW, quale una workstation PAW è e come usarlo in modo corretto ed efficiente.

-   **Rischi legati all'ambiente** -poiché molti altri computer e gli account nell'ambiente sono esposti a rischi originati internet o indirettamente, una workstation PAW deve essere protetto dagli attacchi di risorse compromesse presenti nell'ambiente di produzione. Ciò richiede di limitare gli account che dispongono di accesso alle workstation Paw al minimo necessario per proteggere e monitorare queste workstation specializzate e gli strumenti di gestione.

-   **Manomissione della supply chain** : è possibile rimuovere tutti i possibili rischi di manomissione nella supply chain per hardware e software, ma alcune azioni chiave possono ridurre critici di attacchi immediatamente disponibili per gli utenti malintenzionati. Questo include convalida dell'integrità di tutti i supporti di installazione ([principio di origine pulita](http://aka.ms/cleansource)) e l'utilizzo di un fornitore attendibile e affidabile per hardware e software.

-   **Attacchi fisici** -le workstation Paw può essere fisicamente mobili e usate all'esterno di strutture protette, devono essere protette dagli attacchi che sfruttano l'accesso fisico non autorizzato al computer.

> [!NOTE]
> Una workstation PAW non protegge un ambiente da un antagonista che abbia già ottenuto l'accesso amministrativo in una foresta Active Directory.
> Poiché molte implementazioni esistenti di Active Directory Domain Services hanno funzionato per anni esposte al rischio di furto di credenziali, le organizzazioni devono presunzione della violazione e considerare la possibilità che si siano verificate compromissioni di credenziali di amministratore di dominio o dell'organizzazione. Un'organizzazione che sospetti la compromissione del dominio deve prendere in considerazione l'uso di servizi professionali di risposta agli eventi imprevisti.
>
> Per ulteriori informazioni sulla Guida di risposta e ripristino, vedere "Risposta di attività sospette" e "Recover da una violazione" sezioni di [Mitigating Pass-the-Hash e furto di credenziali altri](https://www.microsoft.com/pth), versione 2.
>
> Visitare [Incident Response and Recovery services di Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) pagina per altre informazioni.

### <a name="paw-hardware-profiles"></a>Profili Hardware PAW
Il personale amministrativo sono anche utenti standard troppo - devono non solo una workstation PAW, ma anche una workstation utente standard per controllare la posta elettronica, esplorare il web e accedere aziendale applicazioni line-of business.  Assicurarsi che gli amministratori siano costantemente produttivi e protetti è essenziale per il successo di qualsiasi distribuzione PAW.  Una soluzione sicura che limiti notevolmente la produttività verrà abbandonata dagli utenti a favore di uno che aumenta la produttività (anche se l'operazione viene eseguita in modo non sicuro).

Per bilanciare l'esigenza di sicurezza con l'esigenza di produttività, Microsoft consiglia di usare uno di questi profili hardware PAW:

-   **Hardware dedicato** -dispositivi dedicati separati per attività utente e le attività amministrative

-   **Uso simultaneo** -singolo dispositivo che è possibile eseguire attività utente e le attività amministrative contemporaneamente sfruttando della virtualizzazione del sistema operativo o la presentazione.

Le organizzazioni possono usare un solo profilo o entrambi. Ci sono problemi di interoperabilità tra i profili hardware e le organizzazioni hanno la flessibilità necessaria per trovare la corrispondenza il profilo hardware alle esigenze e alla situazione di ciascun amministratore.

> [!NOTE]
> È fondamentale che, in tutti questi scenari, il personale amministrativo rilasciato un account utente standard è separato dagli account amministrativi designati. Gli account amministrativi devono essere utilizzati solo nel sistema operativo amministrativo PAW.

Questa tabella riepiloga i vantaggi e gli svantaggi di ogni profilo hardware dal punto di vista della facilità di utilizzo operativo produttività e della sicurezza.  Entrambi gli approcci hardware offrono sicurezza avanzata per gli account amministrativi contro il furto di credenziali e il riutilizzo.

|**Scenario**|**Vantaggi**|**Svantaggi**|
|--------|---------|-----------|
|Hardware dedicato.|-Segnale forte per la riservatezza delle attività<br />-Separazione di sicurezza massima|-Ulteriore spazio<br />-Peso aggiuntivo (per il lavoro remoto)<br />-Costo dell'hardware|
|Uso simultaneo|-Ridurre il costo dell'hardware<br />-Un unico dispositivo|-Condivisione mouse/tastiera singolo crea rischio di errori e rischi accidentali|

Questo materiale sussidiario contiene istruzioni dettagliate per la configurazione PAW per l'approccio hardware dedicato. Se si dispone di requisiti per i profili hardware di uso simultaneo, è possibile adattare le istruzioni riportate in questo materiale o incaricare un'organizzazione di servizi professionali come Microsoft del supporto.

**Hardware dedicato.**

In questo scenario, viene utilizzata una workstation PAW per l'amministrazione completamente separata dal PC che viene utilizzato per le attività quotidiane come messaggio di posta elettronica, la modifica di documenti e attività di sviluppo. Tutte le applicazioni e strumenti di amministrazione vengono installate nella workstation PAW e tutte le applicazioni di produttività vengono installate nella workstation utente standard. Le istruzioni dettagliate in questo materiale sussidiario si basano su questo profilo hardware.

**Uso simultaneo: aggiunta di una VM utente locale**

In questo scenario di uso simultaneo un unico PC viene usato per attività di amministrazione e le attività quotidiane come messaggio di posta elettronica, la modifica di documenti e attività di sviluppo. In questa configurazione, il sistema operativo utente è disponibile durante la disconnessione (per la modifica di documenti e funzionante nel messaggio di posta elettronica memorizzata nella cache locale), ma richiede hardware e il supporto di processi in grado di soddisfare questo stato disconnesso.

![Diagramma che illustra solo PC in uno scenario di uso simultaneo usato sia per le attività quotidiane per attività di amministrazione come messaggio di posta elettronica, modifica di documenti e attività di sviluppo](../media/privileged-access-workstations/PAWFig10.JPG)

L'hardware fisico esegue in locale due sistemi operativi:

-   **Sistema operativo di amministrazione** -l'host fisico esegue Windows 10 nell'host PAW per le attività amministrative

-   **Utente del sistema operativo** -guest della macchina virtuale Hyper-V client A Windows 10 esegue un'immagine aziendale

Con Windows 10Hyper-V, una macchina virtuale guest (anche in esecuzione Windows 10) può avere un'esperienza utente avanzata comprendente audio, video e applicazioni di comunicazione Internet, ad esempio Skype for Business.

In questa configurazione, le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite nella macchina virtuale del sistema operativo utente che dispone di un'immagine Windows 10 aziendale regolare e non è soggetta alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito l'amministratore del sistema operativo.

Per questa configurazione, seguire le istruzioni in questo materiale sussidiario per l'host PAW, aggiungere le funzionalità di Hyper-V Client, creare una VM utente e quindi installare un'immagine aziendale Windows 10 nella macchina virtuale utente.

Lettura [Hyper-V Client](https://technet.microsoft.com/en-us/library/hh857623.aspx) per ulteriori informazioni su questa funzionalità. Si noti che il sistema operativo nelle macchine virtuali guest sarà necessario disporre di una licenza per ogni [prodotto Microsoft licensing](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), descritte anche [qui](https://www.microsoft.com/en-us/Licensing/learn-more/brief-windows-virtual-machine.aspx).

**Uso simultaneo: aggiunta di RemoteApp, di RDP o un'infrastruttura VDI**

In questo scenario di uso simultaneo un unico PC viene usato per entrambe le attività amministrative e attività quotidiane, come posta elettronica, modifica di documenti e lo sviluppo di funzionare. In questa configurazione, i sistemi operativi utente vengono distribuiti e gestiti centralmente (nel cloud o nel Data Center), ma non è disponibile durante la disconnessione.

![Figura che illustra uno scenario di uso simultaneo un unico PC usati per entrambe le attività amministrative e attività quotidiane, come posta elettronica, modifica di documenti e lo sviluppo di lavoro](../media/privileged-access-workstations/PAWFig11.JPG)

L'hardware fisico esegue un solo sistema operativo PAW locale per le attività amministrative e contatta un Microsoft o 3rd party desktop remoto per le applicazioni utente, ad esempio e-mail, modifica di documenti e applicazioni aziendali.

In questa configurazione, le attività quotidiane che non richiedono privilegi amministrativi vengono eseguite nel operativi remoti e le applicazioni che non sono soggetti alle restrizioni applicate all'host PAW. Tutto il lavoro amministrativo viene eseguito l'amministratore del sistema operativo.

Per questa configurazione, seguire le istruzioni in questo materiale sussidiario per l'host PAW, consentire la connettività di rete per i Servizi Desktop remoto e quindi aggiungere collegamenti al desktop dell'utente PAW per accedere alle applicazioni. I servizi desktop remoto possono essere ospitati in molti modi, ad esempio:

-   Un servizio Desktop remoto o VDI esistente (locale o nel cloud)

-   Un nuovo servizio installato in locale o nel cloud

-   Azure RemoteApp con modelli di Office 365 preconfigurati o immagini di installazione

Per ulteriori informazioni su Azure RemoteApp, visitare [questa pagina](https://www.remoteapp.windowsazure.com).

### <a name="paw-scenarios"></a>Scenari PAW
Questa sezione contiene indicazioni sugli scenari che questo materiale sussidiario su PAW deve essere applicato al. Per tutti gli scenari gli amministratori devono essere addestrati a usare solo workstation Paw per il supporto a sistemi remoti. Per favorire un uso corretto e sicuro e, tutti gli utenti PAW deve essere anche necessario incoraggiare per fornire commenti e suggerimenti per migliorare l'esperienza di workstation PAW e questo feedback dovrà essere esaminati attentamente per l'integrazione con il programma PAW.

Per tutti gli scenari nelle fasi successive misure di protezione avanzata aggiuntive e profili hardware diversi in questa guida possono essere utilizzati per soddisfare i requisiti di usabilità o di sicurezza dei ruoli.

> [!NOTE]
> Si noti che questo materiale sussidiario fa esplicitamente distinzione tra che richiedono l'accesso a servizi specifici su internet (ad esempio i portali amministrativi Azure e Office 365) e il "Internet" aperta"di tutti gli host e i servizi.

Vedere il [pagina sul modello a livelli](http://aka.ms/tiermodel) per ulteriori informazioni sulle designazioni di livello.

|**Scenari**|**Usare PAW?**|**Considerazioni sulla sicurezza e ambito**|
|---------|--------|---------------------|
|Amministratori di Active Directory - livello 0|Sì|Una workstation PAW creata con indicazioni fase 1 è sufficiente per questo ruolo.<br /><br />-Una foresta amministrativa può essere aggiunto per fornire la massima protezione per questo scenario. Per ulteriori informazioni sulla foresta amministrativa ESAE, vedere [approccio di progettazione di foresta amministrativa ESAE](http://aka.ms/esae)<br />-Una workstation PAW può essere utilizzata per gestire più domini o più foreste.<br />-Se i controller di dominio sono ospitati in un'infrastruttura come un servizio (IaaS) o una soluzione di virtualizzazione locale, è necessario assegnare una priorità implementazione di workstation Paw per gli amministratori di tali soluzioni|
|Servizi di amministratore di Azure IaaS e PaaS - livello 0 o livello 1 (vedere Considerazioni di progettazione e di ambito)|Sì|Una workstation PAW creata secondo le indicazioni nella fase 2 è sufficiente per questo ruolo.<br /><br />-Devono essere usate workstation Paw per almeno l'amministratore globale e un amministratore della fatturazione delle sottoscrizioni. È inoltre necessario utilizzare workstation Paw per gli amministratori delegati di server critici o sensibili.<br />-Workstation Paw deve essere utilizzata per la gestione del sistema operativo e applicazioni che forniscono la sincronizzazione delle Directory e la federazione delle identità per i servizi cloud, ad esempio [Azure AD Connect](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect/) e Active Directory Federation Services (ADFS).<br />-Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi cloud autorizzati secondo le indicazioni riportate nella fase 2. Nessun accesso a internet aprire deve essere consentito da workstation Paw.<br />-Il Toolkit EMET deve essere configurato per tutti i browser usati nella workstation **Nota:** una sottoscrizione è considerata di livello 0 per una foresta se nella sottoscrizione sono controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se nessun server di livello 0 è ospitato in Azure|
|Amministratore Office 365 Tenant <br />-Livello 1|Sì|Una workstation PAW creata secondo le indicazioni nella fase 2 è sufficiente per questo ruolo.<br /><br />-Devono essere usate workstation Paw per almeno l'amministratore della fatturazione delle sottoscrizioni, amministratore globale, amministratore di Exchange, l'amministratore di SharePoint e ruoli di amministratore Gestione utenti. È necessario anche considerare l'uso di workstation Paw per gli amministratori delegati di dati estremamente critici o sensibili.<br />-Il Toolkit EMET deve essere configurato per tutti i browser usati nella workstation<br />-Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft, secondo le indicazioni riportate nella fase 2. Nessun accesso a internet aprire deve essere consentito da workstation Paw.|
|Amministratore di servizi cloud di altri IaaS o PaaS<br />-Livello 0 o livello 1 (vedere Considerazioni di progettazione e di ambito)||Una workstation PAW creata secondo le indicazioni nella fase 2 è sufficiente per questo ruolo.<br /><br />-Workstation Paw deve essere utilizzata per tutti i ruoli con diritti amministrativi su VM ospitato nel cloud, inclusa la possibilità di installare gli agenti, esportare file disco rigido o accedere all'archiviazione in cui sono archiviati i dischi rigidi con sistemi operativi, dati sensibili o dati aziendali critici.<br />-Le restrizioni di rete in uscita devono consentire la connettività solo ai servizi Microsoft, secondo le indicazioni riportate nella fase 2. Nessun accesso a internet aprire deve essere consentito da workstation Paw.<br />-Il Toolkit EMET deve essere configurato per tutti i browser usati nella workstation. **Nota:** una sottoscrizione è di livello 0 per una foresta se nella sottoscrizione sono controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se nessun server di livello 0 è ospitato in Azure.|
|Amministratori di virtualizzazione<br />-Livello 0 o livello 1 (vedere Considerazioni di progettazione e di ambito)|Sì|Una workstation PAW creata secondo le indicazioni nella fase 2 è sufficiente per questo ruolo.<br /><br />-Workstation Paw deve essere utilizzata per tutti i ruoli con diritti amministrativi su VM, inclusa la possibilità di installare gli agenti, esportare file disco rigido virtuale o accedere all'archiviazione in cui sono archiviati i dischi rigidi con informazioni sul sistema operativo guest, dati sensibili o dati aziendali critici. **Nota:** un sistema di virtualizzazione (e i relativi amministratori) sono considerati di livello 0 per una foresta se nella sottoscrizione sono controller di dominio o altri host di livello 0. Una sottoscrizione è di livello 1 se nessun server di livello 0 è ospitato nel sistema di virtualizzazione.|
|Amministratori della manutenzione dei server<br />-Livello 1|Sì|Una workstation PAW creata secondo le indicazioni nella fase 2 è sufficiente per questo ruolo.<br /><br />-Una workstation PAW deve essere utilizzata per gli amministratori che aggiornano, cookie di patch e risolvere i problemi relativi a server aziendali e le App in esecuzione Windows server, Linux e altri sistemi operativi.<br />-Strumenti di gestione dedicati potrebbero essere necessario aggiungere workstation paw consentire il su scala più ampia di tali attività di amministrazione.|
|Amministratori di Workstation utente <br />-Livello 2|Sì|Una workstation PAW creata secondo le indicazioni specificate nella fase 2 è sufficiente per i ruoli con diritti amministrativi sui dispositivi degli utenti finali (ad esempio l'help desk e supporto deskside ruoli).<br /><br />-Potrebbe essere necessario altre applicazioni di essere installato nelle workstation Paw per abilitare la gestione di ticket e altre funzioni di supporto.<br />-Il Toolkit EMET deve essere configurato per tutti i browser usati nella workstation.<br />    Strumenti di gestione dedicati potrebbero essere necessario aggiungere workstation paw consentire il su scala più ampia di tali attività di amministrazione.|
|SQL, SharePoint o line-of-business (LOB) Admin<br />-Livello 1||Una workstation PAW creata con indicazioni fase 2 è sufficiente per questo ruolo.<br /><br />-Strumenti di gestione aggiuntivi potrebbero essere necessario installare nelle workstation Paw per consentire agli amministratori di gestire le applicazioni senza la necessità di connettersi ai server tramite Desktop remoto.|
|Utenti che gestiscono la presenza nei Social Media|Parzialmente|Una workstation PAW creata secondo le indicazioni nella fase 2 è utilizzabile come punto di partenza per garantire la sicurezza a questo ruolo.<br /><br />-Proteggere e gestire gli account di social media tramite Azure Active Directory (AAD) per la condivisione, protezione e tenere traccia dell'accesso agli account di social media.<br />    Per ulteriori informazioni su questa funzionalità, leggere [questo post di blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-Le restrizioni di rete in uscita devono consentire la connettività a questi servizi. Questo può avvenire consentendo connessioni internet aperte (molto più alto rischio per la sicurezza e l'annullamento di molte garanzie PAW) o consentendo solo indirizzi DNS specifici per il servizio (potrebbe essere difficile da realizzare).|
|Utenti standard|No|Molti passaggi di protezione avanzata possono essere usati per gli utenti standard, PAW è progettata per isolare gli account di accesso a internet aprire che richiedono la maggior parte degli utenti per mansioni.|
|Guest VDI/chiosco multimediale|No|Mentre molti passaggi di protezione avanzata possono essere utilizzati per un sistema chiosco multimediale per gli utenti Guest, l'architettura PAW è progettato per fornire una maggiore sicurezza per gli account a sensibilità elevata, non una maggiore sicurezza per gli account a sensibilità inferiore.|
|Utente VIP (Executive, ricercatori e così via)|Parzialmente|Una workstation PAW creata secondo le indicazioni specificate nella fase 2 può essere utilizzata come punto di partenza per garantire la sicurezza a questi ruoli<br /><br />-Questo scenario è simile a un desktop utente standard, ma in genere ha un profilo dell'applicazione di dimensioni inferiori, più semplice e ben noto. Questo scenario richiede in genere l'individuazione e la protezione dati sensibili, servizi e applicazioni, che possono o potrebbero non essere installate nei computer desktop.<br />-Questi ruoli in genere richiedono un elevato livello di sicurezza e un livello di usabilità molto elevato che richiedono modifiche di progettazione per soddisfare le preferenze dell'utente.|
|Sistemi di controllo industriale (ad esempio SCADA, PCN e DCS)|Parzialmente|Una workstation PAW creata secondo le indicazioni specificate nella fase 2 può essere utilizzata come punto di partenza per garantire la sicurezza a questi ruoli ICS come la maggior parte delle console (inclusi standard comuni come SCADA e PCN) non richiedono la ricerca nelle reti Internet aperte e controllare la posta elettronica.<br /><br />-Le applicazioni usate per il controllo di macchinari fisici avrebbero dovuto essere integrato e i test di compatibilità e protette in modo appropriato|
|Sistema operativo incorporato|No|Mentre molti passaggi di protezione avanzata da PAW possono essere utilizzati per i sistemi operativi incorporati, sarebbe necessario sviluppare per la protezione avanzata in questo scenario una soluzione personalizzata.|

> [!NOTE]
> **Combinazione di scenari** alcuni membri del personale possono avere responsabilità amministrative che si estendono su più scenari.
> In questi casi, la regola chiave da tenere presenti è che dovranno essere seguite le regole del modello di livello in qualsiasi momento. Vedere la pagina sul modello a livelli per ulteriori informazioni.

> [!NOTE]
> **Ridimensionamento del programma PAW** durante il programma PAW viene ridimensionato per comprendere un numero maggiore di amministratori e i ruoli, è necessario continuare a mantenere la conformità agli standard di sicurezza e usabilità. Questo può richiedere di aggiornare l'IT strutture di supporto o crearne di nuovi per risolvere problemi specifici di PAW, ad esempio processo di caricamento PAW, gestione degli incidenti e della configurazione e problematiche di raccogliere i commenti di usabilità.  Un esempio potrebbe essere che l'organizzazione decide di abilitare scenari di lavoro da casa per gli amministratori, che sarebbero necessitano di un cambiamento da workstation Paw desktop a workstation Paw portatili, un passaggio che richiederebbe considerazioni di sicurezza aggiuntive.  Un altro esempio comune consiste nel creare o aggiornare formazione per nuovi amministratori - formazione corsi contenuto all'uso appropriato di una workstation PAW (inclusi perché il relativo importante e quali una workstation PAW è e non è).  Per ulteriori considerazioni devono tenere presenti quando si adatta il programma PAW, vedere la fase 2 delle istruzioni.

Questo materiale sussidiario contiene istruzioni dettagliate per la configurazione PAW per gli scenari, come indicato in precedenza. Se si dispone di requisiti per gli altri scenari, è possibile adattare le istruzioni riportate in questo materiale o incaricare un'organizzazione di servizi professionali come Microsoft del supporto.

Per ulteriori informazioni sull'impegno dei servizi Microsoft per progettare una workstation PAW su misura per il proprio ambiente, contattare il rappresentante Microsoft o visitare [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="paw-installation-instructions"></a>Istruzioni di installazione di PAW
Poiché PAW deve fornire un'origine attendibile e sicura per l'amministrazione, è essenziale che il processo di creazione sia sicuro e attendibile.  Questa sezione fornisce istruzioni dettagliate che consentono di creare il proprio ambiente PAW tramite principi e concetti molto simili a quelle utilizzate da Microsoft IT e Microsoft alle organizzazioni di gestione del servizio e di cloud engineering.

Le istruzioni sono suddivise in tre fasi che si concentrano sulla inserire rapidamente le misure di prevenzione più importanti in posizione progressivamente aumento e diffusione dell'uso di workstation PAW per l'azienda.

-   Fase 1: distribuzione immediata per amministratori di Active Directory

-   Fase 2: estensione di PAW a tutti gli amministratori

-   Fase 3: protezione PAW avanzata

È importante notare che le fasi sempre devono essere eseguite nell'ordine anche se vengono pianificate e implementate all'interno dello stesso progetto complessivo.

#### <a name="phase-1---immediate-deployment-for-active-directory-administrators"></a>Fase 1: distribuzione immediata per amministratori di Active Directory
Scopo: Rendere disponibile rapidamente un'architettura PAW può proteggere i ruoli di amministrazione del dominio e foresta locale.

Ambito: Amministratori di livello 0 tra cui Enterprise Admins, Domain Admins (per tutti i domini) e gli amministratori di altri sistemi di identità autorevoli.

Fase 1 concentra gli amministratori che gestiscono il dominio di Active Directory locale, che sono di importanza critica ruoli spesso destinati dai pirati informatici. Questi sistemi di identità funziona in modo efficace per la protezione di questi amministratori se il controller di dominio Active Directory (controller di dominio) sono ospitati in centri dati in locale, sull'infrastruttura di Azure come servizio (IaaS), o un altro provider IaaS.

Durante questa fase, si creerà la struttura di unità organizzativa (OU) Active Directory amministrativa protetta per ospitare le workstation amministrative con privilegi (PAW), nonché distribuire le workstation Paw stesse.  Questa struttura include anche i criteri di gruppo e i gruppi necessari per il supporto delle workstation PAW.  Si creerà la maggior parte della struttura utilizzando script di PowerShell che sono disponibili in [Raccolta TechNet](http://aka.ms/pawmedia).

Gli script creano le unità organizzative e i gruppi di sicurezza seguenti:

-   Unità organizzative (OU)

    -   Sei nuove OU di primo livello: Admin; Gruppi. Server di livello 1. Workstation; Account utente. e la quarantena Computer.  Ogni OU di primo livello conterrà un numero di unità organizzative figlio.

-   Gruppi

    -   Sei nuovi protetto gruppi globali: manutenzione replica livello 0; Manutenzione Server livello 1; Service Desk Operators; Manutenzione workstation; Utenti PAW. Manutenzione PAW.

Anche se si creerà un numero di oggetti Criteri di gruppo: PAW Configuration - Computer. PAW Configuration - utente; RestrictedAdmin necessaria - Computer. Restrizioni in uscita PAW; Limitare l'accesso a una Workstation; Limitare l'accesso al Server.

La fase 1 prevede i passaggi seguenti:

1.  Completare i prerequisiti

    1.  **Assicurarsi che tutti gli amministratori usino account separati rispettivamente per le attività di amministrazione e l'utente finale** (inclusi posta elettronica, esplorazione di Internet, applicazioni line-of-business e altre attività non amministrative).  L'assegnazione di un account amministrativo per ogni singolo personale autorizzato dal proprio account utente standard è fondamentale per il modello PAW, come solo determinati account saranno autorizzati di accedere a PAW.

        > [!NOTE]
        > Ogni amministratore deve usare i propri account per l'amministrazione.  Non condividere un account amministrativo.

    2.  **Ridurre al minimo il numero di amministratori con privilegiata di livello 0**.  Poiché ogni amministratore deve usare una workstation PAW, riducendo il numero di amministratori riduce il numero di workstation necessarie e i costi associati. Un numero inferiore di amministratori implica anche un'esposizione inferiore dei privilegi e rischi associati. Sebbene sia possibile per gli amministratori in un'unica posizione condividere una workstation PAW, gli amministratori in posizioni fisiche separate richiederà workstation Paw separate.

    3.  **Acquisire l'hardware da un fornitore affidabile che soddisfi tutti i requisiti tecnici**. Microsoft consiglia di acquisire hardware che soddisfi i requisiti tecnici nell'articolo [proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

        > [!NOTE]
        > Architettura PAW installata in hardware senza queste caratteristiche può offrire una protezione significativa, ma le funzionalità di sicurezza avanzate quali Credential Guard e Device Guard non sarà disponibile.  Credential Guard e Device Guard non sono necessari per fase 1 della distribuzione, ma sono estremamente consigliabili nel corso della fase 3 (protezione avanzata).

        Assicurarsi che l'hardware usato per le workstation PAW sia originato da un produttore e il fornitore le cui procedure di protezione sono considerati attendibili dall'organizzazione. Si tratta di un'applicazione del principio di origine pulita per sicurezza della supply chain.

        > [!NOTE]
        > Per ulteriori informazioni sull'importanza di sicurezza della supply chain, visitare [questo sito](https://www.microsoft.com/security/cybersecurity/).

    4.  Acquisire e convalidare i necessari Windows 10 Enterprise Edition e il software dell'applicazione. Ottenere il software necessario per PAW e convalidarlo in base alle indicazioni fornite in [origine pulita per i supporti di installazione](http://aka.ms/cleansource).

        -   Windows 10 Enterprise Edition

        -   [Strumenti di amministrazione Server remota](https://www.microsoft.com/en-us/download/details.aspx?id=45520.) per Windows 10

        -   Windows 10 [baseline di sicurezza](http://aka.ms/win10baselines)

        -   [Enhanced Mitigation Experience Toolkit (EMET)](https://www.microsoft.com/en-us/download/details.aspx?id=49166)

            > [!NOTE]
            > Microsoft pubblica gli hash MD5 per tutti i sistemi operativi e applicazioni in MSDN, ma non tutti i fornitori di software offrono una documentazione simile.  In questi casi, saranno necessarie altre strategie.  Per ulteriori informazioni sulla convalida del software, fare riferimento a [origine pulita](http://aka.ms/cleansource) per i supporti di installazione.

    5.  **Assicurarsi di disporre di server WSUS disponibili nella rete intranet**. È necessario un server WSUS nella rete intranet per scaricare e installare gli aggiornamenti per PAW. Il server WSUS deve essere configurato per approvare automaticamente tutti gli aggiornamenti della sicurezza per Windows 10 o il personale amministrativo devono avere incarico e assumersi la responsabilità di approvare rapidamente gli aggiornamenti software.

        > [!NOTE]
        > Per ulteriori informazioni, vedere la sezione "Approvare automaticamente gli aggiornamenti per installazione" nel [indicazioni di approvazione degli aggiornamenti](https://technet.microsoft.com/en-us/library/cc708458(v=ws.10).aspx).

2.  Distribuire il Framework della OU amministrativa per ospitare le workstation Paw

    1.  Scaricare la libreria di script PAW dalla [Raccolta TechNet](http://aka.ms/PAWmedia)

        > [!NOTE]
        > Scaricare tutti i file e salvarli nella stessa directory e quindi eseguirli nell'ordine specificato di seguito.  Create-PAWGroups dipende dalla struttura della OU creata da Create-PAWOUs e Set-PAWOUDelegation dipende dai gruppi creati da Create-PAWGroups.
        > Non modificare gli script né il file di valori delimitati da virgole (CSV).

    2.  **Eseguire lo script con estensione ps1 Create-PAWOUs**.  Questo script verrà creare la nuova unità organizzativa (OU) in Active Directory e bloccare l'ereditarietà di oggetto Criteri di gruppo per nuove OU come opportuno.

    3.  **Eseguire lo script con estensione ps1 Create-PAWGroups**.  Questo script creerà i nuovi gruppi di sicurezza globale nelle OU appropriate.

        > [!NOTE]
        > Sebbene questo script crea i nuovi gruppi di sicurezza, non li popola automaticamente.

    4.  **Eseguire lo script con estensione ps1 Set-PAWOUDelegation**.  Questo script assegna autorizzazioni ai gruppi appropriati delle nuove OU.

3.  **Spostare gli account di livello 0 alla OU Admin\Tier 0 \ Accounts**. Spostare ogni account è membro di Domain Admin, Enterprise Admins, o di livello 0 gruppi equivalenti (incluse le appartenenze annidate) in questa OU. Se l'organizzazione dispone di gruppi personalizzati che vengono aggiunti a questi gruppi, è necessario spostare questi all'unità Organizzativa 0\Groups Admin\Tier.

    > [!NOTE]
    > Per ulteriori informazioni su quali gruppi di livello 0, vedere "Equivalenza di livello 0" in [protezione materiale di riferimento accesso con privilegi](../securing-privileged-access/securing-privileged-access-reference-material.md).

4.  Aggiungere i membri appropriati ai gruppi pertinenti

    1.  **Gli utenti PAW** : aggiungere gli amministratori di livello 0 con dominio o gruppi Enterprise Admins identificati nel passaggio 1 della fase 1.

    2.  **Manutenzione PAW** : aggiungere almeno un account che verrà utilizzato per la manutenzione PAW e attività di risoluzione dei problemi. Gli account di manutenzione PAW verranno usati solo raramente.

        > [!NOTE]
        > Non aggiungere stesso account utente o il gruppo PAW Users sia manutenzione PAW.  Il modello di sicurezza PAW si basa in parte sul presupposto che l'account utente PAW dispone di diritti con privilegi sui sistemi gestiti o sull'ambiente PAW stesso, ma non entrambi.
        >
        > -   Questo è importante per la creazione di buona procedure e prassi amministrative nella fase 1.
        > -   Questo è di importanza critica per la fase 2 e non solo per evitare l'escalation dei privilegi attraverso PAW man delle workstation Paw si estendono su più livelli.
        >
        > In teoria, il personale non vengono assegnato compiti su più livelli per applicare il principio della separazione dei compiti, ma Microsoft riconosce che molte organizzazioni possono essere limitati personale (o altri requisiti organizzativi) che non consentono di questa separazione completo. In questi casi, il personale stesso possono essere assegnato entrambi i ruoli, ma non utilizzare lo stesso account per queste funzioni.

5.  **Creare l'oggetto Criteri di gruppo "PAW Configuration – Computer" (GPO) e collegarlo all'OU di dispositivi di livello 0** ("dispositivi" in Tier 0 \ Admin).  In questa sezione si creerà un nuovo "PAW Configuration – Computer" oggetto Criteri di gruppo che fornirà protezione specifica per queste workstation Paw.

    > [!NOTE]
    > **Non aggiungere queste impostazioni per il criterio dominio predefinito**.  In questo modo si potrebbero avere conseguenze operazioni nell'intero ambiente Active Directory.  Configurare queste impostazioni solo gli oggetti Criteri di gruppo appena creati descritti qui e applicarle solo alla OU PAW.

    1.  **Accesso manutenzione PAW** : questa impostazione definisce l'appartenenza di gruppi con privilegi specifici per le workstation Paw a un set specifico di utenti. Vai a *agli utenti dei Computer Configuration\Preferences\Control pannello sicurezza\Criteri* e i gruppi e attenersi alla procedura seguente:

        1.  Fare clic su **New** e fare clic su **gruppo locale**

        2.  Selezionare il **aggiornamento** azione e selezionare "Administrators (predefinito)" (non usare il pulsante Sfoglia per selezionare il gruppo domain Administrators).

        3.  Selezionare il **Elimina tutti gli utenti membri** e **eliminare tutti i gruppi membri** caselle di controllo

        4.  Aggiungere PAW Maintenance (oggetto pawmaint) e amministratore (nuovo, non usare il pulsante Sfoglia per selezionare Administrator).

            > [!NOTE]
            > Non aggiungere il gruppo PAW Users all'elenco dei membri per il gruppo Administrators locale.  Per garantire che gli utenti PAW non modifichi accidentalmente o intenzionalmente le impostazioni di sicurezza dell'ambiente PAW stesso, non devono essere membri del gruppo Administrators locale.

            Per ulteriori informazioni sull'utilizzo delle preferenze di criteri di gruppo per modificare l'appartenenza al gruppo, fare riferimento all'articolo di TechNet [configurare un elemento gruppo locale](https://technet.microsoft.com/en-us/library/cc732525.aspx).

    2.  **Limitare l'appartenenza al gruppo locale** -questa impostazione garantisce che l'appartenenza di gruppi di amministratori locale sulla workstation è sempre vuoto

        1.  Passare a Computer Configuration\Preferences\Control pannello sicurezza\Criteri utenti e gruppi e attenersi alla procedura seguente:

            1.  Fare clic su **New** e fare clic su **gruppo locale**

            2.  Selezionare il **aggiornamento** azione e selezionare "Backup Operators (predefinito)" (non usare il pulsante Sfoglia per selezionare il gruppo di dominio Backup Operators).

            3.  Selezionare il **Elimina tutti gli utenti membri** e **eliminare tutti i gruppi membri** caselle di controllo.

            4.  Non aggiungere membri al gruppo.  È sufficiente fare clic su **OK**.  Mediante l'assegnazione di un elenco vuoto, criteri di gruppo verranno automaticamente rimuovere tutti i membri e garantire l'aggiornamento di criteri di gruppo ogni volta un elenco di appartenenze sia vuoto.

        2.  Completare i passaggi precedenti per i gruppi aggiuntivi seguenti:

            -   Operazioni di crittografia

            -   Amministratori di Hyper-V

            -   Network Configuration Operators

            -   Utenti esperti

            -   Utenti Desktop remoto

            -   Replicator

        3.  Restrizioni accesso PAW - questa impostazione limita gli account che possono accedere a PAW. Attenersi alla procedura seguente per configurare questa impostazione:

            1.  Vai ai Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\consenti accesso locale.

            2.  Selezionare Definisci queste impostazioni dei criteri e aggiungere "PAW Users" e gli amministratori (nuovo, non usare il pulsante Sfoglia per selezionare gli amministratori).

        4.  **Bloccare il traffico di rete in entrata** -questa impostazione garantisce che la workstation PAW non sia consentito alcun traffico di rete in entrata non richiesto. Attenersi alla procedura seguente per configurare questa impostazione:

            1.  Vai a Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni sicurezza\windows Firewall con sicurezza avanzata\windows Firewall con sicurezza avanzata e attenersi alla procedura seguente:

                1.  Fare clic su Windows Firewall con sicurezza avanzata e selezionare **importare criteri**.

                2.  Fare clic su **Sì** per accettare la sovrascrittura dei criteri firewall eventualmente esistenti.

                3.  Selezionare PAWFirewall.wfw e seleziona **aprire**.

                4.  Fare clic su **OK**.

                    > [!NOTE]
                    > È possibile aggiungere indirizzi o subnet che devono raggiungere la workstation PAW con traffico indesiderato a questo punto (ad esempio, la scansione o gestione software di protezione.
                    > Le impostazioni nel file WFW abilitare il firewall in modalità "Blocca (predefinito)" per tutti i profili di firewall, disattivare l'unione regole e abilitare la registrazione dei pacchetti ignorati sia corretta. Queste impostazioni bloccano il traffico bloccano pur consentendo la comunicazione bidirezionale per le connessioni avviate dalla workstation PAW, impedire agli utenti con accesso amministrativo locale di creazione di regole firewall locali che sarebbero sostituire le impostazioni oggetto Criteri di gruppo e assicurarsi che sia stato registrato il traffico da e verso PAW.
                    > **Apertura di questo firewall verrà espandere la superficie di attacco per le workstation PAW e aumentare i rischi di sicurezza. Prima di aggiungere tutti gli indirizzi, consultare la sezione sulla gestione e sul funzionamento di PAW in questo materiale sussidiario**.

        5.  **Configurare Windows Update per WSUS** -attenersi alla procedura seguente per modificare le impostazioni per configurare Windows Update per le workstation Paw:

            1.  Passare a Computer Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\aggiornamenti e attenersi alla procedura seguente:

                1.  Abilitare il **criterio Configura Aggiornamenti automatici**.

                2.  Selezionare l'opzione **4 - download automatico e pianificazione dell'installazione**.

                3.  Modificare l'opzione **pianificato installazione giorno** a **0 - ogni giorno** e l'opzione **orario pianificato per l'installazione** preferito dall'organizzazione.

                4.  Abilitare l'opzione **specificare percorso servizio di aggiornamento Microsoft intranet** criteri, e specificare in entrambe le opzioni l'URL del server ESAE WSUS.

        6.  Collegare il "PAW Configuration – Computer" oggetto Criteri di gruppo come indicato di seguito:

            |Criteri|Percorso collegamento|
            |-----|---------|
            |PAW Configuration - Computer |Admin\Tier 0 \ Devices|

6.  **Creare l'oggetto Criteri di gruppo "PAW Configuration - User-utente" (GPO) e collegarlo all'OU di account di livello 0 ("account" in Tier 0 \ Admin)**.  In questa sezione si creerà un nuovo "PAW Configuration - User utente" oggetto Criteri di gruppo che fornirà protezione specifica per queste workstation Paw.

    > [!NOTE]
    > Non aggiungere queste impostazioni per il criterio dominio predefinito

    1.  **Blocca esplorazione internet** : per evitare l'esplorazione accidentale di internet, questa impostazione un indirizzo proxy di un indirizzo di loopback (127.0.0.1).

        1.  Vai a configurazione utente\preferenze\impostazioni windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** \> **elemento Registro di sistema** e configurare le impostazioni seguenti:

            1.  Azione: Sostituisci

            2.  Hive: HKEY_CURRENT_USER

            3.  Il percorso della chiave: Software\microsoft\windows\currentversion\impostazioni Internet

            4.  Nome valore: ProxyEnable

                > [!NOTE]
                > Non selezionare la casella predefinito a sinistra del nome del valore.

            5.  Tipo valore: REG_DWORD

            6.  Dati valore: 1

                1.  un.  Fare clic sulla scheda comune e selezionare **rimuovere questo elemento quando non viene più applicato**.

                2.  Selezionare la scheda comune **destinazione a livello di elemento** e fare clic su **Targeting**.

                3.  Fare clic su **nuovo elemento** e seleziona **gruppo di sicurezza**.

                4.  Selezionare il pulsante "..." e cercare il gruppo PAW Users.

                5.  Fare clic su **nuovo elemento** e seleziona **gruppo di sicurezza**.

                6.  Seleziona il pulsante "…" e cercare il **Cloud Services Admins** gruppo.

                7.  Fare clic su di **Cloud Services Admins** elemento e fare clic su **opzioni elemento**.

                8.  Selezionare **non**.

                9. Fare clic su **OK** nella finestra di destinazione.

            7.  Fare clic su **OK** per completare l'impostazione di criteri di gruppo ProxyServer

        2.  Vai a configurazione utente\preferenze\impostazioni windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** \> **elemento Registro di sistema** e configurare le impostazioni seguenti:

            -   Azione: Sostituisci

            -   Hive: HKEY_CURRENT_USER

            -   Il percorso della chiave: Software\microsoft\windows\currentversion\impostazioni Internet

            -   Nome valore: ProxyServer

                > [!NOTE]
                > Non selezionare la casella predefinito a sinistra del nome del valore.

            -   Tipo valore: REG_SZ

            -   Dati valore: 127.0.0.1: 80

                1.  Fare clic su di **comuni** scheda e selezionare **rimuovere questo elemento quando non viene più applicato**.

                2.  Nel **comuni** scheda selezionare **destinazione a livello di elemento** e fare clic su **Targeting**.

                3.  Fare clic su **nuovo elemento** e gruppo di sicurezza selezionare.

                4.  Seleziona il pulsante "…" e aggiungere il gruppo PAW Users.

                5.  Fare clic su **nuovo elemento** e gruppo di sicurezza selezionare.

                6.  Seleziona il pulsante "…" e cercare il **Cloud Services Admins** gruppo.

                7.  Fare clic su di **Cloud Services Admins** elemento e fare clic su **opzioni elemento**.

                8.  Selezionare **non**.

                9. Fare clic su **OK** nella finestra di destinazione.

        3.  Fare clic su **OK** per completare l'impostazione di criteri di gruppo ProxyServer

    2.  Vai a utente Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Internet Explorer e abilitare le opzioni seguenti. Queste impostazioni impedirà agli amministratori di sostituire manualmente le impostazioni del proxy.

        1.  Abilitare il **Impedisci la modifica di configurazione automatica** impostazioni.

        2.  Abilitare il **Impedisci la modifica delle impostazioni proxy**.

7.  **Impedire agli amministratori l'accesso agli host di livello inferiore**.  In questa sezione verrà configurato Criteri di gruppo per impedire gli account amministrativi con privilegi di accesso agli host di livello inferiore.

    1.  Creare il nuovo **limitare l'accesso a una Workstation** oggetto Criteri di gruppo - questa impostazione impedisce di livello 0 e gli account amministratore di livello 1 accedere alle workstation standard.  Questo oggetto Criteri di gruppo deve essere collegato all'unità Organizzativa di primo livello "Workstation" e sono disponibili le seguenti impostazioni:

        - (i) Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come processo batch, selezionare **definire queste impostazioni dei criteri** e aggiungere i gruppi di livello 0 e livello 1:

            **Gruppi da aggiungere alle impostazioni dei criteri:**

            Enterprise Admins

            Domain Admins

            Schema Admins

            DOMAIN\Administrators.

            Account Operators

            Gruppo Backup Operators

            Operatori di stampa

            Server Operators

            Controller di dominio

            Read-Only Controller di dominio

            Proprietari autori criteri di gruppo

            Operazioni di crittografia

            > [!NOTE]
            > Nota: Gruppi di livello 0 predefiniti, vedere l'equivalenza di livello 0 per altri dettagli.

            Altri gruppi delegati

            > [!NOTE]
            > Nota: Tutti i gruppi personalizzati creati con accesso al livello 0, vedere l'equivalenza di livello 0 per altri dettagli.

            Amministratori di livello 1

            > [!NOTE]
            > Nota: Questo gruppo è stato creato in precedenza nella fase 1

        - (ii) In Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come servizio, selezionare **definire queste impostazioni dei criteri** e aggiungere i gruppi di livello 0 e livello 1:

            **Gruppi da aggiungere alle impostazioni dei criteri:**

            Enterprise Admins

            Domain Admins

            Schema Admins

            DOMAIN\Administrators.

            Account Operators

            Gruppo Backup Operators

            Operatori di stampa

            Server Operators

            Controller di dominio

            Read-Only Controller di dominio

            Proprietari autori criteri di gruppo

            Operazioni di crittografia

            > [!NOTE]
            > Nota: Gruppi di livello 0 predefiniti, vedere l'equivalenza di livello 0 per altri dettagli.

            Altri gruppi delegati

            > [!NOTE]
            > Nota: Tutti i gruppi personalizzati creati con accesso al livello 0, vedere l'equivalenza di livello 0 per altri dettagli.

            Amministratori Teir 1

            > [!NOTE]
            > Nota: Questo gruppo è stato creato in precedenza nella fase 1

    2.  Creare il nuovo **limitare l'accesso al Server** oggetto Criteri di gruppo - questa impostazione limita gli account amministratore di livello 0 di accedere a server di livello 1.  Questo oggetto Criteri di gruppo deve essere collegato all'unità Organizzativa di primo livello "Server di livello 1" e sono disponibili le seguenti impostazioni:

        -   (i) Nel Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come processo batch, selezionare **definire queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:

            **Gruppi da aggiungere alle impostazioni dei criteri:**

            Enterprise Admins

            Domain Admins

            Schema Admins

            DOMAIN\Administrators.

            Account Operators

            Gruppo Backup Operators

            Operatori di stampa

            Server Operators

            Controller di dominio

            Read-Only Controller di dominio

            Proprietari autori criteri di gruppo

            Operazioni di crittografia

            > [!NOTE]
            > Nota: Gruppi di livello 0 predefiniti, vedere l'equivalenza di livello 0 per altri dettagli.

            Altri gruppi delegati

            > [!NOTE]
            > Nota: Tutti i gruppi personalizzati creati con accesso al livello 0, vedere l'equivalenza di livello 0 per altri dettagli.

        -   (ii) In Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso come servizio, selezionare **definire queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:

            **Gruppi da aggiungere alle impostazioni dei criteri:**

            Enterprise Admins

            Domain Admins

            Schema Admins

            DOMAIN\Administrators.

            Account Operators

            Gruppo Backup Operators

            Operatori di stampa

            Server Operators

            Controller di dominio

            Read-Only Controller di dominio

            Proprietari autori criteri di gruppo

            Operazioni di crittografia

            > [!NOTE]
            > Nota: Gruppi di livello 0 predefiniti, vedere l'equivalenza di livello 0 per altri dettagli.

            Altri gruppi delegati

            > [!NOTE]
            > Nota: Tutti i gruppi personalizzati creati con accesso al livello 0, vedere l'equivalenza di livello 0 per altri dettagli.

        -   (iii) In Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente\nega accesso locale selezionare **definire queste impostazioni dei criteri** e aggiungere i gruppi di livello 0:

            **Gruppi da aggiungere alle impostazioni dei criteri:**

            Enterprise Admins

            Domain Admins

            Schema Admins

            Account Operators

            Gruppo Backup Operators

            Operatori di stampa

            Server Operators

            Controller di dominio

            Read-Only Controller di dominio

            Proprietari autori criteri di gruppo

            Operazioni di crittografia

            > [!NOTE]
            > Nota: Gruppi di livello 0 predefiniti, vedere l'equivalenza di livello 0 per altri dettagli.

            Altri gruppi delegati

            > [!NOTE]
            > Nota: Tutti i gruppi personalizzati creati con accesso al livello 0, vedere l'equivalenza di livello 0 per altri dettagli.

8.  Distribuire le workstation Paw

    > [!NOTE]
    > Assicurarsi che la workstation PAW sia disconnessa dalla rete durante il processo di build del sistema operativo.

    1.  Installare Windows 10 tramite il supporto di installazione di origine pulita ottenuto in precedenza.

        > [!NOTE]
        > È possibile utilizzare Microsoft Deployment Toolkit (MDT) o un altro sistema di distribuzione automatica dell'immagine per automatizzare la distribuzione PAW, ma è necessario verificare che il processo di build sia attendibile tanto quanto PAW. Gli antagonisti cercano in particolare immagini aziendali e sistemi di distribuzione (inclusi i file ISO, i pacchetti di distribuzione e così via) come meccanismo di persistenza, in modo non devono essere utilizzate preesistenti sistemi di distribuzione o immagini.
        >
        > Se si intende automatizzare la distribuzione di workstation PAW, è necessario:
        >
        > -   Build del sistema tramite supporti di installazione convalidati sulla base delle indicazioni [origine pulita per i supporti di installazione](http://aka.ms/cleansource).
        > -   Assicurarsi che il sistema di distribuzione automatizzata sia disconnesso dalla rete durante il processo di build del sistema operativo.

    2.  Impostare una password complessa univoca per l'account amministratore locale.  Non utilizzare una password che è stata usata per qualsiasi altro account nell'ambiente.

        > [!NOTE]
        > Microsoft consiglia di usare [locale Administrator Password Solution (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) per gestire la password di amministratore locale per tutte le workstation, incluse le workstation Paw.  Se si usa la soluzione LAPS, assicurarsi di concedere solo al gruppo PAW Maintenance il diritto di lettura di password gestite da LAPS per le workstation Paw.

    3.  Installare Remote Server Administration Tools per Windows 10 tramite il supporto di installazione di origine pulita.

    4.  Installare avanzata EMET Mitigation Experience Toolkit () utilizzando il supporto di installazione di origine pulita.

        > [!NOTE]
        > Tieni presente che, al momento dell'ultimo aggiornamento di questo materiale sussidiario, EMET 5.5 era ancora in versione beta.  Poiché si tratta della prima versione supportata in Windows 10, è inclusa qui nonostante lo stato della versione beta.  Se l'installazione di software beta nella workstation PAW supera la tolleranza al rischio, è possibile ignorare questo passaggio per ora.

    5.  Connettere la workstation PAW alla rete.  Assicurarsi che la workstation PAW possa connettersi al Controller di dominio almeno un (DC).

    6.  Utilizzando un account che sia membro del gruppo di manutenzione PAW, eseguire il comando PowerShell seguente dalla workstation PAW appena creata per aggiungerla al dominio nella OU appropriata:

        `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

        Sostituire i riferimenti a *Fabrikam* con il nome di dominio, come appropriato.  Se il nome di dominio si estende su più livelli (ad esempio child.fabrikam.com), aggiungere i nomi aggiuntivi con il "DC =" identificatore nell'ordine in cui compaiono nel nome di dominio completo del dominio.

        > [!NOTE]
        > Se è stato distribuito un [foresta amministrativa ESAE](http://aka.ms/esae) (per gli amministratori di livello 0 nella fase 1) o un [Microsoft Identity Manager (MIM) privileged access management (PAM)](http://aka.ms/mimpamdeploy) (per livello 1 e 2 admins nella fase 2), è necessario aggiungere la workstation PAW al dominio in tale ambiente anziché al dominio di produzione.

    7.  Applicare tutti gli aggiornamenti di Windows critici e importanti prima di installare qualsiasi altro software (inclusi strumenti di amministrazione, gli agenti e così via).

    8.  Forzare l'applicazione di criteri di gruppo.

        1.  Aprire un prompt dei comandi con privilegi elevati e immettere il comando seguente:

            `Gpupdate /force /sync`

        2.  Riavviare il computer

    9. **(Facoltativo) **Installare gli strumenti aggiuntivi necessari per gli amministratori di Active Directory. Installare altri strumenti o gli script necessari per svolgere i compiti di processo. Assicurarsi di valutare il rischio di esposizione delle credenziali nei computer di destinazione con qualsiasi strumento prima di aggiungerlo a una workstation PAW. Accesso [questa pagina](http://aka.ms/logontypes) per ottenere altre informazioni sulla valutazione di strumenti di amministrazione e i metodi di connessione per il rischio di esposizione delle credenziali. Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni fornite in [origine pulita per i supporti di installazione](http://aka.ms/cleansource).

        > [!NOTE]
        > Utilizzo di un jump server per una posizione centrale per questi strumenti può ridurre la complessità, anche se non funge come un limite di sicurezza.

    10. **(Facoltativo) **Scaricare e installare il software di accesso remoto necessario. Se gli amministratori useranno le workstation PAW in remoto per l'amministrazione, installare il software di accesso remoto secondo le indicazioni di sicurezza dal fornitore della soluzione di accesso remoto. Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in origine pulita per i supporti di installazione.

        > [!NOTE]
        > Valutare con attenzione tutti i rischi impliciti nell'accesso remoto tramite una workstation PAW.  Mentre una sistema PAW mobile consente molti scenari di importanti, tra cui il lavoro da casa, software di accesso remoto può essere vulnerabile agli attacchi e usato per compromettere una workstation PAW.

    11. Convalidare l'integrità del sistema PAW, esaminare la conferma che tutte le impostazioni appropriate siano presenti tramite la procedura seguente:

        1.  Verificare che siano applicati solo i criteri di gruppo PAW specifici dell'architettura paw

            1.  Aprire un prompt dei comandi con privilegi elevati e immettere il comando seguente:

                `Gpresult /scope computer /r`

            2.  Esaminare l'elenco risultante e verificare che l'unico gruppo di criteri che vengono visualizzati sono quelli creati in precedenza.

        2.  Verificare che nessun account utente aggiuntivi è membri di gruppi con privilegi nella workstation PAW tramite la procedura seguente:

            1.  Apri **Modifica utenti e gruppi locali** (lusrmgr.msc), selezionare **gruppi**e verificare che gli unici membri del gruppo Administrators locale siano l'account amministratore locale e il gruppo di sicurezza globale PAW Maintenance.

                > [!NOTE]
                > Il gruppo PAW Users non deve essere un membro del gruppo Administrators locale.  Gli unici membri devono essere l'account amministratore locale e il gruppo di sicurezza globale PAW Maintenance (e gli utenti PAW non deve essere un membro del gruppo globale è).

            2.  Anche con **Modifica utenti e gruppi locali**, assicurarsi che i gruppi seguenti non contengano membri:

                -   Gruppo Backup Operators

                -   Operazioni di crittografia

                -   Amministratori di Hyper-V

                -   Network Configuration Operators

                -   Utenti esperti

                -   Utenti Desktop remoto

                -   Replicator

    12. **(Facoltativo) **Se l'organizzazione utilizza una soluzione di gestione (SIEM) di eventi e informazioni di sicurezza, verificare che la workstation PAW sia [configurato per l'inoltro di eventi al sistema tramite l'inoltro di eventi di Windows (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) o in caso contrario è registrato con la soluzione in modo che la soluzione SIEM riceva attivamente eventi e informazioni dalla workstation PAW.  I dettagli di questa operazione dipendono dalla soluzione SIEM.

        > [!NOTE]
        > Se la soluzione SIEM richiede un agente che viene eseguito come sistema o un account amministratore locale per le workstation Paw, assicurarsi che le soluzioni Siem vengano gestite con lo stesso livello di attendibilità come controller di dominio e dei sistemi di identità.

    13. **(Facoltativo) **Se si sceglie di distribuire LAPS per gestire la password per l'account Administrator locale nella workstation PAW, verificare che la password sia registrata correttamente.

        -   Con un account con autorizzazioni per la lettura di password gestite con LAPS aprire **Active Directory Users and Computers** (dsa.msc).  Assicurarsi che le funzionalità avanzate è abilitata e quindi fare doppio clic su oggetto computer appropriato.  Selezionare la scheda Editor attributi e verificare che il valore per msSVSadmPwd viene popolato con una password valida.

#### <a name="phase-2---extend-paw-to-all-administrators"></a>Fase 2: estensione di PAW a tutti gli amministratori
Ambito: Tutti gli utenti con diritti amministrativi su applicazioni mission-critical e le dipendenze.  Questo deve includere almeno gli amministratori del server applicazioni, l'integrità operativa e soluzioni, le soluzioni di virtualizzazione, sistemi di archiviazione e i dispositivi di rete di monitoraggio di sicurezza.

> [!NOTE]
> Le istruzioni riportate in questa fase presuppongono che la fase 1 è stata completata nel suo complesso.  Iniziare la fase 2 fino a completare tutti i passaggi della fase 1.

Dopo aver verificato che tutti i passaggi sono stati eseguiti, eseguire la procedura seguente per completare la fase 2:

1.  **(Scelta consigliata) **Abilitare **RestrictedAdmin** modalità - abilitare questa funzionalità nei server esistenti e le workstation, quindi imporne l'uso di questa funzionalità. Questa funzionalità richiede che il server di destinazione sia in esecuzione Windows Server 2008 R2 o versione successiva e di destinazione workstation sia in esecuzione Windows 7 o versione successiva.

    1.  Abilitare **RestrictedAdmin** nei server e workstation seguendo le istruzioni disponibili in questa modalità [pagina](http://aka.ms/rdpra).

        > [!NOTE]
        > Prima di abilitare questa funzionalità per i server con connessione internet, valutare il rischio che eventuali antagonisti siano in grado di eseguire l'autenticazione a tali server con un hash della password rubata in precedenza.

    2.  Creare l'oggetto Criteri di gruppo "RestrictedAdmin necessaria – Computer" (GPO). In questa sezione Crea un oggetto Criteri di gruppo che impone l'utilizzo del /RestrictedAdmin commutatore per le connessioni Desktop remoto in uscita, proteggere gli account dal furto di credenziali nei sistemi di destinazione

        -   Passare a Computer Configurazione computer\Criteri\Modelli amministrativi\sistema\delega credenziali\limita delega delle credenziali ai server remoti e impostare su **abilitato**.

    3.  Collegamento di **RestrictedAdmin** necessaria - Computer appropriato livello 1 e/o livello 2 dispositivi utilizzando le opzioni dei criteri seguenti:

        PAW Configuration - Computer

        -> Percorso collegamento: Admin\Tier 0 \ Devices (esistente)

        PAW Configuration - utente

        -> Percorso collegamento: Admin\Tier 0 \ Accounts

        RestrictedAdmin necessaria - Computer

        -> Admin\Tier1\Devices o -> Admin\Tier2\Devices (entrambi sono facoltativi)

        > [!NOTE]
        > Questo non è necessario per i sistemi di livello 0, questi sistemi sono già nel controllo completo di tutte le risorse nell'ambiente.

2.  Spostare gli oggetti di livello 1 nelle OU appropriate.

    1.  Spostare i gruppi di livello 1 per il Admin\Tier 1 OU. Individuare tutti i gruppi di concedono i diritti amministrativi seguenti e spostarli in questa OU.

        -   Amministratore locale in uno o più server

        -   Accesso amministrativo a servizi cloud

        -   Accesso amministrativo alle applicazioni aziendali

    2.  Spostare gli account di livello 1 alla OU Admin\Tier 1. Spostare ogni account è un membro dei gruppi di livello 1 (incluse le appartenenze annidate) in questa OU.

3.  Aggiungere i membri appropriati ai gruppi pertinenti

    -   **Livello 1 Admins** -questo gruppo conterrà gli amministratori di livello 1 che è consentito l'accesso a host di livello 2. Aggiungere tutti i gruppi amministrativi di livello 1 che dispongano di privilegi amministrativi su server o servizi internet.

        > [!NOTE]
        > Se il personale amministrativo compiti di gestire le risorse su più livelli, è necessario creare un account amministrativo separato per ogni livello.

4.  Abilitare Credential Guard per ridurre il rischio di furto di credenziali e riutilizzare.  Credential Guard è una nuova funzionalità di Windows 10 che limita l'accesso all'applicazione le credenziali, impedire gli attacchi contro il furto delle credenziali (inclusi Pass-the-Hash).  Credential Guard è completamente trasparente all'utente finale e richiede l'installazione minima tempo e.  Per ulteriori informazioni su Credential Guard, inclusi i passaggi di distribuzione e i requisiti hardware, fare riferimento all'articolo [proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

    > [!NOTE]
    > Per configurare e usare Credential Guard è necessario abilitare Device Guard.  Tuttavia, non sono necessari per configurare eventuali altre protezioni di Device Guard per usare Credential Guard.

5.  **(Facoltativo) **Consentono la connettività ai servizi Cloud. Questo passaggio consente la gestione di servizi cloud quali Azure e Office 365 con garanzie di sicurezza appropriate. Questo passaggio è anche necessario per Microsoft Intune gestire le workstation Paw.

    > [!NOTE]
    > Ignorare questo passaggio se nessuna connettività cloud è necessaria per l'amministrazione dei servizi cloud o la gestione da Intune.

    Questi passaggi saranno limitano la comunicazione tramite internet per solo i servizi cloud autorizzati (ma non nelle reti internet aperte) e aggiungono misure di protezione per i browser e altre applicazioni che elaborano contenuto da internet. Queste workstation Paw per l'amministrazione non deve mai essere usate per attività di utente standard come le comunicazioni internet e produttività.

    Per abilitare la connettività a workstation PAW servizi attenersi alla procedura seguente:

    1.  Configurare la workstation PAW per consentire solo destinazioni Internet autorizzate.  Come si estende la distribuzione PAW per abilitare l'amministrazione cloud, è necessario consentire l'accesso ai servizi autorizzati escludendo accesso dai siti internet in cui è possano più facilmente montati attacchi contro gli amministratori.

        1.  Creare **Cloud Services Admins** di gruppo e aggiungervi tutti gli account che richiedono l'accesso a servizi cloud in internet.

        2.  Scaricare la workstation PAW *proxy.pac* del file da [Raccolta TechNet](http://aka.ms/pawmedia) e pubblicarlo in un sito Web interno.

            > [!NOTE]
            > È necessario aggiornare il *proxy.pac* file dopo il download per assicurarsi che sia aggiornato e completo.  
            > Microsoft pubblica tutti gli URL di Azure e Office 365 di corrente nell'ufficio [supporto tecnico](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
            >
            > Si potrebbe essere necessario aggiungere altre destinazioni Internet valide corrispondenti per aggiungere a questo elenco per altri provider IaaS, ma non aggiungere la produttività, intrattenimento, notizie o siti all'elenco di ricerca.
            >
            > È necessario anche modificare il file PAC per aggiungere un indirizzo proxy valido da utilizzare per questi indirizzi.

            > [!NOTE]
            > È inoltre possibile limitare l'accesso dalla workstation PAW anche mediante un proxy web di una difesa in profondità. Non è consigliabile usare quest'ultimo stesso senza il file PAC come solo limita accesso workstation paw durante la connessione alla rete aziendale.

            Queste istruzioni si presuppone che si utilizzerà Internet Explorer (o Microsoft Edge) per l'amministrazione di Office 365, Azure e altri servizi cloud. Microsoft consiglia di configurare restrizioni simili per qualsiasi browser di terze parti 3rd necessari per l'amministrazione. Web browser nelle workstation Paw deve essere utilizzato solo per l'amministrazione dei servizi cloud e mai semplicemente per esplorare il web.

        3.  Dopo aver configurato il *proxy.pac* file, aggiornare la configurazione PAW - utente oggetto Criteri di gruppo.

            1.  Vai a configurazione utente\preferenze\impostazioni windows\registro di sistema. Il pulsante destro del Registro di sistema, selezionare **New** \> **elemento Registro di sistema** e configurare le impostazioni seguenti:

                1.  Azione: Sostituisci

                2.  Hive: HKEY _ CURRENT_USER

                3.  Il percorso della chiave: Software\microsoft\windows\currentversion\impostazioni Internet

                4.  Nome valore: AutoConfigUrl

                    > [!NOTE]
                    > Non selezionare la **predefinito** casella a sinistra del nome del valore.

                5.  Tipo valore: REG_SZ

                6.  Dati valore: immettere l'URL completo per il *proxy.pac* file, includendo http:// e il nome del file, ad esempio http://proxy.fabrikam.com/proxy.pac.  L'URL può anche essere un URL con etichetta singola, ad esempio, http://proxy/proxy.pac

                    > [!NOTE]
                    > Il file PAC può anche essere ospitato in una condivisione di file, con la sintassi di file://server.fabrikan.com/share/proxy.pac ma è necessario consentire il protocollo file://. Vedere la sezione "NOTE: File://-based Proxy Scripts Deprecated" di questo [informazioni sulla configurazione del Proxy Web](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog per altri dettagli sulla configurazione il valore del Registro di sistema.

                7.  Fare clic su di **comuni** scheda e selezionare **rimuovere questo elemento quando non viene più applicato**.

                8.  Nel **comuni** scheda selezionare **destinazione a livello di elemento** e fare clic su **Targeting**.

                9. Fare clic su **nuovo elemento** e seleziona **gruppo di sicurezza**.

                10. Seleziona il pulsante "…" e cercare il **Cloud Services Admins** gruppo.

                11. Fare clic su **nuovo elemento** e seleziona **gruppo di sicurezza**.

                12. Seleziona il pulsante "…" e cercare il **gli utenti PAW** gruppo.

                13. Fare clic su di **gli utenti PAW** elemento e fare clic su **opzioni elemento**.

                14. Selezionare **non**.

                15. Fare clic su **OK** nella finestra di destinazione.

                16. Fare clic su **OK** per completare il **AutoConfigUrl** impostazione di criteri di gruppo.

    2.  Applicare le baseline di sicurezza di Windows 10 e collegamento di accesso del servizio Cloud le baseline di sicurezza per Windows e per il servizio cloud accedere (se necessario) alle OU corrette tramite la procedura seguente:

        1.  Estrai il contenuto del file ZIP linee di base della sicurezza di Windows 10.

        2.  Creare questi oggetti Criteri di gruppo, [importare i criteri](https://technet.microsoft.com/en-us/library/cc753786.aspx), impostazioni e [collegamento](https://technet.microsoft.com/en-us/library/cc732979.aspx) per questa tabella. Collegare ciascun criterio a ciascun percorso e assicurarsi di seguire l'ordine della tabella (inferiore voci nella tabella devono essere applicate dopo e maggiore priorità):

            **Criteri:**

            |||
            |-|-|
            |CM Windows 10 - sicurezza di dominio|N/d - non collegare adesso|
            |SCM Windows 10 TH2 - Computer|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |SCM Windows 10 TH2-BitLocker|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |SCM Windows 10 - Credential Guard|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |SCM Internet Explorer - Computer|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |PAW Configuration - Computer|Admin\Tier 0 \ Devices (esistente)|
            ||Admin\Tier 1 \ Devices (nuovo collegamento)|
            ||Admin\Tier 2 \ Devices (nuovo collegamento)|
            |RestrictedAdmin necessaria - Computer|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |SCM Windows 10 - utente|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |SCM Internet Explorer - utente|Admin\Tier 0 \ Devices|
            ||Admin\Tier 1 \ Devices|
            ||Admin\Tier 2 \ Devices|
            |PAW Configuration - utente|Admin\Tier 0 \ Devices (esistente)|
            ||Admin\Tier 1 \ Devices (nuovo collegamento)|
            ||Admin\Tier 2 \ Devices (nuovo collegamento)|

            > [!NOTE]
            > "SCM Windows 10 – protezione del dominio" oggetto Criteri di gruppo può essere collegata al dominio indipendentemente dall'ambiente PAW, ma influisce sull'intero dominio.

6.  **(Facoltativo) **Installare gli strumenti aggiuntivi necessari per amministratori di livello 1. Installare altri strumenti o gli script necessari per svolgere i compiti di processo. Assicurarsi di valutare il rischio di esposizione delle credenziali nei computer di destinazione con qualsiasi strumento prima di aggiungerlo a una workstation PAW. Per ulteriori informazioni sulla valutazione di strumenti di amministrazione e connessione metodi per il rischio di esposizione delle credenziali visitare [questa pagina](http://aka.ms/logontypes). Assicurarsi di procurarsi tutti i supporti di installazione secondo le indicazioni riportate in origine pulita per i supporti di installazione

7.  Identificare e ottenere in modo sicuro software e le applicazioni necessarie per l'amministrazione.  È simile alle operazioni eseguite nella fase 1, ma con un ambito più ampio a causa del maggior numero di applicazioni, servizi e sistemi da proteggere.

    > [!NOTE]
    > Assicurarsi di proteggere le nuove applicazioni (inclusi i web browser) inserendole nelle misure di protezione fornite dal toolkit EMET.

    Esempi di applicazioni e software aggiuntivi:

    -   Microsoft Azure PowerShell

    -   PowerShell di Office 365 (noto anche come Microsoft Online Services Module)

    -   Applicazioni o software di gestione del servizio in base a Microsoft Management Console

    -   Software di gestione del servizio o applicazione proprietaria (non basato su MMC)

        > [!NOTE]
        > Molte applicazioni vengono ora gestite esclusivamente tramite web browser, inclusi molti servizi cloud.  Ciò consente di ridurre il numero di applicazioni che devono essere installati in una workstation PAW, introduce anche il rischio di problemi di interoperabilità del browser.  Potrebbe essere necessario distribuire un browser web non Microsoft in istanze PAW specifiche per consentire l'amministrazione dei servizi specifici.  Se si distribuisce un web browser aggiuntivo, assicurarsi di seguire tutti i principi di origine pulita e proteggere il browser in base alle indicazioni di sicurezza del fornitore.

8.  **(Facoltativo) **Scaricare e installare gli agenti di gestione richiesti.

    > [!NOTE]
    > Se si sceglie di installare gli agenti di gestione aggiuntivi (monitoraggio, sicurezza, gestione della configurazione e così via), è fondamentale assicurarsi che i sistemi di gestione siano attendibili allo stesso livello di controller di dominio e dei sistemi di identità. Per ulteriori istruzioni, vedere Gestione e aggiornamento delle workstation Paw.

9. Valutare l'infrastruttura per identificare i sistemi che richiedono le protezioni di sicurezza aggiuntive fornite da una workstation PAW.  Assicurarsi di conoscere esattamente quali sistemi debbano essere protetti.  Porre domande critiche sulle risorse stesse, ad esempio:

    -   Dove sono i sistemi di destinazione che devono essere gestiti?  Vengono raccolti in un'unica posizione fisica, o connessi a una singola subnet ben definita?

    -   Quanti sistemi sono presenti?

    -   Questi sistemi dipendono altri sistemi (virtualizzazione, archiviazione e così via) e in tal caso, come vengono gestiti?  Come sono i sistemi critici sono esposti a queste dipendenze e quali sono i rischi aggiuntivi associati con tali dipendenze?

    -   Sono critici i servizi gestiti e qual è la perdita prevista se tali servizi sono compromesso?

        > [!NOTE]
        > Includere i servizi cloud in questa valutazione: gli utenti malintenzionati di destinazione sempre più distribuzioni cloud non sicure ed è fondamentale amministrare tali servizi nel modo più sicuro come si farebbe le applicazioni critiche locali.

        Utilizzare questa valutazione per identificare i sistemi specifici che richiedono una protezione aggiuntiva e quindi estendere il programma PAW agli amministratori di tali sistemi.  Esempi comuni di sistemi che traggono grandi vantaggi dall'amministrazione basata su PAW includono SQL Server (locale e SQL Azure), le applicazioni di risorse umane e il software finanziario.

        > [!NOTE]
        > Se una risorsa è gestita da un sistema di Windows, può essere gestito con una workstation PAW, anche se l'applicazione viene eseguita in un sistema operativo diverso da Windows o su una piattaforma cloud non Microsoft.  Ad esempio, il proprietario di un abbonamento Amazon Web Services dovrebbe usare solo una workstation PAW per amministrare tale account.

10. Sviluppare un metodo di richiesta e di distribuzione per la distribuzione di workstation Paw su larga scala nell'organizzazione.  A seconda del numero di workstation Paw si sceglie di distribuire nella fase 2, potrebbe essere necessario automatizzare il processo.

    -   Si consiglia di sviluppare un formale di richiesta e il processo di approvazione per gli amministratori di utilizzare per ottenere una workstation PAW.  Questo processo consentirebbe di standardizzare il processo di distribuzione, assicurare l'affidabilità dei dispositivi PAW e facilitare l'identificazione di lacune nella distribuzione PAW.

    -   Come indicato in precedenza, questa soluzione di distribuzione deve essere separata dai metodi di automazione esistente (che potrebbero già essere stati compromessi) e seguire i principi descritti nella fase 1.

        > [!NOTE]
        > Qualsiasi sistema che gestisce le risorse devono essere gestiti a livello di attendibilità uguale o superiore.

11. Rivedere e se è necessario distribuire profili hardware PAW aggiuntivi.  Il profilo hardware che scelto per la distribuzione nella fase 1 potrebbe non essere appropriato per tutti gli amministratori.  Rivedere i profili hardware e se appropriato, selezionare profili hardware PAW aggiuntivi per soddisfare le esigenze degli amministratori.  Ad esempio, potrebbe essere adatto a un amministratore che viaggia spesso il profilo Hardware dedicato (workstation PAW e ogni giorno distinti usare workstation): in questo caso, è possibile scegliere di distribuire il profilo uso simultaneo (workstation PAW con VM utente) per questo amministratore.

12. Prendere in considerazione il culturale, operative, le comunicazioni e alla formazione che accompagnano una distribuzione PAW estesa.   Una modifica così significativa a un modello amministrativo richiede naturalmente un certo grado di gestione del cambiamento ed è essenziale integrare tale aspetto del progetto di distribuzione stesso.  Considerare come minimo quanto segue:

    -   Come comunicare le modifiche ai dirigenti senior per ottenerne il supporto?  Qualsiasi progetto privo sostegno dei dirigenti senior è probabilmente esito negativo, o di almeno una continua lotta per ottenere i fondi necessari ed essere ampiamente accettato.

    -   Come verrà documentato il nuovo processo per gli amministratori?  Queste modifiche devono essere documentate e comunicate non solo agli amministratori esistenti (che è necessario modificare le proprie abitudini e gestire le risorse in modo diverso), ma anche per nuovi amministratori (quelli promossi all'interno e quelli assunti dall'esterno dell'organizzazione).  È essenziale che la documentazione sia chiara e articulates completamente l'importanza delle minacce, il ruolo dell'ambiente PAW nella protezione degli amministratori e come usare PAW correttamente.

        > [!NOTE]
        > Questo è particolarmente importante per i ruoli con vengono cambiati di frequente, tra cui a titolo esemplificativo addetti all'assistenza.

    -   Come si garantirà la conformità con il nuovo processo?  Mentre il modello PAW include un numero di controlli tecnici per evitare l'esposizione delle credenziali con privilegi, non è possibile evitare completamente tutti i casi di esposizione affidandosi controlli tecnici.  Ad esempio, sebbene sia possibile evitare che un amministratore di accedere a un desktop utente con credenziali con privilegi, il semplice fatto di tentare l'accesso può esporre le credenziali a malware eventualmente installato in quel desktop utente.  È pertanto essenziale descrivere in dettaglio non solo i vantaggi del modello PAW, ma anche i rischi di non conformità.  Questo deve essere completato da controlli e avvisi in modo che l'esposizione delle credenziali può essere rilevati e risolti rapidamente.

#### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: Estendere e migliorare la protezione dati
Ambito: Queste protezioni migliorano i sistemi creati nella fase 1, potenziando la protezione di base con funzionalità avanzate, tra cui multi-factor authentication e le regole di accesso di rete.

> [!NOTE]
> Questa fase può essere eseguita in qualsiasi momento dopo aver completato la fase 1.  Non è dipende dal completamento della fase 2 e pertanto può essere eseguita prima, contemporaneamente o dopo la fase 2.

Attenersi alla procedura seguente per configurare questa fase:

1.  Abilitare multi-factor authentication per gli account con privilegi.  Autenticazione a più fattori aumenta il livello di protezione account richiedendo all'utente di fornire un token fisico oltre alle credenziali.  Autenticazione a più fattori integra i criteri di autenticazione molto efficace, ma non dipendono da criteri di autenticazione per la distribuzione (e, in modo analogo, i criteri di autenticazione non richiedono l'autenticazione a più fattori).  Microsoft consiglia di usare una di queste forme di autenticazione a più fattori:

    -   **Smart card**: una smart card è un dispositivo fisico portatile e resistente alle manomissioni che fornisce una seconda verifica durante il processo di accesso di Windows.  Richiedendo a un utente di presentare una smart card per l'accesso, è possibile ridurre il rischio di furto di credenziali da riutilizzare in modalità remota.  Per informazioni dettagliate sull'accesso con smart card in Windows, fare riferimento all'articolo [panoramica delle Smart Card](https://technet.microsoft.com/en-us/library/hh831433.aspx).

    -   **Smart card virtuale**: una smart card virtuale offre gli stessi vantaggi di sicurezza smart card fisiche, con il vantaggio aggiuntivo del collegamento ad hardware specifico.  Per informazioni dettagliate sulla distribuzione e i requisiti hardware, consultare gli articoli, [panoramica delle Smart Card virtuale](https://technet.microsoft.com/en-us/library/dn593708.aspx) e [iniziare con Smart card virtuali: Guida allo scenario](https://technet.microsoft.com/en-us/library/dn579260.aspx).

    -   **Microsoft Passport**: Microsoft Passport consente agli utenti di eseguire l'autenticazione a un account Microsoft, un account Active Directory, un account di Microsoft Azure Active Directory (Azure AD) o un servizio non Microsoft che supporta l'autenticazione Fast ID Online (FIDO). Dopo una verifica in due passaggi iniziale durante la registrazione di Microsoft Passport, viene impostato un Microsoft Passport nel dispositivo dell'utente e l'utente imposta un movimento, che può essere Windows Hello o un PIN. Credenziali di Microsoft Passport sono una coppia di chiavi asimmetrica, che può essere generata all'interno di ambienti isolati Trusted Platform Module (TPM).
        Per ulteriori informazioni su Microsoft Passport leggere [Panoramica di Microsoft Passport](https://technet.microsoft.com/en-us/library/dn985839%28v=vs.85%29.aspx) articolo.

    -   **Autenticazione a più fattori Azure**: Azure multi-factor authentication (MFA) offre la sicurezza di un secondo fattore di verifica, nonché protezione avanzata tramite Monitoraggio e basata su apprendimento macchina analisi.  Azure MFA consente di proteggere non solo gli amministratori di Azure, ma molte altre soluzioni, tra cui applicazioni web, Azure Active Directory e soluzioni locali come Desktop remoto e accesso remoto.  Per ulteriori informazioni su Azure multi-factor authentication, fare riferimento all'articolo [multi-Factor Authentication](https://azure.microsoft.com/en-us/services/multi-factor-authentication).

2.  **Elenco di elementi consentiti attendibili di applicazioni con Device Guard e/o AppLocker**.  Limitando la possibilità di codice non firmato o non attendibile per l'esecuzione in una workstation PAW, ridurre ulteriormente le probabilità di attività dannose e compromissione.  Windows include due opzioni principali per il controllo delle applicazioni:

    -   **AppLocker**: AppLocker consente agli amministratori di controllare quali applicazioni possono essere eseguite in un sistema specifico.  AppLocker può essere centralmente controllato tramite criteri di gruppo e applicato a utenti specifici o gruppi (per applicazioni destinate agli utenti di workstation Paw).  Per ulteriori informazioni su AppLocker, vedere l'articolo di TechNet [Panoramica di AppLocker](https://technet.microsoft.com/library/hh831440.aspx).

    -   **Device Guard**: la nuova funzionalità di Device Guard offre controllo avanzato applicazione basata su hardware che, a differenza di AppLocker, non può essere ignorato nel dispositivo interessato.  Come AppLocker, Device Guard può essere controllata tramite criteri di gruppo e destinarlo a utenti specifici.  Per ulteriori informazioni sulla limitazione di utilizzo delle applicazioni con Device Guard, consultare l'articolo di TechNet, [Guida alla distribuzione di Device Guard](https://technet.microsoft.com/en-us/library/mt463091(v=vs.85).aspx).

3.  **Utilizzare utenti protetti, criteri di autenticazione e silo di autenticazione per proteggere ulteriormente gli account con privilegi**.  I membri di Protected Users sono soggetti a criteri di sicurezza aggiuntivi che proteggono le credenziali archiviati nell'agente di protezione locale (LSA) e riducono notevolmente il rischio di furto di credenziali e il riutilizzo.  Criteri di autenticazione e silo di controllano la modalità con privilegi gli utenti possono accedere alle risorse nel dominio.  Collettivamente, queste protezioni di rafforzare notevolmente la sicurezza degli utenti con privilegi account.  Per ulteriori dettagli su queste funzionalità, fare riferimento all'articolo web [come configurare gli account protetti](https://technet.microsoft.com/en-us/library/dn518179.aspx).

    > [!NOTE]
    > Queste protezioni hanno lo scopo di completare, non la sostituiscono esistente misure di sicurezza nella fase 1.  Gli amministratori devono comunque usare account separati per l'amministrazione e di uso generale.

### <a name="managing-and-updating-paws"></a>Gestione e l'aggiornamento delle workstation Paw
Workstation Paw devono avere funzionalità antimalware e gli aggiornamenti software devono essere applicati rapidamente per mantenere l'integrità delle workstation stesse.

Gestione di configurazioni aggiuntive, il monitoraggio operativo e gestione della sicurezza consente inoltre con le workstation Paw, ma l'integrazione di queste deve essere considerato con attenzione, perché ogni funzionalità di gestione introduce anche il rischio di compromissione delle workstation PAW attraverso lo strumento corrispondente. L'opportunità di introdurre funzionalità di gestione avanzata dipende da un numero di fattori, tra cui:

-   Lo stato di protezione e procedure delle funzionalità di gestione (incluse le procedure di aggiornamento software per gli account e ruoli amministrativi, lo strumento in questi ruoli e sistemi operativi, lo strumento è ospitato in o gestito da qualsiasi altra dipendenza hardware o software di tale strumento)

-   La frequenza e la quantità delle distribuzioni di software e degli aggiornamenti nelle workstation Paw

-   Requisiti per informazioni dettagliate di inventario e alla configurazione

-   Requisiti di monitoraggio della protezione

-   Gli standard organizzativi e altri fattori specifici dell'organizzazione

Per il principio di origine pulita, tutti gli strumenti utilizzati per gestire o monitorare le workstation Paw devono essere considerato attendibili uguale o superiore il livello delle workstation Paw. Ciò richiede in genere tali strumenti siano gestiti da una workstation PAW per non garantire alcuna dipendenza di sicurezza da workstation con privilegi di livello inferiore.

Questa tabella descrive diversi approcci che possono essere utilizzati per gestire e monitorare le workstation Paw:

|Approccio|Considerazioni|
|------|---------|
|Impostazione predefinita in PAW<br /><br />-Windows Server Update Services<br />-Windows Defender|-Nessun costo aggiuntivo<br />-Consente di eseguire le funzioni di sicurezza di base necessarie<br />-Istruzioni incluse in questa Guida|
|Gestire con [Intune](https://technet.microsoft.com/en-us/library/jj676587.aspx)|<ul><li>Fornisce basato su cloud visibilità e controllo<br /><br /><ul><li>Distribuzione del software</li><li>O gestione degli aggiornamenti software</li><li>Gestione criteri di Windows Firewall</li><li>Protezione antimalware</li><li>Assistenza remota</li><li>Gestione delle licenze software.</li></ul></li><li>Necessità di alcuna infrastruttura di server</li><li>Richiede passaggi "Abilitare la connettività a servizi Cloud" fase 2</li><li>Se il computer PAW non è associato a un dominio, è necessario l'applicazione delle baseline SCM alle immagini locali utilizzando gli strumenti forniti nel download della baseline di sicurezza.</li></ul>|
|Nuove istanze di System Center per la gestione delle workstation Paw|-Fornisce visibilità e controllo della configurazione, la distribuzione del software e aggiornamenti della sicurezza<br />-Richiede un'infrastruttura server separata garantirne la protezione a livello delle workstation Paw e competenze per personale con privilegi elevati|
|Gestione delle workstation Paw con gli strumenti di gestione esistenti|-Rischio significativo di compromissione delle workstation Paw, a meno che l'infrastruttura di gestione esistente non venga elevata al livello di sicurezza delle workstation Paw **Nota:** Microsoft in genere sconsiglia questo approccio, a meno che l'organizzazione dispone di un motivo specifico per usarlo. In base all'esperienza, esiste in genere un costo molto elevato di portare tutti questi strumenti (e le relative dipendenze di sicurezza) al livello di sicurezza delle workstation Paw.<br />-La maggior parte di questi strumenti garantisce visibilità e controllo della configurazione, la distribuzione del software e aggiornamenti della sicurezza|
|Security Scanning o strumenti che richiedono accesso amministratore di monitoraggio|Include uno strumento che consente di installare un agente o che richiede un account con accesso amministrativo locale.<br /><br />-Necessità di portare il controllo della protezione strumento al livello delle workstation Paw.<br />-Eventuale necessità di abbassare di sicurezza delle workstation Paw per supportare la funzionalità strumento (apertura di porte, installare Java o altro middleware e così via), la creazione di una decisione di compromesso di sicurezza,|
|Evento e informazioni di gestione della sicurezza (SIEM)|<ul><li>Se SIEM è senza agenti<br /><br /><ul><li>Possono accedere agli eventi nelle workstation Paw senza accesso amministrativo usando un account di **lettori registri eventi** gruppo</li><li>Necessità di aprire porte di rete per consentire il traffico in ingresso dai server SIEM</li></ul></li><li>Se SIEM richiede un agente, vedere la riga **Security Scanning o strumenti che richiedono accesso amministratore di monitoraggio**.</li></ul>|
|Inoltro di eventi di Windows|-Fornisce un metodo senza agente di inoltro di eventi di sicurezza da workstation Paw a un agente di raccolta o SIEM<br />-Possibilità di accedere agli eventi nelle workstation Paw senza accesso amministrativo<br />-Non è necessario aprire porte di rete per consentire il traffico in ingresso dai server SIEM|

#### <a name="operating-paws"></a>Uso delle workstation Paw
La soluzione PAW deve essere usata secondo gli standard in [standard operativi](http://aka.ms/securitystandards) basati sul principio di origine pulita.

## <a name="related-topics"></a>Argomenti correlati
[Servizi di Microsoft riguardanti la sicurezza informatica coinvolgente](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Taste di Premier: come prevenire Pass-the-Hash e altre forme di furto di credenziali](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Threat Analitica](http://aka.ms/ata)

[Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Panoramica di Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[La protezione delle risorse di valore elevato con secure admin workstation](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Modalità utente isolato in Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolamento di processi in modalità utente e delle funzionalità in Windows 10 con Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Nei processi e funzionalità in modalità utente isolato di Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Prevenzione dei furti di credenziali utilizzando il Windows 10 modalità utente isolato (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Consentendo la convalida KDC ristretta in Kerberos di Windows](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novità dell'autenticazione Kerberos per Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Verifica del meccanismo di autenticazione di dominio Active Directory nella Guida dettagliata di Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Trusted Platform Module](C:\sd\docs\p_ent_keep_secure\p_ent_keep_secure\trusted_platform_module_technology_overview.xml)
