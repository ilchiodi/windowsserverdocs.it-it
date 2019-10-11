---
title: Opzioni di Slmgr.vbs per ottenere informazioni sull'attivazione dei contratti multilicenza
description: Questo articolo elenca le opzioni disponibili per lo script Slmg.vbs e descrive come usarle
TOCTitle: Slmgr.vbs Options
ms.date: 09/24/2019
ms.technology: server-general
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: 3b1ce54fe1e1e855d605aba58a0f0e37f0324469
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963067"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>Opzioni di Slmgr.vbs per ottenere informazioni sull'attivazione dei contratti multilicenza

Questo articolo illustra la sintassi dello script Slmgr.vbs e include tabelle con le descrizioni di ogni opzione della riga di comando.

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> In questo articolo le parentesi quadre \[] racchiudono gli argomenti facoltativi e le parentesi uncinate \<> racchiudono i segnaposto. Quando digiti queste istruzioni, devi omettere le parentesi quadre e sostituire i segnaposto con i valori corrispondenti.

> [!NOTE]
> Per informazioni su altri prodotti software per cui viene usata l'attivazione dei contratti multilicenza, consulta la documentazione specifica per tali applicazioni.

## <a name="using-slmgr-on-remote-computers"></a>Uso di Slmgr sui computer remoti

Per gestire i client remoti usare lo Strumento di gestione dell'attivazione dei contratti multilicenza versione 1.2 o successiva oppure creare script WMI personalizzati compatibili con le differenze tra una piattaforma e l'altra. Per altre informazioni sulle proprietà e sui metodi WMI per l'attivazione dei contratti multilicenza, vedi [Proprietà e metodi WMI per l'attivazione dei contratti multilicenza](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502536(v=ws.11)).

> [!IMPORTANT]
> A causa delle modifiche a WMI introdotte in Windows 7 and Windows Server 2008 R2, lo script Slmgr.vbs non è progettato per funzionare tra piattaforme differenti. L'uso di Slmgr.vbs per gestire un sistema Windows 7 o Windows Server 2008 R2 dal sistema operativo Windows Vista® non è supportato. Il tentativo di gestire un sistema meno recente da Windows 7 o Windows Server 2008 R2 genererà un errore specifico di mancata corrispondenza della versione. Ad esempio, l'esecuzione di **cscript slmgr.vbs \<nome\_computer\_vista\> /dlv** produce l'output seguente:
>  
>> Microsoft (R) Windows Script Host Version 5.8 Copyright (C) Microsoft Corporation. Tutti i diritti sono riservati.
>>  
>> Il computer remoto non supporta questa versione di SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>Opzioni generali di Slmgr.vbs

|Opzione |Descrizione |
| - | - |
|\[\<NomeComputer\>] |Nome di un computer remoto (il nome predefinito è computer locale) |
|\[\<Utente\>] |Account con il privilegio necessario sul computer remoto |
|\[\<Password\>] |Password per l'account con i privilegi necessari sul computer remoto |

## <a name="global-options"></a>Opzioni globali

