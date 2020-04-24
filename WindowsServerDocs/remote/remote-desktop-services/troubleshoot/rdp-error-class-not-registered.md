---
title: I client non riescono a connettersi e ricevono l'errore Classe non registrata
description: Risoluzione dei problemi relativi alla mancata registrazione di una classe con una connessione Desktop remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52e696bd4229b947ea63a379211192b8664a9f93
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857184"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>I client non riescono a connettersi e ricevono l'errore "Classe non registrata"

Quando provi a connetterti a un computer remoto usando un client che esegue Windows 10, versione 1709 o successive, può accadere che il client non riesca a connettersi mentre il server Host sessione Desktop remoto invia un messaggio contenente il codice di errore "Classe non registrata (0x80040154)".

Questo problema si verifica quando l'utente che prova a connettersi ha un profilo utente obbligatorio. Per risolverlo, installa l'aggiornamento di Windows 10 del [24 luglio 2018 - KB4338817 (build sistema operativo 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817).
