---
title: 'che Ksetup: setrealmflags'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29ef5de8130fa9b10ce6d999e16765b0aa2697a4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841284"
---
# <a name="ksetupsetrealmflags"></a>che Ksetup: setrealmflags



Imposta i flag dell'area di autenticazione per l'area di autenticazione specificata. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|
|Flag area di autenticazione|Indica uno dei flag seguenti:</br>-SendAddress</br>- TcpSupported</br>-Delegate</br>- NcSupported</br>-RC4|

## <a name="remarks"></a>Note

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area di autenticazione Kerberos che non si basa sul sistema operativo Windows Server. I computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 possono utilizzare un server Kerberos per amministrare l'autenticazione anziché utilizzare un dominio in cui è in esecuzione un sistema operativo Windows Server e questi sistemi partecipano a un'area di autenticazione Kerberos. Questa voce definisce le funzionalità dell'area di autenticazione. Nella tabella seguente vengono descritte le singole.

|Valore|Flag area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutte|Tutti i flag dell'area di autenticazione sono impostati.|
|0x00|None|Nessun flag area di autenticazione è impostato e non sono abilitate funzionalità aggiuntive.|
|0x01|SendAddress|L'indirizzo IP verrà incluso nei ticket di concessione ticket.|
|0x02|TcpSupported|In questa area di autenticazione sono supportati sia il Transmission Control Protocol (TCP) che il protocollo UDP (User Datagram Protocol).|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione sono attendibili per la delega.|
|0x08|NcSupported|Questa area di autenticazione supporta la canonizzazione dei nomi, che consente gli standard di denominazione DNS e area di autenticazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

I flag dell'area di autenticazione vengono archiviati nel registro di sistema in **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>RealmName</em>. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

È possibile visualizzare i flag dell'area di autenticazione disponibili e impostati visualizzando l'output di **che Ksetup**.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Elencare i flag di area di autenticazione disponibili e impostati per l'area di autenticazione CONTOSO:
```
ksetup
```
Impostare due flag che non sono attualmente impostati:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Eseguire il **che ksetup** comando per verificare che sia impostato il flag dell'area di autenticazione visualizzando l'output e cercando **flag Realm =** .

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)