---
title: Pianificare una distribuzione a più foreste
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto in un ambiente con più foreste in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2f14fdb2fd3ab6f0a89c8d8c1a8853041dcba94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281005"
---
# <a name="plan-a-multi-forest-deployment"></a>Pianificare una distribuzione a più foreste

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo documento vengono descritti i passaggi di pianificazione necessari per la configurazione di Accesso remoto in una distribuzione a più foreste.  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   È necessario un trust bidirezionale.  
  
## <a name="plan-trust-between-forests"></a>Pianificare l'attendibilità tra le foreste  
Quando si decide di abilitare l'accesso alle risorse da una nuova foresta, consentire ai client della nuova foresta di usare DirectAccess o aggiungere server di accesso remoto dalla nuova foresta come punti di ingresso alla distribuzione di Accesso remoto, è necessario verificare che tra le due foreste sia stata configurata un'attendibilità totale, ovvero un trust transitivo bidirezionale. Vedere [Tipi di trust](https://technet.microsoft.com/library/cc775736.aspx). In una distribuzione a più foreste, l'attendibilità totale tra le foreste rappresenta un prerequisito per consentire agli amministratori di eseguire operazioni quali la modifica di oggetti Criteri di gruppo nella nuova foresta, l'utilizzo dei gruppi di sicurezza della nuova foresta come gruppo di sicurezza dei client, l'effettuazione di chiamate remote (WinRM, RPC) ai computer contenuti nella nuova foresta e l'autenticazione dei client remoti dalla nuova foresta.  
  
## <a name="plan-remote-access-administrator-permissions"></a>Pianificare le autorizzazioni di amministratore di Accesso remoto  
Quando viene configurato, Accesso remoto aggiorna e a volte crea oggetti Criteri di gruppo in ogni dominio in cui sono presenti server o client di Accesso remoto. Analogamente agli ambienti a singola foresta, l'amministratore di Accesso remoto deve disporre delle autorizzazioni per scrivere e modificare gli oggetti Criteri di gruppo DirectAccess e i relativi filtri di sicurezza in un ambiente a più foreste ed eventualmente disporre delle autorizzazioni per creare collegamenti per gli oggetti Criteri di gruppo DirectAccess in tutte le foreste interessate. Le autorizzazioni sono necessarie a prescindere dalla foresta a cui appartiene l'amministratore di Accesso remoto.  
  
L'amministratore di Accesso remoto deve, inoltre, essere un amministratore locale in tutti i server di Accesso remoto inclusi quelli della nuova foresta aggiunti come punti di ingresso alla distribuzione di Accesso remoto originale.  
  
## <a name="ClientSG"></a>Pianificare gruppi di sicurezza client  
Nella nuova foresta è necessario configurare almeno un gruppo di sicurezza per i computer client DirectAccess in essa contenuti, in quanto un gruppo di sicurezza non può contenere account da foreste diverse.  
  
> [!NOTE]  
> -   DirectAccess richiede almeno Windows 10&reg; o Windows&reg; gruppo di sicurezza client 8 per ogni foresta. Tuttavia, è consigliabile avere un Windows 10 o gruppo di sicurezza di client Windows 8 per ogni dominio che contiene il client di Windows 10 o Windows 8.  
> -   Quando è abilitata la distribuzione multisito, DirectAccess richiede almeno Windows 7&reg; gruppo di sicurezza di client per foresta per ogni punto di ingresso DirectAccess in cui 7 Windows sono supportati nei computer client. Tuttavia, è consigliabile avere un gruppo di sicurezza di Windows 7 client separato per ogni punto di ingresso per ogni dominio che contiene computer client Windows 7.  
>   
> Per applicare DirectAccess in computer client di altri domini, è necessario creare oggetti Criteri di gruppo in tali domini. L'aggiunta di gruppi di sicurezza attiva la scrittura di nuovi oggetti Criteri di gruppo client per i nuovi domini; pertanto, se all'elenco dei gruppi di sicurezza client di DirectAccess si aggiunge un nuovo gruppo di sicurezza da un nuovo dominio, nel nuovo dominio viene creato automaticamente un oggetto Criteri di gruppo, e i computer client in esso contenuti acquisiscono le impostazioni di DirectAccess tramite l'oggetto Criteri di gruppo client.  
>   
> Se si aggiunge il client da un nuovo dominio a un gruppo di sicurezza esistente già configurato come gruppo di sicurezza dei client DirectAccess, l'oggetto Criteri di gruppo client non verrà creato automaticamente da DirectAccess nel nuovo dominio. Il client del nuovo dominio non riceverà le impostazioni di DirectAccess e non sarà in grado di effettuare la connessione tramite DirectAccess.  
  
## <a name="plan-certification-authorities"></a>Pianificare le autorità di certificazione  
Se per la distribuzione di DirectAccess è configurato l'utilizzo dell'autenticazione OTP (One-Time Password), ogni foresta contiene gli stessi modelli di certificato di firma con valori OID differenti. A causa di ciò, non sarà possibile configurare le foreste come un singola unità di configurazione. Per risolvere questo problema e configurare l'autenticazione OTP in un ambiente con più foreste, vedere la sezione "Configurare OTP in una distribuzione a più foreste" nell'argomento [configurare una distribuzione a più foreste](Configure-a-Multi-Forest-Deployment.md).  
  
Quando si utilizza l'autenticazione con certificato computer IPsec, tutti i computer client e server devono disporre di un certificato rilasciato dalla stessa autorità di certificazione radice o intermedia, a prescindere dalla foresta a cui appartengono.  
  
## <a name="plan-otp-exemptions"></a>Pianificare le esenzioni OTP  
Se si utilizza l'autenticazione OTP di DirectAccess, il gruppo di sicurezza per esenzioni OTP è riservato agli utenti di una singola foresta, in quanto ogni gruppo di sicurezza può contenere solo gli utenti di una singola foresta ed è possibile configurare un unico gruppo di sicurezza.  
  


