---
title: Risolvere i codici di errore di attivazione di Windows
description: Informazioni sulla risoluzione dei problemi relativi ai codici degli errori di attivazione
ms.topic: article
ms.date: 9/18/2019
ms.technology: server-general
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 50c50353316db4288f01893125ecd651db63cbb7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826354"
---
# <a name="resolve-windows-activation-error-codes"></a>Risolvere i codici di errore di attivazione di Windows

> **Utenti domestici**  
> Questo articolo è destinato a professionisti IT e agenti di supporto. Se hai bisogno di altre informazioni sui messaggi di errore di attivazione di Windows, vedi [Informazioni sugli errori di attivazione di Windows](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors).  

Questo articolo fornisce informazioni per la risoluzione dei problemi per rispondere ai messaggi di errore che puoi ricevere quando tenti di usare un codice ad attivazione multipla (MAK) o il Servizio di gestione delle chiavi per eseguire l'attivazione di contratti multilicenza in uno o più computer basati su Windows. Cerca il codice di errore nella tabella seguente e quindi seleziona il collegamento per visualizzare altre informazioni sul codice di errore e su come risolverlo.

Per altre informazioni sull'attivazione dei contratti multilicenza, vedi [Pianificare l'attivazione dei contratti multilicenza](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client).

Per altre informazioni sull'attivazione dei contratti multilicenza per le versioni correnti e recenti di Windows, vedi [Attivazione dei contratti multilicenza [client]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10).

