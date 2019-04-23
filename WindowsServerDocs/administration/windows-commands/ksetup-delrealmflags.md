---
title: ksetup:delrealmflags
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9522d384c4f6996534b47e98dc8d8003bda504cd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877142"
---
# <a name="ksetupdelrealmflags"></a>ksetup:delrealmflags



Rimuove i flag dell'area di autenticazione dall'area di autenticazione specificata.  Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e viene indicato l'area di autenticazione predefinito quando **che Ksetup** viene eseguito.|

## <a name="remarks"></a>Note

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area di autenticazione Kerberos che non si basa sul sistema operativo Windows Server. Computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 è possibile usare un server di Kerberos per amministrare l'autenticazione anziché utilizzare un dominio che esegue un sistema operativo Windows Server e questi sistemi partecipano a un Area di autenticazione Kerberos. Questa voce definisce le funzionalità dell'area di autenticazione. La tabella seguente descrive ciascuno.

|Value|Flag dell'area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutte|Vengono impostati tutti i flag dell'area di autenticazione.|
|0x00|Nessuno|È impostato alcun flag dell'area di autenticazione e nessuna funzionalità aggiuntiva è abilitata.|
|0x01|SendAddress|L'indirizzo IP sarà incluso all'interno di ticket di concessione-ticket.|
|0x02|TcpSupported|Il protocollo TCP (Transmission Control) sia il protocollo UDP (User Datagram) sono supportati in questa area di autenticazione.|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione è trusted per delega.|
|0x08|NcSupported|L'area di autenticazione supporta la rappresentazione canonica di nome, che consente di DNS e dell'area di autenticazione standard di denominazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

Flag dell'area di autenticazione vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** RealmName *. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

È possibile visualizzare i flag dell'area di autenticazione sono disponibili e impostare visualizzando l'output di **che ksetup** o **che ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Esempi

Elencare i flag dell'area di autenticazione disponibili per l'area di autenticazione CONTOSO:
```
Ksetup /listrealmflags
```
Rimuovere due flag presenti nel set:
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Eseguire il **che ksetup** comando per verificare che sia impostato il flag dell'area di autenticazione visualizzando l'output e cercando **flag Realm =**.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)