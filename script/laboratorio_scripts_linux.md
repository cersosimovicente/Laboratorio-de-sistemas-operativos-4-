#  Laboratorio de Scripts Linux — Guía Básica

> **Nivel:** Principiante  
> **Requisitos:** Terminal Linux/macOS o WSL en Windows  
> **Duración estimada:** 2–3 horas

---

## Índice

1. [¿Qué es un script de shell?](#1-qué-es-un-script-de-shell)
2. [Tu primer script](#2-tu-primer-script)
3. [Variables](#3-variables)
4. [Entrada del usuario](#4-entrada-del-usuario)
5. [Condicionales (if/else)](#5-condicionales-ifelse)
6. [Bucles](#6-bucles)
7. [Funciones](#7-funciones)
8. [Ejercicio integrador](#8-ejercicio-integrador)

---

## 1. ¿Qué es un script de shell?

Un **script de shell** es un archivo de texto que contiene una secuencia de comandos que el sistema operativo ejecuta en orden. Permite automatizar tareas repetitivas.

### Verificar qué shell estás usando

```bash
echo $SHELL
```

Salida esperada:
```
/bin/bash
```

---

## 2. Tu primer script

### Paso 1 — Crear el archivo

```bash
nano hola.sh
```

### Paso 2 — Escribir el contenido

```bash
#!/bin/bash
# Mi primer script
echo "¡Hola, mundo!"
echo "La fecha de hoy es: $(date)"
```

> **Nota:** La primera línea `#!/bin/bash` se llama **shebang**. Le dice al sistema qué intérprete usar.

### Paso 3 — Dar permisos de ejecución

```bash
chmod +x hola.sh
```

### Paso 4 — Ejecutar el script

```bash
./hola.sh
```

Salida esperada:
```
¡Hola, mundo!
La fecha de hoy es: lun abr 14 10:30:00 ART 2026
```

---

## 3. Variables

Las variables guardan información para usarla después.

```bash
#!/bin/bash

# Declarar variables (sin espacios alrededor del =)
nombre="Ana"
edad=25
pi=3.14

# Usar variables con $
echo "Nombre: $nombre"
echo "Edad: $edad"
echo "Pi vale: $pi"

# Variable con el resultado de un comando
directorio_actual=$(pwd)
echo "Estás en: $directorio_actual"
```

###  Ejercicio 3

Crea un script llamado `mis_datos.sh` que declare 3 variables (tu nombre, tu ciudad y el año actual) e imprima una frase usando las tres.

**Solución esperada:**
```
Me llamo Carlos, vivo en Buenos Aires y estoy en el año 2026.
```

---

## 4. Entrada del usuario

El comando `read` permite pedirle datos al usuario durante la ejecución.

```bash
#!/bin/bash

echo "¿Cómo te llamas?"
read nombre

echo "¿Cuántos años tenés?"
read edad

echo "Hola, $nombre. Tenés $edad años."
echo "En 10 años tendrás $((edad + 10)) años."
```

### Opciones útiles de `read`

```bash
# -p: mostrar un prompt en la misma línea
read -p "Ingresá tu nombre: " nombre

# -s: entrada silenciosa (para contraseñas)
read -s -p "Ingresá tu contraseña: " password
echo ""  # salto de línea después

# -n: limitar cantidad de caracteres
read -n 1 -p "¿Continuar? (s/n): " respuesta
echo ""
```

###  Ejercicio 4

Crea `saludo_interactivo.sh` que le pregunte al usuario su nombre y su color favorito, e imprima: `"¡Hola [nombre]! Tu color favorito es [color]."`.

---

## 5. Condicionales (if/else)

Permiten tomar decisiones según condiciones.

### Estructura básica

```bash
#!/bin/bash

read -p "Ingresá un número: " numero

if [ $numero -gt 10 ]; then
    echo "El número es mayor que 10"
elif [ $numero -eq 10 ]; then
    echo "El número es exactamente 10"
else
    echo "El número es menor que 10"
fi
```

### Operadores de comparación numérica

| Operador | Significado         |
|----------|---------------------|
| `-eq`    | igual a             |
| `-ne`    | distinto de         |
| `-gt`    | mayor que           |
| `-lt`    | menor que           |
| `-ge`    | mayor o igual que   |
| `-le`    | menor o igual que   |

### Comparación de cadenas de texto

```bash
#!/bin/bash

read -p "¿Cuál es la contraseña? " clave

if [ "$clave" = "linux123" ]; then
    echo " Acceso concedido"
else
    echo " Contraseña incorrecta"
fi
```

### Verificar si un archivo existe

```bash
#!/bin/bash

read -p "Ingresá el nombre del archivo: " archivo

if [ -f "$archivo" ]; then
    echo " El archivo existe"
elif [ -d "$archivo" ]; then
    echo " Eso es un directorio, no un archivo"
else
    echo " No existe"
fi
```

###  Ejercicio 5

Crea `calculadora_simple.sh` que pida dos números y una operación (`+`, `-`, `*`, `/`) y muestre el resultado usando condicionales.

---

## 6. Bucles

### Bucle `for` — iterar sobre una lista

```bash
#!/bin/bash

# Iterar sobre una lista de valores
for fruta in manzana naranja banana pera; do
    echo "Fruta: $fruta"
done
```

### Bucle `for` — iterar sobre un rango de números

```bash
#!/bin/bash

# Del 1 al 5
for i in {1..5}; do
    echo "Número: $i"
done

# Con paso (de 2 en 2)
for i in {0..10..2}; do
    echo "Par: $i"
done
```

### Bucle `for` — iterar sobre archivos

```bash
#!/bin/bash

echo "Archivos .sh en el directorio actual:"
for archivo in *.sh; do
    echo "  → $archivo"
done
```

### Bucle `while`

```bash
#!/bin/bash

contador=1

while [ $contador -le 5 ]; do
    echo "Vuelta número: $contador"
    contador=$((contador + 1))
done
```

### Bucle `while` — menú interactivo

```bash
#!/bin/bash

opcion=""

while [ "$opcion" != "3" ]; do
    echo ""
    echo "=== MENÚ ==="
    echo "1. Mostrar fecha"
    echo "2. Mostrar usuario"
    echo "3. Salir"
    read -p "Elegí una opción: " opcion

    if [ "$opcion" = "1" ]; then
        date
    elif [ "$opcion" = "2" ]; then
        whoami
    elif [ "$opcion" != "3" ]; then
        echo "Opción inválida"
    fi
done

echo "¡Hasta luego!"
```

###  Ejercicio 6

Crea `tabla_multiplicar.sh` que le pida un número al usuario e imprima su tabla de multiplicar del 1 al 10.

**Salida esperada para el número 7:**
```
7 x 1 = 7
7 x 2 = 14
...
7 x 10 = 70
```

---

## 7. Funciones

Las funciones agrupan código reutilizable.

```bash
#!/bin/bash

# Definir una función
saludar() {
    echo "¡Hola, $1!"   # $1 es el primer argumento
}

# Llamar la función
saludar "María"
saludar "mundo"
```

### Función con múltiples parámetros y valor de retorno

```bash
#!/bin/bash

sumar() {
    local a=$1
    local b=$2
    local resultado=$((a + b))
    echo $resultado     # "retornar" un valor con echo
}

# Capturar el resultado
total=$(sumar 8 5)
echo "8 + 5 = $total"
```

### Función que verifica si un número es par o impar

```bash
#!/bin/bash

es_par() {
    local num=$1
    if [ $((num % 2)) -eq 0 ]; then
        echo "par"
    else
        echo "impar"
    fi
}

for n in 1 2 3 4 5; do
    tipo=$(es_par $n)
    echo "$n es $tipo"
done
```

###  Ejercicio 7

Crea `conversor.sh` con una función `celsius_a_fahrenheit` que reciba grados Celsius y devuelva Fahrenheit. La fórmula es: `F = (C * 9/5) + 32`.

---

## 8. Ejercicio Integrador

Crea un script llamado `gestion_notas.sh` que implemente un sistema básico de notas con las siguientes funcionalidades:

### Requisitos

1. Mostrar un **menú** con opciones:
   - Agregar una nota
   - Ver todas las notas
   - Buscar una nota
   - Salir

2. Las notas se guardan en un archivo `notas.txt`

3. Cada nota tiene fecha y hora automática

### Estructura sugerida

```bash
#!/bin/bash

ARCHIVO="notas.txt"

agregar_nota() {
    # Pedir la nota al usuario y guardarla con fecha
}

ver_notas() {
    # Mostrar el contenido del archivo
}

buscar_nota() {
    # Buscar una palabra clave con grep
}

# Bucle del menú principal
while true; do
    # Mostrar opciones y llamar funciones
done
```

### Solución de referencia

```bash
#!/bin/bash

ARCHIVO="notas.txt"

agregar_nota() {
    read -p "Escribí tu nota: " nota
    echo "[$(date '+%d/%m/%Y %H:%M')] $nota" >> "$ARCHIVO"
    echo " Nota guardada."
}

ver_notas() {
    if [ -f "$ARCHIVO" ] && [ -s "$ARCHIVO" ]; then
        echo ""
        echo "=== Tus notas ==="
        cat "$ARCHIVO"
    else
        echo "No hay notas todavía."
    fi
}

buscar_nota() {
    read -p "¿Qué palabra querés buscar? " palabra
    echo ""
    echo "=== Resultados para '$palabra' ==="
    grep -i "$palabra" "$ARCHIVO" 2>/dev/null || echo "No se encontraron coincidencias."
}

opcion=""
while [ "$opcion" != "4" ]; do
    echo ""
    echo "=============================="
    echo "    GESTOR DE NOTAS"
    echo "=============================="
    echo "1. Agregar nota"
    echo "2. Ver todas las notas"
    echo "3. Buscar nota"
    echo "4. Salir"
    read -p "Opción: " opcion

    case "$opcion" in
        1) agregar_nota ;;
        2) ver_notas ;;
        3) buscar_nota ;;
        4) echo "¡Hasta luego!" ;;
        *) echo "Opción inválida." ;;
    esac
done
```

---

## Referencia rápida

| Concepto           | Ejemplo                          |
|--------------------|----------------------------------|
| Shebang            | `#!/bin/bash`                    |
| Comentario         | `# esto es un comentario`        |
| Variable           | `nombre="Lucía"`                 |
| Usar variable      | `echo $nombre`                   |
| Aritmética         | `$((a + b))`                     |
| Leer input         | `read -p "Pregunta: " var`       |
| Condicional        | `if [ condición ]; then ... fi`  |
| Bucle for          | `for i in {1..5}; do ... done`   |
| Bucle while        | `while [ cond ]; do ... done`    |
| Función            | `mifunc() { ... }`               |
| Ejecutar comando   | `resultado=$(comando)`           |
| Escribir archivo   | `echo "texto" >> archivo.txt`    |

---
