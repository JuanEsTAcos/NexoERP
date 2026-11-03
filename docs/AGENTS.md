# # NEXO-ERP: reglas para agentes

ERP web para servicios y comercio (Ventas/POS, Citas, Inventario, Contabilidad,

Notificaciones, Nómina). Proyecto académico: el objetivo es demostrar patrones

de diseño GoF de forma clara y probada.

\## Stack

\- TypeScript estricto (\`strict: true\`, prohibido \`any\`)

\- Monorepo pnpm: \`apps/api\` (Fastify + Zod + Kysely) y \`apps/web\` (React + Vite)

\- PostgreSQL en Docker (\`docker-compose.yml\`), migraciones SQL en \`db/migrations/\`

\- Pruebas: Vitest. Lint: ESLint

\## Comandos

\- \`docker compose up -d\`  → base de datos

\- \`pnpm install\`

\- \`pnpm dev\`              → API (y web cuando exista)

\- \`pnpm test\`             → Vitest

\- \`pnpm typecheck\`        → \`tsc --noEmit\`

\- \`pnpm migrate\`          → aplica migraciones

\## Reglas

1\. Lee \`docs/plan.md\` y la spec del módulo en \`docs/\` antes de escribir código.

2\. Una tarea pequeña por sesión. No toques módulos fuera de la tarea.

3\. Un patrón por archivo, con comentario \`// Patrón: \<Nombre\>\` al inicio.

4\. Los módulos NO se importan entre sí: se comunican por eventos (\`shared/events\`).

5\. La conexión a BD se inyecta (parámetro o contenedor). Sin Singleton clásico.

6\. El checkout POS corre en UNA transacción (Unit of Work). Los eventos se

   guardan en \`outbox\_events\` dentro de esa misma transacción.

7\. Toda pieza con patrón lleva prueba Vitest (caso feliz y caso de error).

8\. No instales dependencias nuevas sin decirlo en el resumen final.

9\. Al terminar: lista archivos cambiados, comandos para verificar y qué falta.

\## Mapa de patrones (dónde vive cada uno)

\| Patrón \| Ruta \|

\|---\|---\|

\| Observer \| \`apps/api/src/shared/events/event-bus.ts\` \|

\| Proxy \| \`apps/api/src/shared/security/permisos.proxy.ts\`, \`cache.proxy.ts\` \|

\| Adapter \| \`apps/api/src/integrations/\*\` \|

\| Builder \| \`modules/ventas/domain/factura.builder.ts\` \|

\| Strategy \| \`modules/ventas/pricing/\*\`, \`modules/ventas/payments/\*\` \|

\| Chain of Responsibility \| \`modules/ventas/validation/cadena-validacion.ts\`, \`modules/nomina/cadena-liquidacion.ts\` \|

\| Command \| \`modules/ventas/pos/commands/\*\` \|

\| Facade \| \`modules/ventas/pos/pos-checkout.facade.ts\` \|

\| State \| \`modules/citas/domain/states/\*\`, estados de pedido \|

\| Factory Method \| \`modules/notificaciones/notificacion.factory.ts\` \|

\## Idioma

Código en inglés o español consistente por módulo (preferido: nombres de

dominio en español, p. ej. \`Pedido\`, \`Factura\`). Comentarios y docs en español.
