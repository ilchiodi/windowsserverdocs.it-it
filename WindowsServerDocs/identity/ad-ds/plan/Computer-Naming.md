---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: La denominazione di computer
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d0dbe2da76a1cf3d1a4dd74183b5dd7106ef17b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="computer-naming"></a>La denominazione di computer

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando un computer che eseguono il sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista viene aggiunto un dominio, per impostazione predefinita, il computer assegna automaticamente un nome. Il nome viene assegnato automaticamente comprende il nome host del computer (vale a dire, nome del Computer nelle proprietà del sistema) e il nome del sistema DNS (Domain Name) del dominio Active Directory che il computer aggiunto (vale a dire suffisso DNS primario nelle proprietà del sistema). La concatenazione del nome host e il nome DNS del dominio è noto come il nome di dominio completo (FQDN). Ad esempio, se un computer con il nome host Server1 viene aggiunto al dominio corp.contoso.com, il nome FQDN del computer è server1.corp.contoso.com.  
  
Se un computer ha già un nome di dominio DNS diverso che è stato in modo statico immesso in una zona DNS o registrato dal servizio Server DNS/dinamico DHCP Host Configuration Protocol () integrato, il nome FQDN del computer è diverso dal nome registrato in precedenza. Il computer può fare riferimento il nome.  
  
Per ulteriori informazioni sulle convenzioni di denominazione in servizi di dominio Active Directory (AD DS), vedere le convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


