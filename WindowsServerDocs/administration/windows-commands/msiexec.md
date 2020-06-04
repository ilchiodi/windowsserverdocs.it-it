---
title: msiexec
description: Argomento di riferimento per il comando msiexec, che fornisce i mezzi per installare, modificare ed eseguire operazioni su Windows Installer dalla riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 122eb0ce-ecbc-4909-a52a-15c3413619af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f84df28104f581873fe1fd86a3abd6a51532b020
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354341"
---
# <a name="msiexec"></a>msiexec

Fornisce i mezzi per installare, modificare ed eseguire operazioni su Windows Installer dalla riga di comando.

## <a name="install-options"></a>Opzioni di installazione

Impostare il tipo di installazione per l'avvio di un pacchetto di installazione.

### <a name="syntax"></a>Sintassi

```
msiexec.exe [/i][/a][/j{u|m|/g|/t}][/x] <path_to_package>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /i | Specifica l'installazione normale. |
| /a | Specifica l'installazione amministrativa. |
| /ju | Annunciare il prodotto all'utente corrente. |
| /JM | Annunciare il prodotto a tutti gli utenti. |
| /j/g | Specifica l'identificatore della lingua utilizzato dal pacchetto annunciato. |
| /j/t | Applica la trasformazione al pacchetto annunciato. |
| /x | Disinstalla il pacchetto. |
| `<path_to_package>` | Specifica il percorso e il nome del file del pacchetto di installazione. |

#### <a name="examples"></a>Esempio

Per installare un pacchetto denominato *example. msi* dall'unità C:, usando un processo di installazione normale, digitare:

```
msiexec.exe /i "C:\example.msi"
```

## <a name="display-options"></a>Opzioni di visualizzazione

È possibile configurare ciò che un utente visualizza durante il processo di installazione, in base all'ambiente di destinazione. Se, ad esempio, si distribuisce un pacchetto a tutti i client per l'installazione manuale, dovrebbe essere disponibile un'interfaccia utente completa. Tuttavia, se si distribuisce un pacchetto usando Criteri di gruppo, che non richiede alcuna interazione con l'utente, non dovrebbe essere presente alcuna interfaccia utente.

### <a name="syntax"></a>Sintassi

```
msiexec.exe /i <path_to_package> [/quiet][/passive][/q{n|b|r|f}]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| `<path_to_package>` | Specifica il percorso e il nome del file del pacchetto di installazione. |
| /quiet | Specifica la modalità non interattiva, ovvero non è necessaria alcuna interazione con l'utente. |
| /passive | Specifica la modalità automatica, ovvero l'installazione Mostra solo un indicatore di stato. |
| /qn | Specifica che non è presente alcuna interfaccia utente durante il processo di installazione. |
| /Qn + | Specifica che non è presente alcuna interfaccia utente durante il processo di installazione, ad eccezione di una finestra di dialogo finale alla fine. |
| /qb | Specifica che è presente un'interfaccia utente di base durante il processo di installazione. |
| /QB + | Specifica che è presente un'interfaccia utente di base durante il processo di installazione, inclusa una finestra di dialogo finale alla fine. |
| /QR | Specifica un'esperienza dell'interfaccia utente ridotta durante il processo di installazione. |
| /QF | Specifica un'esperienza completa dell'interfaccia utente durante il processo di installazione. |

##### <a name="remarks"></a>Commenti

- La casella modale non viene visualizzata se l'installazione viene annullata dall'utente. È possibile usare **qb +!** o **qb! +** per nascondere il pulsante **Annulla** .

#### <a name="examples"></a>Esempio

Per installare il pacchetto *C:\EXAMPLE.msi*, usando un processo di installazione normale e nessuna interfaccia utente, digitare:

```
msiexec.exe /i "C:\example.msi" /qn
```

## <a name="restart-options"></a>Opzioni di riavvio

Se il pacchetto di installazione sovrascrive i file o tenta di modificare i file in uso, potrebbe essere necessario riavviare il computer prima del completamento dell'installazione.

### <a name="syntax"></a>Sintassi

