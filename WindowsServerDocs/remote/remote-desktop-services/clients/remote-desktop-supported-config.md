---
title: Client Desktop remoto - configurazione supportata
description: Informazioni su quali PC è possibile accedere utilizzando i client Desktop remoto
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
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978048"
---
# <a name="remote-desktop-client---supported-configuration"></a>Client Desktop remoto - configurazione supportata

## <a name="supported-pcs"></a>PC supportati
È possibile connettersi al computer che eseguono sistemi operativi Windows seguenti:
- Windows10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
- Windows 8 Professional
- Windows7 Professional
- Windows7 Enterprise
- Windows7 Ultimate
- Windows7 Ultimate
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- WindowsServer 2016
- Windows Server multipunto 2011
- Multipunto Windows Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

I computer seguenti possono essere eseguito il gateway di Desktop remoto:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- WindowsServer 2016
- Windows Small Business Server 2011

Accesso Web desktop remoto o RemoteApp server può fungere da sistemi operativi seguenti:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- WindowsServer 2016

## <a name="unsupported-windows-versions-and-editions"></a>Edizioni e versioni di Windows non supportati

Il client Desktop remoto non si connetterà per le versioni di Windows ed edizioni:

- Windows7 Starter
- Home page di Windows 7
- Home page di Windows 8
- Home page di Windows 8.1
- Windows10 Home

Se si desidera accedere ai computer con una di queste versioni di Windows installate, è consigliabile che aggiornare a una versione di Windows che supporta il protocollo RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Messaggistica Gateway Desktop remoto non è supportata.
Client Desktop remoto non supporta la messaggistica Gateway Desktop remoto. Verificare che il Desktop risorse criteri di accesso remoto (criteri di accesso remoto RD) per il server Gateway Desktop remoto non è specificata **consentire solo i computer con il supporto per la messaggistica Gateway Desktop remoto** oppure non sarà in grado di connettersi.