---
title: Novità della console Windows in Windows Server 2016
description: Elenca le nuove funzionalità importanti nella console di Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: server-general
ms.reviewer: na
ms.suite: na
ms.date: 10/04/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9fc582-033b-4973-84e7-0c6024ecfcbc
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2f05bcffa7c8c4f9e74f3699b9838b8a627af1b1
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082279"
---
# <a name="whats-new-in-the-windows-console-in-windows-server-2016"></a>Novità della console Windows in Windows Server 2016
>Si applica a: Windows Server 2016

L'host della console, ovvero il codice sottostante che supporta tutte le applicazioni in modalità carattere incluso il prompt dei comandi di Windows, il prompt di Windows PowerShell e altri, è stato aggiornato in diversi modi per aggiungere un'ampia gamma di nuove funzionalità.  

## <a name="controlling-the-new-features"></a>Controllo delle nuove funzionalità  
La nuova funzionalità è abilitata per impostazione predefinita, ma è possibile attivare e disattivare ognuna delle nuove funzionalità o ripristinare l'host precedente della console tramite l'interfaccia Proprietà (principalmente nella scheda **Opzioni**) o con queste chiavi del Registro di sistema (tutte le chiavi sono valori DWORD in **HKEY_CURRENT_USER\Console**):  

|Chiave del Registro di sistema|Descrizione|  
|----------------|---------------|  
|ForceV2|1 abilita tutte le nuove funzionalità della console; 0 disabilita tutte le nuove funzionalità. Nota: questo valore non è archiviato nei tasti di scelta rapida, ma solo in questa chiave del Registro di sistema.|  
|LineSelection|1 abilita la selezione della riga; 0 per usare solo la modalità blocco|  
|FilterOnPaste|1 abilita il nuovo comportamento Incolla|  
|LineWrap|1 esegue il wrapping del testo quando si ridimensionano le finestre della console|  
|CtrlKeyShortcutsDisabled|0 abilita i nuovi tasti di scelta rapida; 1 li disabilita|  
|Chiavi ExtendedEdit|1 abilita il set completo dei tasti di selezione della tastiera; 0 lo disabilita|  
|TrimLeadingZeros|1 rimuove gli zeri iniziali nelle selezioni effettuate facendo doppio clic; 0 mantiene gli zeri iniziali|  
|WindowsAlpha|Imposta il valore di opacità tra 30% e 100%. Usare una valore da 0x4C fino a 0xFF o da 76 a 255 per specificare il valore|  
|WordDelimiters|Definisce il carattere usato per saltare una parola intera alla volta durante la selezione di testo con CTRL+MAIUSC+freccia (il valore predefinito è il carattere spazio). Impostare questo valore REG_SZ per contenere tutti i caratteri che devono essere considerati come delimitatori. Nota: questo valore non è archiviato nei tasti di scelta rapida, ma solo in questa chiave del Registro di sistema.|  

Queste impostazioni vengono archiviate nel Registro di sistema per ogni titolo di finestra in HKCU\Console. Le finestre della console aperte da un collegamento dispongono di queste impostazioni archiviate nel collegamento; se il collegamento viene copiato in un altro computer, anche le impostazioni si spostano al nuovo computer. Le impostazioni dei collegamenti sostituiscono tutte le altre impostazioni, incluse le impostazioni predefinite e le impostazioni globali. Tuttavia, se si ripristina la console originale usando **Usa console legacy** nella scheda **Opzioni**, questa impostazione verrà mantenuta per tutte le finestre anche quando il computer verrà riavviato, poiché è un'impostazione globale.  

È possibile preconfigurare o eseguire lo script di queste impostazioni configurando il Registro di sistema in modo appropriato in un file di installazione automatica o con Windows PowerShell.  

Le applicazioni NTVDM a 16 bit ripristinano sempre l'host della console precedente.  