|Opzione |Descrizione |
| - | - |
|\/ipk&nbsp;&lt;CodiceProductKey&gt; |Prova a installare un codice Product Key 5x5. Viene confermata la validità del codice Product Key fornito dal parametro, che è dunque applicabile al sistema operativo installato.<br />In caso contrario, verrà restituito un errore.<br />Se ritenuto valido e applicabile, il codice verrà installato. Se il codice è già installato verrà sostituito automaticamente.<br />Per prevenire l'instabilità nel servizio licenze, è consigliabile riavviare il sistema oppure il servizio di protezione software.<br />Questa operazione deve essere eseguita da una finestra del prompt dei comandi con privilegi elevati oppure il valore del Registro di sistema relativo alle operazioni per utente standard deve essere impostato in modo da consentire agli utenti non privilegiati un ulteriore accesso al servizio di protezione software. |
|/ato&nbsp;\[\<ID&nbsp;attivazione\>] |Per le edizioni definitive e i sistemi di contratti multilicenza con un codice Product Key dell'host del Servizio di gestione delle chiavi o un codice ad attivazione multipla (MAK) installato, **/ato** chiede a Windows di provare a eseguire l'attivazione online.<br />Per i sistemi con un codice Product Key per contratti multilicenza generico (GVLK, Generic Volume License Key) installato, viene effettuato un tentativo di attivazione con il Servizio di gestione delle chiavi. I sistemi impostati in modo da sospendere i tentativi di attivazione con il Servizio di gestione delle chiavi ( **/stao**) provano comunque ad eseguire l'attivazione con tale servizio quando viene usata l'opzione **/ato**.<br />**Nota:** a partire da Windows 8 (e Windows Server 2012), l'opzione **/stao** è deprecata. In alternativa è disponibile l'opzione **/act-type**.<br />Il parametro \<**ID attivazione**\> consente di espandere il supporto di **/ato** in modo da identificare un'edizione di Windows installata nel computer. L'impostazione del parametro \<**ID attivazione**\> consente di isolare gli effetti dell'opzione all'edizione associata all'ID di attivazione. Esegui **slmgr.vbs /dlv all** per ottenere gli ID di attivazione per la versione installata di Windows. Se devono essere supportate altre applicazioni, vedi le indicazioni fornite dall'applicazione specifica per altre istruzioni.<br />L'attivazione con il servizio di gestione delle chiavi non richiede privilegi elevati. Tuttavia, l'attivazione online richiede privilegi elevati, altrimenti sarà necessario impostare il valore del registro di sistema relativo alle operazioni per utente standard in modo da consentire agli utenti non privilegiati un ulteriore accesso al servizio di protezione software. |
|\/dli&nbsp;\[<ID&nbsp;attivazione\>&nbsp;\|&nbsp;All\] |Visualizzare informazioni sulla licenza.<br />Per impostazione predefinita, **/dli** visualizza le informazioni sulla licenza per l'edizione di Windows attiva installata. L'impostazione del parametro \<**ID attivazione**\> consente di visualizzare le informazioni sulla licenza per l'edizione specificata associata all'ID di attivazione. L'impostazione del parametro **All** consente di visualizzare le informazioni sulla licenza per tutti i prodotti installati applicabili.<br />Questa operazione non richiede privilegi elevati. |
|\/dlv&nbsp;\[<ID&nbsp;attivazione\>&nbsp;\|&nbsp;All\] |Visualizzare informazioni dettagliate sulla licenza.<br />Per impostazione predefinita, **/dlv** visualizza le informazioni sulla licenza per il sistema operativo installato. L'impostazione del parametro \<**ID attivazione**\> consente di visualizzare le informazioni sulla licenza per l'edizione specificata associata all'ID di attivazione. L'impostazione del parametro **All** consente di visualizzare le informazioni sulla licenza per tutti i prodotti installati applicabili.<br />Questa operazione non richiede privilegi elevati. |
|\/xpr \[\<ID&nbsp;attivazione\>] |Consente di visualizzare la data di scadenza dell'attivazione per il prodotto. Per impostazione predefinita, si riferisce all'edizione corrente di Windows ed è utile principalmente per i client KMS, perché l'attivazione MAK e definitiva è perpetua.<br />L'impostazione del parametro \<**ID attivazione**\> consente di visualizzare la data di scadenza dell'attivazione dell'edizione specificata associata all'ID di attivazione. Questa operazione non richiede privilegi elevati. |

## <a name="advanced-options"></a>Opzioni avanzate

