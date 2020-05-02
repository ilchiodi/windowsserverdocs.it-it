---
title: 'che Ksetup: setrealmflags'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a32ea03f4f0e76f03c7a0b505563e6bcf972b80
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724576"
---
# <a name="ksetupsetrealmflags"></a>che Ksetup: setrealmflags



Imposta i flag dell'area di autenticazione per l'area di autenticazione specificata.

## <a name="syntax"></a>Sintassi

```
ksetup /setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName>|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|
|Flag area di autenticazione|Indica uno dei flag seguenti:</br>-SendAddress</br>- TcpSupported</br>-Delegate</br>- NcSupported</br>-RC4|

## <a name="remarks"></a>Osservazioni

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area di autenticazione Kerberos che non si basa sul sistema operativo Windows Server. I computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 possono utilizzare un server Kerberos per amministrare l'autenticazione anziché utilizzare un dominio in cui è in esecuzione un sistema operativo Windows Server e questi sistemi partecipano a un'area di autenticazione Kerberos. Questa voce definisce le funzionalità dell'area di autenticazione. Nella tabella seguente vengono descritte le singole.

|valore|Flag area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutti|Tutti i flag dell'area di autenticazione sono impostati.|
|0x00|nessuno|Nessun flag area di autenticazione è impostato e non sono abilitate funzionalità aggiuntive.|
|0x01|SendAddress|L'indirizzo IP verrà incluso nei ticket di concessione ticket.|
|0x02|TcpSupported|In questa area di autenticazione sono supportati sia il Transmission Control Protocol (TCP) che il protocollo UDP (User Datagram Protocol).|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione sono attendibili per la delega.|
|0x08|NcSupported|Questa area di autenticazione supporta la canonizzazione dei nomi, che consente gli standard di denominazione DNS e area di autenticazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

I flag dell'area di autenticazione vengono archiviati nel registro di sistema in **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\**<em>RealmName</em>. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Per popolare il registro di sistema, è possibile usare il comando [che Ksetup: addrealmflags](ksetup-addrealmflags.md) .

È possibile visualizzare i flag dell'area di autenticazione disponibili e impostati visualizzando l'output di **che Ksetup**.

## <a name="examples"></a>Esempi

Elencare i flag di area di autenticazione disponibili e impostati per l'area di autenticazione CONTOSO:
```
ksetup
```
Impostare due flag che non sono attualmente impostati:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Eseguire il **che ksetup** comando per verificare che sia impostato il flag dell'area di autenticazione visualizzando l'output e cercando **flag Realm =**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)