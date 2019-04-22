---
title: Criteri Qualità del servizio (QoS)
description: In questo argomento viene fornita una panoramica dei criteri di qualità del servizio (QoS), che consentono di usare criteri di gruppo per definire le priorità della larghezza di banda di rete del traffico di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820282"
---
# <a name="quality-of-service-qos-policy"></a>Qualità del servizio \(QoS\) criteri

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Puoi utilizzare i criteri QoS come un punto centrale per la gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory creando profili QoS, le cui impostazioni vengono distribuite con Criteri di gruppo.

>[!NOTE]
>  Oltre a questo argomento, è disponibile la documentazione seguente i criteri QoS.  
>   
>  - [Introduzione a criteri QoS](qos-policy-get-started.md)
>  - [Gestire i criteri QoS](qos-policy-manage.md)
>  - [Domande frequenti sui criteri QoS](qos-policy-faq.md)

I criteri QoS vengono applicati a una sessione di accesso utente o un computer come parte di un oggetto Criteri di gruppo \(GPO\) che è stato collegato a un contenitore di Active Directory, ad esempio un dominio, sito o unità organizzativa \(OU\).

Gestione del traffico di QoS si verifica di sotto il livello dell'applicazione, il che significa che le applicazioni esistenti non sono necessario modificarla per beneficiare dei vantaggi forniti dai criteri di QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemi operativi che supportano i criteri QoS

È possibile usare i criteri QoS per la gestione della larghezza di banda per computer o utenti con i seguenti sistemi operativi Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Percorso dei criteri di QoS in Criteri di gruppo

In Windows Server 2016 Editor criteri di gruppo gestione, il percorso ai criteri di QoS per la configurazione del Computer è il seguente.

**Criterio dominio predefinito | Configurazione computer | I criteri | Le impostazioni di Windows | Criteri\-basato QoS**

Questo percorso è illustrato nell'immagine seguente.

![Percorso dei criteri di QoS in Criteri di gruppo](../../media/QoS/QoS-Gp.jpg)

In Windows Server 2016 Editor criteri di gruppo gestione, il percorso ai criteri di QoS per la configurazione utente è il seguente.

**Criterio dominio predefinito | Configurazione utente | I criteri | Le impostazioni di Windows | Criteri\-basato QoS**

Per impostazione predefinita non viene configurato alcun criterio QoS.

## <a name="why-use-qos-policy"></a>Perché usare i criteri QoS?
  
Man mano che aumenta il traffico sulla rete, è sempre più importante bilanciare le prestazioni di rete con il costo del servizio, ma non è in genere facile da definire la priorità e gestire il traffico di rete.

Nella rete, applicazioni di importanza\-critici e della latenza\-applicazioni sensibili devono si contendono la larghezza di banda di rete con il traffico di priorità inferiore. Allo stesso tempo, alcuni utenti e computer con prestazioni di rete specifici potrebbero richiedere requisiti distingue tra i livelli di servizio.

