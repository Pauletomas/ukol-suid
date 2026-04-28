## Technické shrnutí (LPE via SUID & Path Hijacking)

1. **Využitá zranitelnost:** Eskalace privilegií byla provedena pomocí techniky Path Hijacking u binárního souboru `/usr/bin/syscheck`, který má nastaven SUID bit. Program volá systémový příkaz `date` bez uvedení absolutní cesty, což umožnilo podvržení vlastního skriptu v adresáři `/tmp` po jeho přidání na začátek proměnné `PATH`.
2. **Rozdíl mezi UID a EUID:** Při spuštění tohoto programu zůstává `UID` (Real User ID) student, ale `EUID` (Effective User ID) se mění na `root` (vlastník souboru). Díky tomu má proces práva číst chráněné soubory (jako je flag), i když ho spustil běžný uživatel.
3. **Zajištění systému:** Jako administrátor bych zajistil, aby všechny SUID binárky volaly externí utility výhradně pomocí absolutních cest (např. `/usr/bin/date` namísto `date`). Dále je doporučeno v kódu před voláním funkcí typu `system()` vynutit bezpečné a fixní nastavení proměnné prostředí `PATH`.
