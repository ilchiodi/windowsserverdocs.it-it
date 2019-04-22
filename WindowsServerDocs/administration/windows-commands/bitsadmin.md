---
title: bitsadmin
description: Argomento i comandi di Windows per **bitsadmin** -bitsadmin è uno strumento da riga di comando che è possibile usare per creare, scaricare, o caricare i processi e monitorarne l'avanzamento.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da0f05ec716cffb7d7532ebac50a091729a6bb18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821072"
---
# <a name="bitsadmin"></a>bitsadmin

> **Si applica a**: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin è uno strumento da riga di comando che è possibile usare per creare scaricare o caricare i processi e monitorarne l'avanzamento. Lo strumento bitsadmin Usa commutatori per identificare il lavoro da eseguire.  È possibile chiamare `bitsadmin /?` o `bitsadmin /HELP` per ottenere un elenco di opzioni.

La maggior parte dei commutatori è necessario un \<processo\> parametro che viene impostato su nome visualizzato del processo o GUID. Si noti che il nome di visualizzazione del processo potrebbe non essere univoco. Il **/ creare** e **/List** commutatori restituiscono GUID del processo.

Per impostazione predefinita, è possibile accedere alle informazioni sui propri processi. Per accedere a informazioni relative ai processi di un altro utente, è necessario disporre dei privilegi di amministratore. Se il processo è stato creato in uno stato con privilegi elevato, quindi è necessario eseguire bitsadmin da una finestra con privilegi elevata. in caso contrario, si avrà accesso in lettura al processo.

Molte delle opzioni che corrispondono ai metodi nel [BITS interfacce](/windows/desktop/bits/bits-interfaces). Per informazioni dettagliate aggiuntive che potrebbero essere rilevanti per l'utilizzo di un'opzione, vedere il metodo corrispondente.

Usare le opzioni seguenti per creare un processo, impostare e recuperare le proprietà di un processo e monitorare lo stato di un processo. Per esempi che illustrano come usare alcune di queste opzioni per eseguire attività, vedere [bitsadmin esempi](bitsadmin-examples.md).

## <a name="switches"></a>Commutatori

