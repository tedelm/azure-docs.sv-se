---
title: Konfigurera Node. js-appar
description: Lär dig hur du konfigurerar en fördefinierad Node. js-behållare för din app. Den här artikeln visar de vanligaste konfigurations åtgärderna.
ms.devlang: nodejs
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: fdc5129fc395f99cb4c244414ea952b2776dc4dc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79252732"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Konfigurera en Linux Node. js-app för Azure App Service

Node. js-appar måste distribueras med alla nödvändiga NPM-beroenden. App Service distributions motor (kudu) körs `npm install --production` automatiskt åt dig när du distribuerar en [git-lagringsplats](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), eller ett zip- [paket](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) med skapande processer växlat. Om du distribuerar dina filer med [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)måste du dock ladda upp de nödvändiga paketen manuellt.

Den här guiden innehåller viktiga begrepp och instruktioner för Node. js-utvecklare som använder en inbyggd Linux-behållare i App Service. Om du aldrig har använt Azure App Service, följer du [snabb starten för Node. js](quickstart-nodejs.md) och [Node. js med MongoDB-kursen](tutorial-nodejs-mongodb-app.md) först.

## <a name="show-nodejs-version"></a>Visa Node. js-version

Om du vill visa den aktuella Node. js-versionen kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Om du vill visa alla Node. js-versioner som stöds kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Ange Node. js-version

