---
title: Criterio qualità del servizio (QoS)
description: In questo argomento viene fornita una panoramica di qualità del servizio (QoS), che consente di usare criteri di gruppo per stabilire le priorità del traffico larghezza di banda di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>Qualità del servizio \(QoS\) criteri

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare criteri di QoS come un punto centrale di gestione della larghezza di banda di rete tra l'intera infrastruttura di Active Directory mediante la creazione di profili di QoS, le cui impostazioni vengono distribuite con criteri di gruppo.

>[!NOTE]
>  Oltre a questo argomento, è disponibile la documentazione seguente criterio QoS.  
>   
>  - [Guida introduttiva a criteri QoS](qos-policy-get-started.md)
>  - [Gestire i criteri QoS](qos-policy-manage.md)
>  - [Domande frequenti sui criteri QoS](qos-policy-faq.md)

I criteri QoS vengono applicati a una sessione di accesso utente o un computer come parte di un oggetto Criteri di gruppo collegato a un contenitore di Active Directory, ad esempio un dominio, sito o unità organizzativa \(GPO\) \(OU\).

Gestione del traffico QoS si verifica sotto il livello di applicazione, il che significa che le applicazioni esistenti non è necessario essere modificate per beneficiare dei vantaggi forniti dai criteri QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemi operativi che supportano i criteri QoS

È possibile utilizzare i criteri QoS per la gestione della larghezza di banda per computer o utenti con i seguenti sistemi operativi Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Percorso del criterio di QoS in Criteri di gruppo

In Windows Server 2016 Editor criteri di gruppo Management, il percorso ai criteri QoS per la configurazione di Computer è il seguente.

**Criterio dominio predefinito | Configurazione computer | Criteri | Impostazioni di Windows | QoS basata su Policy\**

Questo percorso è illustrato nell'immagine seguente.

![Percorso del criterio di QoS in Criteri di gruppo](../../media/QoS/QoS-Gp.jpg)

In Windows Server 2016 Editor criteri di gruppo Management, il percorso ai criteri QoS per la configurazione utente è il seguente.

**Criterio dominio predefinito | Configurazione utente | Criteri | Impostazioni di Windows | QoS basata su Policy\**

Per impostazione predefinita non è configurati alcun criterio QoS.

## <a name="why-use-qos-policy"></a>Perché usare criteri di QoS?
  
Man mano che aumenta il traffico sulla rete, è sempre più importante per bilanciare le prestazioni della rete con il costo del servizio, ma non è in genere facile da definire la priorità e gestire il traffico di rete.

In rete, applicazioni mission\ critiche e sensibili latency\ devono competere della larghezza di banda contro il traffico di priorità inferiore. Allo stesso tempo, alcuni utenti e computer con prestazioni di rete specifici requisiti potrebbero richiedere differentiated livelli di servizio.

Nel settore delle prestazioni di rete economica e prevedibili spesso prima visualizzazione connessioni \(WAN\) di rete WAN o con le applicazioni sensibili alla latenza, ad esempio voce su IP \(VoIP\) e flussi video. Tuttavia, al fine di fornire i livelli di servizio di rete prevedibili si applica a tutti gli ambienti di rete \ (ad esempio, un imprese LAN isolamento di rete) e a più applicazioni VoIP, ad esempio applicazioni modalità-of\-business personalizzate dell'azienda.
  
QoS basata su criteri è lo strumento di gestione della larghezza di banda di rete che fornisce controllo di rete, in base alle applicazioni, utenti e computer. 

Quando si utilizza criteri di QoS, le applicazioni non è necessario per l'applicazione specifica programmazione interfacce \(APIs\). In questo modo è possibile utilizzare QoS con le applicazioni esistenti. Inoltre, QoS basata su criteri sfrutta l'infrastruttura di gestione esistente, poiché QoS basata su criteri è incorporata in Criteri di gruppo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definire la priorità QoS tramite un Differentiated Services Code Point \(DSCP\)
  
È possibile creare criteri QoS che definiscono la priorità di traffico di rete con un valore \(DSCP\) Differentiated Services Code Point assegnate a diversi tipi di traffico di rete. 

DSCP consente di applicare \(0–63\) un valore all'interno del campo di tipo di servizio \(TOS\) nell'intestazione del pacchetto IPv4 e all'interno del campo della classe di traffico IPv6. 

Il valore DSCP fornisce classificazione del traffico di rete a livello di \(IP\) Internet Protocol router usano per decidere il traffico di Accodamento messaggi comportamento. 

Ad esempio, è possibile configurare i router per inserire uno dei tre code pacchetti con valori DSCP specifici: priorità alta, meglio sforzo o inferiori al massimo sforzo. 

