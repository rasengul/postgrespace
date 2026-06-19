# 📤 Entrega del Set 04 — Procedimientos almacenados y psql

Para cada ejercicio entregarás entre **2 y 3 archivos** según el tipo de paso.

---

## Qué entregar por ejercicio

| Ejercicio | Archivos a entregar | Contenido clave |
|---|---|---|
| **Paso 1** | `paso1.txt` + `paso1.png` | Output de `\conninfo`, `\dt`, `\du` y las dos consultas; captura de la segunda consulta |
| **Paso 2** | `paso2.sql` + `paso2.png` | Las 3 funciones (`costo_total_tutor`, `resumen_tutor`, `mascotas_sin_consulta`) y captura de `resumen_tutor(1)` |
| **Paso 3** | `paso3.sql` + `paso3.png` | Procedimiento completo con la ampliación del 3.5; captura de las dos tablas con filas insertadas |
| **Paso 4** | `paso4.sql` + `paso4.png` | Comandos `CREATE USER`, `GRANT`, `REVOKE`; captura del `ERROR: permission denied` |
| **Paso 5** | `paso5_backup.sql` + `paso5.txt` + `paso5.png` | Backup generado con `pg_dump`; output de terminal de DROP + restauración; captura del conteo final |

> 💡 **Importante:** tus scripts deben **ejecutarse sin error** sobre una base recién
> preparada con `setup.sql`. Antes de entregar, corre `setup.sql` y luego tus scripts
> en orden, y confirma que todo funciona.

---

## Cómo guardar el output de terminal (`.txt`)

1. Ejecuta los comandos en el terminal de VS Code.
2. Selecciona todo el texto relevante (comandos + resultados).
3. Copia y pégalo en un archivo nuevo: **File → New File** en VS Code, pega y guarda.

---

## Dónde van tus archivos

```
entregas/
└── apellido_nombre/
    └── 04-procedimientos-psql/
        ├── paso1.txt
        ├── paso1.png
        ├── paso2.sql
        ├── paso2.png
        ├── paso3.sql
        ├── paso3.png
        ├── paso4.sql
        ├── paso4.png
        ├── paso5_backup.sql
        ├── paso5.txt
        └── paso5.png
```

Reglas de nombre de carpeta: **todo en minúscula, sin tildes, sin `ñ`, sin espacios**.
Ejemplo: María Núñez → `nunez_maria`.

---

## Cómo guardar tu `.sql` y la captura

1. En el **Query Tool**, con tu código escrito, usa **File → Save As** y guarda como `pasoN.sql`.
2. Toma una captura donde se vea **tu SQL** y el **resultado**, y guárdala como `pasoN.png`.

El archivo `paso5_backup.sql` lo genera `pg_dump` directamente en tu carpeta de entrega
— no hay que copiarlo a mano.

---

## Subir tus entregas

Cuando tengas tus archivos en `entregas/apellido_nombre/04-procedimientos-psql/`, haz
**commit** en tu **fork** y abre un **Pull Request** hacia el repositorio del curso.

> ¿No recuerdas cómo? Mira el video tutorial del
> [Set 01](../01-veterinaria/ENTREGA.md#subir-tus-entregas). El proceso es idéntico.
