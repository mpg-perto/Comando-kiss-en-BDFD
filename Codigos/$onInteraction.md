# `$onInteraction` (sin corchetes — genérico)

```bash
$nomention
$suppressErrors
$c[=== RAMA: CORRESPONDER ===]
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
$c[=== RAMA: RECHAZAR ===]
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

**Cambio de estrategia (importante):** en lugar de forzar un mensaje nuevo (lo cual no funcionó tras varias pruebas reales), aprovechamos el comportamiento real y consistente confirmado en Discord: BDFD actualiza el mensaje original con el contenedor que se construya en `$onInteraction`. Así que se reconstruye el **contenedor completo** con:
1. El resumen del beso original (arriba).
2. Un separador.
3. El resultado — correspondido o rechazado — con su propio gif y su propio footer de anime (abajo).

Como ya no se construye ninguna fila de botones (`$addActionRow`) en esta reconstrucción, los botones desaparecen automáticamente al actualizarse el mensaje — no hace falta `$editButton` para desactivarlos, y tampoco `$reply` ni `$channelSendMessage` (que fue justamente lo que falló en las pruebas).

**Por qué esto cumple las reglas de seguridad:** `$checkContains[$customID;kissAccept-$authorID-]` solo es verdadero si el ID de quien pulsó (`$authorID`) coincide exactamente con el `<targetID>` codificado en el botón. Ni el autor del `!kiss`, ni un admin, ni cualquier otro usuario puede pasar esta condición — solo el usuario mencionado. Cualquiera más recibe el mensaje efímero de "no te pertenece" y no se ejecuta ninguna otra acción.

**Por qué no rompe otros sistemas:** el filtro externo `$checkContains[$customID;kissAccept-]` / `kissReject-` asegura que este bloque solo reacciona a IDs con esos prefijos únicos. Cualquier otro `$onInteraction` que se fusione aquí debe usar sus propios prefijos distintos.
