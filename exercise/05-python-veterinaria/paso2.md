# Ejercicio 2 — Consultas con parámetros

> 🎯 **Qué vas a aprender:** a pasar valores variables a tus consultas de forma
> **segura**, y a usar `fetchone()` cuando esperas una sola fila.

---

## Paso 2.0 — El problema de concatenar SQL

Nunca hagas esto:

```python
# ❌ MAL — vulnerable a SQL injection
especie = input("Especie: ")
cursor.execute("SELECT * FROM mascotas WHERE especie = '" + especie + "'")
```

Si alguien escribe `' OR '1'='1` como especie, la consulta devuelve todo.
Si escribe `'; DROP TABLE mascotas; --` borra la tabla.

La solución: **parámetros con `%s`**. psycopg2 los escapa automáticamente.

```python
# ✅ BIEN — psycopg2 escapa el valor por ti
cursor.execute("SELECT * FROM mascotas WHERE especie = %s", (especie,))
```

> 💡 El segundo argumento siempre es una **tupla** — aunque sea un solo valor.
> Por eso lleva la coma: `(especie,)` no `(especie)`.

---

## Paso 2.1 — Buscar mascotas por especie

Crea `paso2.py` en tu carpeta de entrega:

```python
import psycopg2

conn = psycopg2.connect(
    host="localhost",
    database="veterinariadb",
    user="postgres",
    password="1234"
)
cursor = conn.cursor()

# Busca por especie usando parámetro seguro
especie = "Perro"

cursor.execute("""
    SELECT m.nombre, t.nombre AS tutor
    FROM mascotas m
    JOIN tutores t ON t.id_tutor = m.tutor_id
    WHERE m.especie = %s
    ORDER BY m.nombre;
""", (especie,))

resultados = cursor.fetchall()

print(f"=== {especie}s registrados ===")
for nombre, tutor in resultados:
    print(f"  {nombre} — tutor: {tutor}")

cursor.close()
conn.close()
```

Ejecuta:

```bash
python3 entregas/apellido_nombre/05-python-veterinaria/paso2.py
```

Resultado esperado (varía según tus datos):

```
=== Perros registrados ===
  Bobby — tutor: Ana López
  Coco — tutor: Carlos Mendoza
  ...
```

---

## Paso 2.2 — Obtener una sola fila con `fetchone()`

Cuando sabes que la consulta devuelve una sola fila (buscar por ID, por ejemplo),
usa `fetchone()` en lugar de `fetchall()`.

Agrega esto al script:

```python
conn = psycopg2.connect(
    host="localhost", database="veterinariadb",
    user="postgres", password="1234"
)
cursor = conn.cursor()

# Busca un tutor específico por id
tutor_id = 1

cursor.execute("""
    SELECT t.nombre AS tutor,
           COUNT(cv.id_consulta) AS consultas,
           COALESCE(SUM(cv.costo), 0) AS total_gastado
    FROM tutores t
    LEFT JOIN consultas_veterinarias cv ON cv.tutor_id = t.id_tutor
    WHERE t.id_tutor = %s
    GROUP BY t.nombre;
""", (tutor_id,))

fila = cursor.fetchone()

if fila:
    tutor, consultas, total = fila
    print(f"\n=== Resumen del tutor id {tutor_id} ===")
    print(f"  Nombre:    {tutor}")
    print(f"  Consultas: {consultas}")
    print(f"  Total:     ${total:.2f}")
else:
    print(f"No existe tutor con id {tutor_id}")

cursor.close()
conn.close()
```

> 🔎 `fetchone()` devuelve una sola tupla (o `None` si no hay resultados).
> Por eso verificamos `if fila:` antes de desempaquetar.

---

## Paso 2.3 — 🧪 Tu turno: historial completo de un tutor

Escribe una consulta que reciba un `tutor_id` como parámetro y devuelva el
**historial completo** de sus consultas: mascota, fecha, motivo, costo y veterinario.
Usa el JOIN de 4 tablas que ya conoces del Set 02.

<details>
<summary>👀 Ver solución</summary>

```python
tutor_id = 1

cursor.execute("""
    SELECT m.nombre   AS mascota,
           cv.fecha_consulta,
           cv.motivo,
           cv.costo,
           v.nombre   AS veterinario
    FROM consultas_veterinarias cv
    JOIN mascotas    m ON m.id_mascota     = cv.mascota_id
    JOIN veterinarios v ON v.id_veterinario = cv.veterinario_id
    WHERE cv.tutor_id = %s
    ORDER BY cv.fecha_consulta;
""", (tutor_id,))

print(f"\n=== Historial tutor {tutor_id} ===")
for mascota, fecha, motivo, costo, vet in cursor.fetchall():
    print(f"  {fecha}  {mascota}  {motivo}  ${costo}  ({vet})")
```

</details>

---

## ✅ Lo que lograste

* **`%s` con tupla** → parámetros seguros que previenen SQL injection.
* **`fetchone()`** → cuando esperas una sola fila; devuelve `None` si no hay resultados.
* **`fetchall()`** → cuando esperas varias filas; devuelve lista vacía si no hay.
* Hacer JOINs de varias tablas exactamente igual que en pgAdmin, pero desde Python.

> 📤 **Entrega:** `paso2.py` con las dos consultas (especie y resumen de tutor)
> + `paso2.png` con captura del output de ambas.
> Dónde ubicar los archivos: [Entrega](ENTREGA.md).

➡️ **Siguiente:** en el [Ejercicio 3](paso3.md) harás `INSERT`, `UPDATE` y `DELETE`
desde Python — el CRUD completo.