|Opzione |Descrizione |
| - | - |
|\/cpky |Alcune operazioni di manutenzione richiedono la disponibilità del codice Product Key nel Registro di sistema durante le operazioni di Configurazione guidata. L'opzione **/cpky** consente di rimuovere il codice Product Key dal Registro di sistema per prevenirne il furto da parte del malware.<br />Per le installazioni definitive che distribuiscono i codici, è consigliabile eseguire questa opzione. L'opzione non è obbligatoria per MAK e i codici Product Key host del servizio di gestione delle chiavi, perché è il comportamento predefinito per quei codici. Questa opzione è necessaria solo per altri tipi di codici il cui comportamento predefinito non è quello di cancellare il codice dal Registro di sistema.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/ilc&nbsp;&lt;file_licenza&gt; |L'opzione consente di installare il file di licenza specificato dal parametro obbligatorio. È possibile installare queste licenze come misura di risoluzione dei problemi, per supportare l'attivazione basata su token oppure come parte di un'installazione manuale di un'applicazione on-boarded.<br />Le licenze non vengono convalidate durante questo processo: La convalida delle licenze esula dall'ambito di Slmgr.vbs, ma viene invece gestita dal servizio di protezione software al runtime.<br />Questa operazione deve essere eseguita da una finestra del prompt dei comandi con privilegi elevati oppure il valore del Registro di sistema relativo alle **operazioni per utente standard** deve essere impostato in modo da consentire agli utenti non privilegiati un ulteriore accesso al servizio di protezione software. |
|\/rilc |Questa opzione consente di reinstallare tutte le licenze memorizzate nei token %SystemRoot%\system32\oem e %SystemRoot%\System32\spp\. Si tratta di copie valide archiviate durante l'installazione.<br />Qualsiasi licenza corrispondente nell'Archivio dati attendibile verrà sostituita. Eventuali licenze aggiuntive (ad esempio le licenze di pubblicazione di un'autorità attendibile o le licenze per le applicazioni) non saranno interessate.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati oppure il valore del Registro di sistema relativo alle **operazioni per utente standard** deve essere impostato in modo da consentire agli utenti non privilegiati un ulteriore accesso al servizio di protezione software. |
|\/rearm |Questa opzione consente di reimpostare i timer di attivazione. Il processo **/rearm** viene chiamato anche da **sysprep /generalize**.<br />Questa operazione non ha alcun effetto se la voce del Registro di sistema **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm** è impostata su **1**. Per informazioni dettagliate su questa voce del Registro di sistema, vedi [Impostazioni del Registro di sistema per l'attivazione dei contratti multilicenza](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502532(v=ws.11)).<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati oppure il valore del Registro di sistema relativo alle **operazioni per utente standard** deve essere impostato in modo da consentire agli utenti non privilegiati un ulteriore accesso al servizio di protezione software. |
|\/rearm-app &lt;ID&nbsp;applicazione&gt; |Reimposta lo stato della licenza dell'app specificata. |
|\/rearm-sku &lt;ID&nbsp;applicazione&gt; |Reimposta lo stato della licenza della SKU specificata. |
|\/upk&nbsp;\[&lt;ID&nbsp;applicazione&gt;] |Questa opzione consente di disinstallare il codice Product Key della corrente edizione di Windows. Dopo un riavvio, il sistema sarà nello stato senza licenza a meno che non venga installato un nuovo codice Product Key.<br />Facoltativamente, puoi usare il parametro \<**ID attivazione**\> per specificare un prodotto installato differente.<br />Questa operazione deve essere eseguita da una finestra del prompt dei comandi con privilegi elevati. |
|\/dti&nbsp;\[\<ID&nbsp;attivazione\>] |Consente di visualizzare l'ID di installazione per l'attivazione offline. |
|\/atp &lt;ID&nbsp;conferma&gt; |Attiva il prodotto con l'ID di conferma specificato dall'utente. |

## <a name="kms-client-options"></a>opzioni del client KMS

|Opzione |Descrizione |
| - | - |
|\/skms \<Nome\[:Port]&nbsp;\|&nbsp;\:&nbsp;porta\> \[\<ID&nbsp;attivazione\>] |Questa opzione consente di specificare il nome e, facoltativamente, la porta del computer host del servizio di gestione delle chiavi da contattare. L'impostazione di questo valore consente di disabilitare il rilevamento automatico dell'host del servizio di gestione delle chiavi.<br />Se l'host del Servizio di gestione delle chiavi usa solo la versione del protocollo IP 6 (IPv6), l'indirizzo deve essere specificato nel formato \<nomehost\>:\<porta\>. Gli indirizzi IPv6 contengono il carattere dei due punti (:), che non viene analizzato in maniera corretta dallo script Slmgr.vbs.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/skms-domain&nbsp;&lt;FQDN&gt; \[\<ID&nbsp;attivazione\>] |Consente di impostare il dominio DNS specifico in cui è possibile trovare tutti i record SRV del Servizio di gestione delle chiavi. Questa impostazione non ha alcun effetto se lo specifico host del Servizio di gestione delle chiavi è impostato con l'opzione **/skms**. Usare questa opzione, in special modo negli ambienti degli spazi dei nomi indipendenti, per forzare il Servizio di gestione delle chiavi a ignorare l'elenco di ricerca dei suffissi DNS e cercare invece i record degli host del servizio di gestione delle chiavi nel dominio DNS specificato. |
|\/ckms&nbsp;\[\<ID&nbsp;attivazione\>] |Questa opzione consente di rimuovere il nome, l'indirizzo e la porta dell'host del servizio di gestione delle chiavi dal Registro di sistema e di ripristinare il comportamento di individuazione automatica del Servizio di gestione delle chiavi.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/skhc |Questa opzione consente di abilitare la memorizzazione nella cache dell'host del Servizio di gestione delle chiavi (impostazione predefinita). Dopo che il client ha individuato un host del Servizio di gestione delle chiavi attivo, questa impostazione impedisce che la priorità e il peso del DNS (Domain Name System) influiscano su altre comunicazioni con l'host. Se il sistema non riesce più a contattare l'host del Servizio di gestione delle chiavi attivo, il client prova a individuare un nuovo host.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/ckhc |Questa opzione consente di disabilitare la memorizzazione nella cache dell'host del servizio di gestione delle chiavi. Questa impostazione indica al client di usare l'individuazione automatica del DNS ogni volta che tenta l'attivazione con il Servizio di gestione delle chiavi (consigliata quando vengono usati i parametri priority e weight).<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |

## <a name="kms-host-configuration-options"></a>Opzioni di configurazione dell'host del Servizio di gestione delle chiavi

|Opzione |Descrizione |
| - | - |
|\/sai&nbsp;&lt;Intervallo&gt; |Questa opzione consente di impostare l'intervallo in minuti tra i tentativi di connessione al Servizio di gestione delle chiavi per i client disattivati. L'intervallo di attivazione deve essere compreso tra 15 minuti e 30 giorni, anche se è consigliabile usare il valore predefinito (due ore).<br />Il client del Servizio di gestione delle chiavi seleziona inizialmente questo intervallo dal Registro di sistema, ma passa all'impostazione del Servizio di gestione delle chiavi dopo aver ricevuto la prima risposta del servizio.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/sri &lt;Intervallo&gt; |Questa opzione consente di impostare l'intervallo di rinnovo in minuti tra i tentativi di connessione al Servizio di gestione delle chiavi per i client attivati. L'intervallo di rinnovo deve essere compreso tra 15 minuti e 30 giorni. Questa opzione viene impostata inizialmente sia sul lato server che sul lato client del Servizio di gestione delle chiavi. Il valore predefinito è 10.080 minuti (7 giorni).<br />Il client del Servizio di gestione delle chiavi seleziona inizialmente questo intervallo dal Registro di sistema, ma passa all'impostazione del Servizio di gestione delle chiavi dopo aver ricevuto la prima risposta del servizio.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/sprt&nbsp;&lt;Porta&gt; |Questa opzione consente di impostare la porta sulla quale l'host del servizio di gestione delle chiavi è in ascolto per le richieste di attivazione dei client. La porta TCP predefinita è 1688.<br />Questa operazione deve essere eseguita da una finestra del prompt dei comandi con privilegi elevati. |
|\/sdns |Consente di abilitare la pubblicazione del DNS da parte dell'host del servizio di gestione delle chiavi (impostazione predefinita).<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/cdns |Consente di disabilitare la pubblicazione del DNS da parte dell'host del servizio di gestione delle chiavi.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/spri |Consente di impostare la priorità del Servizio di gestione delle chiavi su normale (impostazione predefinita).<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/cpri |Consente di impostare la priorità del Servizio di gestione delle chiavi su bassa.<br />Usare questa opzione per ridurre la contesa da parte del Servizio di gestione delle chiavi in un ambiente con host condivisi. Tieni presente che ciò determinare la scadenza del Servizio di gestione delle chiavi, a seconda delle altre applicazioni o dei ruoli del server attivi. Usare con cautela.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati. |
|\/act-type \[\<Tipo-Attivazione\>] \[\<ID&nbsp;attivazione\>] |Questa opzione consente di impostare un valore nel Registro di sistema che limita l'attivazione di contratti multilicenza a un solo tipo. Il tipo di attivazione **1** limita l'attivazione solo ad Active Directory; il tipo **2** la limita all'attivazione con il servizio di gestione delle chiavi; il tipo **3** all'attivazione basata su token. L'opzione **0** consente qualsiasi tipo di attivazione ed è il valore predefinito. |

## <a name="token-based-activation-configuration-options"></a>Opzioni di configurazione dell'attivazione basata su token

|Opzione |Descrizione |
| - | - |
|/lil |Consente di elencare le licenze di pubblicazione basate su token installate. |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |Consente di rimuovere una licenza di pubblicazione basata su token installata.<br />Questa operazione deve essere eseguita da una finestra del prompt dei comandi con privilegi elevati. |
|\/stao |Consente di impostare il flag **Token-based Activation Only**, disabilitando l'attivazione con il servizio di gestione delle chiavi.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati.<br />Questa opzione è stata rimossa in Windows Server 2012 R2 e Windows 8.1. Usare invece l'opzione **/act–type**. |
|\/ctao |Consente di cancellare il flag per la **sola attivazione basata su token** (impostazione predefinita), abilitando l'attivazione automatica con il servizio di gestione delle chiavi.<br />Questa operazione deve essere eseguita in una finestra del prompt dei comandi con privilegi elevati.<br />Questa opzione è stata rimossa in Windows Server 2012 R2 e Windows 8.1. In alternativa è disponibile l'opzione **/act–type**</strong>. |
|\/ltc |Consente di elencare i certificati di attivazione basati su token che possono attivare il software installato. |
|\/fta &lt;Identificazione&nbsp;certificato&gt; \[&lt;PIN&gt;] |Consente di forzare l'attivazione basata su token usando il certificato identificato. Il PIN facoltativo viene fornito per sbloccare la chiave privata senza un prompt del PIN se vengono usati certificati protetti dall'hardware, ad esempio le smart card. |

## <a name="active-directory-based-activation-configuration-options"></a>Opzioni di configurazione dell'attivazione basata su Active Directory

|Opzione |Descrizione |
| - | - |
|\/ad-activation-online &lt;Codice Product&nbsp;Key&gt; \[\<Nome&nbsp;oggetto&nbsp;attivazione\>] |Consente di raccogliere i dati di Active Directory e avvia l'attivazione della foresta di Active Directory usando le credenziali eseguite dal prompt dei comandi. Non è richiesto l'accesso di amministratore locale, ma è obbligatorio l'accesso in lettura/scrittura al contenitore dell'oggetto attivazione nel dominio radice della foresta. |
|\/ad-activation-get-IID &lt;Codice Product&nbsp;Key&gt; |Questa opzione consente di avviare l'attivazione della foresta di Active Directory in modalità di rete. L'output è l'ID di installazione (IID) che può essere usato per attivare la foresta con linea telefonica se la connettività Internet non è disponibile. Dopo aver fornito l'IID nella chiamata telefonica di attivazione, viene restituito un CID che verrà usato per completare l'attivazione. |
|\/ad-activation-apply-cid &lt;Codice Product&nbsp;Key&gt; &lt;ID&nbsp;conferma&gt; \[\<Nome&nbsp;oggetto&nbsp;attivazione>] |Quando usi questa opzione, per completare l'attivazione immetti il CID fornito dalla chiamata telefonica di attivazione |
|\[/name: &lt;Nome_AO&gt;] |Facoltativamente, è possibile aggiungere l'opzione **/name** a uno qualsiasi di questi comandi per specificare un nome per l'oggetto attivazione archiviato in Active Directory. Il nome non deve superare i 40 caratteri Unicode. Per definire in modo esplicito la stringa del nome, usa le virgolette doppie.<br />In Windows Server 2012 R2 e Windows 8.1, puoi aggiungere il nome direttamente dopo **/ad-activation-online &lt;Codice Product&nbsp;Key&gt;** e **/ad-activation-apply-cid** senza dover usare l'opzione **/name**. |
|\/ao-list |Consente di visualizzare tutti gli oggetti attivazione disponibili sul computer locale. |
|\/del-ao &lt;DN_AO&gt;<br />\/del-ao &lt;RDN_AO&gt; |Consente di eliminare l'oggetto attivazione specificato dalla foresta. |

## <a name="see-also"></a>Vedi anche

- [Guida di riferimento tecnico per l'attivazione dei contratti multilicenza](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529%28v%3dws.11%29)
- [Panoramica dell'attivazione di contratti multilicenza](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612%28v%3dws.11%29)

