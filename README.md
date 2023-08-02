# docker-php-composer
Questo repository contiene un Dockerfile per creare un'immagine Docker personalizzata ottimizzata per l'esecuzione di un'applicazione basata su PHP 8.1 con PHP-FPM. 

La cartella "docker/php" contiene il Dockerfile e altri file di configurazione necessari per creare l'immagine Docker. All'interno della cartella "docker/php/conf.d/", è possibile trovare il file "xdebug.ini", che viene utilizzato per configurare l'estensione Xdebug nell'ambiente di sviluppo.

## Dockerfile
Il Dockerfile utilizza la tecnica del multi-stage build per fornire due immagini: una per l'ambiente di produzione e una per l'ambiente di sviluppo.

L'immagine di produzione (app) è una versione snella e ottimizzata dell'applicazione, contenente solo ciò che è necessario per l'esecuzione in un ambiente di produzione. Vengono installate alcune estensioni PHP essenziali e le dipendenze del progetto vengono gestite utilizzando Composer.

L'immagine di sviluppo (app_dev) è basata sull'immagine di produzione e include Xdebug per agevolare il debugging del codice PHP. Xdebug viene impostato sulla modalità "off" per impostazione predefinita, ma può essere attivato a richiesta.

### Immagine di produzione (app)
L'immagine di produzione contiene solo ciò che è necessario per eseguire l'applicazione in un ambiente di produzione. Vengono installate alcune estensioni PHP utili e Composer viene utilizzato per installare le dipendenze del progetto. Questa immagine è ottimizzata per le prestazioni e non include pacchetti di sviluppo o strumenti di debug.

### Immagine di sviluppo (app_dev)
L'immagine di sviluppo (app_dev) è basata sull'immagine di produzione (app) e include Xdebug per agevolare il debugging del codice PHP. Quando si utilizza questa immagine, Xdebug è impostato sulla modalità "off" per impostazione predefinita, ma può essere attivato durante l'esecuzione di `docker-compose up` o altre modalità di esecuzione specificate.

## Creazione dell'immagine Docker di produzione
Per creare l'immagine Docker di produzione, eseguire il seguente comando nella root del progetto:

```bash
docker build --target app -t [dockerhub_username]/php-composer:x.y -f ./path_dockerfile/Dockerfile .
```

`--target app`: Specifica l'alias "app" all'interno del Dockerfile per eseguire solo la fase di produzione del multi-stage build.

`-t [dockerhub_username]/php-composer:x.y`: Assegna un tag all'immagine Docker creata. 
"dockerhub_username" è il nome dell'utenza su Docker Hub, "php-composer" è il nome dell'immagine, e "x.y" è la versione specifica dell'immagine.

## Caricamento dell'immagine su Docker Hub
Una volta creata l'immagine Docker è possibile caricarla su Docker Hub con i seguenti comandi:
```
docker push [dockerhub_username]/php-composer:x.y
```

Assicurarsi di aver effettuato il login su Docker Hub utilizzando docker login prima di eseguire i comandi di push.

## Utilizzo delle immagini Docker
Puoi utilizzare le immagini Docker create in questo progetto come base per altri progetti o ambienti. Puoi farlo specificando il tag dell'immagine nel Dockerfile di un altro progetto o utilizzando il comando FROM in un altro Dockerfile.

# Esempio di Dockerfile per un altro progetto basato sull'immagine di produzione
```
FROM dockerhub_username/php-composer:x.y
```