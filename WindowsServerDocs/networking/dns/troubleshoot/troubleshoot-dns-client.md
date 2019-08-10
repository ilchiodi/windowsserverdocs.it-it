---
title: Risoluzione dei problemi relativi ai client DNS
description: Questo articolo illustra come risolvere i problemi relativi a DNS dal lato client.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 1f18159d6232bd9e7864b13419b3648c12b9f44f
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917818"
---
# <a name="troubleshooting-dns-clients"></a>Risoluzione dei problemi relativi ai client DNS

Questo articolo illustra come risolvere i problemi dei client DNS.

## <a name="check-ip-configuration"></a>Controllare la configurazione IP

1. Aprire una finestra del prompt dei comandi come amministratore nel computer client.

2. Eseguire il comando seguente:

   ```cmd
   ipconfig /all
   ```

3. Verificare che il client disponga di un indirizzo IP valido, subnet mask e un gateway predefinito per la rete a cui è collegato e in uso.

4. Controllare i server DNS elencati nell'output e verificare che gli indirizzi IP elencati siano corretti.

5. Controllare il suffisso DNS specifico della connessione nell'output e verificare che sia corretto.

Se il client non dispone di una configurazione TCP/IP valida, utilizzare uno dei metodi seguenti:

* Per i client configurati in modo `ipconfig /renew` dinamico, utilizzare il comando per forzare manualmente il rinnovo della configurazione degli indirizzi IP del client con il server DHCP.

* Per i client configurati in modo statico, modificare le proprietà TCP/IP del client per usare le impostazioni di configurazione valide o completare la configurazione DNS per la rete.

## <a name="check-network-connection"></a>Controllare la connessione di rete

### <a name="ping-test"></a>Ping test

Verificare che il client sia in grado di contattare un server DNS preferito o alternativo eseguendo il ping del server DNS preferito in base al relativo indirizzo IP.

Se, ad esempio, il client usa un server DNS preferito di 10.0.0.1, eseguire questo comando al prompt dei comandi:

```cmd
ping 10.0.0.1
```

Se nessun server DNS configurato risponde a un ping diretto del relativo indirizzo IP, questo indica che l'origine del problema è più probabile che la connettività di rete tra il client e i server DNS. In tal caso, attenersi alla procedura di base per la risoluzione dei problemi di rete TCP/IP per risolvere il problema. Tenere presente che il traffico ICMP deve essere consentito attraverso il firewall per consentire il funzionamento del comando ping.

### <a name="dns-query-tests"></a>Test di query DNS

Se il client DNS è in grado di effettuare il ping del server DNS, provare `nslookup` a usare i comandi seguenti per verificare se il server è in grado di rispondere ai client DNS. Poiché nslookup non utilizza la cache DNS del client, la risoluzione dei nomi utilizzerà il server DNS configurato del client.

#### <a name="test-a-client"></a>Testare un client

```cmd
nslookup <client>
```
  
Ad esempio, se il computer client è denominato **CLIENT1**, eseguire questo comando:
  
```cmd
nslookup client1
```
  
Se non viene restituita una risposta con esito positivo, provare a eseguire il comando seguente:
  
```cmd
nslookup <fqdn of client>
```
  
Se, ad esempio, il nome FQDN è **CLIENT1.Corp.contoso.com**, eseguire questo comando:

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> Quando si esegue questo test, è necessario includere il periodo finale.

Se Windows riesce a trovare il nome di dominio completo ma non riesce a trovare il nome breve, controllare la configurazione del suffisso DNS nella scheda DNS delle impostazioni avanzate TCP/IP della scheda di interfaccia di rete. Per ulteriori informazioni, vedere [configurazione della risoluzione DNS](https://docs.microsoft.com/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution).

#### <a name="test-the-dns-server"></a>Testare il server DNS

```cmd
nslookup <DNS Server>
```

Ad esempio, se il server DNS è denominato DC1, eseguire il comando seguente:

```cmd
nslookup dc1
```
Se i test precedenti hanno avuto esito positivo, anche questo test dovrebbe avere esito positivo. Se il test non riesce, verificare la connettività al server DNS.

#### <a name="test-the-failing-record"></a>Testare il record che ha avuto esito negativo

```cmd
nslookup <failed internal record>
```

Se, ad esempio, il record che ha avuto esito negativo è **App1.Corp.contoso.com**, eseguire questo comando:

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>Testare un indirizzo Internet pubblico

```cmd
nslookup <external name>
```

Ad esempio: 
```cmd
nslookup bing.com
```

Se tutti e quattro i test sono stati completati `ipconfig /displaydns` correttamente, eseguire e controllare l'output per il nome che ha avuto esito negativo. Se viene visualizzato il nome "nome inesistente" sotto il nome non riuscito, viene restituita una risposta negativa da un server DNS che è stata memorizzata nella cache del client. 

Per risolvere il problema, cancellare la cache `ipconfig /flushdns`eseguendo.

## <a name="next-step"></a>Passaggio successivo

Se la risoluzione dei nomi è ancora in errore, passare alla sezione [risoluzione dei problemi dei server DNS](troubleshoot-dns-server.md) .
