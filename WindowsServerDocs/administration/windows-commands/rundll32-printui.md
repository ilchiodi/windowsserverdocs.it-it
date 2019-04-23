---
title: rundll32 printui.dll,PrintUIEntry
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/25/2018
ms.openlocfilehash: c90641820bfa01c19ae7bf587c5467d3f9c5a01c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836842"
---
# <a name="rundll32-printuidllprintuientry"></a>rundll32 printui.dll,PrintUIEntry

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di automatizzare molte attività di configurazione della stampante. printui. dll è il file eseguibile che contiene le funzioni utilizzate per le finestre di dialogo di configurazione della stampante. Queste funzioni possono anche essere chiamate all'interno di uno script o un file batch da riga di comando, o possono essere eseguiti in modo interattivo dal prompt dei comandi. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).  
## <a name="syntax"></a>Sintassi  
```  
rundll32 printui.dll PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
È anche possibile usare la sintassi alternative seguente, anche se gli esempi in questo argomento usano la sintassi precedente:  
```  
rundll32 printui.dll,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
## <a name="parameters"></a>Parametri  
Esistono due tipi di parametri: i parametri e modifica di base. I parametri di base specificano la funzione che deve eseguire il comando. Solo uno di questi parametri può essere visualizzati in una riga di comando specificata. Quindi, è possibile modificare il parametro di base utilizzando uno o più dei parametri di modifica se sono applicabili al parametro di base (non tutti i parametri di modifica sono supportati da tutti i parametri di base).  
|Parametri di base|Descrizione|  
|----------|--------|  
|/dl|Elimina la stampante locale.|  
|/dn|Elimina una connessione stampante di rete.|  
|/dd|Consente di eliminare un driver della stampante.|  
|/e|Consente di visualizzare le preferenze di stampa per una stampante specificata.|  
|/ga|Aggiunge una connessione per computer stampante (la connessione è disponibile per qualsiasi utente su quel computer quando effettuano l'accesso).|  
|/ge|Visualizza le connessioni alle stampanti di computer in un computer.|  
|/gd|Elimina una connessione per computer stampante (la connessione viene eliminata al successivo accesso dell'utente).|  
|/ia|Installa un driver della stampante tramite un file. inf.|  
|/ID|Consente di installare un driver della stampante utilizzando la stampante aggiunta guidata Driver.|  
|/if|Consente di installare una stampante utilizzando un file. inf.|  
|/ii|Consente di installare una stampante utilizzando l'installazione guidata stampante con un file. inf.|  
|/il|Consente di installare una stampante utilizzando l'installazione guidata stampante.|  
|/in|Si connette a una stampante di rete remota.|  
|/ip|Consente di installare una stampante utilizzando la stampante di rete (disponibile nell'interfaccia utente dalla gestione stampa) con installazione guidata.|  
|/k|Consente di stampare una pagina di test su una stampante.|  
|/o|Consente di visualizzare la coda per una stampante.|  
|/ p|Visualizza le proprietà di una stampante. Quando si usa questo parametro, è necessario specificare anche un valore per il parametro di modifica **/n [nome]**.|  
|/s|Consente di visualizzare le proprietà di un server di stampa. Se si desidera visualizzare il server di stampa locale, non devi usare un parametro di modifica. Tuttavia, se si desidera visualizzare un server di stampa remoto, è necessario specificare il **/c [nome]** parametro modifica.|  
|/Ss|Specifica il tipo di informazioni per una stampante verrà archiviato. Se nessuno dei valori per **/Ss** vengono specificati, il comportamento predefinito è come se tutti gli elementi sono stati specificati. Usare questo parametro di base con i valori seguenti, inseriti alla fine della riga di comando:<br /><br />-   **2**: Usare per archiviare le informazioni contenute nella struttura printER_INFO_2 s della stampante. Questa struttura contiene le informazioni di base relativo alla stampante, ad esempio nome, nome del server, il nome della porta e il nome della condivisione.<br />-   **7**: Usare per archiviare le informazioni del servizio directory contenute nella struttura printER_INFO_7.<br />-   **c**: Usare per archiviare le informazioni sul profilo di colore per una stampante.<br />-   **d**: Usare per archiviare i dati specifici della stampante, ad esempio l'ID hardware di stampante s.<br />-   **s**: Usare per archiviare il descrittore di sicurezza della stampante s.<br />-   **g**: Usare per archiviare le informazioni nella struttura DEVmode globale stampante s.<br />-   **m**: Usare per archiviare le impostazioni minime per la stampante. Ciò equivale a specificare **2** **1!d**, e **g**.<br />-   **u**: Usare per archiviare le informazioni della stampante s per ogni utente struttura DEVmode.|  
|/Sr|Specifica le informazioni relative a una stampante viene ripristinate e come vengono gestiti i conflitti nelle impostazioni. Usare con i valori seguenti, inseriti alla fine della riga di comando:<br /><br />-   **2**: Usare per ripristinare le informazioni contenute nella struttura printER_INFO_2 s della stampante. Questa struttura contiene le informazioni di base relativo alla stampante, ad esempio nome, nome del server, il nome della porta e il nome della condivisione.<br />-   **7**: Usare per ripristinare le informazioni del servizio directory contenute nella struttura printER_INFO_7.<br />-   **c**: Usare per ripristinare le informazioni sul profilo di colore per una stampante.<br />-   **d**: Per ripristinare i dati specifici della stampante, ad esempio l'ID hardware di stampante s.<br />-   **s**: Usare per ripristinare il descrittore di sicurezza della stampante s.<br />-   **g**: Usare per ripristinare le informazioni contenute nella struttura DEVmode globale stampante s.<br />-   **m**: Usare per ripristinare le impostazioni minime per la stampante. Ciò equivale a specificare **2**, **1!d**, e **g**.<br />-   **u** usare per ripristinare le informazioni contenute in s l'opzione per ogni utente struttura DEVmode.<br />-   **r**: se è diverso dal nome della stampante in fase di ripristino per il nome della stampante archiviato nel file, quindi usare il nome della stampante corrente. Questo non può essere specificato con **f**. Se nessuno di essi **r** né **f** specificato e i nomi non corrispondono, viene eseguito il ripristino delle impostazioni.<br />-   **f**: se è diverso dal nome della stampante in fase di ripristino per il nome della stampante archiviato nel file, quindi usare il nome della stampante nel file. Questo non può essere specificato con **r**. Se nessuno di essi **f** né **r** specificato e i nomi non corrispondono, viene eseguito il ripristino delle impostazioni.<br />-   **p**: se il nome della porta nel file di cui è stato ripristinato dalla non corrisponde al nome di porta corrente della stampante in fase di ripristino per, viene utilizzato il nome della porta stampante s corrente.<br />-   **h**: se la stampante in fase di ripristino per potrebbe non essere condivisa usando il nome di condivisione di risorse nel file di impostazioni salvate, quindi tentare di condividere la stampante con il nome della condivisione corrente o un nuovo nome di condivisione generato se non si specifica **H**né **h** specificato e la stampante in fase di ripristino per non può essere condivisi con il nome della condivisione salvato, quindi ripristino ha esito negativo.<br />-   **h**: se la stampante in fase di ripristino per non può essere condiviso con il nome della condivisione salvato, quindi non condividere la stampante. Se nessuno di essi **H** né **h** specificato e la stampante in fase di ripristino per non può essere condivisi con il nome della condivisione salvato, quindi ripristino ha esito negativo.<br />-   **Ho**: se il driver nel file di impostazioni salvate corrisponde il driver della stampante in fase di ripristino per, quindi il ripristino ha esito negativo.|  
|/Xg|Recupera le impostazioni per una stampante.|  
|/Xs|Imposta le impostazioni per una stampante.|  
|/y|Imposta la stampante viene installata come stampante predefinita.|  
|/?|Visualizza la Guida del prodotto per il comando e i parametri associati.|  
|@[file]|Specifica un file di argomento della riga di comando e inserisce direttamente il testo in tale file nella riga di comando.|  
|Modifica parametri|Descrizione|  
|--------------|--------|  
|/a[file]|Specifica il nome del file binario.|  
|/ b [nome]|Specifica il nome di base della stampante.|  
|/c [nome]|Specifica il nome del computer se l'azione da eseguire in un computer remoto.|  
|/f[file]|Specifica il percorso Universal Naming Convention (UNC) e il nome del nome del file. inf o il nome del file di output, a seconda dell'attività che si sta eseguendo. Uso **/F [file]** per specificare un file con estensione inf dipendenti.|  
|/F[file]|Specifica il percorso UNC e il nome di un file. inf che il file con estensione inf specificato con **/f [file]** dipende.|  
|/h [architettura]|Specifica l'architettura del driver. Usare uno dei seguenti: **x86**, **x64**, o **Itanium**.|  
|/j[provider]|Specifica il nome di provider di stampa.|  
|/l[path]|Specifica il percorso UNC in cui si trovano i file del driver della stampante che si siano utilizzando.|  
|/m [modello]|Specifica il nome del modello di driver. (Questo valore può essere specificato nel file con estensione inf).|  
|/n [nome]|Specifica il nome della stampante.|  
|/q|Esegue il comando senza notifiche all'utente.|  
|/r[port]|Specifica il nome della porta.|  
|/u|Specifica di usare il driver della stampante esistenti, se è già installato.|  
|/t[#]|Specifica l'indice in base zero la pagina iniziale.|  
|/v[version]|Specifica la versione del driver. Se non si specifica anche un valore per **/K**, è necessario specificare uno dei valori seguenti: **di tipo 2 - modalità Kernel** oppure **digitare 3 - modalità utente**.|  
|/w|richiede l'immissione di un driver, se il driver non viene trovato nel file con estensione inf specificato da **/f**.|  
|/Y|Specifica che i nomi di stampante non devono essere automaticamente generati.|  
|/z|Specifica di non automaticamente condividere la stampante in corso l'installazione.|  
|/K|la modifica del significato del parametro **/h [architettura]** per accettare **2** invece di **x86**, **3** anziché **x64**, oppure **4** anziché **Itanium**. Modifica anche il valore del parametro **[versione] /v** per accettare **2** al posto del **digitare 2 - modalità Kernel** e **3** al posto di **digita 3 - modalità utente**.|  
|/Z|Condividere la stampante che si sta installando. Usare solo con il **/if** parametro.|  
|/MW [messaggi]|Visualizza un messaggio di avviso all'utente prima di confermare le modifiche specificate nella riga di comando.|  
|/MQ [messaggi]|Visualizza un messaggio di conferma all'utente prima di confermare le modifiche specificate nella riga di comando.|  
|/W [flags]|Specifica i parametri o le opzioni per l'installazione guidata stampante, la stampante aggiunta guidata Driver e la stampante di rete di installazione guidata.<br /><br />**r**: Abilita le procedure guidate di essere riavviato dall'ultima pagina.|  
|/G [flags]|Specifica i parametri globali e le opzioni che si desidera utilizzare.<br /><br />**w**: Elimina gli avvisi di driver di programma di installazione all'utente.|  
## <a name="remarks"></a>Note  
-   Il **PrintUIEntry** (parola chiave) viene fatta distinzione tra maiuscole e minuscole, ed è necessario immettere la sintassi per questo comando con le maiuscole/minuscole esatte illustrato negli esempi in questo argomento.  
-   Visualizzare [esempi](#BKMK_Examples) in questo documento per la sintassi per alcune attività comuni. Per altri esempi, al prompt dei comandi digitare: **DLL, PrintUIEntry rundll32 /?**  
## <a name="BKMK_Examples"></a>Esempi  
Per aggiungere una nuova stampante remota, benché "printer1", per un computer, Client1, che è visibile per l'account utente in cui viene eseguito questo comando, digitare:  
```  
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1  
```  
Per aggiungere una stampante utilizzando la stampante Aggiungi procedura guidata e l'utilizzo di un file. inf, InfFile.inf, che si trova sull'unità c:. in percorso Infpath, tipo:  
```  
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf  
```  
Per eliminare quella esistente, benché "printer1", in un computer, Client1, digitare:  
```  
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1  
```  
Per aggiungere una connessione per computer stampante, tramite "printer2", per tutti gli utenti di un tipo di computer, Client2 (la connessione verrà applicata quando un utente effettua l'accesso):  
```  
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2  
```  
Per eliminare una connessione per computer stampante, tramite "printer2", per tutti gli utenti di un tipo di computer, Client2 (la connessione verrà eliminata quando un utente effettua l'accesso):  
```  
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2  
```  
Per visualizzare le proprietà del server di stampa, printServer1, digitare:  
```  
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1  
```  
Per visualizzare le proprietà di una stampante, printer3, digitare:  
```  
rundll32 printui.dll PrintUIEntry /p /n\\printer3  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [rundll32](rundll32.md)  
-   [Riferimenti ai comandi di stampa](print-command-reference.md)  
