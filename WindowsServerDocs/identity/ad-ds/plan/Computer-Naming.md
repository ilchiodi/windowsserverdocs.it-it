---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Denominazione dei computer
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2b396778b458976604e527fad0c4649d03239fa6
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624379"
---
# <a name="computer-naming"></a>Denominazione dei computer

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando un computer che esegue il sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista viene aggiunto a un dominio, per impostazione predefinita il computer assegna a se stesso un nome. Il nome assegnato a se stesso comprende il nome host del computer, ovvero il nome del computer in proprietà di sistema, e il nome del Domain Name System (DNS) del dominio Active Directory a cui è stato aggiunto il computer (ovvero il suffisso DNS primario nelle proprietà di sistema). La concatenazione del nome host e del nome DNS del dominio è nota come nome di dominio completo (FQDN). Se, ad esempio, un computer con nome host Server1 viene unito al dominio corp.contoso.com, il nome di dominio completo del computer è server1.corp.contoso.com.

Se un computer dispone già di un nome di dominio DNS che è stato immesso in modo statico in una zona DNS o registrato da un servizio server DNS/Dynamic Host Configuration Protocol (DHCP) integrato, il nome di dominio completo (FQDN) del computer è diverso da quello registrato in precedenza. È possibile fare riferimento al computer con uno dei due nomi.

Per ulteriori informazioni sulle convenzioni di denominazione in Active Directory Domain Services (AD DS), vedere [convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/help/909264/).
