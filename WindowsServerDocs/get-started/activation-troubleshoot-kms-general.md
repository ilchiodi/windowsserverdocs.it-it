---
title: Linee guida per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi
description: Questo articolo include informazioni sul Servizio di gestione delle chiavi e suggerisce gli strumenti e gli approcci da adottare per la risoluzione dei problemi di attivazione
ms.topic: troubleshooting
ms.date: 9/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: fc673d2c3e1404dbd750d4c0ef05ec6db50017aa
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963077"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>Linee guida per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi (KMS)

Come parte del processo di distribuzione, molti clienti aziendali configurano il Servizio di gestione delle chiavi (KMS) per abilitare l'attivazione di Windows nel proprio ambiente. Si tratta di un processo semplice di configurazione dell'host del Servizio di gestione delle chiavi, dopo il quale i client del Servizio di gestione delle chiavi individuano l'host e tentano di eseguire l'attivazione in modo autonomo. Ma cosa succede se il processo non funziona? Che cosa fare in questo caso? Questo articolo illustra le risorse necessarie per risolvere questo problema. Per altre informazioni sulle voci del registro eventi e sullo script Slmgr.vbs, vedi la [Guida di riferimento tecnico per l'attivazione dei contratti multilicenza](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529(v=ws.11)).

## <a name="kms-overview"></a>Panoramica del Servizio di gestione delle chiavi

Iniziamo con un rapido aggiornamento sull'attivazione tramite il Servizio di gestione delle chiavi. Questo servizio è basato su un modello client-server. Concettualmente, è simile al DHCP. Invece di distribuire gli indirizzi IP ai client su richiesta, il Servizio di gestione delle chiavi abilita l'attivazione del prodotto. Il Servizio di gestione delle chiavi è anche un modello di rinnovo, in cui i client eseguono tentativi di riattivazione a intervalli regolari. Il Servizio di gestione delle chiavi prevede due ruoli: l'*host* e il *client*.

- L'**host** esegue il servizio di attivazione e abilita l'attivazione nell'ambiente. Per configurare un host, devi installare un codice del Servizio di gestione delle chiavi dal Volume License Service Center (VLSC) e quindi attivare il servizio.
- Il **client del Servizio di gestione delle chiavi** è il sistema operativo Windows che è distribuito nell'ambiente e deve essere attivato. I client possono eseguire qualsiasi edizione di Windows che usa l'attivazione dei contratti multilicenza. I client del Servizio di gestione delle chiavi vengono forniti con un codice preinstallato, denominato GVLK (Generic Volume License Key) o codice di configurazione del client del Servizio di gestione delle chiavi. La presenza del codice GVLK è ciò che identifica un client del Servizio di gestione delle chiavi. I client del Servizio di gestione delle chiavi usano i record SRV DNS (_vlmcs._tcp) per identificare l'host e quindi provano automaticamente a individuare e usare questo servizio per eseguire l'attivazione in modo autonomo. Durante il periodo di tolleranza predefinito di 30 giorni, i tentativi di attivazione vengono eseguiti ogni due ore. Dopo l'attivazione, i client del Servizio di gestione delle chiavi provano a rinnovare l'attivazione ogni sette giorni.

Dal punto di vista della risoluzione dei problemi, può essere necessario esaminare entrambi i lati (host e client) per determinare cosa sta accadendo.

## <a name="kms-host"></a>Host del Servizio di gestione delle chiavi

Sull'host del Servizio di gestione delle chiavi devono essere esaminate due aree. Prima di tutto, verifica lo stato del servizio di gestione delle licenze software dell'host. In secondo luogo, controlla il Visualizzatore eventi per determinare gli eventi correlati alla gestione delle licenze o all'attivazione.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs e il servizio di gestione delle licenze software

Per visualizzare l'output dettagliato del servizio di gestione delle licenze software, apri una finestra del prompt dei comandi con privilegi elevati e digita **slmgr.vbs /dlv** al prompt dei comandi. Lo screeshot seguente mostra i risultati di questo comando su uno degli host del Servizio di gestione delle chiavi in Microsoft.

![Output di Slmgr sull'host del Servizio di gestione delle chiavi](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

Di seguito sono riportati i campi più importanti per la risoluzione dei problemi. Le informazioni cercate possono variare a seconda del problema da risolvere.

- **Informazioni sulla versione**. Nella parte superiore dell'output di **slmgr.vbs /dlv** è riportata la versione del servizio di gestione delle licenze software. Questa informazione può essere utile per determinare se è installata la versione corrente del servizio. Ad esempio, gli aggiornamenti al Servizio di gestione delle chiavi in Windows Server 2003 supportano codici di host differenti. In base a questi dati puoi valutare se la versione installata è quella corrente e se supporta il codice dell'host del Servizio di gestione delle chiavi che stai provando a installare. Per altre informazioni su questi aggiornamenti, vedi [È disponibile un aggiornamento per Windows Vista e Windows Server 2008 per estendere il supporto dell'attivazione del Servizio di gestione delle chiavi per Windows 7 e Windows Server 2008 R2](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t).
- **Nome**. Indica l'edizione di Windows installata nel sistema host del Servizio di gestione delle chiavi. Questa informazione può essere importante in caso di problemi di aggiunta o modifica del codice dell'host del Servizio di gestione delle chiavi (ad esempio, per verificare che il codice sia supportato nella specifica edizione del sistema operativo).
- **Descrizione**. Consente di identificare il codice installato. Questo campo è utile per verificare quale codice è stato usato per attivare il servizio e se il codice è corretto per i client del Servizio di gestione delle chiavi distribuiti.
- **License Status**. Indica lo stato del sistema host del Servizio di gestione delle chiavi. Il valore deve essere **Licensed**. Qualsiasi altro valore indica che si è verificato un errore e può essere necessario riattivare l'host.
- **Current Count**. Visualizza un conteggio compreso tra **0** e **50**. Si tratta di un conteggio cumulativo (per più sistemi operativi) che indica il numero di sistemi validi che hanno tentato l'attivazione entro un periodo di 30 giorni.  
  
  Se il conteggio è **0**, significa che il servizio è stato attivato di recente oppure che all'host del Servizio di gestione delle chiavi non sono connessi client validi.  
  
  Il conteggio non può superare **50**, indipendentemente dal numero di sistemi validi presenti nell'ambiente. Questo avviene perché il conteggio è impostato in modo da memorizzare nella cache solo il doppio del limite massimo dei criteri di licenza restituiti da un client del Servizio di gestione delle chiavi. Il limite massimo dei criteri è attualmente impostato dal sistema operativo client Windows, che richiede un conteggio di almeno **25** sull'host del Servizio di gestione delle chiavi per essere attivato in modo autonomo. Di conseguenza, il conteggio massimo sull'host del Servizio di gestione delle chiavi è pari a 2x25, ovvero 50. Tieni presente che negli ambienti che contengono solo client del Servizio di gestione delle chiavi con Windows Server, il numero massimo sull'host del Servizio di gestione delle chiavi è **10**. Questo è dovuto al fatto che la soglia per le edizioni di Windows Server è **5** (2x5, ovvero 10).  
  
  Un problema comune correlato al conteggio riguarda il caso in cui l'ambiente include un host del Servizio di gestione delle chiavi attivato e un numero sufficiente di client, ma il conteggio rimane fermo a uno. Il problema principale è dovuto al fatto che l'immagine del client distribuita non è stata configurata correttamente (**sysprep /generalize**) e che i sistemi non hanno ID computer client univoci (CMID). Per altre informazioni, vedi [Client del Servizio di gestione delle chiavi](#kms-client) e [Il conteggio corrente del Servizio di gestione delle chiavi rimane invariato quando si aggiungono alla rete nuovi computer client basati su Windows Vista o Windows 7](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista). Uno dei tecnici di escalation del supporto ha anche pubblicato informazioni su questo problema nel post di blog [KMS Host Client Count not Increasing Due to Duplicate CMID'S](https://blogs.technet.microsoft.com/askcore/2009/10/16/kms-host-client-count-not-increasing-due-to-duplicate-cmids/) (Il conteggio dei client dell'host del Servizio di gestione delle chiavi non aumenta a causa di ID computer client duplicati).  
  
  Un altro motivo per cui il conteggio potrebbe non aumentare è dato dalla presenza di un numero eccessivo di host del Servizio di gestione delle chiavi nell'ambiente e dalla distribuzione dei client tra tutti gli host.
- **Listening on Port**. Per le comunicazioni con il Servizio di gestione delle chiavi viene usato un meccanismo RPC anonimo. Per impostazione predefinita, i client usano la porta TCP 1688 per connettersi all'host del Servizio di gestione delle chiavi. Verifica che questa porta sia aperta tra i client e l'host del Servizio di gestione delle chiavi. Puoi modificare o configurare la porta sull'host del Servizio di gestione delle chiavi. Durante le comunicazioni, l'host del Servizio di gestione delle chiavi invia la designazione della porta ai client. Se modifichi la porta su un client del Servizio di gestione delle chiavi, la designazione della porta viene sovrascritta quando il client contatta l'host.

Spesso viene chiesto di spiegare la sezione dell'output di **slmgr.vbs /dlv** relativa alle richieste cumulative. In genere questi dati non sono utili per la risoluzione dei problemi. L'host del Servizio di gestione delle chiavi gestisce un record costantemente aggiornato dello stato di ogni client del Servizio di gestione delle chiavi che esegue un tentativo di attivazione o riattivazione. Le richieste non riuscite indicano i client non supportati dall'host del Servizio di gestione delle chiavi. Se, ad esempio, un client del Servizio di gestione delle chiavi di Windows 7 tenta di eseguire l'attivazione su un host attivato usando un codice del Servizio di gestione delle chiavi di Windows Vista, l'attivazione non riesce. Le righe "Requests with License Status" descrivono tutti i possibili stati delle licenze, passati e presenti. Dal punto di vista della risoluzione dei problemi, questi dati sono rilevanti solo se il conteggio non aumenta come previsto. In tal caso, il numero di richieste non riuscite dovrebbe aumentare. Questo indica che devi controllare il codice Product Key usato per attivare il sistema host del Servizio di gestione delle chiavi. Tieni inoltre presente che i valori delle richieste cumulative vengono reimpostati solo se reinstalli il sistema host del Servizio di gestione delle chiavi.

### <a name="useful-kms-host-events"></a>Eventi utili dell'host del Servizio di gestione delle chiavi

#### <a name="event-id-12290"></a>ID evento 12290

L'host del Servizio di gestione delle chiavi registra l'ID evento 12290 quando un client contatta l'host per l'attivazione. L'ID evento 12290 fornisce una quantità notevole di informazioni che puoi usare per determinare il tipo di client che ha contattato l'host e il motivo per cui si è verificato un errore. Il segmento seguente relativo a un ID evento 12290 deriva dal registro eventi Key Management Service dell'host del Servizio di gestione delle chiavi.  

![Evento 12290 del Servizio di gestione delle chiavi](./media/ee939272.kms_12290_event(en-us,technet.10).png)

Nei dettagli dell'evento sono incluse le informazioni seguenti:

- **Conteggio minimo necessario per l'attivazione**. Il client del Servizio di gestione delle chiavi segnala che il conteggio restituito dall'host deve essere **5** per consentire l'attivazione. Ciò significa che si tratta di un sistema operativo Windows Server, anche se non è indicata un'edizione specifica. Se i client non vengono attivati, verifica che il conteggio sull'host sia sufficiente.
- **Client Machine ID (CMID)** . Questo è un valore univoco in ogni sistema. Se il valore non è univoco, significa che un'immagine non è stata preparata correttamente per la distribuzione (**sysprep /generalize**). Questo problema si manifesta nell'host del Servizio di gestione delle chiavi quando il conteggio rimane invariato, anche se è presente un numero sufficiente di client nell'ambiente. Per altre informazioni, vedi [Il conteggio corrente del Servizio di gestione delle chiavi rimane invariato quando si aggiungono alla rete nuovi computer client basati su Windows Vista o Windows 7](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **Stato della licenza e data/ora di scadenza dello stato**. Questo è lo stato della licenza corrente del client. Può essere utile per distinguere un client che esegue il primo tentativo di attivazione da uno che prova a eseguire la riattivazione. La voce relativa alla data e all'ora indica il tempo in cui lo stato del client rimarrà invariato, se non vi sono modifiche.

Se durante il tentativo di risolvere un problema relativo a un client non trovi un ID evento 12290 corrispondente sull'host del Servizio di gestione delle chiavi, significa che il client non si connette all'host. Di seguito sono riportati alcuni motivi per cui può non essere presente una voce relativa all'ID evento 12290:

- Si è verificata un'interruzione della rete.
- L'host non viene risolto o non è registrato nel DNS.
- Il firewall sta bloccando la porta TCP 1688.
   La porta potrebbe essere bloccata in molte posizioni all'interno dell'ambiente, incluso il sistema host del Servizio di gestione delle chiavi. Per impostazione predefinita, l'host del Servizio di gestione delle chiavi ha un'eccezione di firewall per il servizio, ma non è abilitata automaticamente. È necessario abilitare l'eccezione.
- Il registro eventi è pieno.

I client del Servizio di gestione delle chiavi registrano due eventi corrispondenti, con ID 12288 e 12289. Per informazioni su questi eventi, vedi la sezione [Client del Servizio di gestione delle chiavi](#kms-client).

#### <a name="event-id-12293"></a>ID evento 12293

Un altro evento pertinente da cercare nell'host del Servizio di gestione delle chiavi è quello identificato dall'ID 12293. Questo evento indica che l'host non ha pubblicato i record necessari nel DNS. È noto che questa situazione può causare errori. Si tratta quindi di una verifica da effettuare *dopo* aver configurato l'host e *prima* di distribuire i client. Per altre informazioni sui problemi correlati al DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="kms-client"></a>Client del Servizio di gestione delle chiavi

Per la risoluzione dei problemi di attivazione dei client vengono usati gli stessi strumenti dell'host, ovvero Slmgr e il Visualizzatore eventi.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs e il servizio di gestione delle licenze software

Per visualizzare l'output dettagliato del servizio di gestione delle licenze software, apri una finestra del prompt dei comandi con privilegi elevati e digita **slmgr.vbs /dlv** al prompt dei comandi. Lo screenshot seguente mostra i risultati di questo comando su uno degli host del Servizio di gestione delle chiavi in Microsoft.

![Output di Slmgr sul client del Servizio di gestione delle chiavi](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

L'elenco seguente include i campi più importanti per la risoluzione dei problemi. Le informazioni cercate possono variare a seconda del problema da risolvere.

- **Nome**. Indica l'edizione di Windows installata nel sistema client del Servizio di gestione delle chiavi. Usa questo valore per verificare che la versione di Windows che stai tentando di attivare possa usare il Servizio di gestione delle chiavi. Ad esempio, il supporto tecnico ha rilevato eventi imprevisti in cui i clienti hanno tentato di installare il codice di configurazione del client del Servizio di gestione delle chiavi in un'edizione di Windows in cui non viene usata l'attivazione dei contratti multilicenza, ad esempio Windows Vista Ultimate.
- **Descrizione**. Questo valore mostra il codice installato. VOLUME_KMSCLIENT indica che il codice di configurazione del client del Servizio di gestione delle chiavi (o GVLK) è installato (configurazione predefinita per i supporti con contratti multilicenza) e che il sistema tenta automaticamente di eseguire l'attivazione usando un host del Servizio di gestione delle chiavi. Se vengono visualizzati altri elementi, ad esempio un codice ad attivazione multipla (MAK), dovrai reinstallare il codice GVLK per configurare il sistema come client del Servizio di gestione delle chiavi. Il codice può essere installato manualmente con **slmgr.vbs /ipk &lt;*GVLK*&gt;** (come descritto in [Chiavi di configurazione di client del Servizio di gestione delle chiavi](kmsclientkeys.md)) oppure tramite lo Strumento di gestione dell'attivazione dei contratti multilicenza (VAMT, Volume Activation Management Tool). Per altre informazioni su questo strumento, vedi [Documentazione tecnica sullo Strumento di gestione dell'attivazione dei contratti multilicenza (VAMT)](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-management-tool).
- **Partial Product Key**. Come per il campo **Name**, puoi usare questa informazione per determinare se nel computer è installato il codice di configurazione del client del Servizio di gestione delle chiavi corretto, ovvero se il codice corrisponde al sistema operativo installato nel client. Per impostazione predefinita, il codice corretto è presente nei sistemi creati usando i supporti del portale Volume License Service Center (VLSC). In alcuni casi, i clienti possono usare l'attivazione tramite codice ad attivazione multipla (MAK) finché nell'ambiente non è presente un numero sufficiente di sistemi per supportare l'attivazione tramite il Servizio di gestione delle chiavi. Per la transizione dal codice ad attivazione multipla al Sistema di gestione delle chiavi è necessario installare il codice di configurazione del client del servizio. Usa VAMT per installare questo codice e verifica che venga applicato il codice corretto.
- **License Status**. Questo valore mostra lo stato del sistema client del Servizio di gestione delle chiavi. Per un sistema attivato tramite il Servizio di gestione delle chiavi, questo valore deve essere **Licensed**. Qualsiasi altro valore potrebbe indicare che si è verificato un problema. Se, ad esempio, l'host del Servizio di gestione delle chiavi funziona correttamente e il client non viene attivato, ad esempio rimane in uno stato **Grace**, è possibile che si sia verificato un problema che impedisce al client di raggiungere il sistema host, ad esempio un problema del firewall, un'interruzione di rete o qualcosa di simile.
- **Client Machine ID (CMID)** . Ogni client del Servizio di gestione delle chiavi deve avere un CMID univoco. Come indicato nella sezione [Host del Servizio di gestione delle chiavi](#kms-host), un problema comune correlato al conteggio riguarda il caso in cui l'ambiente include un host del Servizio di gestione delle chiavi attivato e un numero sufficiente di client, ma il conteggio rimane fermo a **1**. Per altre informazioni, vedi [Il conteggio corrente del Servizio di gestione delle chiavi rimane invariato quando si aggiungono alla rete nuovi computer client basati su Windows Vista o Windows 7](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **KMS Machine Name from DNS**. Questo valore indica il nome di dominio completo (FQDN) dell'host del Servizio di gestione delle chiavi che il client ha usato correttamente per l'attivazione e la porta TCP usata per le comunicazioni.
- **KMS Host Caching**. Il valore finale indica se la memorizzazione nella cache è abilitata. Per impostazione predefinita, è abilitata. Ciò significa che il client del Servizio di gestione delle chiavi memorizza nella cache il nome host usato per l'attivazione e comunica direttamente con questo host (invece di eseguire query sul DNS) quando è il momento di eseguire la riattivazione. Se il client non riesce a contattare l'host del Servizio di gestione delle chiavi memorizzato nella cache, esegue una query sul DNS per individuare un nuovo host.

### <a name="useful-kms-client-events"></a>Eventi utili del client del Servizio di gestione delle chiavi

#### <a name="event-id-12288-and-event-id-12289"></a>ID evento 12288 e 12289

Quando un client del Servizio di gestione delle chiavi viene attivato o riattivato correttamente, vengono registrati due eventi identificati dagli ID 12288 e 12289. Il segmento seguente relativo a un ID evento 12288 deriva dal registro eventi Key Management Service del client del Servizio di gestione delle chiavi.

![ID evento 12288 del client del Servizio di gestione delle chiavi](./media/ee939272.client_12288(en-us,technet.10).png)

Se viene visualizzato solo l'ID evento 12288 (senza un ID evento 12289 corrispondente), significa che il client del Servizio di gestione delle chiavi non è riuscito a raggiungere l'host del Servizio di gestione delle chiavi, che l'host non ha risposto o che il client non ha ricevuto la risposta. In questo caso, verifica che l'host del Servizio di gestione delle chiavi sia individuabile e che i client possano contattarlo.  

I dati più rilevanti dell'ID evento 12288 sono riportati nella sezione Info. In questa sezione è ad esempio riportato lo stato corrente del client insieme al nome di dominio completo e alla porta TCP usati dal client durante il tentativo di attivazione. Puoi usare il nome di dominio completo per risolvere i casi in cui il conteggio su un host del Servizio di gestione delle chiavi rimane invariato. Se, ad esempio, sono disponibili troppi host del Servizio di gestione delle chiavi per i client (sistemi legittimi o non autorizzati), il conteggio verrà probabilmente distribuito fra tutti gli host.

L'esito negativo di un'attivazione non significa sempre che sul client si è verificato solo l'evento 12288 e non l'evento 12289. Un problema di attivazione o riattivazione non riuscita può essere associato a entrambi gli eventi. In questo caso, devi esaminare il secondo evento per verificare il motivo dell'errore.

![ID evento 12289 del client del Servizio di gestione delle chiavi](./media/ee939272.client_12289(en-us,technet.10).png)

La sezione Info dell'ID evento 12289 include le informazioni seguenti:

- **Activation Flag**. Questo valore indica se l'attivazione ha avuto esito positivo (**1**) o negativo (**0**).
- **Current Count on the KMS Host**. Questo valore riflette il valore del conteggio sull'host del Servizio di gestione delle chiavi quando viene eseguito il tentativo di attivazione del client. Se l'attivazione ha esito negativo, è possibile che il conteggio non sia sufficiente per il sistema operativo del client o che non siano presenti sistemi sufficienti nell'ambiente per la generazione del conteggio.

## <a name="what-does-support-ask-for"></a>Quali informazioni vengono richieste dal supporto tecnico?

Se devi chiamare il supporto tecnico per risolvere un problema di attivazione, in genere ti verranno chieste le informazioni seguenti:

- Output di **Slmgr.vbs /dlv** generato dai sistemi host e client del Servizio di gestione delle chiavi. Indipendentemente dall'uso di wscript o cscript per l'esecuzione del comando, puoi usare CTRL+C per copiare l'output e incollarlo nel Blocco note e quindi inviarlo al contatto del supporto tecnico.
- Registri eventi dell'host (registro Key Management Service) e dei sistemi client (registro Application) del Servizio di gestione delle chiavi

## <a name="see-also"></a>Vedi anche
- [Ask the Core Team: #Activation](https://blogs.technet.microsoft.com/askcore/tag/Activation/) (Blog del team di supporto per Windows Core: tag Activation)


