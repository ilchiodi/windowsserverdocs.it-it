---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Utilizzando il modello di foresta di domini organizzativi
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875912"
---
# <a name="using-the-organizational-domain-forest-model"></a>Utilizzando il modello di foresta di domini organizzativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nel modello di foresta aziendale dominio diversi autonomi gruppi ogni proprietario di un dominio all'interno di un insieme di strutture. Ogni gruppo controlla l'amministrazione del servizio a livello di dominio, che consente loro di gestire determinati aspetti della gestione dei servizi in modo autonomo, mentre il proprietario della foresta controlla gestione dei servizi a livello di foresta.  

Nella figura seguente è illustrato un modello di foresta dominio aziendale.  

![utilizzando il modello di insieme di strutture di dominio organizzazione](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomia dei servizi a livello di dominio

Il modello di foresta aziendale dominio consente la delega dell'autorità per la gestione dei servizi a livello di dominio. Nella tabella seguente sono elencati i tipi di gestione dei servizi che possono essere controllati a livello di dominio.  

|Tipo di gestione dei servizi|Attività associate|  
|------------------------------|--------------------|  
|Gestione delle operazioni di controller di dominio|-Creazione e rimozione di controller di dominio<br />-Monitoraggio il funzionamento dei controller di dominio<br />-La gestione dei servizi in esecuzione sui controller di dominio<br />-Backup e il ripristino della directory|  
|Configurazione delle impostazioni a livello di dominio|-Creazione di dominio e utente di dominio criteri account, ad esempio password, Kerberos e criteri di blocco account<br />-Creazione e applicazione dei criteri di gruppo a livello di dominio|  
|Delega dell'amministrazione a livello di dati|-Creazione di unità organizzative (OU) e la delega dell'amministrazione<br />-Risoluzione dei problemi nella struttura dell'unità Organizzativa che non dispongono dei diritti di accesso sufficienti per risolvere proprietari di unità Organizzative|  
|Gestione dei trust esterni|-Stabilire relazioni di trust con domini esterni alla foresta|  

Altri tipi di gestione dei servizi, ad esempio schema o gestione della topologia di replica, sono di responsabilità del proprietario dell'insieme di strutture.  

## <a name="domain-owner"></a>Proprietario del dominio

In un modello di foresta del dominio aziendale, i proprietari del dominio sono responsabili di attività di gestione del servizio a livello di dominio. I proprietari del dominio dispongono autorità tramite l'intero dominio, nonché l'accesso a tutti gli altri domini nella foresta. Per questo motivo, i proprietari del dominio devono essere selezionati dal proprietario della foresta di utenti attendibili.  

Delegare la gestione del servizio a livello di dominio a un proprietario di dominio, se vengono soddisfatte le condizioni seguenti:  

- Tutti i gruppi che fanno parte dell'insieme di strutture attendibili il nuovo proprietario del dominio e le procedure di gestione del servizio del nuovo dominio.  

- Il proprietario della foresta e tutti gli altri proprietari di dominio, considera attendibile il nuovo proprietario del dominio.  

- L'utente accetta tutti i proprietari del dominio nella foresta che il nuovo proprietario del dominio disponga di gestione di amministratori del servizio e criteri di selezione e le procedure uguale o più rigoroso di proprie.  

- L'utente accetta tutti i proprietari del dominio nella foresta che i controller di dominio gestiti dal proprietario del nuovo dominio nel nuovo dominio siano fisicamente protetto.  

Si noti che se un'insieme di strutture proprietario delegati a livello di dominio gestione dei servizi a un proprietario di dominio, altri gruppi potrebbero scegliere di non partecipare a tale foresta se si considera attendibile il proprietario del dominio.  

Tutti i proprietari del dominio è necessario tenere presente che se una di queste condizioni cambiare in futuro, potrebbero diventare necessario spostare i domini aziendali in una distribuzione più foreste.  

> [!NOTE]  
> Un altro modo per ridurre al minimo i rischi di protezione per un dominio di Active Directory di Windows Server 2008 è utilizzare la separazione dei ruoli amministratore, che richiede la distribuzione di un controller di dominio di sola lettura (RODC) nell'infrastruttura di Active Directory. Un RODC è un nuovo tipo di controller di dominio nel sistema operativo Windows Server 2008 che ospita le partizioni di sola lettura del database di Active Directory. Prima del rilascio di Windows Server 2008, qualsiasi operazione di manutenzione di server in un controller di dominio doveva essere eseguita da un amministratore di dominio. In Windows Server 2008, è possibile delegare le autorizzazioni amministrative locali per un RODC per qualsiasi utente di dominio senza concedergli i diritti amministrativi per il dominio o altri controller di dominio. Questo consente all'utente delegato per accedere a un RODC ed eseguire operazioni di manutenzione, ad esempio l'aggiornamento di un driver, sul server. Tuttavia, questo utente delegato non può accedere a qualsiasi altro controller di dominio o eseguire altre attività amministrative nel dominio. In questo modo, qualsiasi attendibile l'utente può essere delegata la possibilità di gestire in modo efficace il RODC senza compromettere la protezione del resto del dominio. Per ulteriori informazioni sui controller, vedere [Active Directory Domain Services: I controller di dominio di sola lettura](https://go.microsoft.com/fwlink/?LinkId=106616).  