```
msiexec.exe /i <path_to_package> [/norestart][/promptrestart][/forcerestart]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| `<path_to_package>` | Specifica il percorso e il nome del file del pacchetto di installazione. |
| /norestart | Arresta il riavvio del dispositivo al termine dell'installazione. |
| /promptrestart | Richiede all'utente se è necessario riavviare il computer. |
| /forcerestart | Riavvia il dispositivo al termine dell'installazione. |

#### <a name="examples"></a>Esempio

Per installare il pacchetto *C:\EXAMPLE.msi*, usando un processo di installazione normale senza riavvio alla fine, digitare:

```
msiexec.exe /i "C:\example.msi" /norestart
```

## <a name="logging-options"></a>Opzioni di registrazione

Se è necessario eseguire il debug del pacchetto di installazione, è possibile impostare i parametri per creare un file di log con informazioni specifiche.

### <a name="syntax"></a>Sintassi

```
msiexec.exe [/i][/x] <path_to_package> [/L{i|w|e|a|r|u|c|m|o|p|v|x+|!|*}] <path_to_log>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /i | Specifica l'installazione normale. |
| /x | Disinstalla il pacchetto. |
| `<path_to_package>` | Specifica il percorso e il nome del file del pacchetto di installazione. |
| /li | Attiva la registrazione e include i messaggi di stato nel file di log di output. |
| /lw | Attiva la registrazione e include avvisi non irreversibili nel file di log di output. |
| /le | Attiva la registrazione e include tutti i messaggi di errore nel file di log di output. |
| /la | Attiva la registrazione e include informazioni sul momento in cui un'azione è stata avviata nel file di log di output. |
| /lr | Attiva la registrazione e include i record specifici dell'azione nel file di log di output. |
| /lu | Attiva la registrazione e include le informazioni sulla richiesta dell'utente nel file di log di output. |
| /lc | Attiva la registrazione e include i parametri iniziali dell'interfaccia utente nel file di log di output. |
| /LM | Attiva la registrazione e include informazioni sulla memoria insufficiente o sull'uscita irreversibile nel file di log di output. |
| /lo | Attiva la registrazione e include i messaggi di spazio fuori disco nel file di log di output. |
| /lp | Attiva la registrazione e include le proprietà del terminale nel file di log di output. |
| /lp | Attiva la registrazione e include le proprietà del terminale nel file di log di output. |
| /LV | Attiva la registrazione e include l'output dettagliato nel file di log di output. |
| /lp | Attiva la registrazione e include le proprietà del terminale nel file di log di output. |
| /lx | Attiva la registrazione e include informazioni di debug aggiuntive nel file di log di output. |
| /l + | Attiva la registrazione e aggiunge le informazioni a un file di log esistente. |
| /l! | Attiva la registrazione e Scarica ogni riga nel file di log. |
| /l | Attiva la registrazione e registra tutte le informazioni, eccetto le informazioni dettagliate (**/LV**) o le informazioni di debug aggiuntive (**/LX**). |
| `<path_to_logfile>` | Specifica il percorso e il nome del file di log di output. |

#### <a name="examples"></a>Esempio

Per installare il pacchetto *C:\EXAMPLE.msi*, usando un processo di installazione normale con tutte le informazioni di registrazione fornite, incluso l'output dettagliato, e archiviando il file di log di output in *C:\package.log*, digitare:

```
msiexec.exe /i "C:\example.msi" /L*V "C:\package.log"
```

## <a name="update-options"></a>Opzioni di aggiornamento

È possibile applicare o rimuovere gli aggiornamenti utilizzando un pacchetto di installazione.

### <a name="syntax"></a>Sintassi

```
msiexec.exe [/p][/update][/uninstall[/package<product_code_of_package>]] <path_to_package>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| / p | Installa una patch. Se si esegue l'installazione invisibile all'utente, è necessario impostare anche la proprietà REINSTALLMODE su *ecmus* e reinstallare su *All*. In caso contrario, la patch aggiorna solo il file MSI memorizzato nella cache del dispositivo di destinazione. |
| /Update | Opzione Installa patch. Se si stanno applicando più aggiornamenti, è necessario separarli con un punto e virgola (;). |
| /Package | Installa o configura un prodotto. |

#### <a name="examples"></a>Esempio

```
msiexec.exe /p "C:\MyPatch.msp"
msiexec.exe /p "C:\MyPatch.msp" /qb REINSTALLMODE="ecmus" REINSTALL="ALL"
msiexec.exe /update "C:\MyPatch.msp"
```

```
msiexec.exe /uninstall {1BCBF52C-CD1B-454D-AEF7-852F73967318} /package {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

Dove il primo GUID è il GUID della patch e il secondo è il codice prodotto MSI a cui è stata applicata la patch.

## <a name="repair-options"></a>Opzioni di ripristino

È possibile utilizzare questo comando per ripristinare un pacchetto installato.

### <a name="syntax"></a>Sintassi

```
msiexec.exe [/f{p|o|e|d|c|a|u|m|s|v}] <product_code>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /fp | Ripristina il pacchetto se manca un file. |
| /fo | Ripristina il pacchetto se manca un file o se è installata una versione precedente. |
| /Fe | Ripristina il pacchetto se il file è mancante o se è installata una versione uguale o precedente. |
| /FD | Ripristina il pacchetto se il file è mancante o se è installata una versione diversa. |
| /fc | Ripristina il pacchetto se il file è mancante o se checksum non corrisponde al valore calcolato. |
| /fa | Forza la reinstallazione di tutti i file. |
| /fu | Ripristina tutte le voci del registro di sistema obbligatorie specifiche dell'utente. |
| /FM | Ripristina tutte le voci del registro di sistema richieste specifiche del computer. |
| /FS | Ripristina tutti i collegamenti esistenti. |
| /fc | Viene eseguito dall'origine e memorizza nuovamente nella cache il pacchetto locale. |

#### <a name="examples"></a>Esempio

Per forzare la reinstallazione di tutti i file in base al codice prodotto MSI da ripristinare, *{AAD3D77A-7476-469F-ADF4-04424124E91D}*, digitare:

```
msiexec.exe /fa {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

## <a name="set-public-properties"></a>Imposta proprietà pubbliche

È possibile impostare le proprietà pubbliche tramite questo comando. Per informazioni sulle proprietà disponibili e su come impostarle, vedere [proprietà pubbliche](https://docs.microsoft.com/windows/win32/msi/public-properties).

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Opzioni della riga di comando msiexec. exe](https://docs.microsoft.com/windows/win32/msi/command-line-options)

- [Opzioni della riga di comando del programma di installazione standard](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options)
