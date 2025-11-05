# Plataforma de Gestión de Productos  

Este proyecto implementa una plataforma de administración de productos para una tienda en línea, utilizando el **sitio administrativo de Django**.  
Permite crear, editar, visualizar y eliminar productos, controlando los niveles de acceso mediante **usuarios, grupos y permisos** del sistema `Auth`.
El proyecto buscar mostrar cómo opera exclusivamente el admin, por lo que no tiene html asociado; es exclusivamente Python y Django.

---

## 📂 Estructura del Proyecto

plataforma_productos/
├─ manage.py
├─ tienda/ # Configuración principal del proyecto
│ ├─ settings.py
│ ├─ urls.py
│ └─ ...
├─ productos/ # Aplicación para gestionar productos
│ ├─ models.py # Definición del modelo Producto
│ ├─ admin.py # Personalización del panel administrativo
│ ├─ migrations/
│ └─ ...
├─ requirements.txt
└─ README.md

---

## Instalación y ejecución

### 1️⃣ Clonar el repositorio

```bash
git clone https://github.com/usuario/plataforma_productos.git
cd plataforma_productos
```
2️⃣ Crear y activar entorno virtual

Windows (PowerShell)

python -m venv .venv
.venv\Scripts\activate

macOS / Linux

python3 -m venv .venv
source .venv/bin/activate

3️⃣ Instalar dependencias
pip install -r requirements.txt

4️⃣ Aplicar migraciones
python manage.py migrate

5️⃣ Crear superusuario
python manage.py createsuperuser

6️⃣ Ejecutar servidor
python manage.py runserver

7️⃣ Ingresar al panel administrativo

👉 http://127.0.0.1:8000/admin/

## Modelo `Producto`

**Campos principales:**

- **nombre:** `CharField(max_length=150)`  
  Nombre del producto (texto corto).

- **descripcion:** `TextField(blank=True)`  
  Descripción opcional y de texto libre.

- **precio:** `DecimalField(max_digits=10, decimal_places=2)`  
  Precio con dos decimales.  
  Incluye un validador `MinValueValidator(0)` para evitar valores negativos.

- **stock:** `PositiveIntegerField(default=0)`  
  Cantidad disponible en inventario.  
  También validado para no permitir negativos.

- **fecha_creacion:** `DateTimeField(auto_now_add=True)`  
  Fecha y hora de creación (se asigna automáticamente y no se puede editar).

**Comportamiento adicional:**

- Los productos se ordenan del más reciente al más antiguo (`ordering = ['-fecha_creacion']`).
- El método `__str__` muestra el nombre y el precio del producto (por ejemplo: `Camiseta ($19990)`).

---

## Grupos y Roles de Usuario

**1. Administradores**
- Permisos: `view`, `add`, `change`, `delete`
- Acceso total a la gestión de productos.
- Pueden crear, editar y eliminar productos.

**2. Gestores de Productos**
- Permisos: `view`, `add`, `change`
- Pueden crear y modificar productos.
- No pueden eliminar.

**3. Solo Lectura**
- Permiso: `view`
- Solo pueden visualizar los productos existentes.
- No pueden crear, editar ni eliminar.

**4. Superusuario**
- Tiene todos los permisos del sistema (ignora las restricciones anteriores).
- Acceso completo al panel administrativo.

> ⚠️ Solo los usuarios con `is_staff=True` pueden acceder al panel `/admin/`.

---

## Usuarios de Prueba

**1. admin_prod**  
- Contraseña: `12345678`  
- Grupo: Administradores  
- Permisos: puede crear, editar y eliminar productos.

**2. gestor_prod**  
- Contraseña: `12345678`  
- Grupo: Gestores de Productos  
- Permisos: puede crear y editar, pero no eliminar.

**3. lector_prod**  
- Contraseña: `12345678`  
- Grupo: Solo Lectura  
- Permisos: solo puede visualizar los productos.

**4. superuser**  
- Contraseña: *(definida al crearlo)*  
- Permisos: acceso total (superusuario).  
- No pertenece a ningún grupo, ya que tiene privilegios globales.

---

# Sistema de autenticación y control de acceso

El acceso a /admin/ está limitado a usuarios autenticados con is_staff=True.

Los permisos se gestionan mediante User, Group y Permission del modelo auth.

El panel administrativo muestra u oculta botones según permisos:

Si el usuario no tiene delete_producto, no verá el botón “Eliminar”.

Si intenta acceder manualmente a /delete/, Django devuelve 403 Forbidden.

# Manejo de errores

Acceso sin autenticación → redirección a /admin/login/.

Credenciales inválidas → mensaje de error claro.

Acceso sin permiso → respuesta 403 Forbidden.

# Entrega

Incluye:

Código fuente completo del proyecto Django

requirements.txt

Usuarios y roles configurados

✨ Proyecto desarrollado por Tatu Vergara ✨
🎵 Músicx · 🧠 Desarrolladorx Fullstack
