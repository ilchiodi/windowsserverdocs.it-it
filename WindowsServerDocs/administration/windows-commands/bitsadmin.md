---
title: bitsadmin
description: Argomento di riferimento per il comando Bitsadmin, che è uno strumento da riga di comando utilizzato per creare, scaricare o caricare processi e monitorarne lo stato di avanzamento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a829ce21c4571188fb5ffeb9a0a1d991637d07
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710040"
---
# <a name="bitsadmin"></a>bitsadmin

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin è uno strumento da riga di comando utilizzato per creare, scaricare o caricare processi e per monitorarne lo stato di avanzamento. Lo strumento Bitsadmin usa le opzioni per identificare le attività da eseguire. È possibile chiamare `bitsadmin /?` o `bitsadmin /help` per ottenere un elenco di opzioni.

Per la maggior parte `<job>` delle opzioni è necessario un parametro, impostato sul nome visualizzato del processo o sul GUID. Non è necessario che il nome visualizzato di un processo sia univoco. Le opzioni **/create** e **/List** restituiscono il GUID di un processo.

Per impostazione predefinita, è possibile accedere alle informazioni sui processi personali. Per accedere alle informazioni per i processi di un altro utente, è necessario disporre dei privilegi di amministratore. Se il processo è stato creato con privilegi elevati, è necessario eseguire **BITSAdmin** da una finestra con privilegi elevati. in caso contrario, si avrà accesso in sola lettura al processo.

