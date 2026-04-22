# Base Reusable de Invitaciones (Multi-evento)

Guia practica para reutilizar esta base en nuevos eventos sin tocar la logica.

## 1) Estructura Firebase usada

Todo vive bajo el evento activo (`eventId`):

- `eventos/{eventId}/config`
- `eventos/{eventId}/invitados`
- `eventos/{eventId}/rsvp`
- `eventos/{eventId}/deseos`

Notas:

- El sistema ya esta endurecido para priorizar rutas por evento.
- El fallback legacy esta desactivado por defecto en `config.js` (`legacyFallback: false`).

## 2) Como crear un nuevo evento

1. Duplica esta base (carpeta o repo).
2. En `config.js`, ajusta:
   - `config.event.defaultEventId`
   - textos del evento (pareja, fecha, lugares, links, etc.)
3. Define un `eventId` unico y estable (ej: `ana-luis-2028`).
4. Publica/levanta la base.
5. Si vas a abrir una URL sin query params, ese `defaultEventId` sera el activo.

Ejemplo:

```js
event: {
  defaultEventId: "ana-luis-2028",
  eventIdParam: "eventId",
  legacyFallback: { read: false, write: false, subscribe: false }
}
```

## 3) Como sembrar invitados

Funcion principal:

- `migrateLocalGuestsToFirebase(eventId, options)`

Donde ejecutarla:

- En consola del navegador (recomendado desde `admin.html` o `index.html`, con `database.js` cargado).

Ejemplos:

```js
await migrateLocalGuestsToFirebase("ana-luis-2028")
```

```js
await migrateLocalGuestsToFirebase("ana-luis-2028", { force: true })
```

```js
await migrateLocalGuestsToFirebase("ana-luis-2028", { dryRun: true })
```

## 4) Como sembrar config del evento

Funcion principal:

- `seedEventConfigToFirebase(eventId, options)`

Donde ejecutarla:

- En consola del navegador (igual que arriba).

Ejemplos:

```js
await seedEventConfigToFirebase("ana-luis-2028")
```

```js
await seedEventConfigToFirebase("ana-luis-2028", { force: true })
```

```js
await seedEventConfigToFirebase("ana-luis-2028", { dryRun: true })
```

## 5) Como acceder a la invitacion

Formato URL por invitado:

`/index.html?eventId={eventId}&id={guestId}`

Ejemplo:

`/index.html?eventId=ana-luis-2028&id=15`

Notas:

- `eventId` define el evento.
- `id` identifica al invitado para nombre/pases/RSVP.

## 6) Como acceder al panel admin

Formato URL:

`/admin.html?admin={ADMIN_KEY}&eventId={eventId}`

Ejemplo:

`/admin.html?admin=TD-ADMIN-2026&eventId=ana-luis-2028`

Notas:

- `ADMIN_KEY` actual vive en `admin.js` (`const ADMIN_KEY = "TD-ADMIN-2026"`).
- Desde admin puedes: crear/editar/desactivar, copiar links, QR, exportar CSV.

## 7) Checklist rapido antes de entregar al cliente

- [ ] Imagenes cargan bien (desktop y movil).
- [ ] Audio correcto (archivo existe y reproduce al abrir invitacion).
- [ ] Links externos correctos (mapas, playlist, calendario, etc.).
- [ ] RSVP completo: carga invitado, confirma, bloquea reconfirmacion.
- [ ] Buenos deseos: enviar, listar y refrescar en tiempo real.
- [ ] Dashboard: metricas/filtros/export CSV por `eventId` correcto.
- [ ] Admin: alta/edicion/desactivacion, links y QR funcionando.
- [ ] Validar URL final de invitado y URL de admin con `eventId` correcto.

## 8) Como clonar esta base para futuros eventos

1. Copia proyecto.
2. Cambia `defaultEventId` y contenidos en `config.js`.
3. Sube assets del evento (`images/`, `audio/`).
4. Si aplica, siembra `config` e `invitados` en Firebase.
5. Prueba 1 invitado real (`id`) de punta a punta (invitacion -> RSVP -> dashboard/admin).
6. Entrega links finales.

## Comandos utiles (consola navegador)

```js
// Sembrar invitados
await migrateLocalGuestsToFirebase("ana-luis-2028")

// Sembrar config del evento
await seedEventConfigToFirebase("ana-luis-2028")

// (Opcional) limpiar marcas locales de migracion
clearGuestsMigrationMark("ana-luis-2028")
clearEventConfigMigrationMark("ana-luis-2028")
```
# modeloelegance
