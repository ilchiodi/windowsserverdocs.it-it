---
title: Indicazioni per lo spostamento in Windows Server 2016
description: Indicazioni per il passaggio a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/18/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 08a92188446d018edd36a638abb30745721bd601
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63687470"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>Indicazioni per lo spostamento in Windows Server 2016

>Si applica a: Windows Server 2016


|Se si utilizza:|Windows Server 2012 R2 o Windows Server 2012|Windows Server 2008 R2 o Windows Server 2008|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Infrastruttura dei ruoli di Windows Server**|Scegliere l'aggiornamento o la migrazione in base alle [linee guida specifiche per i ruoli](https://technet.microsoft.com/windowsserver/jj554790).|- Per sfruttare i vantaggi offerti dalle nuove funzionalità di Windows Server 2016, distribuire nuovo hardware o installare Windows Server 2016 in una macchina virtuale in un host esistente. Alcune nuove funzionalità hanno un funzionamento ottimale in un host fisico di Windows Server 2016 che esegue Hyper-V. <br>- Seguire le [linee guida specifiche per i ruoli](https://technet.microsoft.com/windowsserver/jj554790).|
|**Gestione server Microsoft e carichi di lavoro dell'applicazione**|- Gli aggiornamenti dell'applicazione devono includere la *migrazione* a Windows Server 2016. Vedere l'[elenco di compatibilità](Server-Application-Compatibility.md). <br>- Gli aggiornamenti a Windows Server 2016 (escluso l'aggiornamento di applicazioni) devono usare le linee guida specifiche dell'applicazione.|- Per sfruttare i vantaggi offerti dalle nuove funzionalità di Windows Server 2016, distribuire nuovo hardware o installare Windows Server 2016 in una macchina virtuale in un host esistente. Alcune nuove funzionalità hanno un funzionamento ottimale in un host fisico di Windows Server 2016 che esegue Hyper-V. Seguire le linee guida sulla migrazione in base alle esigenze. <br>- In alternativa, continuare a usare il sistema operativo corrente e avviare l'esecuzione in una macchina virtuale su un host di Windows Server 2016 o Microsoft Azure. Contattare il rivenditore EA, TAM o Microsoft per le opzioni di supporto esteso tramite [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Carichi di lavoro dell'applicazione ISV**|- Gli aggiornamenti a Windows Server 2016 devono usare le linee guida specifiche dell'applicazione. <br>- Per altre informazioni sulla compatibilità tra applicazioni Windows Server e applicazioni non Microsoft, visitare il portale [Windows Server Logo Certification](https://msdn.microsoft.com/enterprisecloudcertified).|- Per sfruttare i vantaggi offerti dalle nuove funzionalità di Windows Server 2016, distribuire nuovo hardware o installare Windows Server 2016 in una macchina virtuale in un host esistente. Alcune nuove funzionalità hanno un funzionamento ottimale in un host fisico di Windows Server 2016 che esegue Hyper-V. Seguire le linee guida sulla migrazione in base alle esigenze. <br>- In alternativa, continuare a usare il sistema operativo corrente e avviare l'esecuzione in una macchina virtuale su un host di Windows Server 2016 o Microsoft Azure. Contattare il rivenditore EA, TAM o Microsoft per le opzioni di supporto esteso tramite [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Carichi di lavoro dell'applicazione personalizzata**|- Consultare gli sviluppatori dell'applicazione sulla compatibilità con Windows Server 2016 e le linee guida per l'aggiornamento. <br>- Usare Microsoft Azure per testare l'applicazione in Windows Server 2016 prima del passaggio. <br>- Vedere le opzioni complete nella sezione successiva.|- Consultare gli sviluppatori dell'applicazione in merito alla compatibilità con Windows Server 2016 e alle linee guida per l'aggiornamento. <br>- Usare Microsoft Azure per testare l'applicazione in Windows Server 2016 prima del passaggio. <br>- Per sfruttare i vantaggi offerti dalle nuove funzionalità di Windows Server 2016, distribuire nuovo hardware o installare Windows Server 2016 in una macchina virtuale in un host esistente. Alcune nuove funzionalità hanno un funzionamento ottimale in un host fisico di Windows Server 2016 che esegue Hyper-V. <br>- Vedere le opzioni complete nella sezione successiva.|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>Opzioni complete per lo spostamento a Windows Server 2016 di server che eseguono applicazioni personalizzate o "interne" nelle versioni precedenti di Windows Server

Per consentire a utenti e clienti di sfruttare i vantaggi delle funzionalità in Windows Server 2016 sono disponibili più opzioni rispetto al passato, con un impatto minimo per i servizi correnti e i carichi di lavoro.

- Provare a usare il sistema operativo più recente con l'applicazione scaricando la versione di valutazione di [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) per l'esecuzione di test in locale. Al termine del test e dopo aver confermato la qualità, è possibile eseguire una semplice conversione di licenze con un codice di licenza per attivazione singola (è necessario il riavvio).

- È anche possibile usare [Microsoft Azure](https://azure.microsoft.com) per un periodo di prova a scopo di test, per assicurarsi che l'applicazione personalizzata funzioni nel sistema operativo server più recente. Al termine del test e dopo aver confermato la qualità, [eseguire la migrazione alla versione più recente di Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade#upgrade) in locale. 

- In alternativa, al termine del test e dopo aver confermato la qualità, è possibile usare [Microsoft Azure](https://azure.microsoft.com) come percorso permanente per il servizio o l'applicazione personalizzata. In questo modo, il vecchio server rimane disponibile fino a quando non si è pronti per passare al nuovo server in Azure.

    - Se si dispone già di Software Assurance per Windows Server, è possibile ottenere un risparmio in termini economici con la distribuzione tramite [vantaggio Azure Hybrid Use](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- Nella maggior parte dei casi, [Microsoft Azure](https://azure.microsoft.com) può essere usato per ospitare la stessa applicazione nella versione precedente di Windows Server attualmente in esecuzione. Eseguire la migrazione dell'applicazione e del carico di lavoro a una macchina virtuale con il sistema operativo desiderato tramite le immagini di [Azure Marketplace](https://azure.microsoft.com/marketplace/).

    - Se si dispone già di Software Assurance per Windows Server, è possibile ottenere un risparmio in termini economici con la distribuzione tramite [Vantaggio Azure Hybrid Use](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- Il programma [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx) per Windows Server offre nuovi vantaggi in termini di diritti di versione. Insieme a un elenco di altri vantaggi, i server con Software Assurance possono essere aggiornati alla versione più recente di Windows Server al momento opportuno, senza dover acquistare una nuova licenza. 

## <a name="additional-resources"></a>Risorse aggiuntive

- [Funzionalità rimosse o deprecate in Windows Server 2016](deprecated-features.md)
- Per opzioni generali di aggiornamento e migrazione del server, visitare [Upgrade and conversion options for Windows Server 2016](Supported-Upgrade-Paths.md) (Opzioni di aggiornamento e conversione per Windows Server 2016).
- Per altre informazioni sul ciclo di vita del prodotto e livelli di supporto, vedere il [Criteri relativi al ciclo di vita del supporto - Domande frequenti](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq).

