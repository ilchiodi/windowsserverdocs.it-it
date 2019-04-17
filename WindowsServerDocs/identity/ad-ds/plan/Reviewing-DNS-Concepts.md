---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Revisione dei concetti relativi a DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>Revisione dei concetti relativi a DNS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema DNS (Domain Name) è un database distribuito che rappresenta uno spazio dei nomi. Lo spazio dei nomi contiene tutte le informazioni necessarie per i client possono cercare qualsiasi nome. Qualsiasi server DNS può rispondere alle richieste su qualsiasi nome all'interno dello spazio dei nomi. Un server DNS risponde alle query in uno dei modi seguenti:  
  
-   Se la risposta è nella cache, la risposta alla query dalla cache.  
  
-   Se la risposta è in una zona ospitata dal server DNS, la risposta alla query dalla relativa area. Una zona è una parte della struttura DNS archiviata in un server DNS. Quando un server DNS ospita una zona, è autorevole per i nomi in tale area (ovvero, il server DNS può rispondere alle richieste per ogni nome nell'area). Ad esempio, un server che ospita la zona contoso.com può rispondere alle richieste per ogni nome di contoso.com.  
  
-   Se il server non può rispondere alla query dalla cache o zone, richiede altri server per la risposta.  
  
È importante comprendere le funzionalità dei componenti di base del DNS, ad esempio la delega, risoluzione dei nomi ricorsiva e zone DNS integrate in Active Directory, perché hanno un impatto diretto della progettazione della struttura logica di Active Directory.  
  
Per ulteriori informazioni su DNS e servizi di dominio Active Directory (AD DS), vedere [DNS e Active Directory](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Delega  
Per un server DNS rispondere alle query su qualsiasi nome, deve avere un percorso diretto o indiretto a ogni area nello spazio dei nomi. Questi percorsi vengono creati tramite la delega. Una delega è un record in una zona padre contenente un server dei nomi autorevole per la zona del successivo livello della gerarchia. Le deleghe rendono possibili per i server in una zona per fare riferimento ai client al server in altre aree. Nella figura seguente mostra un esempio di delega.  
  
![Concetti DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
Il server radice DNS ospita la zona radice rappresentata da un punto (. ). La zona radice contiene una delega a una zona del successivo livello della gerarchia, la zona com. La delega nella zona radice indica il server radice DNS che, per trovare la zona com, deve contattare il server Com. Analogamente, la delega della zona com indica al server Com che, per trovare la zona contoso.com, deve contattare il server di Contoso.  
  
> [!NOTE]  
> La delega utilizza due tipi di record. Il record di risorse nome server (NS) fornisce il nome di un server autorevole. Host (A) e record di risorse host (AAAA) forniscono IP versione 4 (IPv4) e IP versione 6 (IPv6) gli indirizzi di un server autorevole.  
  
Questo sistema di zone e deleghe crea un struttura ad albero gerarchica che rappresenta lo spazio dei nomi DNS. Ogni zona rappresenta un livello nella gerarchia e ogni delega rappresenta un ramo dell'albero.  
  
Tramite la gerarchia delle zone e deleghe, un server radice DNS può individuare qualsiasi nome dello spazio dei nomi DNS. La zona radice include deleghe che conducono direttamente o indirettamente a tutte le altre zone nella gerarchia. Qualsiasi server che è possibile eseguire una query il server radice DNS possono utilizzare le informazioni nelle deleghe per trovare qualsiasi nome dello spazio dei nomi.  
  
## <a name="recursive-name-resolution"></a>Risoluzione dei nomi ricorsiva  
Risoluzione dei nomi ricorsiva è il processo mediante il quale un server DNS utilizza la gerarchia delle zone e deleghe per rispondere alle query per cui non è autorevole.  
  
In alcune configurazioni, i server DNS includono i parametri radice (ovvero, un elenco di nomi e indirizzi IP) che consentono loro di eseguire una query server radice DNS. In altre configurazioni, i server inoltrano tutte le query che non rispondono a un altro server. Hint di inoltro e radice sono entrambi i metodi che è possono utilizzare i server DNS per risolvere le query per cui non sono autorevoli.  
  
### <a name="resolving-names-by-using-root-hints"></a>Risoluzione dei nomi mediante parametri radice  
I parametri radice consentono a qualsiasi server DNS individuare il server radice DNS. Dopo che un server DNS individua il server radice DNS, è possibile risolvere una query per tale spazio dei nomi. Nella figura seguente viene descritto come DNS risolve un nome utilizzando i parametri radice.  
  
![Concetti DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
In questo esempio, si verificano gli eventi seguenti:  
  
1.  Un client invia una query ricorsiva a un server DNS per richiedere l'indirizzo IP che corrisponde al nome ftp.contoso.com. Una query ricorsiva indica che il client richiede una risposta definitiva alla query. La risposta alla query ricorsiva deve essere un indirizzo valido o un messaggio che indica che è Impossibile trovare l'indirizzo.  
  
2.  Poiché il server DNS non è autorevole per il nome e dispone della risposta nella cache, il server DNS utilizza parametri radice per trovare l'indirizzo IP del server radice DNS.  
  
3.  Il server DNS utilizza una query iterativa per porre il server radice DNS per risolvere il nome ftp.contoso.com. Una query iterativa indica che il server accetterà un riferimento a un altro server al posto di una risposta definitiva alla query. Poiché il nome ftp.contoso.com termina con l'etichetta com, il server radice DNS restituisce un riferimento al server Com che ospita la zona com.  
  
4.  Il server DNS utilizza una query iterativa per porre il server Com per risolvere il nome ftp.contoso.com. Poiché il nome ftp.contoso.com termina con il nome contoso.com, il server Com restituisce un riferimento al server di Contoso che ospita il contoso.com la zona.  
  
5.  Il server DNS utilizza una query iterativa per porre il server di Contoso per risolvere il nome ftp.contoso.com. Il server di Contoso trova la risposta nei dati di zona e quindi restituisce la risposta al server.  
  
6.  Il server restituisce quindi il risultato al client.  
  
### <a name="resolving-names-by-using-forwarding"></a>Risoluzione dei nomi tramite inoltro  
Inoltro consente la risoluzione dei nomi di route a server specifici anziché utilizzare i parametri radice. Nella figura seguente viene descritto come DNS risolve un nome utilizzando l'inoltro.  
  
![Concetti DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
In questo esempio, si verificano gli eventi seguenti:  
  
1.  Un client esegue una query su un server DNS per il nome ftp.contoso.com.  
  
2.  Il server DNS inoltra la query in un altro server DNS, noto come un server d'inoltro.  
  
3.  Poiché il server d'inoltro non è autorevole per il nome e dispone della risposta nella cache, Usa parametri radice per trovare l'indirizzo IP del server radice DNS.  
  
4.  Il server d'inoltro utilizza una query iterativa per porre il server radice DNS per risolvere il nome ftp.contoso.com. Poiché il nome ftp.contoso.com termina con il nome com, il server radice DNS restituisce un riferimento al server Com che ospita la zona com.  
  
5.  Il server d'inoltro utilizza una query iterativa per porre il server Com per risolvere il nome ftp.contoso.com. Poiché il nome ftp.contoso.com termina con il nome contoso.com, il server Com restituisce un riferimento al server di Contoso che ospita il contoso.com la zona.  
  
6.  Il server d'inoltro utilizza una query iterativa per porre il server di Contoso per risolvere il nome ftp.contoso.com. Il server di Contoso trova la risposta nel relativo file di zona e quindi restituisce la risposta al server.  
  
7.  Il server d'inoltro restituisce quindi il risultato al server DNS originale.  
  
8.  Il server DNS originale restituisce quindi il risultato al client.  
  