> [!NOTE]  
> Se si verificano problemi con le nuove impostazioni della console che non possono essere risolti con le opzioni specifiche elencate di seguito, è sempre possibile ripristinare la console originale impostando ForceV2 su 0 o con il controllo **Usa console legacy** in **Opzioni**.  

## <a name="console-behavior"></a>Comportamento della console  
È possibile ridimensionare a piacere la finestra della console catturando un bordo con il mouse e trascinandolo. Le barre di scorrimento vengono visualizzate solo se si impostano manualmente le dimensioni della finestra tramite **Layout** nella scheda **Proprietà** o se la riga più lunga del testo nel buffer è più grande delle dimensioni della finestra corrente.  

La nuova finestra della console supporta ora il ritorno a capo automatico. Tuttavia, se è stato usato l'API della console per modificare il testo in un buffer, la console lascerà il testo come era in origine.  

Le finestre della console possono ora essere semi-trasparenti (trasparenza minima pari al 30%). È possibile regolare la trasparenza dal menu Proprietà o con i comandi della tastiera seguenti:  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Aumento della trasparenza|CTRL+MAIUSC+segno più (+) o CTRL+MAIUSC+scorrere verso l'alto|  
|Diminuzione della trasparenza|CTRL+MAIUSC+segno meno (-) o CTRL+MAIUSC+scorrere verso il basso|  
|Modalità schermo intero|ALT+INVIO|  

## <a name="selection"></a>Selezione  
Sono disponibili molte opzioni nuove per selezionare testo e righe, nonché per contrassegnare il testo e usare la cronologia del buffer. La console tenta di evitare conflitti con le applicazioni che potrebbero usare le stesse chiavi.  

**Per gli sviluppatori:** se si verifica un conflitto, in genere è possibile controllare il comportamento di utilizzo delle modalità line input, processed input e echo input da parte dell'applicazione con l'API SetConsoleMode(). Se l'esecuzione avviene in modalità processed input si applicano le scelte rapide seguenti, ma nelle altre modalità l'applicazione deve gestirle. Le combinazioni di tasti non elencate qui sotto funzionano come nelle versioni precedenti della console. È anche possibile provare a risolvere i conflitti con varie impostazioni della scheda **Opzioni**. Se non è possibile risolvere i conflitti in alcun modo, è sempre possibile ripristinare la console originale.  

È ora possibile usare la selezione "fare clic e trascinare" al di fuori della modalità QuickEdit. Questa selezione può selezionare il testo su più righe come nel Blocco note, anziché solo in un blocco rettangolare. Le operazioni di copia non richiedono più la rimozione delle interruzioni di riga. Oltre alla selezione "fare clic e trascinare", sono disponibili queste combinazioni:  

**Selezione testo**  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Spostare il cursore di un carattere verso sinistra, estendendo la selezione|MAIUSC+freccia SINISTRA|  
|Spostare il cursore di un carattere verso destra, estendendo la selezione|MAIUSC+freccia DESTRA|  
|Seleziona il testo riga per riga dal punto di inserimento|MAIUSC+freccia SU|  
|Estende la selezione di testo alla riga successiva dal punto di inserimento|MAIUSC+freccia GIÙ|  
|Se il cursore si trova nella riga che attualmente è in fase di modifica, usare questo comando una volta per estendere la selezione all'ultimo carattere nella riga di input. Usarlo una seconda volta per estendere la selezione al margine destro.|MAIUSC+FINE|  
|Se il cursore **non** si trova nella riga che attualmente è in fase di modifica, usare questo comando per selezionare tutto il testo dal punto di inserimento al margine destro.|MAIUSC+FINE|  
|Se il cursore si trova nella riga che attualmente è in fase di modifica, usare questo comando una volta per estendere la selezione al carattere immediatamente successivo al prompt di comando. Usarlo una seconda volta per estendere la selezione al margine destro.|MAIUSC+HOME|  
|Se il cursore **non** si trova nella riga che attualmente è in fase di modifica, usare questo comando per estendere la selezione al margine sinistro.|MAIUSC+HOME|  
|Estendere la selezione alla schermata successiva|MAIUSC+PGGIÙ|  
|Estendere la selezione alla schermata precedente|MAIUSC+PGSU|  
|Estendere la selezione di una parola verso destra. È possibile definire i delimitatori per "parola" con la chiave del Registro di sistema WordDelimiters.|CTRL+MAIUSC+freccia DESTRA|  
|Estendere la selezione di una parola verso sinistra|CTRL+MAIUSC+HOME|  
|Estendere la selezione all'inizio del buffer dello schermo|CTRL+MAIUSC+FINE|  
|Selezionare tutto il testo dopo il prompt, se il cursore si trova nella riga corrente e la riga non è vuota|CTRL+A|  
|Selezionare l'intero buffer, se il cursore **non** di trova nella riga corrente|CTRL+A|  

