---
title: Client Desktop remoto - configurazione supportata
description: Informazioni sui computer a cui puoi accedere tramite i client Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748969"
---
# <a name="remote-desktop-client---supported-configuration"></a>Client Desktop remoto - configurazione supportata

## <a name="supported-pcs"></a>PC supportati
È possibile connettersi ai computer che eseguono sistemi operativi Windows seguenti:
- Windows 10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
- Windows 8 Professional
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 Ultimate
- Windows 7 Ultimate
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

I seguenti computer possono eseguire il gateway Desktop remoto:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

I sistemi operativi seguenti può essere utilizzato come server Accesso Web desktop remoto o RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Edizioni e versioni di Windows non supportata

Il client Desktop remoto non consentono di connettersi a queste versioni di Windows e le edizioni:

- Windows 7 Starter
- Home page di Windows 7
- Windows 8 Home
- Windows 8.1 Home
- Windows 10 Home

Se si desidera accedere ai computer che dispongono di una di queste versioni di Windows installate, è consigliabile che l'aggiornamento a una versione di Windows che supporta il protocollo RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>La messaggistica Gateway Desktop remoto non è supportata.
Il Client Desktop remoto non supporta la messaggistica Gateway Desktop remoto. Verificare che il Desktop risorsa criteri di accesso remoto (criterio di autorizzazione RISORSE desktop remoto) per il server Gateway Desktop remoto non è specificato **consentire solo ai computer con il supporto per la messaggistica Gateway Desktop remoto** o non sarà in grado di connettersi.