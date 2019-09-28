---
title: change user
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 771e39182c17b9a6710e49eff2f5302e539bdbb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379567"
---
# <a name="change-user"></a>change user

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la modalità di installazione per il server host sessione Desktop remoto (host sessione Desktop remoto).
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Parametri
> 
> | Parametro |                                                                                                 Descrizione                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / esecuzione  |                                                                Consente il mapping dei file ini nella home directory. Questa è l'impostazione predefinita.                                                                 |
> | /Install  | Disabilita il mapping dei file ini nella home directory. Tutti i file ini vengono letti e scritti nella directory di sistema. Quando si installano applicazioni in un server Host sessione Desktop remoto, è necessario disabilitare il mapping dei file ini. |
> |  /query   |                                                                             Visualizza l'impostazione corrente per il mapping dei file ini.                                                                              |
> |    /?     |                                                                                     Visualizza la guida al prompt dei comandi.                                                                                     |
> 
> ## <a name="remarks"></a>Note
> - Utilizzare **change user /install** prima di installare un'applicazione per creare file. ini per l'applicazione nella directory di sistema. Questi file vengono utilizzati come origine quando vengono creati i file ini specifici dell'utente. Dopo aver installato l'applicazione, utilizzare **utente o eseguire** per ripristinare il mapping dei file ini standard.
> - La prima volta che si esegue l'applicazione, la ricerca della home directory per i file ini. Se i file ini non vengono trovati nella home directory, ma si trovano nella directory di sistema, Servizi Desktop remoto copia i file ini nella home directory, assicurando che ogni utente dispone di una copia univoca dei file ini dell'applicazione. Nella home directory vengono creati nuovi file ini.
> - Ogni utente deve disporre di una copia dei file. ini per un'applicazione. Ciò impedisce che le istanze in cui i diversi utenti potrebbero avere configurazioni incompatibili dell'applicazione (ad esempio Directory predefinite o diverse risoluzioni dello schermo).
> - Quando il sistema è in modalità di installazione (vale a dire **change user /install**), si verifica quanto segue. Tutte le voci del registro di sistema create sono nascoste in **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**, nella sottochiave **\SOFTWARE** o **\MACHINE** . Le sottochiavi aggiunte a **HKEY_CURrenT_USER** vengono copiate nella sottochiave **\SOFTWARE** e le sottochiavi aggiunte a **HKEY_LOCAL_MACHINE** vengono copiate nella sottochiave **\MACHINE** . Se l'applicazione esegue una query sulla directory Windows utilizzando chiamate di sistema, ad esempio GetWindowsdirectory, il server Host sessione Desktop remoto restituisce la directory SystemRoot. Se vengono aggiunte le voci di file con estensione ini utilizzando chiamate di sistema, ad esempio WritePrivateProfileString, vengono aggiunti al file. ini nella directory systemroot.
> - Quando il sistema torna alla modalità di esecuzione (ovvero **modifica utente/Execute Accoda**) e l'applicazione tenta di leggere una voce del registro di sistema in **HKEY_CURrenT_USER** che non esiste, Servizi Desktop remoto verifica se una copia della chiave esiste in sottochiave **Server\Install di \Terminal** . In caso contrario, le sottochiavi vengono copiate nel percorso appropriato in **HKEY_CURrenT_USER**. Se l'applicazione tenta di leggere da un file ini che non esiste, Servizi Desktop remoto cerca tale file nella directory principale del sistema. Se il file. ini si trova nella radice del sistema, viene copiato nella sottodirectory \Windows della home directory dell'utente. Se l'applicazione esegue una query sulla directory Windows, il server Host sessione Desktop remoto restituisce la sottodirectory \Windows della Home directory dell'utente.
> - Quando si accede, Servizi Desktop remoto controlla se i file ini di sistema sono più recenti rispetto ai file ini sul computer. Se la versione del sistema più recente, il file ini viene sostituito o unito con la versione più recente. Ciò dipende dal fatto che il bit INISYNC 0x40 sia impostato per il file ini. La versione precedente del file ini viene rinominata come ctx. Se i valori del registro di sistema nella sottochiave **\Terminal Server\Install** sono più recenti della versione in **HKEY_CURrenT_USER**, la versione delle sottochiavi viene eliminata e sostituita con le nuove sottochiavi di **\Terminal Server\Install**.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per disattivare il mapping dei file ini nella home directory, digitare:
>   ```
>   change user /install
>   ```
> - Per abilitare il mapping dei file ini nella home directory, digitare:
>   ```
>   change user /execute
>   ```
> - Per visualizzare l'impostazione corrente per il mapping dei file ini, digitare:
>   ```
>   change user /query
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [ &#40;Servizi Desktop remoto&#41; riferimento ai comandi di Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