Per altre informazioni sull'attivazione dei contratti multilicenza per le versioni precedenti di Windows, vedi l'articolo 929712 della Knowledge Base  [Informazioni sull'attivazione dei contratti multilicenza per Windows Vista, Windows Server 2008, Windows Server 2008 R2 e Windows 7](https://support.microsoft.com/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi).

## <a name="diagnostic-tool"></a>Strumento di diagnostica

L'Assistente di supporto e ripristino di Microsoft semplifica la risoluzione dei problemi relativi all'attivazione mediante il Servizio di gestione delle chiavi di Windows. Scarica lo strumento di diagnostica da [qui](https://aka.ms/SaRA-WindowsActivation).

Questo strumento proverà ad attivare Windows. Se viene restituito un codice di errore di attivazione, verranno visualizzate soluzioni mirate per i codici di errore conosciuti.

Sono supportati i codici di errore seguenti: 0xC004F038, 0xC004F039, 0xC004F041, 0xC004F074, 0xC004C008.

## <a name="summary-of-error-codes"></a>Riepilogo dei codici di errore

|Codice di errore |Messaggio di errore |Tipo di&nbsp;attivazione|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |In questo computer non è in esecuzione una copia autentica di Windows.  |MAK<br />Client del Servizio di gestione delle chiavi |
|[0x80070005](#0x80070005-access-denied) |Accesso negato: l'azione richiesta prevede privilegi elevati. |MAK<br />Client del Servizio di gestione delle chiavi<br />Host del Servizio di gestione delle chiavi |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |Nome DNS inesistente. |Client del Servizio di gestione delle chiavi |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |Codice "Product Key" immesso non valido. Controllare il codice "Product Key" e riprovare. |MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |Server RPC non disponibile. |Client del Servizio di gestione delle chiavi |
|[0x8007232A](#0x8007232a-dns-server-failure) |Errore del server DNS.  |Host del Servizio di gestione delle chiavi  |
|[0x8007232B](#0x8007232b-dns-name-does-not-exist) |Nome DNS inesistente. |Client del Servizio di gestione delle chiavi |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |Nessun record trovato per la query DNS specificata. |Client del Servizio di gestione delle chiavi |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |Nome DNS inesistente.  |Client del Servizio di gestione delle chiavi |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |Server di attivazione: impossibile attivare il computer. |MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |Server di attivazione: il "Product Key" specificato non è valido. |MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |Server di attivazione: il codice "Product Key" specificato è bloccato |MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |Server di attivazione: impossibile utilizzare il codice "Product Key" specificato. |Servizio di gestione delle chiavi |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |Server di attivazione: è stato superato il limite massimo del codice ad attivazione multipla. |MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |Server di attivazione: è stato superato il limite massimo di estensione del codice ad attivazione multipla. |MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |Servizio di protezione software: il periodo di prova è scaduto. |MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |Servizio gestione licenze software: binding ID hardware oltre il livello di tolleranza. |MAK<br />Client del Servizio di gestione delle chiavi<br />Host del Servizio di gestione delle chiavi |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |Servizio di protezione software: codice "Product Key" non disponibile. |MAK<br />Client del Servizio di gestione delle chiavi |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |Servizio di protezione software: il formato dei dati per l'attivazione offline non è corretto. |MAK<br />Client del Servizio di gestione delle chiavi |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |Servizio di protezione software: impossibile attivare il computer con un codice "Product Key" per contratti multilicenza. |Client del Servizio di gestione delle chiavi<br />Host del Servizio di gestione delle chiavi |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |Servizio di protezione software: impossibile attivare il computer. Il conteggio restituito dal Servizio di gestione delle chiavi è insufficiente. Contattare l'amministratore del sistema. |Client del Servizio di gestione delle chiavi |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |Servizio di protezione software: impossibile attivare il computer. Il Servizio di gestione delle chiavi non è abilitato. |Client del Servizio di gestione delle chiavi |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |Servizio di protezione software: il server di gestione delle chiavi non è attivato. Per attivarlo,  |Client del Servizio di gestione delle chiavi |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |Servizio di protezione software: impossibile utilizzare il servizio di gestione delle chiavi specificato. |Client del Servizio di gestione delle chiavi |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |Servizio di protezione software: codice "Product Key" non valido. |MAK<br />Servizio di gestione delle chiavi<br />Client del Servizio di gestione delle chiavi |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |Servizio di protezione software: codice "Product Key" bloccato. |MAK<br />Servizio di gestione delle chiavi |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |Servizio di protezione software: il periodo di tolleranza per copie non autentiche è scaduto. |MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |Servizio di protezione software: l'applicazione è in esecuzione entro il periodo valido per la copia non autentica. |MAK<br />Client del Servizio di gestione delle chiavi |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |Servizio di protezione software: impossibile attivare il computer. Il Servizio di gestione delle chiavi ha determinato che il timestamp della richiesta non è valido.  |Client del Servizio di gestione delle chiavi |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |Servizio di protezione software: impossibile attivare il computer. Impossibile contattare un Servizio di gestione delle chiavi. Per altre informazioni vedere il registro eventi applicazioni.  |Client del Servizio di gestione delle chiavi |

## <a name="causes-and-resolutions"></a>Cause e soluzioni

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21 In questo computer non è in esecuzione una copia autentica di Windows  

#### <a name="possible-cause"></a>Possibile causa

Questo problema può verificarsi per diversi motivi. Il motivo più probabile è dovuto all'installazione di Language Pack (MUI) in computer che eseguono edizioni di Windows non concesse in licenza per Language Pack aggiuntivi.  

> [!NOTE]
> Non si tratta necessariamente di un'indicazione di manomissione. Alcune applicazioni possono installare supporto multilingue anche quando l'edizione di Windows non è concessa in licenza per tali Language Pack.  

Questo problema può verificarsi anche se Windows è stato modificato da malware per consentire l'installazione di altre funzionalità. Questo problema può essere dovuto anche al danneggiamento di alcuni file di sistema.  

#### <a name="resolution"></a>Risoluzione

Per risolvere questo problema, devi reinstallare il sistema operativo.  

### <a name="0x80070005-access-denied"></a>0x80070005 Accesso negato

Il testo completo di questo messaggio di errore è simile al seguente:

> Accesso negato: l'azione richiesta prevede privilegi elevati.

#### <a name="possible-cause"></a>Possibile causa

Controllo dell'account utente impedisce l'esecuzione di processi di attivazione in una finestra del prompt dei comandi senza privilegi elevati.  

#### <a name="resolution"></a>Risoluzione

Esegui **slmgr.vbs** da un prompt dei comandi con privilegi elevati. A tale scopo, fai clic sul pulsante **Start**, fai clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegli **Esegui come amministratore**.  

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b Nome DNS inesistente

#### <a name="possible-cause"></a>Possibile causa

Questo problema può verificarsi se il client del Servizio di gestione delle chiavi non trova i record di risorse SRV del Servizio di gestione delle chiavi in DNS.  

#### <a name="resolution"></a>Risoluzione

Per altre informazioni sulla risoluzione di questi problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490 Il codice Product Key immesso non funziona

Il testo completo di questo messaggio di errore è simile al seguente:
> Il codice Product Key immesso non funziona. Controllare il codice "Product Key" e riprovare.  

#### <a name="possible-cause"></a>Possibile causa

Questo problema si verifica per l'immissione di un codice MAK non valido o a causa di un problema noto in Windows Server 2019.  

#### <a name="resolution"></a>Risoluzione

Per risolvere il problema e attivare il computer, esegui **slmgr -ipk <chiave 5x5>** in un prompt dei comandi con privilegi elevati.

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA Server RPC non disponibile

#### <a name="possible-cause"></a>Possibile causa

Le impostazioni del firewall non sono configurate nell'host del Servizio di gestione delle chiavi oppure i record SRV DNS non sono aggiornati.  

#### <a name="resolution"></a>Risoluzione

Verifica che nell'host del Servizio di gestione delle chiavi sia abilitata l'eccezione del firewall per il Servizio di gestione delle chiavi (porta TCP 1688).

Verifica che i record SRV DNS puntino a un host del Servizio di gestione delle chiavi valido. 

Risolvere i problemi relativi alle connessioni di rete.  

Per altre informazioni sulla risoluzione di questi problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A Errore del server DNS

#### <a name="possible-cause"></a>Possibile causa

Nel sistema si sono verificati problemi di rete o DNS.

#### <a name="resolution"></a>Risoluzione

Risolvere i problemi relativi alla rete e a DNS.  

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B Nome DNS inesistente

#### <a name="possible-cause"></a>Possibile causa

Il client del Servizio di gestione delle chiavi non trova i record di risorse SRV del server del Servizio di gestione delle chiavi in DNS.  

#### <a name="resolution"></a>Risoluzione

Verifica che sia stato installato un host del Servizio di gestione delle chiavi e che sia abilitata la pubblicazione DNS (impostazione predefinita). Se DNS non è disponibile, fai puntare il client del Servizio di gestione delle chiavi all'host del Servizio di gestione delle chiavi usando **slmgr.vbs /skms <*nome_host_Servizio_di_gestione_delle_chiavi*>** .  

Se non disponi di un host del Servizio di gestione delle chiavi, richiedi e installa un codice MAK. Attiva quindi il sistema.

Per altre informazioni sulla risoluzione di questi problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D Nessun record trovato per la query DNS

#### <a name="possible-cause"></a>Possibile causa

Il client del Servizio di gestione delle chiavi non trova record SRV del Servizio di gestione delle chiavi in DNS.

#### <a name="resolution"></a>Risoluzione

Risolvere i problemi relativi alle connessioni di rete e a DNS. Per altre informazioni sulla risoluzione di questi problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328 Nome DNS inesistente

#### <a name="possible-cause"></a>Possibile causa

Questo problema può verificarsi se il client del Servizio di gestione delle chiavi non trova i record di risorse SRV del Servizio di gestione delle chiavi in DNS.

#### <a name="resolution"></a>Risoluzione

Per altre informazioni sulla risoluzione di questi problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100 Server di attivazione: impossibile attivare il computer

#### <a name="possible-cause"></a>Possibile causa

Il codice MAK non è supportato.  

#### <a name="resolution"></a>Risoluzione

Per risolvere questo problema, verifica che il codice MAK usato corrisponda a quello fornito da Microsoft. Per verificare che il codice MAK sia valido, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001 Server di attivazione: il codice "Product Key" specificato non è valido

#### <a name="possible-cause"></a>Possibile causa

Il codice MAK immesso non è valido.

#### <a name="resolution"></a>Risoluzione

Verifica che il codice corrisponda al codice MAK fornito da Microsoft. Per assistenza, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003 Server di attivazione: il codice "Product Key" specificato è bloccato

#### <a name="possible-cause"></a>Possibile causa

Il codice MAK è bloccato nel server di attivazione.

#### <a name="resolution"></a>Risoluzione

Per ottenere un nuovo codice MAK, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers). Dopo aver ottenuto il nuovo codice MAK, prova di nuovo a installare e attivare Windows.  

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008 Server di attivazione: impossibile utilizzare il codice "Product Key" specificato

#### <a name="possible-cause"></a>Possibile causa

La chiave del Servizio di gestione delle chiavi ha superato il limite di attivazione. Una chiave host del Servizio di gestione delle chiavi può essere attivata fino a 10 volte su un massimo di sei computer diversi.  

#### <a name="resolution"></a>Risoluzione

Se hai bisogno di altre attivazioni, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).  

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020 Server di attivazione: è stato superato il limite massimo del codice ad attivazione multipla

#### <a name="possible-cause"></a>Possibile causa

Il codice MAK ha superato il limite di attivazione. Per impostazione predefinita, i codici MAK possono essere attivati per un numero limitato di volte.

#### <a name="resolution"></a>Risoluzione

Se hai bisogno di altre attivazioni, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021 Server di attivazione: è stato superato il limite massimo di estensione del codice ad attivazione multipla

#### <a name="possible-cause"></a>Possibile causa

Il codice MAK ha superato il limite di attivazione. Per impostazione predefinita, i codici MAK eseguono l'attivazione per un numero limitato di volte.

#### <a name="resolution"></a>Risoluzione

Se hai bisogno di altre attivazioni, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009 Servizio di protezione software: il periodo di prova è scaduto

#### <a name="possible-cause"></a>Possibile causa

Il periodo di prova è scaduto prima dell'attivazione del sistema. Nel sistema è ora attivo lo stato Notifiche.  

#### <a name="resolution"></a>Risoluzione

Per assistenza, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F Servizio gestione licenze software: binding ID hardware oltre il livello di tolleranza

#### <a name="possible-cause"></a>Possibile causa

L'hardware è cambiato oppure nel sistema sono stati aggiornati i driver.  

#### <a name="resolution"></a>Risoluzione

Se usi l'attivazione MAK, esegui l'attivazione online o telefonica per riattivare il sistema durante il periodo di prova.  

Se usa l'attivazione tramite Servizio di gestione delle chiavi , riavvia Windows o esegui **slmgr.vbs /ato**.

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014 Servizio di protezione software: codice "Product Key" non disponibile

#### <a name="possible-cause"></a>Possibile causa

Nessun codice Product Key installato nel sistema.  

#### <a name="resolution"></a>Risoluzione

Se usi l'attivazione MAK, installa un codice Product Key MAK. 

Se usi l'attivazione tramite Servizio di gestione delle chiavi, cerca una chiave di configurazione del Servizio di gestione delle chiavi nel file Pid.txt, disponibile nel supporto di installazione nella cartella \sources. Installa la chiave.

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C Servizio di protezione software: il formato dei dati per l'attivazione offline non è corretto

#### <a name="possible-cause"></a>Possibile causa

Il sistema ha rilevato che i dati immessi durante l'attivazione telefonica non sono validi.  

#### <a name="resolution"></a>Risoluzione

Verifica che il CID sia stato immesso correttamente.  

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035 Chiave per contratti multilicenza non valida

Il testo completo di questo messaggio di errore è simile al seguente:

> Errore: Chiave per contratti multilicenza non valida. Per l'attivazione è necessario modificare il codice "Product Key" in un codice ad attivazione multipla o in un codice di tipo retail. Devi avere una licenza di sistema operativo valida E una licenza di aggiornamento Windows 7 per contratti multilicenza oppure una licenza completa per Windows 7 rilasciata da un rivenditore. QUALSIASI ALTRA INSTALLAZIONE DEL SOFTWARE COMPORTA LA VIOLAZIONE DEL CONTRATTO E DELLE LEGGI IN VIGORE SUL COPYRIGHT.  

Il testo dell'errore è corretto, ma ambiguo. Questo errore indica che nel BIOS del computer non è presente un marcatore di Windows che lo identifica come sistema OEM con un'edizione di Windows valida. Questa informazione è necessaria per l'attivazione dei client del Servizio di gestione delle chiavi. Il significato più specifico di questo codice è "Errore: Chiave per contratti multilicenza non valida."

#### <a name="possible-cause"></a>Possibile causa

Le edizioni Windows 7 per contratti multilicenza vengono concesse in licenza solo per l'aggiornamento. Microsoft non supporta l'installazione di un sistema operativo con contratto multilicenza in un computer in cui non è installato un sistema operativo originale.  

#### <a name="resolution"></a>Risoluzione

Per eseguire l'attivazione, è necessario completare una delle operazioni seguenti:

- Sostituire il codice Product Key con un codice ad attivazione multipla (MAK) o un codice di tipo Retail. Devi avere una licenza di sistema operativo valida E una licenza di aggiornamento Windows 7 per contratti multilicenza oppure una licenza completa per Windows 7 rilasciata da un rivenditore.
  > [!NOTE]
  > Se durante il tentativo di attivazione viene restituito l'errore 0x80072EE2, usa in alternativa il metodo di attivazione telefonica illustrato di seguito.
- Per eseguire l'attivazione telefonica, segui questa procedura:
   1. Esegui **slmgr /dti** e prendi nota del valore dell'ID di installazione. </li>
   1. Contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers) e fornisci l'ID di installazione in modo da ricevere un ID di conferma.</li>
   1. Per completare l'attivazione usando l'ID di conferma, esegui **slmgr /atp &lt;ID conferma&gt;** .

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038 Il conteggio restituito dal Servizio di gestione delle chiavi è insufficiente

Il testo completo di questo messaggio di errore è simile al seguente:

> Servizio di protezione software: impossibile attivare il computer. Il conteggio restituito dal Servizio di gestione delle chiavi è insufficiente. Contattare l'amministratore del sistema.  

#### <a name="possible-cause"></a>Possibile causa

Il conteggio nell'host del Servizio di gestione delle chiavi non è abbastanza alto. Per Windows Server, il conteggio nel Servizio di gestione delle chiavi deve essere superiore o uguale a 5. Per Windows (client), il conteggio nel Servizio di gestione delle chiavi deve essere superiore o uguale a 25.  

#### <a name="resolution"></a>Risoluzione
Per poter usare il Servizio di gestione delle chiavi per l'attivazione di Windows, devi disporre di più computer nel pool del Servizio di gestione delle chiavi. Per ottenere il conteggio corrente nell'host del Servizio di gestione delle chiavi, esegui **Slmgr.vbs /dli**.  

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039 Il Servizio di gestione delle chiavi non è abilitato

Il testo completo di questo messaggio di errore è simile al seguente:

> Servizio di protezione software: impossibile attivare il computer. Il Servizio di gestione delle chiavi non è abilitato.  

#### <a name="possible-cause"></a>Possibile causa

Il Servizio di gestione delle chiavi non ha risposto alla richiesta.

#### <a name="resolution"></a>Risoluzione

Risolvere il problema di connessione di rete tra l'host del Servizio di gestione delle chiavi e il client. Verifica che la porta TCP 1688 predefinita non sia bloccata da un firewall o comunque filtrata.

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041 Servizio di protezione software: il server di gestione delle chiavi non è attivato

Il testo completo di questo messaggio di errore è simile al seguente:

> Servizio di protezione software: il server di gestione delle chiavi non è attivato. Per attivarlo,  

#### <a name="possible-cause"></a>Possibile causa

L'host del Servizio di gestione delle chiavi non è attivato.  

#### <a name="resolution"></a>Risoluzione

Attiva l'host del Servizio di gestione delle chiavi usando l'attivazione online o telefonica.  

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042 Servizio di protezione software: impossibile utilizzare il servizio di gestione delle chiavi specificato

#### <a name="possible-cause"></a>Possibile causa

Questo errore si verifica quando il client del Servizio di gestione delle chiavi contatta un host del Servizio di gestione delle chiavi che non è riuscito ad attivare il software client. Questo può essere un problema comune in ambienti misti contenenti ad esempio host del Servizio di gestione delle chiavi specifici di applicazioni e sistemi operativi.  

#### <a name="resolution"></a>Risoluzione

Se usi host del Servizio di gestione delle chiavi specifici per attivare applicazioni o sistemi operativi specifici, verifica che i client del Servizio di gestione delle chiavi si connettano agli host corretti.

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050 Servizio di protezione software: codice "Product Key" non valido

#### <a name="possible-cause"></a>Possibile causa

Questo problema può essere causato da un errore di digitazione della chiave del Servizio di gestione delle chiavi oppure dall'inserimento di una chiave Beta in una versione rilasciata del sistema operativo.  

#### <a name="resolution"></a>Risoluzione

Installare la chiave del Servizio di gestione delle chiavi appropriata nella versione di Windows corrispondente. Controllare l'ortografia. Se la chiave viene copiata e incollata, verifica che le lineette non siano state sostituite da trattini.  

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051 Servizio di protezione software: codice "Product Key" bloccato

#### <a name="possible-cause"></a>Possibile causa

Il server di attivazione ha riscontrato che il codice Product Key specificato è stato bloccato da Microsoft.  

#### <a name="resolution"></a>Risoluzione

Richiedi un nuovo codice MAK o una nuova chiave del Servizio di gestione delle chiavi, installala nel sistema ed esegui l'attivazione.

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064 Servizio di protezione software: il periodo di tolleranza per copie non autentiche è scaduto

#### <a name="possible-cause"></a>Possibile causa

Lo strumento di attivazione di Windows ha rilevato che il sistema non è originale.  

#### <a name="resolution"></a>Risoluzione

Per assistenza, contatta i [centri di attivazione delle licenze Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065 Servizio di protezione software: l'applicazione è in esecuzione entro il periodo valido per la copia non autentica

#### <a name="possible-cause"></a>Possibile causa

Lo strumento di attivazione di Windows ha rilevato che il sistema non è originale. L'esecuzione del sistema proseguirà durante il periodo di tolleranza per copie non autentiche.  

#### <a name="resolution"></a>Risoluzione

Richiedi e installa un codice Product Key autentico e attiva il sistema durante il periodo di tolleranza. In caso contrario, il sistema passerà allo stato Notifiche alla fine del periodo di tolleranza.

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C Il timestamp della richiesta non è valido

Il testo completo di questo messaggio di errore è simile al seguente:

> Servizio di protezione software: impossibile attivare il computer. Il Servizio di gestione delle chiavi ha determinato che il timestamp della richiesta non è valido.  

#### <a name="possible-cause"></a>Possibile causa

La differenza tra l'ora di sistema nel computer client e l'ora nell'host del Servizio di gestione delle chiavi è eccessiva. La sincronizzazione dell'ora è importante per la sicurezza del sistema e della rete per diversi motivi.  

#### <a name="resolution"></a>Risoluzione

Per risolvere questo problema, cambia l'ora di sistema nel client in modo da sincronizzarla con l'host del Sistema di gestione delle chiavi. È consigliabile usare un'origine di data e ora NTP (Network Time Protocol) o Active Directory Domain Services per la sincronizzazione dell'ora. Per questo errore viene usata l'ora UTP e non viene preso in considerazione il fuso orario.  

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074 Impossibile contattare un servizio di gestione delle chiavi

Il testo completo di questo messaggio di errore è simile al seguente:

> Servizio di protezione software: impossibile attivare il computer. Impossibile contattare un Servizio di gestione delle chiavi. Per altre informazioni vedere il registro eventi applicazioni.  

#### <a name="possible-cause"></a>Possibile causa

Tutti i sistemi host del Servizio di gestione delle chiavi hanno restituito un errore.  

#### <a name="resolution"></a>Risoluzione

Nel registro eventi dell'applicazione identifica tutti gli eventi con ID evento 12288 associati al tentativo di attivazione. Risolvi gli errori di questi eventi.

Per altre informazioni sulla risoluzione dei problemi correlati a DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).  
