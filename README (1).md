# Proyecto Integrador – MVP (tkinter)

Pequeño MVP de escritorio en **Python + tkinter** que muestra varios patrones UI básicos: ventana principal, pantalla de bienvenida, formulario con validación y guardado a archivo, lista con operaciones CRUD simples, tabla con `Treeview` (carga CSV) y un `Canvas` de dibujo.

> Probado en Python 3.10+ (tkinter viene en la librería estándar de la mayoría de distribuciones de Python).

---

## 🧭 Estructura sugerida del repo

```
proyecto/
├─ app/
│  ├─ __init__.py
│  ├─ win_home.py
│  ├─ win_form.py
│  ├─ win_list.py
│  ├─ win_table.py
│  └─ win_canvas.py
├─ data/
│  └─ sample.csv      # archivo de ejemplo para la tabla (Treeview)
└─ main.py
```

> **Nota:** Los módulos importan como `from app.win_* import ...`, por eso se recomienda la carpeta `app/` y el `__init__.py`. Si hoy tienes los archivos en la raíz, muévelos a `app/` y añade `__init__.py` vacío para evitar errores de importación.

---

## ▶️ Cómo ejecutar

1. Crea y activa un entorno (opcional pero recomendado).
2. Asegúrate de tener Python 3.10+.
3. Coloca un `data/sample.csv` con cabeceras `nombre,valor1,valor2` (puedes copiar el ejemplo de abajo).
4. Ejecuta:
   ```bash
   python main.py
   ```

### CSV de ejemplo
```
nombre,valor1,valor2
Alice,10,20
Bob,15,30
Carol,12,18
```

---

## 🧩 Módulos y responsabilidades

### `main.py`
Ventana raíz y hub de navegación del MVP. Crea el `Tk()`, el contenedor principal `ttk.Frame` y botones que abren cada subventana (Home, Formulario, Lista, Tabla y Canvas). Finaliza con `root.mainloop()`.


### `app/win_home.py`
Pequeña pantalla de **bienvenida**. Muestra dos botones:
- “Mostrar mensaje”: dispara un `messagebox.showinfo` con *“¡Equipo listo!”*.
- “Cerrar”: destruye la *toplevel window*.


### `app/win_form.py`
Formulario minimalista con **validación** y **guardado** a archivo de texto:
- Campos: `Nombre` (requerido) y `Edad` (entero).
- Botón **Guardar**:
  - Valida que `Nombre` no esté vacío y que `Edad` sea dígito (`str.isdigit()`).
  - Pide ruta con `filedialog.asksaveasfilename(...)`.
  - Persiste un `.txt` con el contenido:
    ```
    Nombre: <nombre>
    Edad: <edad>
    ```
  - Muestra `messagebox.showinfo` en éxito o `showerror` en validaciones.
- Botón **Cerrar**: cierra la ventana.


### `app/win_list.py`
Lista con **CRUD básico** apoyada en `tk.Listbox`:
- Campo `Entry` para capturar un ítem.
- **Agregar**: inserta el texto (si no está vacío) y limpia el `Entry`.
- **Eliminar seleccionado**: borra el ítem con `lb.curselection()`.
- **Limpiar**: elimina todos los ítems con `lb.delete(0, "end")`.
- Mensajería de **advertencia** (`showwarning`) si se intenta agregar vacío.


### `app/win_table.py`
Tabla de solo lectura con `ttk.Treeview` que **carga un CSV**:
- Define 3 columnas: `nombre`, `valor1`, `valor2` y configura `heading/column`.
- Calcula la ruta del CSV como `<repo>/data/sample.csv` usando `pathlib`.
- Si el archivo **no existe**, muestra un `showwarning` y retorna (evita crashear).
- Si existe, lo lee con `csv.DictReader` y hace `tv.insert(...)` por cada fila.
- Botón **Cerrar** para destruir la ventana.


### `app/win_canvas.py`
Demostración simple de dibujo con `tk.Canvas`:
- Dibuja un rectángulo, un óvalo, una línea y un texto.
- Fondo blanco, área ~440×240 px.
- Botón **Cerrar**.


---

## 📝 Flujo general

1. `main.py` crea la **ventana principal** con botones de navegación.
2. Cada botón llama a su **subventana** (`Toplevel`) aislada.
3. Las subventanas no bloquean el `mainloop`; puedes abrir varias a la vez.
4. Cada subventana es **autónoma** y se cierra con su botón “Cerrar”.

---

## 🧯 Manejo de errores y validaciones

- **Formulario**: validación de campos con feedback inmediato (`showerror`).
- **Tabla**: verificación de existencia de `data/sample.csv` para evitar excepciones de E/S; muestra advertencia y retorna con gracia.
- **Lista**: aviso cuando el texto está vacío al intentar agregar.

---

## 🛠️ Extensiones recomendadas

- **Persistencia** en `win_list.py`: guardar/cargar la lista a JSON.
- **Filtros/ordenamiento** en `win_table.py`: ordenar columnas al clic y filtrar por texto.
- **Dibujo interactivo** en `win_canvas.py`: permitir arrastrar/soltar, elegir color/grosor.
- **Tema visual**: usar `ttk.Style()` para un look más consistente (por ejemplo, “clam”).
- **Paquete instalable**: convertir en `pip install -e .` con un `pyproject.toml`.
- **Tests** de lógica no-UI (validadores, parsers) con `pytest`.

---

## 💡 Consejos de empaquetado y distribución

- Mantén la estructura de paquete (`app/`) para que los imports funcionen.
- Si distribuyes para usuarios finales sin Python, considera `pyinstaller`:
  ```bash
  pyinstaller -F -w main.py
  ```
  - `-F` crea un ejecutable único.
  - `-w` omite la consola en sistemas con GUI.

---

## 📚 APIs de tkinter usadas (referencia rápida)

- **Ventanas**: `tk.Tk`, `tk.Toplevel`
- **Contenedores**: `ttk.Frame`
- **Widgets**: `ttk.Label`, `ttk.Button`, `ttk.Entry`, `tk.Listbox`, `ttk.Treeview`, `tk.Canvas`
- **Mensajes/Diálogos**: `messagebox.showinfo/showwarning/showerror`, `filedialog.asksaveasfilename`
- **Layout**: `pack`, `grid`
- **Archivos**: `csv.DictReader`, `pathlib.Path`

---

## 🔍 Troubleshooting

- **Error de importación `app.win_*`**: verifica que exista la carpeta `app/` y un `__init__.py` dentro; mueve los módulos ahí.
- **Tabla vacía**: confirma que `data/sample.csv` existe y tiene las cabeceras correctas.
- **Caracteres extraños** al abrir el TXT en Windows: asegúrate de guardar con `encoding="utf-8"` (ya lo hace el script) y abrir con un editor compatible.

---

## ✨ Créditos

- Autoría: Equipo integrador / curso
- Tecnologías: Python 3.x, tkinter (stdlib)