---
title: ksetup:listrealmflags
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7db6caf4e63ea59fa40892679d3de0cfaca661e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438021"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



Elenca i flag dell'area di autenticazione disponibili che possono essere restituiti da **che ksetup**. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Parametri

Nessuno

## <a name="remarks"></a>Note

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area autenticazione Kerberos non basate su Windows. Computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 è possibile usare un server di Kerberos non basate su Windows per amministrare l'autenticazione invece di usare un dominio che esegue un sistema operativo Windows Server. Questi sistemi partecipano a un'area di autenticazione Kerberos anziché in un dominio di Windows. Questa voce definisce le funzionalità dell'area di autenticazione. La tabella seguente descrive ciascuno.

|Value|Flag dell'area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutte|Vengono impostati tutti i flag dell'area di autenticazione.|
|0x00|Nessuno|È impostato alcun flag dell'area di autenticazione e nessuna funzionalità aggiuntiva è abilitata.|
|0x01|SendAddress|L'indirizzo IP sarà incluso all'interno di ticket granting ticket.|
|0x02|TcpSupported|Il protocollo TCP (Transmission Control) e il protocollo UDP (User Datagram) sono supportati in questa area di autenticazione.|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione è trusted per delega.|
|0x08|NcSupported|L'area di autenticazione supporta la rappresentazione canonica di nome, che consente di DNS e dell'area di autenticazione standard di denominazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

Flag dell'area di autenticazione vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>nome area di autenticazione</em>. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

## <a name="BKMK_Examples"></a>Esempi

Elencare i flag dell'area di autenticazione noti nel computer in uso:
```
ksetup /listrealmflags
```
Impostare i flag dell'area di autenticazione disponibili che **che Ksetup** non conosce digitando uno dei seguenti comandi nella riga di comando:
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)