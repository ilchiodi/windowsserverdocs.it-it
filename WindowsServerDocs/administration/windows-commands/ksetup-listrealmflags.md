---
title: 'che Ksetup: listrealmflags'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 265f988d85deb7602e91677626d207bc3a7873ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841494"
---
# <a name="ksetuplistrealmflags"></a>che Ksetup: listrealmflags



Elenca i flag dell'area di autenticazione disponibili che possono essere restituiti da **che ksetup**. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Parametri

None

## <a name="remarks"></a>Note

I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area autenticazione Kerberos non basate su Windows. I computer che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 possono utilizzare un server Kerberos non basato su Windows per amministrare l'autenticazione anziché utilizzare un dominio in cui è in esecuzione un sistema operativo Windows Server. Questi sistemi partecipano a un'area di autenticazione Kerberos anziché in un dominio di Windows. Questa voce definisce le funzionalità dell'area di autenticazione. Nella tabella seguente vengono descritte le singole.

|Valore|Flag area di autenticazione|Descrizione|
|-----|----------|-----------|
|0xF|Tutte|Tutti i flag dell'area di autenticazione sono impostati.|
|0x00|None|Nessun flag area di autenticazione è impostato e non sono abilitate funzionalità aggiuntive.|
|0x01|SendAddress|L'indirizzo IP verrà incluso nei ticket di concessione ticket.|
|0x02|TcpSupported|Il protocollo TCP (Transmission Control) e il protocollo UDP (User Datagram) sono supportati in questa area di autenticazione.|
|0x04|Delegato|Tutti gli utenti in questa area di autenticazione sono attendibili per la delega.|
|0x08|NcSupported|L'area di autenticazione supporta la rappresentazione canonica di nome, che consente di DNS e dell'area di autenticazione standard di denominazione.|
|0x80|RC4|L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS.|

Flag dell'area di autenticazione vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>nome area di autenticazione</em>. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. È possibile utilizzare il [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando per popolare il Registro di sistema.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)