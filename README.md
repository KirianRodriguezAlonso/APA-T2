# Segunda tarea de APA 2023: Manejo de números primos

## Nom i cognoms: Kirian Rodríguez Alonso

## Fichero `primos.py`

- El alumno debe escribir el fichero `primos.py` que incorporará distintas funciones relacionadas con el manejo
  de los números primos.

- El fichero debe incluir una cadena de documentación que incluirá el nombre del alumno y los tests unitarios
  de las funciones incluidas.

- Cada función deberá incluir su propia cadena de documentación que indicará el cometido de la función, los
  argumentos de la misma y la salida proporcionada.

- Se valorará lo pythónico de la solución; en concreto, su claridad y sencillez, y el uso de los estándares marcados
  por PEP-8. También se valorará su eficiencia computacional.

### Determinación de la *primalidad* y descomposición de un número en factores primos

Incluya en el fichero `primos.py` las tres funciones siguientes:

- `esPrimo(numero)`   Devuelve `True` si su argumento es primo, y `False` si no lo es.
- `primos(numero)`    Devuelve una **tupla** con todos los números primos menores que su argumento.
- `descompon(numero)` Devuelve una **tupla** con la descomposición en factores primos de su argumento.

### Obtención del mínimo común múltiplo y el máximo común divisor

Usando las tres funciones del apartado anterior (y cualquier otra que considere conveniente añadir), escriba otras
dos que calculen el máximo común divisor y el mínimo común múltiplo de sus argumentos:

- `mcm(numero1, numero2)`:  Devuelve el mínimo común múltiplo de sus argumentos.
- `mcd(numero1, numero2)`:  Devuelve el máximo común divisor de sus argumentos.

Estas dos funciones deben cumplir las condiciones siguientes:

- Aunque se trate de una solución sub-óptima, en ambos casos deberá partirse de la descomposición en factores
  primos de los argumentos usando las funciones del apartado anterior.

- Aunque también sea sub-óptimo desde el punto de vista de la programación, ninguna de las dos funciones puede
  depender de la otra; cada una debe programarse por separado.

### Obtención del mínimo común múltiplo y el máximo común divisor para un número arbitrario de argumentos

Escriba las funciones `mcmN()` y `mcdN()`, que calculan el mínimo común múltiplo y el máximo común divisor para un
número arbitrario de argumentos:

- `mcmN(*numeros)`:  Devuelve el mínimo común múltiplo de sus argumentos.
- `mcdN(*numeros)`:  Devuelve el máximo común divisor de sus argumentos.

### Tests unitarios

La cadena de documentación del fichero debe incluir los tests unitarios de las cinco funciones. En concreto, deberán
comprobarse las siguientes condiciones:

- `esPrimo(numero)`:  Al ejecutar `[ numero for numero in range(2, 50) if esPrimo(numero) ]`, la salida debe ser
                      `[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]`.
- `primos(numeor)`: Al ejecutar `primos(50)`, la salida debe ser `(2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47)`.
- `descompon(numero)`: Al ejecutar `descompon(36 * 175 * 143)`, la salida debe ser `(2, 2, 3, 3, 5, 5, 7, 11, 13)`.
- `mcm(num1, num2)`: Al ejecutar `mcm(90, 14)`, la salida debe ser `630`.
- `mcd(num1, num2)`: Al ejecutar `mcd(924, 780)`, la salida debe ser `12`.
- `mcmN(numeros)`: Al ejecutar `mcm(42, 60, 70, 63)`, la salida debe ser `1260`.
- `mcdN(numeros)`: Al ejecutar `mcd(840, 630, 1050, 1470)`, la salida debe ser `210`.

### Entrega

#### Ejecución de los tests unitarios

Inserte a continuación una captura de pantalla que muestre el resultado de ejecutar el fichero `primos.py` con la opción
*verbosa*, de manera que se muestre el resultado de la ejecución de los tests unitarios.

<img src="tests_unitarios.png" width="250" align="center">


#### Código desarrollado

