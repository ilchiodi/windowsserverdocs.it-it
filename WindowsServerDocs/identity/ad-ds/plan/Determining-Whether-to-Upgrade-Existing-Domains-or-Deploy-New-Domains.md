---
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: Scelta tra aggiornamento di domini esistenti o distribuzione di nuovi domini
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 60f3dca9f4930fe9d1a717d741a818194a3db0f0
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624279"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>Scelta tra aggiornamento di domini esistenti o distribuzione di nuovi domini

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni dominio della progettazione sarà un nuovo dominio o un dominio aggiornato esistente. Gli utenti di domini esistenti che non vengono aggiornati devono essere spostati in nuovi domini.

Lo stato di trasferimento degli account tra domini può influisca sugli utenti finali. Prima di decidere se spostare gli utenti in un nuovo dominio o aggiornare i domini esistenti, valutare i vantaggi amministrativi a lungo termine di un nuovo dominio di servizi di dominio Active Directory rispetto al costo di spostamento degli utenti nel dominio.

Per ulteriori informazioni sull'aggiornamento dei domini Active Directory a Windows Server 2008, vedere [aggiornamento di domini Active Directory a domini di servizi di dominio Active Directory di Windows server 2008 e Windows server 2008 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

Per ulteriori informazioni sulla ristrutturazione dei domini di servizi di dominio Active Directory all'interno e tra foreste, vedere la [Guida alla migrazione e alla ristrutturazione dei domini Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)).

Per un foglio di lavoro che consente di documentare i piani per i domini nuovi e aggiornati, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da supporto [per i processi per Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e aprire "pianificazione del dominio" (DSSLOGI_5. doc).