Le sfide di fornire livelli di prestazioni di rete alternativa conveniente e prevedibile spesso inizialmente visualizzate tramite rete WAN \(WAN\) connessioni o con le applicazioni sensibili alla latenza, ad esempio di voice over IP \(VoIP\) e lo streaming di video. Tuttavia, al fine di offrire livelli di servizio di rete prevedibili si applica a tutti gli ambienti di rete \(ad esempio, un'azienda LAN\)e a più applicazioni VoIP, ad esempio di riga personalizzato della società\-di\-applicazioni aziendali.
  
QoS basata su criteri è lo strumento di gestione della larghezza di banda di rete che fornisce controllo di rete - basato su applicazioni, gli utenti e computer. 

Quando si usa criteri di QoS, le applicazioni non devono essere scritti per applicazioni specifiche interfacce di programmazione \(API\). Questo offre la possibilità di usare la funzionalità QoS con le applicazioni esistenti. Inoltre, QoS basata su criteri consente di sfruttare l'infrastruttura di gestione esistente, poiché QoS basata su criteri è incorporata in Criteri di gruppo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definire la priorità QoS tramite un punto di codice servizi differenziati \(DSCP\)
  
È possibile creare criteri di QoS che definiscono priorità di traffico di rete con un Differentiated Services Code Point \(DSCP\) valore assegnato a diversi tipi di traffico di rete. 

Il DSCP consente di applicare un valore \(0 – 63\) all'interno del tipo di servizio \(TOS\) campo nell'intestazione del pacchetto IPv4 e all'interno del campo di classe di traffico IPv6. 

Il valore di DSCP fornisce classificazione del traffico di rete a Internet Protocol \(IP\) livello, quali router consente di decidere di comportamento del traffico di Accodamento messaggi. 

Ad esempio, è possibile configurare i router per collocare i pacchetti con valori DSCP specifici in uno dei tre code: priorità elevata massimo sforzo o inferiori a massimo sforzo. 

Il traffico di rete critiche, si trova nella coda con priorità alta, non ha preferenza sul resto del traffico.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limite larghezza di banda usata per ogni applicazione con limitazione della velocità

È anche possibile limitare il traffico di rete in uscita di un'applicazione specificando una limitazione della velocità in Criteri di QoS.

Un criterio di QoS che definisce i limiti della limitazione determina la frequenza del traffico di rete in uscita. Ad esempio, per gestire i costi della WAN, un reparto IT può implementare un contratto di servizio che specifica che un file server non può fornire mai download oltre a una specifica velocità.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usare i criteri QoS per applicare DSCP valori e applicherà la limitazione di velocità

È anche possibile usare criteri QoS per applicare i valori di DSCP e limitare le tariffe per il traffico di rete in uscita ai seguenti:

- Applicazione mittente e il percorso di directory

- I prefissi di indirizzo o indirizzi IPv4 o IPv6 di origine e destinazione

- Protocollo: Transmission Control Protocol \(TCP\) e User Datagram Protocol \(UDP\)

- Porte di origine e destinazione e gli intervalli di porte \(TCP o UDP\)

- Specifici gruppi di utenti o computer mediante la distribuzione in Criteri di gruppo

Tramite questi controlli, è possibile specificare un criterio QoS con un valore DSCP 46 per un'applicazione, VoIP consentono ai router collocare i pacchetti VoIP in una coda a bassa latenza, oppure è possibile usare un criterio di QoS per limitare un set di traffico in uscita del server su 512 kilobyte al secondo <c1/>KBps\) durante l'invio dalla porta TCP 443.

È anche possibile applicare i criteri QoS a una specifica applicazione che ha requisiti di larghezza di banda speciali. Per altre informazioni, vedere [scenari relativi ai criteri di QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantaggi dei criteri QoS

Con i criteri di QoS, è possibile configurare e imporre i criteri di QoS che non possono essere configurati sul router e commutatori. I criteri QoS offre i vantaggi seguenti.
  
1. **Livello di dettaglio:** È difficile creare criteri QoS a livello di utente nel router o switch, soprattutto se il computer dell'utente configurate tramite l'assegnazione di indirizzi IP dinamici o se il computer non è connesso a interruttore fisso o porte di router, come accade spesso con computer portatili. Al contrario, i criteri QoS rende più semplice configurare un utente\-il livello di criteri di QoS in un controller di dominio e propagare i criteri al computer dell'utente.
2. **Flessibilità**. Indipendentemente dal fatto dove o come un computer si connette alla rete, viene applicato il criterio di QoS: il computer può connettersi tramite Ethernet o Wi-Fi da qualsiasi posizione. Per utente\-il livello di criteri di QoS, i criteri QoS criterio viene applicato in qualsiasi dispositivo compatibile in qualsiasi posizione in cui l'utente accede.
3. **Sicurezza:** Se il reparto IT non crittografa il traffico degli utenti dall'inizio alla fine tramite Internet Protocol security \(IPsec\), è possibile classificare il traffico sui router basato su tutte le informazioni sopra il livello IP nel pacchetto \(, ad esempio, un La porta TCP\). Tuttavia, tramite criteri di QoS, è possibile classificare i pacchetti a livello di dispositivo finale per indicare la priorità dei pacchetti nell'intestazione IP prima vengono crittografati i payload IP e i pacchetti vengono inviati.
4. **Prestazioni:** Alcune funzioni di QoS, ad esempio la limitazione, sono meglio eseguire quando si trovano più vicino all'origine. I criteri QoS consente di spostare tali funzioni QoS più vicini all'origine.
5. **Facilità di gestione:** I criteri QoS migliora la gestibilità di rete in due modi:

    **a**. Poiché si basa su criteri di gruppo, è possibile utilizzare i criteri QoS per configurare e gestire un set di criteri QoS per utente/computer quando necessario e nel computer di un controller di dominio centrale.

    **b**. I criteri QoS facilita la configurazione utente/computer fornendo un meccanismo per specificare i criteri da Uniform Resource Locator \(URL\) invece di specificare criteri basati sugli indirizzi IP della ognuno dei server in cui i criteri QoS necessitano da applicare. Si supponga, ad esempio, che la rete dispone di un cluster di server che condividono un URL comune. Usando i criteri QoS, è possibile creare un criterio basato sull'URL comune, invece di creare un criterio per ogni server del cluster, con tutti i criteri in base all'indirizzo IP di ogni server.

Per l'argomento successivo in questa Guida, vedere [Introduzione a criteri di QoS](qos-policy-get-started.md).