Il traffico di rete cruciali, sia presente nella coda di priorità alta, ha preferenza sul resto del traffico.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limite di larghezza di banda utilizzata per ogni applicazione con velocità in uscita

È inoltre possibile limitare il traffico di rete in uscita di un'applicazione, specificando una velocità in criterio QoS.

Un criterio QoS che definisce i limiti di limitazione determina la velocità del traffico di rete in uscita. Per gestire i costi della WAN, ad esempio, un reparto IT può implementare un contratto di servizio che specifica che un file server non può fornire mai download oltre un determinato tasso.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Utilizzare i criteri QoS per applicare DSCP valori e limitare le velocità di

Inoltre, è possibile utilizzare criteri QoS per applicare i valori DSCP e limitare le velocità in uscita del traffico di rete per le operazioni seguenti:

- Applicazione di invio e percorso della directory

- Origine e destinazione IPv4 o IPv6, indirizzi o prefissi di indirizzo

- Protocollo - Transmission Control Protocol \(TCP\) e \(UDP\) Datagram Protocol utente

- Porte di origine e di destinazione e intervalli di porte \(TCP or UDP\)

- Specifici gruppi di utenti o computer tramite la distribuzione di criteri di gruppo

Utilizzando questi controlli, è possibile specificare un criterio QoS con un valore DSCP pari al 46 per un'applicazione VoIP, l'abilitazione di router inserire i pacchetti VoIP in una coda di bassa latenza oppure è possibile utilizzare un criterio QoS per limitare un set di traffico in uscita dei server di 512 KB al secondo \(KBps\) durante l'invio da porta TCP 443.

È anche possibile applicare criteri QoS a una specifica applicazione che ha requisiti speciali della larghezza di banda. Per ulteriori informazioni, vedere [scenari dei criteri QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantaggi del criterio QoS

Con i criteri QoS, è possibile configurare e applicare i criteri QoS che non possono essere configurati nel router e commutatori. Criterio QoS offre i vantaggi seguenti.
  
1. **Livello di dettaglio:** è difficile creare criteri QoS a livello di utente router o commutatori, soprattutto se computer il è configurato con IP dinamico assegnazione di indirizzi o se il computer non è connesso al commutatore fissa o le porte di router, come avviene spesso con i computer portatili. Al contrario, i criteri QoS rende più semplice configurare un criterio di QoS di livello dall'utente in un controller di dominio e distribuire i criteri per i computer dell'utente.
2. **Flessibilità**. Indipendentemente dalla dove o come un computer si connette alla rete, viene applicato il criterio QoS - il computer può connettersi tramite Ethernet o Wi-Fi da qualsiasi posizione. Per i criteri QoS a livello di dall'utente, il criterio QoS viene applicato su qualsiasi dispositivo compatibile in qualsiasi posizione in cui l'utente accede.
3. **Sicurezza:** se il reparto IT crittografa il traffico degli utenti da to end tramite Internet Protocol security \(IPsec\), è possibile classificare il traffico sui router in base a tutte le informazioni sopra il livello IP nel pacchetto \ (ad esempio, un port\ TCP). Tuttavia, tramite criteri di QoS, è possibile classificare i pacchetti il dispositivo di fine per indicare la priorità dei pacchetti nell'intestazione IP prima che i payload IP sono crittografati e i pacchetti vengono inviati.
4. **Prestazioni:** alcune funzioni di QoS, ad esempio la limitazione, meglio vengono eseguite quando sono più vicini all'origine. Criterio QoS Sposta tali funzioni QoS più vicini all'origine.
5. **Gestibilità:** criterio QoS per semplificare la gestione di rete in due modi:

    **a**. Poiché si basa su criteri di gruppo, è possibile utilizzare criteri QoS per configurare e gestire un set di criteri QoS per utente/computer quando necessario e nel computer di un controller di dominio centrale.

    **b**. Criterio QoS facilita la configurazione utente/computer, fornendo un meccanismo per specificare i criteri da \(URL\) Uniform Resource Locator anziché specificare gli indirizzi IP di ogni server in cui devono essere applicati i criteri di QoS in base ai criteri. Si supponga ad esempio che la rete dispone di un cluster di server che condividono un URL comune. Tramite criteri di QoS, è possibile creare un criterio in base all'URL comuni, invece di creare un criterio per ogni server del cluster, con ogni criterio in base all'indirizzo IP di ogni server.

Per l'argomento successivo in questa Guida, vedere [Guida introduttiva a criteri di QoS](qos-policy-get-started.md).

