Individuare i certificati di sorveglianza HGS. È necessario un certificato di firma e un certificato di crittografia per inizializzare il cluster del servizio HGS.
Il modo più semplice per fornire un certificato per servizio HGS consiste nel creare un file PFX protetto da password per ogni certificato che contiene le chiavi pubbliche e private. Se si usa chiavi basate su moduli di protezione hardware o altri certificati non esportabile, assicurarsi che il certificato sia installato nell'archivio certificati del computer locale prima di continuare.
Per altre informazioni sui certificati da usare, vedere [ottenere i certificati per HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs).

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->