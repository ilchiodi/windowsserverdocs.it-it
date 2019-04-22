> [!Note] 
> Quando si esegue lo strumento di diagnostica Guarded Fabric (Get-HgsTrace - RunDiagnostics), è possibile che venga restituito uno stato non valido che afferma che la configurazione HTTPS sia danneggiata, quando in realtà non lo è o non viene utilizzata. Questo errore può essere restituito indipendentemente dalla modalità di attestazione HGS. Le possibili cause originarie sono le seguenti:
>
> - HTTPS è effettivamente configurato erroneamente/danneggiato<br>
> - Si dovrà utilizzare l'attestazione amministratore e la relazione di trust è danneggiata<br>
> &nbsp;&nbsp;&nbsp;&nbsp;-Si tratta del tutto indipendentemente dal fatto che HTTPS sia configurato correttamente, in modo non corretto, o non è in uso.<br>
>
> Tenere presente che la diagnostica restituirà questo stato non valido solo quando la destinazione è un host Hyper-V. Se la diagnostica è destinata al Servizio Sorveglianza host, lo stato restituito sarà corretto.

<!-- Appears in guarded-fabric-setting-up-the-host-guardian-service-hgs.md and guarded-fabric-troubleshoot-diagnostics.md
-->
