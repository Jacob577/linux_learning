## FDS on mac

### Step 1
Installera FDS, antingen manuellt eller genom terminalen. 

```bash
# Om du har systemet på svenska, byt ut Downloads mot nerladdningar
cd ~Downloads

wget https://github.com/firemodels/fds/releases/download/FDS6.7.9/FDS6.7.9_SMV6.7.21_rls_osx.sh

bash FDS6.7.9/FDS6.7.9_SMV6.7.21_rls_osx.sh

# Följ instruktionerna, det är bara att klicka massa enter och ett "yes" typ
```

### Step 2 
I detta steget kommer du manuellt ändra system paths men när du kör kommandot `source ~/.bashrc` kommer du bli promptad om du gjort rätt eller inte. Blir du promptad att du behöver ha priviligerade rättigheter eller dylikt, lägg till `brew` framför dina kommandon. ex: `brew nano ~/.bashrc`. 

I slutet av din installering kommer det komma upp två rader som er ut typ såhär: 
```bash
source /home/fesg/FDS/FDS6/bin/FDS6VARS.sh
source /home/fesg/FDS/FDS6/bin/SMV6VARS.sh
```

Dessa ska du lägga till i `~/.bashrc` filen. 

Kör kommandona
```bash
nano ~/.bashrc 

# Gå till botten och copiera in båda source raderna
# ctrl+x och sedan y för att komma ut ur filen och spara.

# Skriv sedan
source ~/.bashrc
```

Får du inte till detta steget är det viktigt att du tar bort de raderna du lagt till längst ner i `bashrc` filen.

### Step 3
Kolla så sakerna fungerar

Skriv: `fds -v` så ser du om saker och ting fungerar. 

För macs kan det ibland vara ett problem att köra smokeview, du kan behöva navigera till filen och manuellt skriva `smokeview <filename>` för att starta smokeview. 