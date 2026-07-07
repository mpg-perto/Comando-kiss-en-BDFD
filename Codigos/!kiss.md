# Comando `!kiss`

```bash
$nomention
$c[=== SISTEMA KISS – VALIDACIONES ===]
$if[$mentioned[1]==]
$addContainer[kissErr;#e74c3c]
$addTextDisplay[## ❌ Beso fallido
Tienes que mencionar a alguien. Ejemplo: `!kiss @usuario`;kissErr]
$elseif[$mentioned[1]==$authorID]
$addContainer[kissErr;#e74c3c]
$addTextDisplay[## ❌ Beso fallido
No puedes besarte a ti mismo... eso sería raro. 😅;kissErr]
$elseif[$isBot[$mentioned[1]]==true]
$addContainer[kissErr;#e74c3c]
$addTextDisplay[## ❌ Beso fallido
No puedes besar a un bot.;kissErr]
$else
$httpGet[https://nekos.best/api/v2/kiss]
$if[$httpStatus!=200]
$addContainer[kissErr2;#e74c3c]
$addTextDisplay[## ❌ Beso fallido
No se pudo contactar con la API de nekos.best. Inténtalo de nuevo.;kissErr2]
$else
$addContainer[kissMain;#ff6fa5]
$addTextDisplay[## 💋 ¡Beso enviado!
<@$authorID> le da un dulce beso a <@$mentioned[1]>...;kissMain]
$addMediaGallery[kissGallery;kissMain]
$addMediaGalleryItem[$httpResult[results;0;url];Beso animado;no;kissGallery]
$addSeparator[yes;small;kissMain]
$addTextDisplay[-# Anime: $httpResult[results;0;anime_name];kissMain]
$addActionRow[kissRow;kissMain]
$addButtonCV2[kissAccept-$mentioned[1]-$authorID;💗 Corresponder;success;no;;kissRow]
$addButtonCV2[kissReject-$mentioned[1]-$authorID;❌ Rechazar;danger;no;;kissRow]
$endif
$endif
```

**Nota sobre el ID de los botones:** el formato es `kissAccept-<targetID>-<initiatorID>` / `kissReject-<targetID>-<initiatorID>`. Esto permite, en el `$onInteraction`, verificar quién puede pulsar (comparando `<targetID>` con el ID de quien pulsa) y también recuperar quién iba a besar a quién, sin depender de variables globales (`$setVar`) que podrían chocar si dos `!kiss` ocurren a la vez.
