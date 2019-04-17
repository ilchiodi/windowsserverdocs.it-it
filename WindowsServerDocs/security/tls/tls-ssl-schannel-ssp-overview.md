---
title: TLS / SSL (SSP Schannel) Panoramica
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS / SSL (SSP Schannel) Panoramica

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT introduce l'implementazione di TLS/SSL in Windows tramite Security Service Provider (SSP) Schannel applicazioni pratiche, le modifiche nell'implementazione di Microsoft e i requisiti software, oltre ad altre risorse per Windows Server 2012 e Windows 8.

**Did you mean:**

-   [Pacchetto di protezione Schannel](https://msdn.microsoft.com/library/ms678421.aspx)

-   [Canale sicuro](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [Protocollo di sicurezza Layer di trasporto](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>Descrizione \(Schannel\) il client
Schannel è un \(SSP\) Security Support Provider che implementa lo \(SSL\) SSL (Secure Sockets Layer) e Transport Layer Security \(TLS\) protocolli di autenticazione standard Internet.

Il \(SSPI\) Security Support Provider Interface è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla security\ tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider \(SSPs\), tra cui SSP. Schannel.

Le versioni di protocollo Transport Layer Security \(TLS\) 1.0, 1.1 e 1.2, il protocollo Secure Sockets Layer \(SSL\) protocollo \(PCT\) versioni 2.0 e 3.0, Datagram Transport Layer Security \(DTLS\) versione 1.0 e Private Communications trasporto sono basate sulla crittografia a chiave pubblica. La suite di protocolli di autenticazione \(Schannel\) canale di protezione fornisce tali protocolli. Tutti i protocolli Schannel usano un modello di connessione tra client/server.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Un problema quando si amministra una rete riguarda la protezione dei dati inviati tra le applicazioni su una rete non attendibile. È possibile utilizzare il client per autenticare i server e i computer client e quindi usare il protocollo per crittografare i messaggi tra le entità autenticate.

Ad esempio, è possibile utilizzare il client per:

-   Transazioni protette SSL\ con un sito Web e\-e-commerce

-   Accesso client autenticato a un sito Web protetto SSL\

-   Accesso remoto

-   Accesso SQL

-   E\ mail

## <a name="BKMK_SOFT"></a>Requisiti software
Il protocollo Transport utilizzano un modello client\server e sono basate sull'autenticazione del certificato, che richiede un'infrastruttura a chiave pubblica.

## <a name="BKMK_INSTALL"></a>Informazioni su Server Manager
Sono necessari per l'implementazione di TLS, SSL o Schannel procedure di configurazione.

