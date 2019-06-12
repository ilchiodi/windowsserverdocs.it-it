---
title: change user
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bedef9f996554a3b5745b47f646204646fdfa9a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434408"
---
# <a name="change-user"></a>change user

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la modalità di installazione per il server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Parametri
> 
> | Parametro |                                                                                                 Descrizione                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / esecuzione  |                                                                Consente il mapping dei file ini nella home directory. Questa è l'impostazione predefinita.                                                                 |
> | /Install  | Disabilita il mapping dei file ini nella home directory. Tutti i file ini vengono letti e scritti nella directory di sistema. Quando si installano applicazioni in un server Host sessione Desktop remoto, è necessario disabilitare mapping dei file ini. |
> |  /query   |                                                                             Visualizza l'impostazione corrente per il mapping dei file ini.                                                                              |
> |    /?     |                                                                                     Visualizza la guida al prompt dei comandi.                                                                                     |
> 
> ## <a name="remarks"></a>Note
> - Utilizzare **change user /install** prima di installare un'applicazione per creare file. ini per l'applicazione nella directory di sistema. Questi file vengono utilizzati come origine quando vengono creati i file ini specifici dell'utente. Dopo aver installato l'applicazione, utilizzare **utente o eseguire** per ripristinare il mapping dei file ini standard.
> - La prima volta che si esegue l'applicazione, la ricerca della home directory per i file ini. Se i file ini non vengono trovati nella home directory, ma si trovano nella directory di sistema, Servizi Desktop remoto copia i file ini nella home directory, assicurando che ogni utente dispone di una copia univoca dei file ini dell'applicazione. Nella home directory vengono creati nuovi file ini.
> - Ogni utente deve disporre di una copia dei file. ini per un'applicazione. Ciò impedisce che le istanze in cui i diversi utenti potrebbero avere configurazioni incompatibili dell'applicazione (ad esempio Directory predefinite o diverse risoluzioni dello schermo).
> - Quando il sistema è in modalità di installazione (vale a dire **change user /install**), si verifica quanto segue. Tutte le voci del Registro di sistema che vengono create sono nascosti sotto **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**, in entrambi i **\SOFTWARE** sottochiave o le **\MACHINE** sottochiave. Sottochiavi aggiunti **HKEY_CURrenT_USER** vengono copiate nella **\SOFTWARE** sottochiave e le sottochiavi aggiunti a **HKEY_LOCAL_MACHINE** vengono copiate nella **\ MACCHINA** sottochiave. Se l'applicazione esegue query nella directory di Windows utilizzando chiamate di sistema, ad esempio GetWindowsdirectory, il server Host sessione Desktop remoto restituisce la directory systemroot. Se vengono aggiunte le voci di file con estensione ini utilizzando chiamate di sistema, ad esempio WritePrivateProfileString, vengono aggiunti al file. ini nella directory systemroot.
> - Quando il sistema torna alla modalità di esecuzione (vale a dire **utente o eseguire**), e l'applicazione tenta di leggere una voce del Registro di sistema sotto **HKEY_CURrenT_USER** che non esiste, Servizi Desktop remoto controlli per verificare l'esistenza di una copia della chiave con il **\Terminal Server\Install** sottochiave. In caso affermativo, le sottochiavi vengono copiate nel percorso appropriato **HKEY_CURrenT_USER**. Se l'applicazione tenta di leggere da un file ini che non esiste, Servizi Desktop remoto cerca tale file nella directory principale del sistema. Se il file. ini si trova nella radice del sistema, viene copiato nella sottodirectory \Windows della home directory dell'utente. Se l'applicazione esegue query nella directory di Windows, il server Host sessione Desktop remoto restituisce la sottodirectory \Windows della home directory dell'utente.
> - Quando si accede, Servizi Desktop remoto controlla se i file ini di sistema sono più recenti rispetto ai file ini sul computer. Se la versione del sistema più recente, il file ini viene sostituito o unito con la versione più recente. Ciò dipende dal fatto che il bit INISYNC 0x40 sia impostato per il file ini. La versione precedente del file ini viene rinominata come ctx. Se i valori del Registro di sistema sotto la **\Terminal Server\Install** sono più recenti della versione nella sottochiave **HKEY_CURrenT_USER**, la versione delle sottochiavi viene eliminata e sostituita con le nuove sottochiavi dal **\Terminal Server\Install**.
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
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
