# `$onInteraction`

```rb
$nomention
$suppressErrors
$if[$checkContains[$customID;kissAccept-]==true]
$if[$checkContains[$customID;kissAccept-$authorID-]==true]
$httpGet[https://nekos.best/api/v2/kiss]
$if[$httpStatus!=200]
$addContainer[kissMain;#e74c3c]
$addTextDisplay[## ❌ No se pudo contactar con la API. Inténtalo de nuevo.;kissMain]
$else
$addContainer[kissMain;#ff6fa5]
$addTextDisplay[## 💋 Beso enviado
<@$replaceText[$customID;kissAccept-$authorID-;;1]> le dio un dulce beso a <@$authorID>...;kissMain]
$addSeparator[yes;small;kissMain]
$addTextDisplay[## 💞 ¡Correspondido!
<@$authorID> le devolvió el beso. ¡Qué bonito! 🥰;kissMain]
$addMediaGallery[kissGallery;kissMain]
$addMediaGalleryItem[$httpResult[results;0;url];Beso correspondido;no;kissGallery]
$addSeparator[yes;small;kissMain]
$addTextDisplay[-# Anime: $httpResult[results;0;anime_name];kissMain]
$endif
$else
$ephemeral
Esta interacción no te pertenece.
$endif
$endif
$if[$checkContains[$customID;kissReject-]==true]
$if[$checkContains[$customID;kissReject-$authorID-]==true]
$httpGet[https://nekos.best/api/v2/slap]
$if[$httpStatus!=200]
$addContainer[kissMain;#e74c3c]
$addTextDisplay[## ❌ No se pudo contactar con la API. Inténtalo de nuevo.;kissMain]
$else
$addContainer[kissMain;#f04747]
$addTextDisplay[## 💋 Beso enviado
<@$replaceText[$customID;kissReject-$authorID-;;1]> le dio un dulce beso a <@$authorID>...;kissMain]
$addSeparator[yes;small;kissMain]
$addTextDisplay[## 💔 Rechazado
<@$authorID> rechazó el beso... ¡Auch! 😳;kissMain]
$addMediaGallery[kissGallery;kissMain]
$addMediaGalleryItem[$httpResult[results;0;url];Rechazo;no;kissGallery]
$addSeparator[yes;small;kissMain]
$addTextDisplay[-# Anime: $httpResult[results;0;anime_name];kissMain]
$endif
$else
$ephemeral
Esta interacción no te pertenece.
$endif
$endif
```

> [!NOTE]
> El código tiene un `$ephemeral` aunque sea por gusto ya que CV2 en BDFD no da respuestas efímeras a botones
