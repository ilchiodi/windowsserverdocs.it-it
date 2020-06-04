---
title: mode
description: Argomento di riferimento per il comando Mode, che visualizza lo stato del sistema, modifica le impostazioni di sistema o riconfigura le porte o i dispositivi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4c895c59bb527b8bfb6973a72d0d4e163cb2ace
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354581"
---
# <a name="mode"></a>mode

Visualizza lo stato del sistema, modifica le impostazioni di sistema o riconfigura le porte o i dispositivi. Se utilizzata senza parametri, in **modalità** vengono visualizzati tutti gli attributi controllabili della console di e i dispositivi com disponibili.

## <a name="serial-port"></a>Porta seriale

Configura una porta di comunicazione seriale e imposta l'handshake di output.

### <a name="syntax"></a>Sintassi

```
mode com<m>[:] [baud=<b>] [parity=<p>] [data=<d>] [stop=<s>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parametri

| Parametro  | Descrizione |
| ---------- | ----------- |
| `com<m>[:]` | Specifica il numero della porta di comunicazione asincrona prncnfg. vbshronous. |
| `baud=<b>`  | Specifica la velocità di trasmissione in bit al secondo. I valori validi includono:<ul><li>da **11** a 110 baud</li><li>**15** -150 baud</li><li>**30** -300 baud</li><li>**60** -600 baud</li><li>**12** -1200 baud</li><li>**24** -2400 baud</li><li>**48** -4800 baud</li><li>**96** -9600 baud</li><li>da **19** a 19.200 baud</li></ul> |
| `parity=<p>` | Specifica il modo in cui il sistema usa il bit di parità per verificare la presenza di errori di trasmissione. I valori validi includono:<ul><li>**n** -None</li><li>**e** -even (valore predefinito)</li><li>**o** -dispari</li><li>**m** -Mark</li><li>**s** -spazio</li></ul>Non tutti i dispositivi supportano l'utilizzo dei parametri **m** o **s** . |
| `data=<d>` | Specifica il numero di bit di dati in un carattere. I valori validi sono compresi tra **5** e **8**. Il valore predefinito è **7**. Non tutti i dispositivi supportano i valori **5** e **6**. |
| `stop=<s>` | Specifica il numero di bit di arresto che definiscono la fine di un carattere: **1**, **1,5**o **2**. Se la velocità in baud è **110**, il valore predefinito è **2**. In caso contrario, il valore predefinito è **1**. Non tutti i dispositivi supportano il valore **1,5**. |
| `to={on | off}` | Specifica se il dispositivo usa un'elaborazione di timeout infinita. Il valore predefinito è **off**. Se si attiva **questa opzione,** il dispositivo non verrà mai arrestato in attesa di ricevere una risposta da un host o da un computer client. |
| `xon={on | off}` | Specifica se il sistema consente il protocollo XON/XOFF. Questo protocollo fornisce il controllo di flusso per le comunicazioni seriali, migliorando l'affidabilità, ma riducendo le prestazioni. |
| `odsr={on | off}` | Specifica se il sistema attiva l'handshake di output del set di dati (DSR). |
| `octs={on | off}` | Specifica se il sistema attiva l'handshake di output Clear to Send (CTS). |
| `dtr={on | off | hs}` | Specifica se il sistema attiva l'handshake di output di Data Terminal Ready (DTR). Impostando questo valore **su on** Mode, viene fornito un segnale costante che indica che il terminale è pronto per l'invio dei dati. L'impostazione di questo valore sulla modalità **HS** fornisce un segnale di handshake tra i due terminali. |
| `rts={on | off | hs | tg}` | Specifica se il sistema attiva l'handshake di output della richiesta di invio (RTS). Impostando questo valore **su on** Mode, viene fornito un segnale costante che indica che il terminale è pronto per l'invio dei dati. L'impostazione di questo valore sulla modalità **HS** fornisce un segnale di handshake tra i due terminali. L'impostazione di questo valore sulla modalità **TG** consente di passare da uno stato pronto a un altro e non pronto. |
| `idsr={on | off}` | Specifica se il sistema attiva la sensibilità del DSR. È necessario attivare questa opzione per usare l'handshake DSR.
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="device-status"></a>Stato dei dispositivi

Visualizza lo stato di un dispositivo specificato. Se usato senza parametri, **mode** Visualizza lo stato di tutti i dispositivi installati nel sistema.

### <a name="syntax"></a>Sintassi

```
mode [<device>] [/status]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<device>` | Specifica il nome del dispositivo per il quale si desidera visualizzare lo stato. I nomi standard includono, LPT1: tramite LPT3:, COM1: tramite COM9: e CON. |
| /status | Richiede lo stato di tutte le stampanti parallele reindirizzate. È anche possibile usare **/sta** come una versione abbreviata di questo comando. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="redirect-printing"></a>Reindirizza stampa

Reindirizza l'output della stampante. Per reindirizzare la stampa, è necessario essere membri del gruppo Administrators.

> [!NOTE]
Per configurare il sistema in modo che invii l'output della stampante parallela a una stampante seriale, è necessario usare il comando **mode** due volte. Per la prima volta, è necessario usare la **modalità** per configurare la porta seriale. La seconda volta, è necessario usare la **modalità** per reindirizzare l'output della stampante parallela alla porta seriale specificata nel comando First **mode** .

### <a name="syntax"></a>Sintassi

```
mode LPT<n>[:]=COM<m>[:]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| LPT `<n>` [:] | Specifica il numero di LPT da configurare. In genere, ciò significa fornire un valore da **LTP1: tramite LTP3:**, a meno che il sistema non includa un supporto speciale per le porte parallele. Questo parametro è obbligatorio.|
| COM `<m>` [:] | Specifica la porta COM da configurare. In genere, ciò significa fornire un valore da **COM1: tramite COM9:**, a meno che il sistema non disponga di hardware speciale per altre porte com. Questo parametro è obbligatorio. |
| /? | Visualizza la guida al prompt dei comandi.|

