---
title: Criteri Qualità del servizio (QoS)
description: Questo argomento fornisce una panoramica dei criteri di qualità del servizio (QoS), che consente di usare Criteri di gruppo per assegnare priorità alla larghezza di banda del traffico di rete di applicazioni e servizi specifici in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8854197b11f6533681d8f842af816ca0776d7040
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315466"
---
# <a name="quality-of-service-qos-policy"></a>Qualità del servizio \(criteri di\) QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.

>[!NOTE]
>  Oltre a questo argomento, è disponibile la documentazione seguente relativa ai criteri QoS.  
>   
>  - [Introduzione con criteri QoS](qos-policy-get-started.md)
>  - [Gestisci criteri QoS](qos-policy-manage.md)
>  - [Domande frequenti sui criteri QoS](qos-policy-faq.md)

I criteri QoS vengono applicati a una sessione di accesso utente o a un computer come parte di un oggetto Criteri di gruppo \(GPO\) collegato a un contenitore di Active Directory, ad esempio un dominio, un sito o un'unità organizzativa \(OU\).

La gestione del traffico QoS si verifica sotto il livello dell'applicazione, il che significa che non è necessario modificare le applicazioni esistenti per trarre vantaggio dai vantaggi offerti dai criteri QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemi operativi che supportano i criteri QoS

È possibile utilizzare i criteri QoS per gestire la larghezza di banda per i computer o gli utenti con i seguenti sistemi operativi Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Posizione dei criteri QoS in Criteri di gruppo

In Windows Server 2016 Editor Gestione Criteri di gruppo il percorso dei criteri QoS per la configurazione del computer è il seguente.

**Criterio dominio predefinito | Configurazione computer | Criteri | Impostazioni di Windows | QoS basata su criteri\-**

Questo percorso è illustrato nella figura seguente.

![Posizione dei criteri QoS in Criteri di gruppo](../../media/QoS/QoS-Gp.jpg)

In Windows Server 2016 Editor Gestione Criteri di gruppo il percorso dei criteri QoS per la configurazione utente è il seguente.

**Criterio dominio predefinito | Configurazione utente | Criteri | Impostazioni di Windows | QoS basata su criteri\-**

Per impostazione predefinita, non sono configurati criteri QoS.

## <a name="why-use-qos-policy"></a>Perché usare i criteri QoS?
  
Con l'aumentare del traffico sulla rete, è sempre più importante bilanciare le prestazioni di rete con il costo del servizio, ma il traffico di rete non è in genere facile da definire in ordine di priorità e gestire.

Nella rete, Mission\-critical and latence\-le applicazioni sensibili devono competere per la larghezza di banda di rete rispetto al traffico con priorità più bassa. Allo stesso tempo, alcuni utenti e computer con requisiti specifici per le prestazioni di rete potrebbero richiedere livelli di servizio distinti.

Le esigenze di offrire livelli di prestazioni di rete convenienti e prevedibili vengono spesso visualizzate prima Wide Area Network \(connessioni WAN\) o con applicazioni sensibili alla latenza, come Voice over IP \(\) VoIP e streaming video. Tuttavia, l'obiettivo finale di fornire livelli di servizio di rete prevedibili si applica a qualsiasi ambiente di rete \(, ad esempio, una rete locale delle aziende\)e a più di applicazioni VoIP, ad esempio la linea personalizzata della società\-di applicazioni aziendali\-.
  
QoS basata su criteri è lo strumento di gestione della larghezza di banda di rete che fornisce il controllo di rete basato su applicazioni, utenti e computer. 

Quando si usano i criteri QoS, le applicazioni non devono essere scritte per interfacce di programmazione dell'applicazione specifiche \(API\). Questo consente di usare QoS con le applicazioni esistenti. Inoltre, la funzionalità QoS basata su criteri sfrutta l'infrastruttura di gestione esistente, perché la funzionalità QoS basata su criteri è incorporata in Criteri di gruppo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definire la priorità QoS tramite un punto di codice servizi differenziati \(DSCP\)
  
È possibile creare criteri QoS che definiscono la priorità del traffico di rete con un punto di codice servizi differenziati \(DSCP\) valore assegnato a diversi tipi di traffico di rete. 

Il DSCP consente di applicare un valore \(0-63\) all'interno del tipo di servizio \(TOS\) campo nell'intestazione di un pacchetto IPv4 e all'interno del campo della classe di traffico in IPv6. 

Il valore DSCP fornisce la classificazione del traffico di rete a livello di protocollo Internet \(IP\), che i router utilizzano per decidere il comportamento di Accodamento del traffico. 

È possibile, ad esempio, configurare i router in modo da inserire i pacchetti con valori DSCP specifici in una delle tre code seguenti: priorità alta, massimo sforzo o inferiore rispetto al migliore sforzo. 