[addfile Bitsadmin](bitsadmin-addfile.md)  
[addfileset Bitsadmin](bitsadmin-addfileset.md)  
[addfilewithranges Bitsadmin](bitsadmin-addfilewithranges.md)  
[cache Bitsadmin](bitsadmin-cache.md)  
[Annulla Bitsadmin](bitsadmin-cancel.md)  
[Bitsadmin completo](bitsadmin-complete.md)  
[creare Bitsadmin](bitsadmin-create.md)  
[bitsadmin getaclflags](bitsadmin-getaclflags.md)  
[bitsadmin getbytestotal](bitsadmin-getbytestotal.md)  
[bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)  
[bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)  
[getcompletiontime Bitsadmin](bitsadmin-getcompletiontime.md)  
[getcreationtime Bitsadmin](bitsadmin-getcreationtime.md)  
[getcustomheaders Bitsadmin](bitsadmin-getcustomheaders.md)  
[bitsadmin getdescription](bitsadmin-getdescription.md)  
[getdisplayname Bitsadmin](bitsadmin-getdisplayname.md)  
[geterror Bitsadmin](bitsadmin-geterror.md)  
[bitsadmin geterrorcount](bitsadmin-geterrorcount.md)  
[bitsadmin getfilestotal](bitsadmin-getfilestotal.md)  
[bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)  
[gethelpertokenflags Bitsadmin](bitsadmin-gethelpertokenflags.md)  
[gethelpertokensid Bitsadmin](bitsadmin-gethelpertokensid.md)  
[Bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
[getmaxdownloadtime bitsadmin](bitsadmin-getmaxdownloadtime.md)  
[bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)  
[getmodificationtime Bitsadmin](bitsadmin-getmodificationtime.md)  
[bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)  
[getnotifycmdline Bitsadmin](bitsadmin-getnotifycmdline.md)  
[getnotifyflags Bitsadmin](bitsadmin-getnotifyflags.md)  
[getnotifyinterface Bitsadmin](bitsadmin-getnotifyinterface.md)  
[bitsadmin getowner](bitsadmin-getowner.md)  
[bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)  
[getpriority Bitsadmin](bitsadmin-getpriority.md)  
[bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)  
[bitsadmin getproxylist](bitsadmin-getproxylist.md)  
[bitsadmin getproxyusage](bitsadmin-getproxyusage.md)  
[bitsadmin getreplydata](bitsadmin-getreplydata.md)  
[getreplyfilename Bitsadmin](bitsadmin-getreplyfilename.md)  
[getreplyprogress Bitsadmin](bitsadmin-getreplyprogress.md)  
[bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)  
[bitsadmin getstate](bitsadmin-getstate.md)  
[gettemporaryname Bitsadmin](bitsadmin-gettemporaryname.md)  
[gettype Bitsadmin](bitsadmin-gettype.md)  
[bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)  
[Guida Bitsadmin](bitsadmin-help.md)  
[info Bitsadmin](bitsadmin-info.md)  
[bitsadmin list](bitsadmin-list.md)  
[listfiles Bitsadmin](bitsadmin-listfiles.md)  
[Bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
[monitoraggio bitsadmin](bitsadmin-monitor.md)  
[nowrap Bitsadmin](bitsadmin-nowrap.md)  
[Bitsadmin Caching](bitsadmin-peercaching.md)  
[Bitsadmin peer](bitsadmin-peers.md)  
[rawreturn Bitsadmin](bitsadmin-rawreturn.md)  
[bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)  
[removecredentials Bitsadmin](bitsadmin-removecredentials.md)  
[bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)  
[reimpostazione Bitsadmin](bitsadmin-reset.md)  
[Riprendi Bitsadmin](bitsadmin-resume.md)  
[setaclflag Bitsadmin](bitsadmin-setaclflag.md)  
[setclientcertificatebyid Bitsadmin](bitsadmin-setclientcertificatebyid.md)  
[setclientcertificatebyname Bitsadmin](bitsadmin-setclientcertificatebyname.md)  
[setcredentials Bitsadmin](bitsadmin-setcredentials.md)  
[setcustomheaders Bitsadmin](bitsadmin-setcustomheaders.md)  
[bitsadmin setdescription](bitsadmin-setdescription.md)  
[setdisplayname Bitsadmin](bitsadmin-setdisplayname.md)  
[sethelpertoken Bitsadmin](bitsadmin-sethelpertoken.md)  
[sethelpertokenflags Bitsadmin](bitsadmin-sethelpertokenflags.md)  
[Bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
[setmaxdownloadtime bitsadmin](bitsadmin-setmaxdownloadtime.md)  
[bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)  
[setnoprogresstimeout Bitsadmin](bitsadmin-setnoprogresstimeout.md)  
[setnotifycmdline Bitsadmin](bitsadmin-setnotifycmdline.md)  
[setnotifyflags Bitsadmin](bitsadmin-setnotifyflags.md)  
[bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)  
[setpriority Bitsadmin](bitsadmin-setpriority.md)  
[setproxysettings Bitsadmin](bitsadmin-setproxysettings.md)  
[setreplyfilename Bitsadmin](bitsadmin-setreplyfilename.md)  
[setsecurityflags Bitsadmin](bitsadmin-setsecurityflags.md)  
[bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)  
[Sospendi Bitsadmin](bitsadmin-suspend.md)  
[bitsadmin takeownership](bitsadmin-takeownership.md)  
[trasferimento Bitsadmin](bitsadmin-transfer.md)  
[bitsadmin util](bitsadmin-util.md)  
[incapsulamento Bitsadmin](bitsadmin-wrap.md)  
