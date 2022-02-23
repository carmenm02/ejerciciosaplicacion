# ejerciciosaplicacion

## Ejercicio 8:

algoritmo descomponer
 #Descompone `ca`en `componentes`en `dimensión` cadenas #separadas por `separador`

Entrada 
  ca: CADENA # La a descomponer
  separador: CARACTER # El carácter que separa las cadenas
  componentes: TABLA(CADENA) # Los componentes de ´ca´
  dimensión: ENTERO # La cantidad de componentes

Precondición
  ca ≠ NULO
  separador ≠ NULO
  está_definido (componentes)

variable
  L : ENTERO # La longitud de `ca`
  k : ENTERO # Última ocurrencia del `separador`
  i : ENTERO #índice del siquiente componente a registrar
  r : ENTERO # Posición siguiente ocurrencia del `separador`

inicialización
L ⟵ longitud (ca) # índice del último carácter
k ⟵ 0
  # Indice de la última ocurrencia del `separador`encontrada
i ⟵ 1 #Indice del siguiente componente a registrar
r ⟵ posición (ca, 1, L) # Búsqueda de la primera ocurrencia de `separador`

mientras que
  k < L y r ≠ AUSENTE
  invariante
    (H)
  variante de control
    L - k

repetir
  afirmación
    ítem (Ca, k) = separador y item (ca,r) = separador
  # Se copia la cadena `ca` entre los caracteres de indices k + 1 y r - 1
  componentes[i] ⟵ sub_cadena (ca, k+l, r-1)
  dimensión ⟵ i
  afirmación
    componer (componentes, 1, i, separador) = sub_cadena (ca, 1, r-1)
      # componentes [1..i] contiene descomposición de ca[1..r - 1] dimensión = i
    ítem (ca, k) = separador
      # es la penúltima ocurrencia de `separador`
    ítem (ca, r) = separador
      # es la última ocurrencia de `separador`
      
  #Ajusta la cantidad de componentes
  i ⟵ i + 1
  afirmación
    componer (componentes, 1, i-1, separador) = sub cadena (ca, 1, r - 1）
      #componentes[1..i - 1] contiene descomposición de ca[1..r-1] dimensión = i - 1
      
  #La última ocurrencia encontrada de `separador`en el indice k k ⟵ r
  afirmación
    componer (componentes, 1, i-1, separador) = sub_cadena (ca, 1, k-1)
      #componentes [1.. i - 1] contiene descomposición de ca[1..k - 1] 
    dimensión = i - 1
    ítem (ca, k) = separador y item (ca, r) = separador
      # es la última ocurrencia de `separador`
      
  r ⟵ posición (ca, k + 1, L)
  afirmación
    componer (componentes, 1, i - 1, separador) = sub_cadena (ca, 1, k - 1)
      # componentes [1..i - 1] contiene descomposición de ca[1..k - 1]
    dimensión = i - 1
    ítem (ca, k) = separador
    r = posición (ca, k + 1, L, separador)

fin mientras que

si
  k < L

entonces
  afirmación
    r = AUSENTE
  componentes [i] ⟵ sub_cadena (ca, k + 1, L)
  dimensión ⟵ i
fin si

post condición
  # `ca`y `separador`no se modifican
  antiguo (ca) = ca
  antiguo (separador) = separador
  # el tamaño de `componentes' es suficiente
  indice min (componentes) + dimensión - 1 < indice_max (componentes)
  
  # Solo se modifican las `dimensión`primeras celdas de `componentes`
  son_idénticas
  (
    componentes,
    indice_min (componentes) + dimensión,
    indice_max (componentes),
    antiguo (componentes),
    indice_min (componentes) + dimensión,
    indice_max (componentes)
  )
  
  # `ca` es igual a la composición de las `dimensión primeras celdas de `componentes`
  ca = componer 
    (
      sub_tabla
        (
          componentes,
          indice_min (componentes),
          indice_min (componentes) + dimensión - 1
        ),
      separador
    )

fin descomponer

# Ejercicio 9

Algoritmo índice_palabra
  # La primera palabra de `diccionario`, después de la palabra de índice `ìnicio`, que empieza por ìnicial`.

Entrada
  inicial : CARACTER
    # La inicial a buscar
  diccionario : TABLA(CADENA)
    # Objetivo de la búsqueda
  inicio : ENTERO
    # Índice de la primera celda a observar

Resultado : ENTERO

precondición
  es_alfabético(inicial)
  está_definido(diccionario)
  índice_válido(diccionario, inicio)

post condición
  #AUSENTE si no hay palabra con la inicial `ìnicial` en sub_tabla (diccionario, inicio, índice_max(diccionario))
  Resultado = AUSENTE => (∀k ∈ Z)
    (
      inicio <= k <= índice_max(diccionario) =>
      ítem(diccionario[k], 1) ≠ inicial
    )
  # Resultado es el indice de una palabra de inicial `ìnicial` en sub_tabla(diccionario, inicio,índice_max(diccionario))
  Resultado ≠ AUSENTE =>
    (
      inicio <= Resultado <= índice_max(diccionario)
      y
      ítem(diccionario[Resultado], 1) = inicial
    )

fin índice_palabra