**Modifica di testo**  

È possibile copiare e incollare il testo nella console usando i comandi della tastiera. CTRL+C ora svolge due funzioni. Se non è stato selezionato testo quando viene usato, invia il comando di interruzione come di consueto. Se è stato selezionato testo, al primo clic copia il testo e cancella la selezione; al secondo clic invia il comando di interruzione. Ecco altri comandi di modifica:  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Incollare il testo nella riga di comando|CTRL+V|  
|Copiare il testo selezionato negli appunti|CTRL+INS|  
|Copiare il testo selezionato negli appunti; inviare il comando di interruzione|CTRL+C|  
|Incollare il testo nella riga di comando|MAIUSC+INS|  

**Modalità contrassegno**  

Per attivare la modalità contrassegno in qualsiasi momento, fare clic con il pulsante destro del mouse in un punto qualsiasi della barra del titolo della console, scegliere **Modifica** e selezionare **Contrassegna** dal menu visualizzato. È inoltre possibile digitare CTRL+M. In modalità contrassegno, usare il tasto ALT per identificare l'inizio di una selezione di ritorno a capo automatico. Se **Consenti selezione con ritorno a capo automatico righe** è disabilitata, la modalità contrassegno seleziona il testo in un blocco. In modalità contrassegno, CTRL+MAIUSC+freccia esegue la selezione in base al carattere e non in base alla parola come in modalità normale. Oltre alle chiavi di selezione nella sezione **Modifica di testo**, in modalità contrassegno sono disponibili queste combinazioni:  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Attivare la modalità contrassegno per spostare il cursore nella finestra|CTRL+M|  
|Iniziare la selezione con ritorno a capo automatico in modalità contrassegno, insieme ad altre combinazioni di tasti|ALT|  
|Spostare il cursore nella direzione specificata|Tasti di direzione|  
|Spostare il cursore di una pagina nella direzione specificata|Tasti PAG|  
|Spostare il cursore all'inizio del buffer|CTRL+HOME|  
|Spostare il cursore alla fine del buffer|CTRL+FINE|  

**Navigazione nella cronologia**  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Spostare verso l'alto una riga nella cronologia di output|CTRL+freccia SU|  
|Spostare verso il basso una riga nella cronologia di output|CTRL+freccia GIÙ|  
|Spostare il riquadro di visualizzazione all'inizio del buffer se la riga di comando è vuota o eliminare tutti i caratteri a sinistra del cursore se la riga di comando non è vuota|CTRL+HOME|  
|Spostare il riquadro di visualizzazione nella riga di comando se la riga di comando è vuota o eliminare tutti i caratteri a destra del cursore se la riga di comando non è vuota|CTRL+FINE|  

**Altri comandi della tastiera**  

|A tale scopo:|Usare questa combinazione di tasti:|  
|---------------|-----------------------------|  
|Aprire la finestra di dialogo Trova|CTRL+F|  
|Chiudere la finestra della console|ALT+F4|  