Om du vill ställa in din app på en [Node. js-version som stöds](#show-nodejs-version)kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Den här inställningen anger vilken Node. js-version som ska användas, både vid körning och under automatisk paket återställning i kudu.

> [!NOTE]
> Du bör ange Node. js-versionen i projektets `package.json`. Distributions motorn körs i en separat behållare som innehåller alla Node. js-versioner som stöds.

## <a name="customize-build-automation"></a>Anpassa Bygg automatisering

Om du distribuerar din app med hjälp av git-eller zip-paket med build-automatisering aktiverat, App Service bygga automatiserings steg i följande ordning:

1. Kör anpassat skript om det anges `PRE_BUILD_SCRIPT_PATH`av.
1. Kör `npm install` utan några flaggor, som innehåller NPM `preinstall` och `postinstall` skript och som också `devDependencies`installeras.
1. Kör `npm run build` om ett build-skript har angetts i *Package. JSON*.
1. Kör `npm run build:azure` om en build: Azure-skript anges i *Package. JSON*.
1. Kör anpassat skript om det anges `POST_BUILD_SCRIPT_PATH`av.

> [!NOTE]
> Som beskrivs i [NPM-dokument](https://docs.npmjs.com/misc/scripts), skript `prebuild` som `postbuild` heter och körs före `build`och efter, om de anges. `preinstall`och `postinstall` kör före respektive efter `install`.

`PRE_BUILD_COMMAND`och `POST_BUILD_COMMAND` är miljövariabler som är tomma som standard. Definiera `PRE_BUILD_COMMAND`för att köra kommandon för att skapa för bygge. Definiera `POST_BUILD_COMMAND`för att köra kommandon efter kompilering.

I följande exempel anges de två variablerna för en serie kommandon, avgränsade med kommatecken.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Ytterligare miljövariabler för att anpassa Bygg automatisering finns i [Oryx-konfiguration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

Mer information om hur App Service kör och skapar Node. js-appar i Linux finns i [Oryx-dokumentation: så här identifieras och skapas Node. js-appar](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md).

## <a name="configure-nodejs-server"></a>Konfigurera Node. js-Server

Node. js-behållare levereras med [PM2](https://pm2.keymetrics.io/), en produktions process hanterare. Du kan konfigurera appen så att den startar med PM2, eller med NPM, eller med ett anpassat kommando.

- [Kör anpassat kommando](#run-custom-command)
- [Kör NPM-start](#run-npm-start)
- [Kör med PM2](#run-with-pm2)

### <a name="run-custom-command"></a>Kör anpassat kommando

App Service kan starta din app med ett anpassat kommando, till exempel en körbar fil som *Run.sh*. Om du till exempel vill `npm run start:prod`köra kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Kör NPM-start

För att starta din app `npm start`med, se bara till `start` att det finns ett skript i *Package. JSON* -filen. Ett exempel:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Om du vill använda en anpassad *Package. JSON* -modul i projektet kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>Kör med PM2

Behållaren startar automatiskt appen med PM2 när en av de vanliga Node. js-filerna hittas i projektet:

- *bin/www*
- *server.js*
- *app. js*
- *index. js*
- *hostingstart. js*
- En av följande [PM2-filer](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process. JSON* och *eko system. config. js*

Du kan också konfigurera en anpassad start fil med följande fil namns tillägg:

- En *. js* -fil
- En [PM2-fil](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) med fil namns tillägget *. JSON*, *. config. js*, *. yaml*eller *. yml*

Om du vill lägga till en anpassad start fil kör du följande kommando i [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Felsöka via en fjärranslutning

> [!NOTE]
> Fjärrfelsökning är för närvarande en för hands version.

Du kan felsöka Node. js-appen via fjärr anslutning i [Visual Studio Code](https://code.visualstudio.com/) om du konfigurerar den att [köras med PM2](#run-with-pm2), förutom när du kör den med hjälp av *. config. js, *. yml eller *. yaml*.

I de flesta fall krävs ingen extra konfiguration för din app. Om din app körs med en *process. JSON* -fil (standard eller anpassad), måste den ha en `script` egenskap i JSON-roten. Ett exempel:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Om du vill konfigurera Visual Studio Code för fjärrfelsökning, installerar du [App Service-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Följ instruktionerna på sidan anknytning och logga in på Azure i Visual Studio Code.

I Azure Explorer letar du reda på den app som du vill felsöka, högerklickar på den och väljer **Starta fjärrfelsökning**. Klicka på **Ja** för att aktivera den för din app. App Service startar en tunnel-proxy för dig och kopplar fel söknings programmet. Du kan sedan göra förfrågningar till appen och se fel söknings verktyget pausas vid Bryt punkter.

När du är färdig med fel sökningen stoppar du fel sökningen genom att välja **Koppla från**. När du uppmanas bör du klicka på **Ja** om du vill inaktivera fjärrfelsökning. Om du vill inaktivera det senare högerklickar du på din app igen i Azure Explorer och väljer **inaktivera fjärrfelsökning**.

## <a name="access-environment-variables"></a>Få åtkomst till miljövariabler

I App Service kan du [Ange inställningar för appar](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) utanför appens kod. Sedan kan du komma åt dem med hjälp av standard-Node. js-mönstret. Om du till exempel vill få åtkomst till en appinställning med namnet `NODE_ENV` använder du följande kod:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Kör grunt/Bower/Gulp

Som standard körs `npm install --production` kudu när den identifierar en Node. js-app distribueras. Om din app kräver något av de populära automatiserings verktygen, till exempel grunt, Bower eller Gulp, måste du ange ett [anpassat distributions skript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) för att köra det.

Om du vill göra det möjligt för lagrings platsen att köra dessa verktyg måste du lägga till dem i beroenden i *Package. JSON.* Ett exempel:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

I ett lokalt terminalfönster ändrar du katalogen till lagrings platsens rot och kör följande kommandon:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Din databas rot har nu två ytterligare filer: *. distribution* och *Deploy.sh*.

Öppna *Deploy.sh* och hitta `Deployment` avsnittet som ser ut så här:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Avsnittet slutar med att köras `npm install --production`. Lägg till kod avsnittet du måste köra det nödvändiga verktyget *i slutet* av `Deployment` avsnittet:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Se ett [exempel i Mean. js-exemplet](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135)där distributions skriptet också kör ett anpassat `npm install` kommando.

### <a name="bower"></a>Bower

Det här kodfragmentet körs `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Det här kodfragmentet körs `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Det här kodfragmentet körs `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>Identifiera HTTPS-sessionen

I App Service sker [SSL-avslutning](https://wikipedia.org/wiki/TLS_termination_proxy) på lastbalanserare för nätverk, så alla HTTPS-begäranden når din app som okrypterade HTTP-begäranden. Om din applogik behöver kontrollera om användarbegäranden är krypterade eller inte kan du kontrollera `X-Forwarded-Proto`-rubriken.

Med populära ramverk får du åtkomst till `X-Forwarded-*` information i standardappens mönster. I [Express](https://expressjs.com/)kan du använda [betrodda proxyservrar](https://expressjs.com/guide/behind-proxies.html). Ett exempel:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Få åtkomst till diagnostikloggar

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Öppna SSH-session i webbläsare

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Felsökning

Prova följande när en fungerande Node. js-app fungerar annorlunda i App Service eller innehåller fel:

- [Åtkomst till logg strömmen](#access-diagnostic-logs).
- Testa appen lokalt i produktions läge. App Service kör Node. js-appar i produktions läge, så du måste se till att projektet fungerar som förväntat i produktions läge lokalt. Ett exempel:
    - Beroende på *Package. JSON*kan olika paket installeras i produktions läge (`dependencies` vs. `devDependencies`).
    - Vissa webb ramverk kan distribuera statiska filer på ett annat sätt i produktions läge.
    - Vissa webb ramverk kan använda anpassade Start skript när de körs i produktions läge.
- Kör din app i App Service i utvecklings läge. I [Mean. js](https://meanjs.org/)kan du till exempel ställa in appen i utvecklings läge i körningen genom [ `NODE_ENV` att ställa in appens inställning](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings).

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Självstudie: Node. js-app med MongoDB](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [Vanliga frågor och svar om App Service Linux](app-service-linux-faq.md)