#### <a name="examples"></a>Esempio

Per reindirizzare una stampante seriale che opera a 4800 baud con pari parità ed è connessa alla porta COM1 (la prima connessione seriale nel computer), digitare:

```
mode com1 48,e,,,b
mode lpt1=com1
```

Per reindirizzare l'output della stampante parallela da LPT1 a COM1 e quindi stampare un file con LPT1, digitare il comando seguente prima di stampare il file:

```
mode lpt1
```

Questo comando impedisce il reindirizzamento del file da LPT1 a COM1.

## <a name="select-code-page"></a>Seleziona tabella codici

Configura o esegue una query sulle informazioni della tabella codici per un dispositivo selezionato.

### <a name="syntax"></a>Sintassi

```
mode <device> codepage select=<yyy>
mode <device> codepage [/status]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<device>` |Specifica il dispositivo per cui si desidera selezionare una tabella codici. CON è l'unico nome valido per un dispositivo. Questo parametro è obbligatorio. |
| codepage | Specifica la tabella codici da usare con il dispositivo specificato. È anche possibile usare **CP** come versione abbreviata di questo comando. Questo parametro è obbligatorio. |
| Select =`<yyy>` | Specifica il numero della tabella codici da usare con il dispositivo. Le tabelle codici supportate, per paese/area geografica o lingua, includono:<ul><li>**437:** Stati Uniti</li><li>**850:** Multilingue (alfabeto latino)</li><li>**852:** Slavo (Latino II)</li><li>**855:** Cirillico (Russo)</li><li>**857:** Turco</li><li>**860:** Portoghese</li><li>**861:** Islandese</li><li>**863:** Canadese-francese</li><li>**865:** Nordico</li><li>**866:** Russo</li><li>**869:** Greco moderno</li></ul> Questo parametro è obbligatorio. |
| /status | Visualizza il numero delle tabelle codici correnti selezionate per il dispositivo specificato. È anche possibile usare **/sta** come una versione abbreviata di questo comando. Indipendentemente dal fatto che si specifichi **/status**, il comando della tabella codici della **modalità** Visualizza i numeri delle tabelle codici selezionate per il dispositivo specificato. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="display-mode"></a>Modalità di visualizzazione

Modifica la dimensione del buffer dello schermo del prompt dei comandi

### <a name="syntax"></a>Sintassi

```
mode con[:] [cols=<c>] [lines=<n>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| con [:] | Indica che la modifica viene applicata alla finestra del prompt dei comandi. Questo parametro è obbligatorio. |
| colonne =`<c>` | Specifica il numero di colonne nel buffer dello schermo del prompt dei comandi. L'impostazione predefinita è 80 colonne, ma è possibile impostarla su qualsiasi valore. Se non si usa l'impostazione predefinita, i valori tipici sono le colonne 40 e 135. L'uso di valori non standard può causare problemi nell'app del prompt dei comandi. |
| righe =`<n>` | Specifica il numero di righe nel buffer dello schermo del prompt dei comandi. Il valore predefinito è 25, ma è possibile impostarlo su qualsiasi valore. Se non si usa l'impostazione predefinita, l'altro valore tipico è 50 righe. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="typematic-rate"></a>Frequenza velocità ripetizione

Imposta la frequenza di velocità ripetizione della tastiera. La velocità velocità ripetizione è la velocità con cui Windows può ripetere un carattere quando si preme il tasto su una tastiera.

> [!NOTE]
> Alcune tastiere non riconoscono questo comando.

### <a name="syntax"></a>Sintassi

```
mode con[:] [rate=<r> delay=<d>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| con [:] | Specifica la tastiera. Questo parametro è obbligatorio. |
| frequenza =`<r>` | Specifica la frequenza con cui un carattere viene ripetuto sullo schermo quando si tiene premuto un tasto. Il valore predefinito è 20 caratteri al secondo per le tastiere compatibili con IBM e 21 per le tastiere compatibili con IBM PS/2, ma è possibile usare qualsiasi valore compreso tra 1 e 32. Se si imposta questo parametro, è necessario impostare anche il parametro **delay** .|
| ritardo =`<d>` | Specifica la quantità di tempo che deve trascorrere dopo che è stato premuto un tasto e premuto prima che l'output dei caratteri si ripeta. Il valore predefinito è 2 (. 50 secondi), ma è anche possibile usare 1 (. 25 secondi), 3 (. 75 secondi) o 4 (1 secondo). Se si imposta questo parametro, è necessario impostare anche il parametro **rate** . |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)