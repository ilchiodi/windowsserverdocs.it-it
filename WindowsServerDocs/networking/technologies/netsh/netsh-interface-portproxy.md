---
title: Comandi Netsh per il proxy delle porte dell'interfaccia
description: Usare i comandi di netsh interface proxy delle porte per fungere da intermediari tra reti IPv4 e IPv6 e applicazioni.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 8ac4f8e7cd0aed5a81e89672354622dd81afce2a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446186"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandi del proxy delle porte dell'interfaccia di Netsh

Usare la **netsh interface proxy delle porte** comandi per fungere da intermediari tra reti IPv4 e IPv6 e applicazioni. È possibile utilizzare i comandi seguenti per definire il servizio proxy nei modi seguenti:

-   Computer configurati con IPv4 e dell'applicazione i messaggi inviati ad altre applicazioni e i computer configurati con IPv4.

-   Applicazione e i computer configurati con IPv4 messaggi inviati da configurati con IPv6 e computer le applicazioni.

-   Applicazione e i computer configurati con IPv6 messaggi inviati da configurati con IPv4 e computer le applicazioni.

-   Computer configurati con IPv6 e dell'applicazione i messaggi inviati ad altre applicazioni e i computer configurati con IPv6.

Quando si scrivono i file batch o script usando i comandi seguenti, ogni comando deve iniziare con **netsh interface proxy delle porte**. Ad esempio, quando si usa la **eliminare v4tov6** comando per specificare che il server proxy delle porte consente di eliminare una porta di IPv4 e l'indirizzo nell'elenco di indirizzi IPv4 per il quale è in ascolto il server, il file batch o lo script deve usare la sintassi seguente:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

I comandi di proxy delle porte di interfaccia di netsh disponibili sono:

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [eliminare v4tov4](#delete-v4tov4)

-   [eliminare v4tov6](#delete-v4tov6)

-   [eliminare v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [Mostra tutto](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [Mostra v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [Mostra v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>aggiungere v4tov4

Il server proxy delle porte in ascolto dei messaggi inviati a una porta specifica e indirizzo IPv4 e mappe di una porta e IPv4 di indirizzi per inviare i messaggi ricevuti dopo avere stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v4tov6"></a>aggiungere v4tov6

Il server proxy delle porte in ascolto dei messaggi inviati a una porta specifica e l'indirizzo IPv4 e mappe di una porta e IPv6 indirizzi per inviare i messaggi ricevuti dopo avere stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 in cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v6tov4"></a>aggiungere v6tov4

Il server proxy delle porte in ascolto dei messaggi inviati a una porta specifica e l'indirizzo IPv6 e mappe una porta e IPv4 indirizzi a cui inviare i messaggi ricevuti dopo avere stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v6tov6"></a>aggiungere v6tov6

Il server proxy delle porte in ascolto dei messaggi inviati a una porta specifica e l'indirizzo IPv6 e mappe una porta e IPv6 indirizzi a cui inviare i messaggi ricevuti dopo avere stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="delete-v4tov4"></a>eliminare v4tov4

Il server proxy delle porte Elimina un indirizzo IPv4 dall'elenco di indirizzi per il quale è in ascolto il server e porte IPv4.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v4tov6"></a>eliminare v4tov6

Il server proxy delle porte Elimina una porta di IPv4 e l'indirizzo nell'elenco di indirizzi IPv4 per il quale è in ascolto il server.

### <a name="syntax"></a>Sintassi

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v6tov4"></a>eliminare v6tov4

Il server proxy delle porte Elimina una porta IPv6 e l'indirizzo nell'elenco di indirizzi IPv6 per il quale è in ascolto il server.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v6tov6"></a>eliminare v6tov6

Il server proxy delle porte Elimina un indirizzo IPv6 nell'elenco di indirizzi IPv6 per il quale è in ascolto il server.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="reset"></a>reimpostare

Reimposta lo stato di configurazione di IPv6.

### <a name="syntax"></a>Sintassi

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte create con il **aggiungere v4tov4** comando oppure aggiunge una nuova voce all'elenco che porta/indirizzo coppie.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte create con il **aggiungere v4tov6** comando oppure aggiunge una nuova voce all'elenco che porta/indirizzo coppie.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 in cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte create con il **aggiungere v6tov4** comando oppure aggiunge una nuova voce all'elenco che porta/indirizzo coppie.

### <a name="syntax"></a>Sintassi

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte create con il **aggiungere v6tov6** comando oppure aggiunge una nuova voce all'elenco che porta/indirizzo coppie.

### <a name="syntax"></a>Sintassi

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Specifica la porta IPv6, dal numero di porta o un servizio il nome, su cui rimanere in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|  **connectport**   |        Specifica la porta IPv6, dal numero di porta o un servizio il nome, a cui connettersi. Se **connectport** non viene specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui rimanere in ascolto. I valori accettati sono indirizzi IP, nome NetBIOS del computer o nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                   Specifica il protocollo da usare.                                                                                   |

## <a name="show-all"></a>visualizzazione dell'elenco completo

Visualizza tutti i parametri di proxy delle porte, tra cui coppie/indirizzo di porta per v4tov4 v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintassi

`show all`


## <a name="show-v4tov4"></a>Mostra v4tov4

Visualizza i parametri di v4tov4 proxy delle porte.

### <a name="syntax"></a>Sintassi

`show v4tov4`

## <a name="show-v4tov6"></a>Mostra v4tov6

Visualizza i parametri di v4tov6 proxy delle porte.

### <a name="syntax"></a>Sintassi

`show v4tov6`

## <a name="show-v6tov4"></a>Mostra v6tov4

Visualizza i parametri di v6tov4 proxy delle porte.

### <a name="syntax"></a>Sintassi

`show v6tov4`


## <a name="show-v6tov6"></a>Mostra v6tov6

Visualizza i parametri di v6tov6 proxy delle porte.

### <a name="syntax"></a>Sintassi

`show v6tov6`

---
