---
title: mode
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 528277075f7448c86ca2d660c5e65c59098afbc0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839434"
---
# <a name="mode"></a>mode



Visualizza lo stato del sistema, modifica le impostazioni di sistema o riconfigura le porte o i dispositivi. Se utilizzata senza parametri, in **modalità** vengono visualizzati tutti gli attributi controllabili della console di e i dispositivi com disponibili.

È possibile utilizzare la **modalità** per eseguire le attività seguenti. ogni attività utilizza una sintassi diversa:
-   [Per configurare una porta di comunicazione seriale](#BKMK_1)
-   [Per visualizzare lo stato di tutti i dispositivi o di un singolo dispositivo](#BKMK_2)
-   [Per reindirizzare l'output da una porta parallela a una porta di comunicazione seriale](#BKMK_3)
-   [Per selezionare, aggiornare o visualizzare i numeri delle tabelle codici per la console](#BKMK_4)
-   [Per modificare le dimensioni del buffer dello schermo del prompt dei comandi](#BKMK_5)
-   [Per impostare la frequenza velocità ripetizione della tastiera](#BKMK_6)

## <a name="to-configure-a-serial-communications-port"></a><a name=BKMK_1></a>Per configurare una porta di comunicazione seriale

### <a name="syntax"></a>Sintassi

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parametri

|  Parametro  |                                                                                                                                                                                     Descrizione                                                                                                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Com\<M > [:]  |                                                                                                                                                      Specifica il numero della porta di comunicazione asincrona prncnfg. vbshronous.                                                                                                                                                      |
|  baud =\<B >  | Specifica la velocità di trasmissione in bit al secondo. Nella tabella seguente sono elencate le abbreviazioni valide per *B* e le relative tariffe.</br>-   **11** = 110 baud</br>-   **15** = 150 baud</br>-   **30** = 300 baud</br>-   **60** = 600 baud</br>-   **12** = 1200 baud</br>-   **24** = 2400 baud</br>-   **48** = 4800 baud</br>-   **96** = 9600 baud</br>-   **19** = 19.200 baud |
| parità =\<P > |                              Specifica il modo in cui il sistema usa il bit di parità per verificare la presenza di errori di trasmissione. Nella tabella seguente sono elencati i valori validi per *P*. Il valore predefinito è **e**. Non tutti i computer supportano i valori **m** e **s**.</br>-   **n** = None</br>-   **e** = even</br>-   **o** = dispari</br>-   **m** = contrassegno</br>-   **s** = spazio                              |
|  Data =\<D >  |                                                                                                    Specifica il numero di bit di dati in un carattere. I valori validi per **d** sono compresi tra 5 e 8. Il valore predefinito è 7. Non tutti i computer supportano i valori 5 e 6.                                                                                                     |
|  Stop =\<S >  |                                                                                  Specifica il numero di bit di arresto che definiscono la fine di un carattere: 1, 1,5 o 2. Se la velocità in baud è 110, il valore predefinito è 2. In caso contrario, il valore predefinito è 1. Non tutti i computer supportano il valore 1,5.                                                                                   |
|   a = {on    |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   XOn = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  odsr = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  PTOM = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   DTR = {on   |                                                                                                                                                                                         Disattivato                                                                                                                                                                                         |
|   RTS = {on   |                                                                                                                                                                                         Disattivato                                                                                                                                                                                         |
|  IDSR = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|     /?      |                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                         |

## <a name="to-display-the-status-of-all-devices-or-of-a-single-device"></a><a name=BKMK_2></a>Per visualizzare lo stato di tutti i dispositivi o di un singolo dispositivo

### <a name="syntax"></a>Sintassi

```
mode [<Device>] [/status]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> \<dispositivo|Specifica il nome del dispositivo per il quale si desidera visualizzare lo stato.|
|/status|Richiede lo stato di tutte le stampanti parallele reindirizzate. È possibile abbreviare l'opzione della riga di comando **/status** come **/sta**.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

Se utilizzata senza parametri, in **modalità** viene visualizzato lo stato di tutti i dispositivi installati nel sistema.

## <a name="to-redirect-output-from-a-parallel-port-to-a-serial-communications-port"></a><a name=BKMK_3></a>Per reindirizzare l'output da una porta parallela a una porta di comunicazione seriale

### <a name="syntax"></a>Sintassi

```
mode lpt<N>[:]=com<M>[:]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|LPT\<N > [:]|Obbligatoria. Specifica la porta parallela. I valori validi per *N* sono compresi tra 1 e 3.|
|com\<M > [:]|Obbligatoria. Specifica la porta seriale. I valori validi per *M* sono compresi tra 1 e 4.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

Per reindirizzare la stampa, è necessario essere membri del gruppo Administrators.

### <a name="examples"></a>Esempi

Per configurare il sistema in modo che invii l'output della stampante parallela a una stampante seriale, è necessario usare il comando **mode** due volte. Per la prima volta, usare la **modalità** per configurare la porta seriale. La seconda volta, usare la **modalità** per reindirizzare l'output della stampante parallela alla porta seriale specificata nel comando First **mode** .

Se, ad esempio, la stampante seriale funziona a 4800 baud con pari parità ed è connessa alla porta COM1 (la prima connessione seriale nel computer), digitare:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Se si reindirizza l'output della stampante parallela da LPT1 a COM1, ma si decide di stampare un file usando LPT1, digitare il comando seguente prima di stampare il file:
```
mode lpt1
```
Questo comando impedisce il reindirizzamento del file da LPT1 a COM1.

## <a name="to-select-refresh-or-display-the-numbers-of-the-code-pages-for-the-console"></a><a name=BKMK_4></a>Per selezionare, aggiornare o visualizzare i numeri delle tabelle codici per la console

### <a name="syntax"></a>Sintassi

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> \<dispositivo|Obbligatoria. Specifica il dispositivo per cui si desidera selezionare una tabella codici. CON è l'unico nome valido per un dispositivo.|
|tabella codici Select =|Obbligatoria. Specifica la tabella codici da usare con il dispositivo specificato. È possibile abbreviare **codepage** **SELECT** come **CP** **SEL**.|
|> \<YYY|Obbligatoria. Specifica il numero della tabella codici da selezionare. L'elenco seguente mostra ogni tabella codici supportata e il paese/area geografica o la lingua.</br>437: Stati Uniti</br>850: multilingue (alfabeto latino)</br>852: slave (Latin II)</br>855: cirillico (Russo)</br>857: turco</br>860: Portoghese</br>861: islandese</br>863: canadese-francese</br>865: Nordic</br>866: russo</br>869: greco moderno|
|CodePage|Obbligatoria. Visualizza il numero di tabelle codici (se presenti) selezionate per il dispositivo specificato.|
|/status|Visualizza il numero delle tabelle codici correnti selezionate per il dispositivo specificato. È possibile abbreviare **/status** in **/sta**. Indica se si specifica **/status**, la tabella codici della **modalità** Visualizza i numeri delle tabelle codici selezionate per il dispositivo specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="to-change-the-size-of-the-command-prompt-screen-buffer"></a><a name=BKMK_5></a>Per modificare le dimensioni del buffer dello schermo del prompt dei comandi

### <a name="syntax"></a>Sintassi

```
mode con[:] [cols=<C>] [lines=<N>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|con [:]|Obbligatoria. Indica che la modifica viene applicata alla finestra del prompt dei comandi.|
|colonne =\<C >|Specifica il numero di colonne nel buffer dello schermo del prompt dei comandi.|
|righe =\<N >|Specifica il numero di righe nel buffer dello schermo del prompt dei comandi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="to-set-the-keyboard-typematic-rate"></a><a name=BKMK_6></a>Per impostare la frequenza velocità ripetizione della tastiera

### <a name="syntax"></a>Sintassi

```
mode con[:] [rate=<R> delay=<D>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|con [:]|Obbligatoria. Fa riferimento alla tastiera.|
|frequenza =\<R >|Specifica la frequenza con cui un carattere viene ripetuto sullo schermo quando si tiene premuto un tasto.|
|ritardo =\<D >|Specifica la quantità di tempo che deve trascorrere dopo che è stato premuto un tasto e premuto prima che l'output dei caratteri si ripeta.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

- La frequenza velocità ripetizione è la frequenza con cui un carattere si ripete quando si tiene premuto il tasto per tale carattere. La frequenza velocità ripetizione ha due componenti, la frequenza e il ritardo. Alcune tastiere non riconoscono questo comando.
- Uso di **rate =** <em>R</em>

  I valori validi sono compresi tra 1 e 32. Questi valori sono uguali a circa 2-30 caratteri al secondo. Il valore predefinito è 20 per le tastiere compatibili con IBM e 21 per le tastiere compatibili con IBM PS/2. Se si imposta la frequenza, è necessario impostare anche il ritardo.
- Uso di **delay**=*D*

  I valori validi per *D* sono 1, 2, 3 e 4 (che rappresentano 0,25, 0,50, 0,75 e 1 secondo). Il valore predefinito è 2. Se si imposta il ritardo, è necessario impostare anche la frequenza.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)