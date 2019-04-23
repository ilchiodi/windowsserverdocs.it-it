---
title: mode
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde0e9786f72823f446202f1c87ad8e9e181d29c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848202"
---
# <a name="mode"></a>mode



Consente di visualizzare lo stato del sistema o modifica le impostazioni di sistema riconfigura le porte o dispositivi. Se utilizzata senza parametri, **modalità** Visualizza tutti gli attributi controllabili della console e dispositivi COM disponibili.

È possibile usare **modalità** per eseguire le attività seguenti, ovvero ogni attività Usa una sintassi diversa:
-   [Per configurare una porta di comunicazione seriale](#BKMK_1)
-   [Per visualizzare lo stato di tutti i dispositivi o di un singolo dispositivo](#BKMK_2)
-   [Per reindirizzare l'output da una porta parallela a una porta di comunicazione seriale](#BKMK_3)
-   [Per selezionare, aggiornare o visualizzare i numeri delle tabelle codici per la console di](#BKMK_4)
-   [Per modificare le dimensioni del buffer dello schermo del prompt dei comandi](#BKMK_5)
-   [Per impostare la velocità di ripetizione](#BKMK_6)

## <a name="BKMK_1"></a>Per configurare una porta di comunicazione seriale

### <a name="syntax"></a>Sintassi

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|COM\<M > [::]|Specifica il numero di porta di comunicazione Prncnfg.vbshronous async.|
|baud=\<B>|Specifica la velocità di trasmissione in bit al secondo. La tabella seguente elenca le abbreviazioni valide per *B* e le tariffe correlate.</br>-   **11** = 110 baud</br>-   **15** = 150 baud</br>-   **30** = 300 baud</br>-   **60** = 600 baud</br>-   **12** = 1200 baud</br>-   **24** = 2400 baud</br>-   **48** = 4800 baud</br>-   **96** = 9600 baud</br>-   **19** = 19.200 in baud|
|parità =\<P >|Specifica come il sistema utilizza il bit di parità per verificare la presenza di errori di trasmissione. Nella tabella seguente sono elencati i valori validi per *P*. Il valore predefinito è **elettronica**. Non tutti i computer supportano i valori **m** e **s**.</br>-   **n** = none</br>-   **e** = even</br>-   **o** = dispari</br>-   **m** = mark</br>-   **s** = spazio|
|Data =\<1!d >|Specifica il numero di bit di dati in un carattere. I valori validi per **1!d** sono compresi nell'intervallo da 5 a 8. Il valore predefinito è 7. Non tutti i computer supportano i valori con 5 e 6.|
|arrestare =\<S >|Specifica il numero di bit di stop che definiscono la fine di un carattere: 1, 1,5 o 2. Se la velocità in baud è impostato su 110, il valore predefinito è 2. In caso contrario, il valore predefinito è 1. Non tutti i computer supportano il valore 1.5.|
|a = {on | off}|Specifica se l'elaborazione di timeout infinito è attivata o disattivata. Il valore predefinito è.|
|XON = {on | off}|Specifica se il protocollo xon / xoff per il controllo di flusso di dati è attivata o disattivata.|
|odsr = {on | off}|Specifica se la sincronizzazione dell'output che usa il circuito di Set di dati pronto (DSR) è attivata o disattivata.|
|PTOM = {on | off}|Specifica se la sincronizzazione dell'output che usa il circuito a inviare CTS (Clear) è attivata o disattivata.|
|DTR = {on | Disattivato | hs}|Specifica se il circuito DTR Data Terminal Ready () è attivato o disattivato o impostato su handshake.|
|RTS = {on | Disattivato | hs | tg}|Specifica se il circuito richiesta da inviare (RTS) è impostato su on, off, handshake o attiva/disattiva.|
|IDSR = {on | off}|Specifica se la sensibilità del circuito DSR è attivata o disattivata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_2"></a>Per visualizzare lo stato di tutti i dispositivi o di un singolo dispositivo

### <a name="syntax"></a>Sintassi

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Device>|Specifica il nome del dispositivo per il quale si desidera visualizzare lo stato.|
|/status|Richiede lo stato di tutte le stampanti parallele reindirizzate. È possibile abbreviare il **/status** opzione della riga di comando come **/sta**.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

Se utilizzata senza parametri, **modalità** Visualizza lo stato di tutti i dispositivi che sono installati nel sistema.

## <a name="BKMK_3"></a>Per reindirizzare l'output da una porta parallela a una porta di comunicazione seriale

### <a name="syntax"></a>Sintassi

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|LPT\<N > [::]|Obbligatorio. Specifica la porta parallela. I valori validi per *N* sono compresi nell'intervallo da 1 a 3.|
|COM\<M > [::]|Obbligatorio. Specifica la porta seriale. I valori validi per *M* sono compresi nell'intervallo da 1 a 4.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

È necessario essere un membro del gruppo Administrators per reindirizzare la stampa.

### <a name="examples"></a>Esempi

Per configurare il sistema in modo che invia output su stampante parallele a una stampante seriale, è necessario usare il **modalità** comando due volte. La prima volta, usare **modalità** per configurare la porta seriale. La seconda volta, usare **modalità** reindirizzare l'output della stampante parallele alla porta seriale è specificato nel primo **modalità** comando.

Ad esempio, se la stampante seriale opera a velocità in baud 4800 con parità pari ed è connessa alla porta COM1 (la prima connessione seriale nel computer in uso), digitare:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Se si reindirizza l'output di stampante parallela da LPT1 a COM1 ma quindi si decide che si desidera stampare un file usando LPT1, digitare il comando seguente per stampare il file:
```
mode lpt1
```
Questo comando impedisce il reindirizzamento del file da LPT1 a COM1.

## <a name="BKMK_4"></a>Per selezionare, aggiornare o visualizzare i numeri delle tabelle codici per la console di

### <a name="syntax"></a>Sintassi

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Device>|Obbligatorio. Specifica il dispositivo per il quale si desidera selezionare una tabella codici. ESEGUITA è l'unico nome valido per un dispositivo.|
|Selezionare la tabella codici =|Obbligatorio. Specifica la tabella codici da usare con il dispositivo specificato. È possibile abbreviare **codepage** **seleziona** come **cp** **sel**.|
|\<YYY &GT;|Obbligatorio. Specifica il numero di tabella codici da selezionare. Nell'elenco seguente viene illustrato ogni codice pagina in cui è supportato e il paese/regione o lingua.</br>437: Stati Uniti</br>850: Multilingue (latino I)</br>852: Slavo (latino II)</br>855: Cirillico (russo)</br>857: Turco</br>860: Portoghese</br>861: Islandese</br>863: Francese (Canada)</br>865: Area lingue nordiche</br>866: Russo</br>869: Greco moderno|
|tabella codici|Obbligatorio. Consente di visualizzare i numeri del codice delle pagine (se presente) che sono selezionati per il dispositivo specificato.|
|/status|Visualizza i numeri delle tabelle codici corrente selezionate per il dispositivo specificato. È possibile abbreviare **/status** al **/sta**. Se specifica **/status**, **codepage modalità** Visualizza i numeri delle tabelle codici sono selezionati per il dispositivo specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_5"></a>Per modificare le dimensioni del buffer dello schermo del prompt dei comandi

### <a name="syntax"></a>Sintassi

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|con[:]|Obbligatorio. Indica che la modifica si applica alla finestra del prompt dei comandi.|
|Cols =\<C >|Specifica il numero di colonne nel buffer dello schermo del prompt dei comandi.|
|righe =\<N >|Specifica il numero di righe nel buffer dello schermo del prompt dei comandi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_6"></a>Per impostare la velocità di ripetizione

### <a name="syntax"></a>Sintassi

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|con[:]|Obbligatorio. Si riferisce alla tastiera.|
|frequenza =\<R >|Specifica la frequenza con cui viene ripetuto un carattere sullo schermo quando si tiene premuto un tasto.|
|delay=\<D>|Specifica la quantità di tempo che deve trascorrerà dopo premere e tenere premuto un tasto prima che si ripete l'output di caratteri.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

-   La velocità di ripetizione è la velocità di ripetizione di un carattere quando si tiene premuto il tasto per tale carattere. La velocità di ripetizione ha due componenti, la frequenza e il ritardo. Alcune tastiere non riconoscono questo comando.
-   Using **rate=***R*

    I valori validi sono compresi tra 1 e 32. Questi valori sono uguali a circa 2 e 30 caratteri al secondo. Il valore predefinito è 20 per tastiere compatibili con IBM AT e 21 per tastiere compatibili IBM PS/2. Se si imposta la frequenza, è necessario impostare anche il ritardo.
-   Usando **ritardo**=*1!d*

    I valori validi per *1!d* sono 1, 2, 3 e 4 (che rappresentano 0,25, 0.50, 0.75 e 1 secondo). Il valore predefinito è 2. Se si imposta il ritardo, è necessario impostare anche la velocità.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)