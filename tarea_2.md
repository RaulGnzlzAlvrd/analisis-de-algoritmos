# Análisis de Algoritmos.
# Tarea 2.

> Nombre: González Alvarado Raúl
>
> No. cuenta: 313245312

## Ejercicio 1.

	SubsecuenciaProdMax(X)
		// Creamos dos arreglos de tamaño n + 1 (n = size(X)) donde vamos a guardar los candidatos
		// Vamos a guardar tuplas de la forma (k, [i,j]) donde k es el producto de
		// los elementos desde la entrada i hasta la entrada j.
		// Nota: (1, [0 ,0]) Representa el arreglo vacío y su producto será 1.
		M = [(1, [0,0])] // Arreglo donde vamos a guardar los máximos
		m = [(1, [0,0])] // Arreglo donde vamos a guardar los mínimos 

		// Iteramos por la lista, e es el elemento, 
		// i es el índice del elemento (empezando en 1)
		for e, i in X do:
			// Almacenamos el producto (prod_i-1_M) almacenado el la tupla 
			// que está en i - 1 de los máximos (M) y también los índices [i_M, j_M]
			(prod_i-1_M, [i_M, j_M]) = M[i - 1]

			// Almacenamos el producto (prod_i-1_m) almacenado el la tupla 
			// que está en i - 1 de los mínimos (m) y también los índices [i_m, j_m]
			(prod_i-1_m, [i_m, j_m]) = m[i - 1]

			// Almacenamos el siguiente candidato resultante de multiplicar
			// el máximo anterior (prod_i-1_M) con el elemento actual (e),
			// y guardamos los indices que tendría
			aux_M = (e * prod_i-1_M, [i_M, j_M + 1])

			// Almacenamos el siguiente candidato resultante de multiplicar
			// el mínimo anterior (prod_i-1_m) con el elemento actual (e),
			// y guardamos los indices que tendría
			aux_m = (e * prod_i-1_m, [i_m, j_m + 1])

			// Almacenamos el producto del elemento actual (e), sin multiplicarlo
			// por nada anterior, sus índices son solo su pocisión
			aux_e = (e, [i, i])

			Asignamos el nuevo máximo y mínimo en la posición i de M y m
			M[i] = max(aux_M, aux_m, aux_e)
			m[i] = min(aux_M, aux_m, aux_e)
		endfor
		// Aquí ya terminamos de recorrer la entrada X lo que nos llevó tiempo n (size(X))

		// Almacenamos el máximo de los elementos en M y regresamos los índices
		(max_prod, [i_max, j_max]) = max(M) // Calculamos el máximo
		return [i_max, j_max] // Regresamos los índices

Solo almacenamos el máximo (M) y el mínimo (m) ya que al multiplicarlos con el siguiente elemento (e) puede pasar que e aumente al máximo, y disminuya más al mínimo, que disminuya al máximo y que aumente al mínimo, o que incremente a ambos o que decremente a ambos, por último también consideramos tomar solo 'e' como si empezaramos una nueva secuencia desde su posición.

## Ejercicio 2.

#### a) 5 inversiones de [2,3,8,6,1]  

Las 5 inversiones son: [(2,1), (3,1), (8,1), (6,1), (8,6)]

#### b) ¿Qué arreglo tine más inversiones con elementos del conjunto {1,...,n}? ¿Cuántas tiene?

El arreglo con más inversiones sería [n, n-1, n-2, ..., 3, 2, 1] ya que se tiene invertido al tener el último elemento al inicio entonces tenemos n - 1 inversiones sin importar el orden de todos los otros, si le agregamos que el segundo más grande está en segunda posición se sigue conservando la inversión con n pero a su vez tiene otras n - 2 inversiones, así si tenemos el arreglo [n, n - 1, ..., 1] tendremos la máxima cantidad de inversiones. El total de inversiones sería de:
	
	num_inversiones = (n-1) + (n-2) + ... + 2 + 1 = (n(n-1)) / 2 

#### c) Algoritmo que de el número de inversiones de cualquier permutación de n elementos.

El algoritmo consistiría en primero ordenar el arreglo con cualquier algoritmo de ordenamiento de tiempo O(nlogn) (por ejemplo con merge sort). Una vez ordenado se le podemos asignar un índice a cada elemento de nuestro arreglo original (llamemoslo X) X, dicho índice dependerá de la posición que tiene una vez estando ordenado.

	Por ejemplo:
	X = [2.2, 5, -4, -1, 9, 8, 45]
	Se ordenan:
	[-4, -1, 2.2, 5, 8, 9, 45]

	Entonces nuestro arreglo final sería (llamemoslo X'):
	X' = [(2.2, 3), (5, 4), (-4, 1), (-1, 2), (9, 6), (8, 5), (45, 7)]
	Con (e, i) tq e es el elemento de X y con i su posición como si estuviera ya ordenado.

Con X' ahora sigue aplicar el algoritmo visto en clase para encontrar el número de inversiones en el que se va introduciendo en un árbol con aristas etiquetadas y con n hojas lo elementos de uno en uno, así si tenemos el elemento (e, i) introducimos e en la hoja número i.

Cada que se introduzca un nuevo elemento al árbol vamos incrementando en uno la etiqueta de las aristas que están desde la hoja donde se introdujo ***e*** hasta la raíz. Cada que actualicemos una arista izquierda (***izq***) nos fijamos en la arista derecha (***der***) del padre de ***izq*** y sumamos la etiqueta de ***der*** a un contador global ***num_inversiones*** (que almacena el numero de inversiones contadas hasta el momento).

Vamos introduciendo elementos hasta que terminemos de iterar X'.
Cuando terminemos, ***num_inversiones*** será el número de inversiones en X.

El tiempo de este algoritmo es O(nlogn) ya que para ordenar el primer arreglo nos toma tiempo nlogn y luego para insertar todos los elementos de X' al árbol también nos toma nlogn.

#### Ejercicio 3.