Molti dei commutatori corrispondono ai metodi nelle [interfacce bits](https://docs.microsoft.com/windows/win32/bits/bits-interfaces). Per ulteriori dettagli che potrebbero essere rilevanti per l'utilizzo di un'opzione, vedere il metodo corrispondente.

Usare le opzioni seguenti per creare un processo, impostare e recuperare le proprietà di un processo e monitorare lo stato di un processo. Per esempi che illustrano come usare alcune di queste opzioni per eseguire attività, vedere [esempi di Bitsadmin](bitsadmin-examples.md).

## <a name="available-switches"></a>Opzioni disponibili

- [/AddFile Bitsadmin](bitsadmin-addfile.md)
- [/ADDFILESET Bitsadmin](bitsadmin-addfileset.md)
- [/ADDFILEWITHRANGES Bitsadmin](bitsadmin-addfilewithranges.md)
- [/cache Bitsadmin](bitsadmin-cache.md)
- [Bitsadmin/cache/Delete](bitsadmin-cache-and-delete.md)
- [Bitsadmin/cache/DeleteUrl](bitsadmin-cache-and-deleteurl.md)
- [Bitsadmin/cache/getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
- [Bitsadmin/cache/getlimit](bitsadmin-cache-and-getlimit.md)
- [Bitsadmin/cache/Help](bitsadmin-cache-and-help.md)
- [Bitsadmin/cache/info](bitsadmin-cache-and-info.md)
- [Bitsadmin/cache/list](bitsadmin-cache-and-list.md)
- [Bitsadmin/cache/setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
- [Bitsadmin/cache/setlimit](bitsadmin-cache-and-setlimit.md)
- [Bitsadmin/cache/Clear](bitsadmin-cache-clear.md)
- [/Cancel Bitsadmin](bitsadmin-cancel.md)
- [/completo Bitsadmin](bitsadmin-complete.md)
- [/Create Bitsadmin](bitsadmin-create.md)
- [/examples Bitsadmin](bitsadmin-examples.md)
- [/GETACLFLAGS Bitsadmin](bitsadmin-getaclflags.md)
- [/getbytestotal Bitsadmin](bitsadmin-getbytestotal.md)
- [/getbytestransferred Bitsadmin](bitsadmin-getbytestransferred.md)
- [/GetClientCertificate Bitsadmin](bitsadmin-getclientcertificate.md)
- [/getcompletiontime Bitsadmin](bitsadmin-getcompletiontime.md)
- [/GetCreationTime Bitsadmin](bitsadmin-getcreationtime.md)
- [/getcustomheaders Bitsadmin](bitsadmin-getcustomheaders.md)
- [/GetDescription Bitsadmin](bitsadmin-getdescription.md)
- [/GetDisplayName Bitsadmin](bitsadmin-getdisplayname.md)
- [/GetError Bitsadmin](bitsadmin-geterror.md)
- [/GetErrorCount Bitsadmin](bitsadmin-geterrorcount.md)
- [/getfilestotal Bitsadmin](bitsadmin-getfilestotal.md)
- [/getfilestransferred Bitsadmin](bitsadmin-getfilestransferred.md)
- [/gethelpertokenflags Bitsadmin](bitsadmin-gethelpertokenflags.md)
- [/gethelpertokensid Bitsadmin](bitsadmin-gethelpertokensid.md)
- [/GetHttpMethod Bitsadmin](bitsadmin-gethttpmethod.md)
- [/getmaxdownloadtime Bitsadmin](bitsadmin-getmaxdownloadtime.md)
- [/getminretrydelay Bitsadmin](bitsadmin-getminretrydelay.md)
- [/getmodificationtime Bitsadmin](bitsadmin-getmodificationtime.md)
- [/getnoprogresstimeout Bitsadmin](bitsadmin-getnoprogresstimeout.md)
- [/getnotifycmdline Bitsadmin](bitsadmin-getnotifycmdline.md)
- [/getnotifyflags Bitsadmin](bitsadmin-getnotifyflags.md)
- [/getnotifyinterface Bitsadmin](bitsadmin-getnotifyinterface.md)
- [/GetOwner Bitsadmin](bitsadmin-getowner.md)
- [/getpeercachingflags Bitsadmin](bitsadmin-getpeercachingflags.md)
- [/GetPriority Bitsadmin](bitsadmin-getpriority.md)
- [/getproxybypasslist Bitsadmin](bitsadmin-getproxybypasslist.md)
- [/getproxylist Bitsadmin](bitsadmin-getproxylist.md)
- [/getproxyusage Bitsadmin](bitsadmin-getproxyusage.md)
- [/getreplydata Bitsadmin](bitsadmin-getreplydata.md)
- [/getreplyfilename Bitsadmin](bitsadmin-getreplyfilename.md)
- [/getreplyprogress Bitsadmin](bitsadmin-getreplyprogress.md)
- [/getsecurityflags Bitsadmin](bitsadmin-getsecurityflags.md)
- [/GetState Bitsadmin](bitsadmin-getstate.md)
- [/gettemporaryname Bitsadmin](bitsadmin-gettemporaryname.md)
- [/GetType Bitsadmin](bitsadmin-gettype.md)
- [/getvalidationstate Bitsadmin](bitsadmin-getvalidationstate.md)
- [/Help Bitsadmin](bitsadmin-help.md)
- [/info Bitsadmin](bitsadmin-info.md)
- [/List Bitsadmin](bitsadmin-list.md)
- [/ListFiles Bitsadmin](bitsadmin-listfiles.md)
- [/makecustomheaderswriteonly Bitsadmin](bitsadmin-makecustomheaderswriteonly.md)
- [/monitor Bitsadmin](bitsadmin-monitor.md)
- [/nowrap Bitsadmin](bitsadmin-nowrap.md)
- [/peercaching Bitsadmin](bitsadmin-peercaching.md)
- [Bitsadmin/peercaching/getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
- [Bitsadmin/peercaching/Help](bitsadmin-peercaching-and-help.md)
- [Bitsadmin/peercaching/setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
- [/Peers Bitsadmin](bitsadmin-peers.md)
- [Bitsadmin/Peers/Clear](bitsadmin-peers-and-clear.md)
- [Bitsadmin/Peers/Discover](bitsadmin-peers-and-discover.md)
- [Bitsadmin/Peers/Help](bitsadmin-peers-and-help.md)
- [Bitsadmin/Peers/List](bitsadmin-peers-and-list.md)
- [/rawreturn Bitsadmin](bitsadmin-rawreturn.md)
- [/removeclientcertificate Bitsadmin](bitsadmin-removeclientcertificate.md)
- [/removecredentials Bitsadmin](bitsadmin-removecredentials.md)
- [/REPLACEREMOTEPREFIX Bitsadmin](bitsadmin-replaceremoteprefix.md)
- [/Reset Bitsadmin](bitsadmin-reset.md)
- [/Resume Bitsadmin](bitsadmin-resume.md)
- [/setaclflag Bitsadmin](bitsadmin-setaclflag.md)
- [/setclientcertificatebyid Bitsadmin](bitsadmin-setclientcertificatebyid.md)
- [/setclientcertificatebyname Bitsadmin](bitsadmin-setclientcertificatebyname.md)
- [/SetCredentials Bitsadmin](bitsadmin-setcredentials.md)
- [/setcustomheaders Bitsadmin](bitsadmin-setcustomheaders.md)
- [/SetDescription Bitsadmin](bitsadmin-setdescription.md)
- [/SetDisplayName Bitsadmin](bitsadmin-setdisplayname.md)
- [/sethelpertoken Bitsadmin](bitsadmin-sethelpertoken.md)
- [/sethelpertokenflags Bitsadmin](bitsadmin-sethelpertokenflags.md)
- [/SetHttpMethod Bitsadmin](bitsadmin-sethttpmethod.md)
- [/setmaxdownloadtime Bitsadmin](bitsadmin-setmaxdownloadtime.md)
- [/setminretrydelay Bitsadmin](bitsadmin-setminretrydelay.md)
- [/setnoprogresstimeout Bitsadmin](bitsadmin-setnoprogresstimeout.md)
- [/setnotifycmdline Bitsadmin](bitsadmin-setnotifycmdline.md)
- [/setnotifyflags Bitsadmin](bitsadmin-setnotifyflags.md)
- [/setpeercachingflags Bitsadmin](bitsadmin-setpeercachingflags.md)
- [/SetPriority Bitsadmin](bitsadmin-setpriority.md)
- [/setproxysettings Bitsadmin](bitsadmin-setproxysettings.md)
- [/setreplyfilename Bitsadmin](bitsadmin-setreplyfilename.md)
- [/setsecurityflags Bitsadmin](bitsadmin-setsecurityflags.md)
- [/setvalidationstate Bitsadmin](bitsadmin-setvalidationstate.md)
- [/Suspend Bitsadmin](bitsadmin-suspend.md)
- [/TakeOwnership Bitsadmin](bitsadmin-takeownership.md)
- [/Transfer Bitsadmin](bitsadmin-transfer.md)
- [/util Bitsadmin](bitsadmin-util.md)
- [Bitsadmin/util/enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
- [Bitsadmin/util/GETIEPROXY](bitsadmin-util-and-getieproxy.md)
- [Bitsadmin/util/Help](bitsadmin-util-and-help.md)
- [Bitsadmin/util/REPAIRSERVICE](bitsadmin-util-and-repairservice.md)
- [Bitsadmin/util/SETIEPROXY](bitsadmin-util-and-setieproxy.md)
- [Bitsadmin/util/Version](bitsadmin-util-and-version.md)
- [/Wrap Bitsadmin](bitsadmin-wrap.md)