Inserte a continuación el contenido del fichero `primos.py` usando los comandos necesarios para que se realice el
realce sintáctico en Python del mismo.
```python
"""
Kirian Rodríguez Alonso
Pràctica 2: Manejo números primos
"""

def esPrimo(numero):
    """
    Devuelve True si su argumento es primo, y False si no lo es.
    >>> [numero for numero in range(2,50) if esPrimo(numero)]
    [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
    """
    for prueba in range(2,int(numero**0.5+1)):
        if numero % prueba == 0:
            return False
    return True

def primos(numero):
    """
    Devuelve una tupla con todos los números primos menores que su argumento.
    >>> primos(50)
    (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47)
    """
    return tuple([prueba for prueba in range(2,numero) if esPrimo(prueba)])

def descompon(numero):
    """
    Devuelve una tupla con la descomposicion en factores primos de su argumento
    >>> descompon(36 * 175 * 143) 
    (2, 2, 3, 3, 5, 5, 7, 11, 13)
    """

    factores=tuple()
    for factor in primos(numero+1):
        while numero%factor==0:
            numero=numero//factor
            factores=factores+(factor,)
    return factores



def fact2dic(numero1, numero2):
    
    factores1 = descompon(numero1)
    factores2 = descompon(numero2)
    factores = set(factores1) | set(factores2)
    "comprensiones -> {exprKey : exprVal for elemento in iterable if condicion}"
    dic1 = {factor: 0 for factor in factores}
    dic2 = {factor: 0 for factor in factores}
    
    for factor in factores1: 
        dic1[factor] += 1

    for factor in factores2: 
        dic2[factor] += 1
    
    return dic1, dic2

def mcm(numero1, numero2):
    """
    Devuelve el mínimo común múltiplo de sus argumentos.
    >>> mcm(90, 14)
    630
    """

    dic1, dic2 = fact2dic(numero1,numero2)
    mcm = 1
    for factor in dic1:
        mcm *= factor**max(dic1[factor], dic2[factor])
    return mcm


def mcd(numero1, numero2):
    """
    Devuelve el máximo común divisor de sus argumentos.
    >>> mcd(924, 780)
    12
    """
    dic1, dic2 = fact2dic(numero1,numero2)
    mcd = 1
    for factor in dic1:
        mcd *= factor**min(dic1[factor], dic2[factor])
    return mcd

def mcmN(*numero1):
    """
    Devuelve el mínimo común múltiplo de los numeros dados en la funcion
    >>> mcmN(42, 60, 70, 63)
    1260
    """
    descompon1, lista = (), []
    vtotal = 1
    for i in range(len(numero1)):
        descompon1 += (descompon(numero1[i]), )
        for j in descompon(numero1[i]):
            lista.append(j)
    vlista = set(lista)
    dic1 = {item:0 for item in vlista}
    for i in range(len(numero1)):
        valores = {item:0 for item in vlista}
        for n in descompon1[i]:
            valores[n] += 1
        for n in vlista:
            if(dic1[n] < valores[n]):
               dic1[n] = valores[n]  
    for n in vlista:
        vtotal *= n**dic1[n]
    return vtotal

def mcdN(*numero1):
    """
    Devuelve el mínimo común divisor de sus argumentos.
    >>> mcdN(840, 630, 1050, 1470)
    210
    """
    descompon1, lista = (), []
    vtotal = 1    
    for i in range(len(numero1)):
        descompon1 += (descompon(numero1[i]), )
        for j in descompon(numero1[i]):
            lista.append(j)
    vlista = set(lista)
    dic1 = {item:0 for item in vlista}
    for i in descompon1[0]:
        dic1[i] += 1
    for i in range(len(numero1)):
        dic = {item:0 for item in vlista}
        for num in descompon1[i]:
            dic[num] += 1
        for num in vlista:
            if(dic1[num] > dic[num]):
               dic1[num] = dic[num]  
    for num in vlista:
        vtotal *= num**dic1[num]
    return vtotal

import doctest
doctest.testmod()
```
#### Subida del resultado al repositorio GitHub ¿y *pull-request*?

El fichero `primos.py`, la imagen con la ejecución de los tests unitarios y este mismo fichero, `README.md`, deberán
subirse al repositorio GitHub mediante la orden `git push`. Si los profesores de la asignatura consiguen montar el
sistema a tiempo, la entrega se formalizará realizando un *pull-request* al propietario del repositorio original.

El fichero `README.md` deberá respetar las reglas de los ficheros Markdown y visualizarse correctamente en el repositorio,
incluyendo la imagen con la ejecución de los tests unitarios y el realce sintáctico del código fuente insertado.
