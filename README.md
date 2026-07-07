# Sistema `!kiss` estilo Nekotina — BDFD + Components V2

Sistema de comando `!kiss @usuario` para BDFD, con CV2, GIFs de la API de nekos.best, y botones de "Corresponder ❤️" / "Rechazar ❌" que solo puede pulsar el usuario mencionado.

---

## 💋 Flujo 1 — Enviar el beso (`!kiss @usuario`)

El comando valida que se haya mencionado a alguien, que no sea el propio autor, y que no sea un bot. Si todo es correcto, llama a `https://nekos.best/api/v2/kiss`, arma un contenedor CV2 con el gif, el nombre del anime (extraído del JSON, nunca inventado) y dos botones.

![Beso](Assents/Beso.jpg)

Código completo: [`Codigos/!kiss.md`](Codigos/!kiss.md)

---

## 💞 Flujo 2 — Corresponder

Solo el usuario mencionado puede pulsar "💗 Corresponder". Al hacerlo, se llama de nuevo a la API de kiss y el mensaje se actualiza agregando el resultado debajo del resumen original, sin la fila de botones.

![Aceptar](Assents/Aceptar.jpg)

> ⚠️ Esta captura es un **marcador temporal**: este flujo concreto (botón Corresponder) todavía no se confirmó funcionando en Discord durante las pruebas — solo se verificó el flujo de Rechazar. Reemplázala en cuanto lo pruebes con éxito.

---

## 💔 Flujo 3 — Rechazar

Solo el usuario mencionado puede pulsar "❌ Rechazar". Llama a `https://nekos.best/api/v2/slap` y actualiza el mensaje agregando el resultado de rechazo debajo, con su propio gif y su propio footer de anime.

![Rechazar](Assents/Rechazar.jpg)

Código completo: [`Codigos/$onInteraction.md`](Codigos/$onInteraction.md)

---

## Instalación

1. Crea el comando `!kiss` en **BDFD** con el código de [`Codigos/!kiss.md`](Codigos/!kiss.md).
2. Crea (o fusiona con uno existente) el callback `$onInteraction` con el código de [`Codigos/$onInteraction.md`](Codigos/$onInteraction.md).
2. Prueba los 3 mensajes de error, y luego ambos botones con el usuario correcto y con otro usuario distinto.
