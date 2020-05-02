---
title: change user
description: Argomento di riferimento per il comando Cambia utente, che modifica la modalità di installazione per il server host sessione Desktop remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ae082ebd2b5cf98be891d8f557f9e42d7724d22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716088"
---
# <a name="change-user"></a>change user

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica la modalità di installazione per il server host sessione Desktop remoto.

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
change user {/execute | /install | /query}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| / esecuzione | Consente il mapping dei file ini nella home directory. Si tratta dell'impostazione predefinita. |
| /install | Disabilita il mapping dei file ini nella home directory. Tutti i file ini vengono letti e scritti nella directory di sistema. Quando si installano applicazioni su un server host sessione Desktop remoto, è necessario disabilitare il mapping dei file ini. |
| /query | Visualizza l'impostazione corrente per il mapping dei file ini. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Utilizzare **change user /install** prima di installare un'applicazione per creare file. ini per l'applicazione nella directory di sistema. Questi file vengono utilizzati come origine quando vengono creati i file ini specifici dell'utente. Dopo aver installato l'applicazione, utilizzare **utente o eseguire** per ripristinare il mapping dei file ini standard.

- La prima volta che si esegue l'app, Cerca la Home Directory per i relativi file ini. Se i file ini non vengono trovati nella Home Directory, ma si trovano nella directory di sistema, Servizi Desktop remoto copia i file ini nella Home Directory, assicurando che ogni utente disponga di una copia univoca dei file ini dell'applicazione. Nella home directory vengono creati nuovi file ini.

- Ogni utente deve disporre di una copia dei file. ini per un'applicazione. Ciò impedisce che le istanze in cui i diversi utenti potrebbero avere configurazioni incompatibili dell'applicazione (ad esempio Directory predefinite o diverse risoluzioni dello schermo).

- Quando il sistema esegue **Change user/install**, vengono eseguite diverse operazioni. Tutte le voci del registro di sistema create sono nascoste in **HKEY_LOCAL_MACHINE \Software\microsoft\windows NT\Currentversion\Terminal Server\Install**, nella sottochiave **\SOFTWARE** o **\MACHINE** . Sottochiavi aggiunti **HKEY_CURRENT_USER** vengono copiate nel **\SOFTWARE** sottochiave e le sottochiavi aggiunti **HKEY_LOCAL_MACHINE** vengono copiate nel **\MACHINE** sottochiave. Se l'applicazione esegue una query sulla directory Windows utilizzando chiamate di sistema, ad esempio GetWindowsdirectory, il server Host sessione Desktop remoto restituisce la directory SystemRoot. Se vengono aggiunte le voci di file con estensione ini utilizzando chiamate di sistema, ad esempio WritePrivateProfileString, vengono aggiunti al file. ini nella directory systemroot.

- Quando il sistema torna a **modificare l'utente/Execute Accoda**e l'applicazione tenta di leggere una voce del registro di sistema in **HKEY_CURRENT_USER** che non esiste, Servizi Desktop remoto verifica se una copia della chiave esiste nella sottochiave **\Terminal Server\Install** . In caso contrario, le sottochiavi vengono copiate nel percorso appropriato in **HKEY_CURRRENT_USER**. Se l'applicazione tenta di leggere da un file ini che non esiste, Servizi Desktop remoto cerca tale file nella directory principale del sistema. Se il file. ini si trova nella radice del sistema, viene copiato nella sottodirectory \Windows della home directory dell'utente. Se l'applicazione esegue una query sulla directory Windows, il server Host sessione Desktop remoto restituisce la sottodirectory \Windows della Home directory dell'utente.

- Quando si accede, Servizi Desktop remoto controlla se i file ini di sistema sono più recenti rispetto ai file ini sul computer. Se la versione del sistema più recente, il file ini viene sostituito o unito con la versione più recente. Ciò dipende dal fatto che il bit INISYNC 0x40 sia impostato per il file ini. La versione precedente del file ini viene rinominata come ctx. Se i valori del Registro di sistema sotto la **\Terminal Server\Install** sono più recenti della versione nella sottochiave **HKEY_CURRENT_USER**, la versione delle sottochiavi viene eliminata e sostituita con le nuove sottochiavi da **\Terminal Server\Install**.

## <a name="examples"></a>Esempi

- Per disattivare il mapping dei file ini nella home directory, digitare:

  ```
  change user /install
  ```

- Per abilitare il mapping dei file ini nella home directory, digitare:

  ```
  change user /execute
  ```

- Per visualizzare l'impostazione corrente per il mapping dei file ini, digitare:

  ```
  change user /query
  ```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Change](change.md)

- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