Il traffico di rete cruciale, che si trova nella coda con priorità alta, ha la preferenza su altro traffico.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limitare l'uso della larghezza di banda di rete per applicazione con frequenza di limitazione

È anche possibile limitare il traffico di rete in uscita di un'applicazione specificando una velocità di limitazione nei criteri QoS.

Un criterio QoS che definisce i limiti di limitazione determina la frequenza di traffico di rete in uscita. Per gestire i costi WAN, ad esempio, un reparto IT può implementare un contratto di servizio che specifica che un file server non può mai fornire download oltre una tariffa specifica.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usare i criteri QoS per applicare i valori DSCP e le frequenze di limitazione

È anche possibile usare i criteri QoS per applicare i valori DSCP e le frequenze di limitazione per il traffico di rete in uscita ai seguenti elementi:

- Invio del percorso dell'applicazione e della directory

- Prefissi di indirizzo o indirizzi IPv4 o IPv6 di origine e di destinazione

- Protocollo-Transmission Control Protocol \(TCP\) e User Datagram Protocol \(UDP\)

- Porte e intervalli di porte di origine e destinazione \(TCP o UDP\)

- Gruppi specifici di utenti o computer tramite la distribuzione in Criteri di gruppo

Usando questi controlli, è possibile specificare un criterio QoS con un valore DSCP di 46 per un'applicazione VoIP, abilitando i router a collocare i pacchetti VoIP in una coda a bassa latenza oppure è possibile usare un criterio QoS per limitare un set di traffico in uscita dei server a 512 kilobyte al secondo \(KBps\) durante l'invio dalla porta TCP 443.

È anche possibile applicare i criteri QoS a una particolare applicazione con requisiti di larghezza di banda specifici. Per ulteriori informazioni, vedere la pagina relativa agli [scenari dei criteri QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantaggi dei criteri QoS

Con i criteri QoS, è possibile configurare e applicare i criteri QoS che non possono essere configurati nei router e nei commutatori. I criteri QoS offrono i vantaggi seguenti.
  
1. **Livello di dettaglio:** È difficile creare criteri QoS a livello di utente su router o commutatori, soprattutto se il computer dell'utente è configurato tramite l'assegnazione di indirizzi IP dinamici o se il computer non è connesso a un commutatore fisso o a porte router, come spesso accade con i computer portatili. Al contrario, i criteri QoS semplificano la configurazione dei criteri QoS di livello\-utente in un controller di dominio e la propagazione dei criteri al computer dell'utente.
2. **Flessibilità**. Indipendentemente dalla posizione o dal modo in cui un computer si connette alla rete, vengono applicati i criteri QoS. il computer può connettersi usando il Wi-Fi o Ethernet da qualsiasi posizione. Per i criteri QoS di livello\-utente, il criterio QoS viene applicato a qualsiasi dispositivo compatibile in qualsiasi posizione in cui l'utente esegue l'accesso.
3. **Sicurezza:** Se il reparto IT crittografa il traffico degli utenti da un lato all'altro usando Internet Protocol Security \(IPsec\), non è possibile classificare il traffico sui router in base a qualsiasi informazione al di sopra del livello IP nel pacchetto \(ad esempio, una porta TCP\). Tuttavia, usando i criteri QoS, è possibile classificare i pacchetti nel dispositivo finale per indicare la priorità dei pacchetti nell'intestazione IP prima che i payload IP vengano crittografati e i pacchetti vengano inviati.
4. **Prestazioni:** Alcune funzioni QoS, ad esempio la limitazione delle richieste, vengono eseguite meglio quando sono più vicine all'origine. Il criterio QoS sposta tali funzioni QoS più vicine all'origine.
5. **Gestibilità:** I criteri QoS migliorano la gestibilità della rete in due modi:

    **oggetto**. Poiché è basato su Criteri di gruppo, è possibile utilizzare i criteri QoS per configurare e gestire un set di criteri QoS utente/computer quando necessario e in un computer controller di dominio centrale.

    **b**. I criteri QoS facilitano la configurazione di utenti/computer fornendo un meccanismo per specificare i criteri tramite Uniform Resource Locator URL \(\) invece di specificare criteri basati sugli indirizzi IP di ogni server in cui devono essere applicati i criteri QoS. Si supponga, ad esempio, che la rete disponga di un cluster di server che condividono un URL comune. Usando i criteri QoS, è possibile creare un criterio basato sull'URL comune, anziché creare un criterio per ogni server nel cluster, con ogni criterio basato sull'indirizzo IP di ogni server.

Per l'argomento successivo di questa guida, vedere [Introduzione con criteri QoS](qos-policy-get-started.md).

