---
title: Pianificare una distribuzione a più foreste
description: Questo argomento fa parte della Guida distribuire accesso remoto in un ambiente a più foreste in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d7661841d10aff634be8f125640e1561ca9490b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860434"
---
# <a name="plan-a-multi-forest-deployment"></a>Pianificare una distribuzione a più foreste

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo documento vengono descritti i passaggi di pianificazione necessari per la configurazione di Accesso remoto in una distribuzione a più foreste.  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   È necessario un trust bidirezionale.  
  
## <a name="plan-trust-between-forests"></a>Pianificare l'attendibilità tra le foreste  
Quando si decide di abilitare l'accesso alle risorse da una nuova foresta, consentire ai client della nuova foresta di usare DirectAccess o aggiungere server di accesso remoto dalla nuova foresta come punti di ingresso alla distribuzione di Accesso remoto, è necessario verificare che tra le due foreste sia stata configurata un'attendibilità totale, ovvero un trust transitivo bidirezionale. Vedere [Tipi di trust](https://technet.microsoft.com/library/cc775736.aspx). In una distribuzione a più foreste, l'attendibilità totale tra le foreste rappresenta un prerequisito per consentire agli amministratori di eseguire operazioni quali la modifica di oggetti Criteri di gruppo nella nuova foresta, l'utilizzo dei gruppi di sicurezza della nuova foresta come gruppo di sicurezza dei client, l'effettuazione di chiamate remote (WinRM, RPC) ai computer contenuti nella nuova foresta e l'autenticazione dei client remoti dalla nuova foresta.  
  
## <a name="plan-remote-access-administrator-permissions"></a>Pianificare le autorizzazioni di amministratore di Accesso remoto  
Quando viene configurato, Accesso remoto aggiorna e a volte crea oggetti Criteri di gruppo in ogni dominio in cui sono presenti server o client di Accesso remoto. Analogamente agli ambienti a singola foresta, l'amministratore di Accesso remoto deve disporre delle autorizzazioni per scrivere e modificare gli oggetti Criteri di gruppo DirectAccess e i relativi filtri di sicurezza in un ambiente a più foreste ed eventualmente disporre delle autorizzazioni per creare collegamenti per gli oggetti Criteri di gruppo DirectAccess in tutte le foreste interessate. Le autorizzazioni sono necessarie a prescindere dalla foresta a cui appartiene l'amministratore di Accesso remoto.  
  
L'amministratore di Accesso remoto deve, inoltre, essere un amministratore locale in tutti i server di Accesso remoto inclusi quelli della nuova foresta aggiunti come punti di ingresso alla distribuzione di Accesso remoto originale.  
  
## <a name="plan-client-security-groups"></a><a name="ClientSG"></a>Pianificare i gruppi di sicurezza client  
Nella nuova foresta è necessario configurare almeno un gruppo di sicurezza per i computer client DirectAccess in essa contenuti, in quanto un gruppo di sicurezza non può contenere account da foreste diverse.  
  
> [!NOTE]  
> -   DirectAccess richiede almeno un gruppo di sicurezza client Windows 10&reg; o Windows&reg; 8 per ogni foresta. Tuttavia, è consigliabile disporre di un gruppo di sicurezza client Windows 10 o Windows 8 per ogni dominio che contiene client Windows 10 o Windows 8.  
> -   Quando è abilitata la funzionalità multisito, DirectAccess richiede almeno un gruppo di sicurezza client&reg; Windows 7 per ogni foresta per ogni punto di ingresso DirectAccess in cui sono supportati i computer client Windows 7. Tuttavia, è consigliabile disporre di un gruppo di sicurezza client Windows 7 separato per ogni punto di ingresso per ogni dominio che contiene i client Windows 7.  
>   
> Per applicare DirectAccess in computer client di altri domini, è necessario creare oggetti Criteri di gruppo in tali domini. L'aggiunta di gruppi di sicurezza attiva la scrittura di nuovi oggetti Criteri di gruppo client per i nuovi domini; pertanto, se all'elenco dei gruppi di sicurezza client di DirectAccess si aggiunge un nuovo gruppo di sicurezza da un nuovo dominio, nel nuovo dominio viene creato automaticamente un oggetto Criteri di gruppo, e i computer client in esso contenuti acquisiscono le impostazioni di DirectAccess tramite l'oggetto Criteri di gruppo client.  
>   
> Se si aggiunge il client da un nuovo dominio a un gruppo di sicurezza esistente già configurato come gruppo di sicurezza dei client DirectAccess, l'oggetto Criteri di gruppo client non verrà creato automaticamente da DirectAccess nel nuovo dominio. Il client del nuovo dominio non riceverà le impostazioni di DirectAccess e non sarà in grado di effettuare la connessione tramite DirectAccess.  
  
## <a name="plan-certification-authorities"></a>Pianificare le autorità di certificazione  
Se per la distribuzione di DirectAccess è configurato l'utilizzo dell'autenticazione OTP (One-Time Password), ogni foresta contiene gli stessi modelli di certificato di firma con valori OID differenti. A causa di ciò, non sarà possibile configurare le foreste come un singola unità di configurazione. Per risolvere questo problema e configurare OTP in un ambiente a più foreste, vedere la sezione "configurare OTP in una distribuzione a più foreste" nell'argomento [configurare una distribuzione](Configure-a-Multi-Forest-Deployment.md)a più foreste.  
  
Quando si utilizza l'autenticazione con certificato computer IPsec, tutti i computer client e server devono disporre di un certificato rilasciato dalla stessa autorità di certificazione radice o intermedia, a prescindere dalla foresta a cui appartengono.  
  
## <a name="plan-otp-exemptions"></a>Pianificare le esenzioni OTP  
Se si utilizza l'autenticazione OTP di DirectAccess, il gruppo di sicurezza per esenzioni OTP è riservato agli utenti di una singola foresta, in quanto ogni gruppo di sicurezza può contenere solo gli utenti di una singola foresta ed è possibile configurare un unico gruppo di sicurezza.  
  


