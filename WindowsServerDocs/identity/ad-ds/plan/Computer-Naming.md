---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Denominazione dei computer
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842162"
---
# <a name="computer-naming"></a>Denominazione dei computer

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando un computer che eseguono il sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista viene aggiunto un dominio, per impostazione predefinita nel computer assegna a se stesso un nome. Il nome stesso assegna comprende il nome host del computer (vale a dire, nome del Computer nelle proprietà del sistema) e il nome di sistema DNS (Domain Name) del dominio Active Directory che il computer aggiunto (vale a dire, suffisso DNS primario nelle proprietà del sistema). La concatenazione del nome host e il nome DNS del dominio è noto come il nome di dominio completo (FQDN). Ad esempio, se un computer con l'host denominato Server1 join al dominio corp.contoso.com, il nome FQDN del computer è server1.corp.contoso.com.  
  
Se un computer ha già un nome di dominio DNS diverso in modo statico è stato immesso in una zona DNS o registrato dal servizio Server DNS dinamici Host Configuration Protocol (DHCP) integrato, il nome FQDN del computer è diverso dal nome che è stato registrato in precedenza. Il computer può fare riferimento il nome.  
  
Per altre informazioni sulle convenzioni di denominazione in Active Directory Domain Services (AD DS), vedere le convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


