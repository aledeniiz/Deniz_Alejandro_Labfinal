https://github.com/aledeniiz/Deniz_Alejandro_Labfinal
# Deniz_Alejandro_Labfinal
def construir_laberinto(muro):
    
    filas, columnas = 5, 5

    
    laberinto = [[' ' for _ in range(columnas)] for _ in range(filas)]

    # Colocar muros en el laberinto
    for coordenada in muro:
        fila, columna = coordenada
        laberinto[fila][columna] = 'X'

    
    laberinto[4][4] = 'S'

    return laberinto


muro = ((0,1), (0,2), (0,3), (0,4), (1,1), (2,1), (2,3), (3,3), (4,0), (4,1), (4,2), (4,3))


laberinto = construir_laberinto(muro)


for fila in laberinto:
    print(' '.join(fila))

from collections import deque

def encontrar_camino(laberinto):
    filas, columnas = len(laberinto), len(laberinto[0])
    entrada = (0, 0)
    salida = (filas - 1, columnas - 1)

    # Función auxiliar para obtener los vecinos válidos de una celda
    def obtener_vecinos(celda):
        x, y = celda
        vecinos = [(x-1, y), (x+1, y), (x, y-1), (x, y+1)]
        return [(nx, ny) for nx, ny in vecinos if 0 <= nx < filas and 0 <= ny < columnas and laberinto[nx][ny] != 'X']

    # Inicialización de la cola para el BFS
    cola = deque([(entrada, [])])
    visitado = set()

    while cola:
        celda_actual, camino_actual = cola.popleft()

        if celda_actual == salida:
            return camino_actual

        if celda_actual not in visitado:
            visitado.add(celda_actual)
            for vecino in obtener_vecinos(celda_actual):
                cola.append((vecino, camino_actual + [obtener_direccion(celda_actual, vecino)]))

    return []

def obtener_direccion(celda_actual, celda_siguiente):
    dx, dy = celda_siguiente[0] - celda_actual[0], celda_siguiente[1] - celda_actual[1]

    if dx == 1:
        return 'Abajo'
    elif dx == -1:
        return 'Arriba'
    elif dy == 1:
        return 'Derecha'
    elif dy == -1:
        return 'Izquierda'

laberinto = construir_laberinto(muro)
camino = encontrar_camino(laberinto)

print(camino)
