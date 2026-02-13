# Creaci-n-de-Pol-gonos-en-Blender-
**Esto es parte de mis tareas de Graficación de la carrera ingeniería en sistemas computacionales, empezamos con un código divertido.**

Abrimos Blender y vamos a una ventana llamada Scripts(estara al final).

Ahora explicare el siguente codigo por partes(al finalizar la explicacion estara completo el codigo).

1.
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

Estas líneas son como "limpiar el escritorio". Antes de crear algo nuevo, el código selecciona todo lo que ya existe en tu escena de Blender y lo borra para que no se amontone.

2.La función crear_poligono_2d es como una receta de cocina. Tú le dices cuántos lados quieres y de qué radio (tamaño) será, y ella hace el trabajo.

3.
malla = bpy.data.meshes.new(nombre)
objeto = bpy.data.objects.new(nombre, malla)
bpy.context.collection.objects.link(objeto)

Blender necesita tres cosas para que algo exista:

Malla (Mesh): Los datos puros (puntos y líneas).
Objeto: El "contenedor" que sostiene esa malla.
Vincular: Poner ese objeto dentro de la escena para que lo puedas ver.

4.Aquí es donde ocurre la magia. Para que el polígono sea perfecto (como un hexágono o un pentágono), el código imagina que hay un círculo invisible.
El ángulo: Divide los 360 grados del círculo entre el número de lados.
Coseno y Seno: Son funciones matemáticas que le dicen a Blender: "Si estoy en este ángulo y a esta distancia del centro, pon el punto aquí".

5.
for i in range(lados):
aristas.append((i, (i + 1) % lados))

Esto es como el juego de "unir los puntos". El código dice: "Conecta el punto 0 con el 1, el 1 con el 2...", y al final, conecta el último con el primero para cerrar la figura.

6.
malla.from_pydata(vertices, aristas, [])

Finalmente, esta línea toma toda la lista de puntos y líneas que calculamos y "rellena" la malla de Blender con esa información.

Captura de pantalla del resultado final.

https://github.com/MitziBibble/Creaci-n-de-Pol-gonos-en-Blender-/issues/1#issue-3934904050

import bpy
import math

bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

def crear_poligono_2d(nombre, lados, radio):
    
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)

  
    bpy.context.collection.objects.link(objeto)

    vertices = []
    aristas = []

    
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0))  # Z = 0 para 2D

    for i in range(lados):
        aristas.append((i, (i + 1) % lados))


    malla.from_pydata(vertices, aristas, [])
    malla.update()

crear_poligono_2d("Poligono2D", lados=6, radio=5)
