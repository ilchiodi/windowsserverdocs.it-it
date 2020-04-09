---
title: Risoluzione dei problemi di SMB multicanale
description: Introduce i metodi di risoluzione dei problemi SMB multicanale.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 210bc2057f25dc196fe9d76495c42f76c8b36311
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815344"
---
# <a name="smb-multichannel-troubleshooting"></a>Risoluzione dei problemi di SMB multicanale

In questo articolo viene descritto come risolvere i problemi relativi a SMB multicanale.

## <a name="check-the-network-interface-status"></a>Verificare lo stato dell'interfaccia di rete

Verificare che il binding per l'interfaccia di rete sia impostato su **true** nel client SMB (MS\_client) e nel server SMB (MS\_Server). Quando si esegue il comando seguente, l'output dovrebbe mostrare **true** in **Enabled** per entrambe le interfacce di rete:

```PowerShell
Get-NetAdapterBinding -ComponentID ms_server,ms_msclient
```

Successivamente, assicurarsi che l'interfaccia di rete sia elencata nell'output dei comandi seguenti:

```PowerShell
Get-SmbServerNetworkInterface
```

```PowerShell
Get-SmbClientNetworkInterface
```

È anche possibile eseguire il comando **Get-NetAdapter** per visualizzare l'indice dell'interfaccia per verificare il risultato. L'indice dell'interfaccia Mostra tutti gli adapter SMB attivi associati attivamente all'interfaccia appropriata.

## <a name="check-the-firewall"></a>Controllare il firewall

Se è presente solo un indirizzo IP locale del collegamento e nessun indirizzo instradabile pubblicamente, è probabile che il profilo di rete sia impostato su **pubblico**. Questo significa che SMB è bloccato al firewall per impostazione predefinita.

Il comando seguente consente di visualizzare il profilo di connessione in uso. Per recuperare queste informazioni, è anche possibile usare il centro di rete e condivisione.

**Get-NetConnectionProfile**

Nel gruppo **condivisione file e stampanti** controllare le regole in ingresso del firewall per assicurarsi che "SMB-in" sia abilitato per il profilo corretto.

![Regole SMB-in](media/smb-multichannel-troubleshooting-1.png)

È anche possibile abilitare la **condivisione di file e stampanti** nella finestra **centro di rete e condivisione** . A tale scopo, selezionare **Modifica impostazioni di condivisione avanzate** nel menu a sinistra e quindi selezionare **Attiva condivisione file e stampanti** per il profilo. Questa opzione Abilita le regole firewall per la condivisione di file e stampanti.

![Modifica impostazioni di condivisione avanzate](media/smb-multichannel-troubleshooting-2.png)

## <a name="capture-client-and-server-sided-traffic-for-troubleshooting"></a>Acquisire il traffico lato client e server per la risoluzione dei problemi

Sono necessarie le informazioni di traccia della connessione SMB che iniziano dall'handshake TCP a tre vie. Prima di avviare l'acquisizione, è consigliabile chiudere tutte le applicazioni, in particolare Esplora risorse. Riavviare il servizio **workstation** nel client SMB, avviare l'acquisizione pacchetti, quindi riprodurre il problema.

Verificare che la connessione SMBv3.*x* sia negoziata e che nessun elemento tra il server e il client influisca sulla negoziazione del dialetto. SMBv2 e versioni precedenti non supportano Multichannel.

Cercare l'interfaccia di rete\_\_pacchetti INFO. Questo è il punto in cui il client SMB richiede un elenco di adapter dal server SMB. Se questi pacchetti non vengono scambiati, il multicanale non funziona.

Il server risponde restituendo un elenco di interfacce di rete valide. Quindi, il client SMB li aggiunge all'elenco degli adapter disponibili per il multicanale. A questo punto, il multicanale dovrebbe avviarsi e, almeno, provare ad avviare la connessione.

Per informazioni, vedi gli articoli seguenti:

- [Richieste dell'applicazione 3.2.4.20.10 query sulle interfacce di rete del server](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/147adde4-d936-4597-924a-8caa3429c6b0)

- [2.2.32.5 NETWORK\_INTERFACE\_risposta informazioni](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/fcd862d1-1b85-42df-92b1-e103199f531f)

- [3.2.5.14.11 gestione di una risposta delle interfacce di rete](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/5459722b-1eaa-4ead-b465-284363264cad)

Negli scenari seguenti non è possibile utilizzare un adapter:

- Si è verificato un problema di routing nel client. Questa situazione è in genere causata da una tabella di routing non corretta che impone il traffico sull'interfaccia errata.

- Sono stati impostati vincoli multicanale. Per ulteriori informazioni, vedere [New-SmbMultichannelConstraint](https://docs.microsoft.com/powershell/module/smbshare/new-smbmultichannelconstraint).

- Qualcosa ha bloccato i pacchetti di richiesta e risposta dell'interfaccia di rete.

- Il client e il server non possono comunicare sull'interfaccia di rete aggiuntiva. Ad esempio, l'handshake TCP a tre vie ha avuto esito negativo, la connessione è bloccata da un firewall, l'installazione della sessione non è riuscita e così via.

Se l'adapter e il relativo indirizzo IPv6 sono nell'elenco inviato dal server, il passaggio successivo consiste nel verificare se le comunicazioni vengono tentate su tale interfaccia. Filtrare la traccia in base all'indirizzo locale del collegamento e al traffico SMB e cercare un tentativo di connessione. Se si tratta di una traccia NetConnection, è anche possibile esaminare gli eventi di Windows Filtering Platform (WFP) per verificare se la connessione è bloccata.
