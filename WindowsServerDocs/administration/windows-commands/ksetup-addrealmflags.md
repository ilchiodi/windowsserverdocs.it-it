---
title: 'che Ksetup: addrealmflags'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26f06e23f40216fe31403280d8946c56825967d6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724715"
---
# <a name="ksetupaddrealmflags"></a>che Ksetup: addrealmflags



Aggiunge i flag dell'area di autenticazione aggiuntivi per l'area di autenticazione specificato.

## <a name="syntax"></a>Sintassi

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome area di autenticazione|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

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

Flag dell'area di autenticazione vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\**<em>nome area di autenticazione</em>. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

È possibile visualizzare i flag dell'area di autenticazione sono disponibili e impostare la visualizzazione dell'output di che ksetup o /dumpstate che ksetup.

## <a name="examples"></a>Esempi

Elencare i flag dell'area di autenticazione disponibili per l'area di autenticazione CONTOSO:
```
Ksetup /listrealmflags
```
Impostare due flag per l'area di autenticazione CONTOSO:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Aggiungere un flag ulteriori che non è attualmente nel set:
```
ksetup /addrealmflags CONTOSO SendAddress
```
Eseguire il **che ksetup** comando per verificare che sia impostato il flag dell'area di autenticazione visualizzando l'output e cercando **flag Realm =**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)