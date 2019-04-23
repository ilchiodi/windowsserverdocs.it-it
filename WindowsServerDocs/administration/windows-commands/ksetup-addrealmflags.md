---
title: ksetup:addrealmflags
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbc878bd0ee25ad92c640710ab6b46bbc0eaf62a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827682"
---
# <a name="ksetupaddrealmflags"></a>ksetup:addrealmflags



Aggiunge i flag dell'area di autenticazione aggiuntivi per l'area di autenticazione specificato. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome area di autenticazione|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

## <a name="remarks"></a>Note

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area di autenticazione Kerberos che non si basa sul sistema operativo Windows Server. Computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 è possibile usare un server di Kerberos per amministrare l'autenticazione anziché utilizzare un dominio che esegue un sistema operativo Windows Server e questi sistemi partecipano a un Area di autenticazione Kerberos. Questa voce definisce le funzionalità dell'area di autenticazione. La tabella seguente descrive ciascuno.

|Value|Flag dell'area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutte|Vengono impostati tutti i flag dell'area di autenticazione.|
|0x00|Nessuno|È impostato alcun flag dell'area di autenticazione e nessuna funzionalità aggiuntiva è abilitata.|
|0x01|SendAddress|L'indirizzo IP sarà incluso all'interno di ticket granting ticket.|
|0x02|TcpSupported|Il protocollo TCP (Transmission Control) sia il protocollo UDP (User Datagram) sono supportati in questa area di autenticazione.|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione è trusted per delega.|
|0x08|NcSupported|L'area di autenticazione supporta la rappresentazione canonica di nome, che consente di DNS e dell'area di autenticazione standard di denominazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

Flag dell'area di autenticazione vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** Realm-name *. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

È possibile visualizzare i flag dell'area di autenticazione sono disponibili e impostare la visualizzazione dell'output di che ksetup o /dumpstate che ksetup.

## <a name="BKMK_Examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)