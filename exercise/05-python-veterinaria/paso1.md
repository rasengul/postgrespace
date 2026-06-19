# Ejercicio 1 — Conexión y primera consulta

> 🎯 **Qué vas a aprender:** a instalar `psycopg2`, conectar Python a PostgreSQL y
> ejecutar tu primera consulta desde un script, sin tocar pgAdmin.

---

## Paso 1.0 — ¿Qué es psycopg2?

`psycopg2` es la biblioteca que hace de puente entre Python y PostgreSQL.
Tu script Python le pasa SQL a `psycopg2`, y `psycopg2` lo ejecuta en el servidor
y te devuelve los resultados como listas de Python.

```
script.py  →  psycopg2  →  PostgreSQL  →  resultados  →  script.py
```

---

## Paso 1.1 — Instala psycopg2

Abre el terminal de VS Code:

```bash
pip install psycopg2-binary
```

Verifica que se instaló:

```bash
python3 -c "import psycopg2; print(psycopg2.__version__)"
```

Verás algo como `2.9.x`. ✅

> 💡 `psycopg2-binary` incluye todo en un solo paquete, sin dependencias externas.
> Es la forma recomendada para desarrollo y aprendizaje.

---

## Paso 1.2 — Prepara la veterinaria

Asegúrate de que `veterinariadb` está activa y con datos. En pgAdmin ejecuta:

```sql
SELECT COUNT(*) FROM mascotas;
-- Debe dar 8
```

Si da error o 0, ejecuta primero [`../04-procedimientos-psql/setup.sql`](../04-procedimientos-psql/setup.sql).

---

## Paso 1.3 — Tu primer script: conectar y listar

Crea el archivo `paso1.py` en tu carpeta de entrega:

```
entregas/apellido_nombre/05-python-veterinaria/paso1.py
```

Escribe este código:

```python
import psycopg2

# Datos de conexión
conn = psycopg2.connect(
    host="localhost",
    database="veterinariadb",
    user="postgres",
    password="1234"
)

# Cursor: el objeto que ejecuta SQL
cursor = conn.cursor()

# Ejecuta una consulta
cursor.execute("SELECT nombre, especie FROM mascotas ORDER BY nombre;")

# Obtén todos los resultados
mascotas = cursor.fetchall()

# Muéstralos
print("=== Mascotas registradas ===")
for mascota in mascotas:
    nombre, especie = mascota
    print(f"  {nombre} ({especie})")

print(f"\nTotal: {len(mascotas)} mascotas")

# Cierra la conexión
cursor.close()
conn.close()
```

Ejecuta el script desde el terminal:

```bash
python3 entregas/apellido_nombre/05-python-veterinaria/paso1.py
```

Resultado esperado:

```
=== Mascotas registradas ===
  Bobby (Perro)
  Canela (Conejo)
  Coco (Perro)
  ...

Total: 8 mascotas
```

✅

---

## Paso 1.4 — Entiende el flujo

| Línea | Qué hace |
|---|---|
| `psycopg2.connect(...)` | Abre la conexión al servidor PostgreSQL |
| `conn.cursor()` | Crea el cursor (el "ejecutor" de SQL) |
| `cursor.execute("SELECT ...")` | Envía el SQL al servidor |
| `cursor.fetchall()` | Trae todos los resultados como lista de tuplas |
| `cursor.close()` / `conn.close()` | Libera los recursos de la conexión |

> 🔎 `fetchall()` devuelve una lista de **tuplas**. Cada tupla es una fila:
> `('Bobby', 'Perro')`. Por eso desempaquetamos con `nombre, especie = mascota`.

---

## Paso 1.5 — 🧪 Tu turno: cuenta por especie

Modifica el script para que también imprima cuántas mascotas hay de cada especie,
ordenado de más a menos.

<details>
<summary>👀 Ver solución</summary>

```python
cursor.execute("""
    SELECT especie, COUNT(*) AS total
    FROM mascotas
    GROUP BY especie
    ORDER BY total DESC;
""")

print("\n=== Por especie ===")
for especie, total in cursor.fetchall():
    print(f"  {especie}: {total}")
```

</details>

---

## ✅ Lo que lograste

* Instalar `psycopg2` y conectar Python a PostgreSQL.
* Ejecutar un `SELECT` desde Python y recorrer los resultados.
* Entender el flujo: `connect → cursor → execute → fetch → close`.

> 📤 **Entrega:** `paso1.py` en tu carpeta de entrega (ya está ahí si lo creaste
> en la ruta correcta) + `paso1.png` con captura del output en la terminal.
> Dónde ubicar los archivos: [Entrega](ENTREGA.md).

➡️ **Siguiente:** en el [Ejercicio 2](paso2.md) harás consultas con parámetros
para buscar mascotas y obtener historiales.
