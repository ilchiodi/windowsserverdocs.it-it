---
title: 'che Ksetup: listrealmflags'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0646be8daaad4bc3303389cfca1f3a09136fe1a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724620"
---
# <a name="ksetuplistrealmflags"></a>che Ksetup: listrealmflags



Elenca i flag dell'area di autenticazione disponibili che possono essere restituiti da **che ksetup**.

## <a name="syntax"></a>Sintassi

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Parametri

nessuno

## <a name="remarks"></a>Osservazioni

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area autenticazione Kerberos non basate su Windows. I computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 possono utilizzare un server Kerberos non basato su Windows per amministrare l'autenticazione anziché utilizzare un dominio in cui è in esecuzione un sistema operativo Windows Server. Questi sistemi partecipano a un'area di autenticazione Kerberos anziché in un dominio di Windows. Questa voce definisce le funzionalità dell'area di autenticazione. Nella tabella seguente vengono descritte le singole.

|valore|Flag area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutti|Tutti i flag dell'area di autenticazione sono impostati.|
|0x00|nessuno|Nessun flag area di autenticazione è impostato e non sono abilitate funzionalità aggiuntive.|
|0x01|SendAddress|L'indirizzo IP verrà incluso nei ticket di concessione ticket.|
|0x02|TcpSupported|Il protocollo TCP (Transmission Control) e il protocollo UDP (User Datagram) sono supportati in questa area di autenticazione.|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione sono attendibili per la delega.|
|0x08|NcSupported|L'area di autenticazione supporta la rappresentazione canonica di nome, che consente di DNS e dell'area di autenticazione standard di denominazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

I flag dell'area di autenticazione vengono archiviati nel registro di sistema HKEY_LOCAL_MACHINE<em>nome dell'area di autenticazione</em> **\system\currentcontrolset\control\lsa\kerberos\domains\\**. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Per popolare il registro di sistema, è possibile usare il comando [che Ksetup: addrealmflags](ksetup-addrealmflags.md) .

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)