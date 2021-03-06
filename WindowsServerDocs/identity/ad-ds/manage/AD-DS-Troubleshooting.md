---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: Risoluzione dei problemi relativi ad Active Directory Domain Services
description: Panoramica della sezione relativa alla risoluzione dei problemi per servizi di dominio Active Directory
ms.author: joflore
author: MicrosoftGuyJFlo
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b9e1cf812234bc3b0bd81db7045ed83208f3a0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824364"
---
# <a name="ad-ds-troubleshooting"></a>Risoluzione dei problemi relativi ad Active Directory Domain Services

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Questa sezione include indicazioni e procedure per la risoluzione dei problemi che possono verificarsi durante la replica Active Directory. Viene illustrato come rispondere alle voci del registro eventi del servizio directory e come interpretare i messaggi che potrebbero essere segnalati da strumenti quali Repadmin. exe e Dcdiag. exe.

Repadmin. exe e Dcdiag. exe sono disponibili in tutti i controller di dominio che eseguono Windows Server 2012 R2 o versioni successive. Per ulteriori informazioni sull'utilizzo di questi strumenti per la risoluzione dei problemi, vedere gli articoli seguenti.

- [Configurazione di un computer per la risoluzione dei problemi Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [Risoluzione dei problemi di replica di Active Directory](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

Un'altra tecnologia utile è Event Tracing for Windows (ETW). È possibile utilizzare ETW per risolvere i problemi di comunicazione LDAP tra i controller di dominio. Per ulteriori informazioni, vedere [utilizzo di ETW per la risoluzione dei problemi relativi alle connessioni LDAP](../manage/troubleshoot/troubleshoot-ldap-using-etw.md).

È anche possibile installare Strumenti di amministrazione remota del server (strumenti di amministrazione remota del server) in un server membro che esegue Windows 10. Per informazioni sull'installazione di strumenti di amministrazione remota del server, vedere [strumenti di amministrazione remota del server](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
