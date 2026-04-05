# Tarea 1 - Sistemas Distribuidos 2026-1
## Plataforma de caché distribuida para análisis de edificaciones

### Descripción
Sistema distribuido que implementa una plataforma de caché para consultas geoespaciales
sobre el dataset Google Open Buildings, enfocado en la Región Metropolitana de Santiago de Chile.

### Arquitectura
El sistema está compuesto por 4 servicios principales:

- **generador_trafico**: Genera consultas sintéticas siguiendo distribución Zipf o uniforme
- **servicio_cache**: Intercepta consultas y gestiona hits/misses usando Redis
- **generador_respuestas**: Procesa consultas Q1-Q5 sobre datos precargados en memoria
- **almacenamiento_metricas**: Registra y expone métricas del sistema

### Requisitos
- Docker Desktop instalado y corriendo
- Puerto 8001, 8002, 8003 y 6379 disponibles

### Despliegue

1. Clona el repositorio:
```bash
git clone https://github.com/Rostertag1/tarea1-sistemas-distribuidos.git
cd tarea1-sistemas-distribuidos/entrega_1
```

2. Configura los parámetros en el archivo `.env`:
REDIS_MAXMEMORY=200mb
REDIS_POLICY=allkeys-lru
TTL_SEGUNDOS=60
DISTRIBUCION=zipf
TOTAL_CONSULTAS=1000
INTERVALO_SEGUNDOS=0.1

3. Levanta el sistema:
```bash
docker compose up --build
```

### Parámetros configurables (.env)

| Parámetro | Descripción | Valores posibles |
|---|---|---|
| REDIS_MAXMEMORY | Tamaño máximo de la caché | 50mb, 200mb, 500mb |
| REDIS_POLICY | Política de evicción | allkeys-lru, allkeys-lfu, allkeys-random |
| TTL_SEGUNDOS | Tiempo de vida de las claves | cualquier entero positivo |
| DISTRIBUCION | Distribución del tráfico | zipf, uniforme |
| TOTAL_CONSULTAS | Número de consultas a generar | cualquier entero positivo |

### Endpoints disponibles

| Servicio | Endpoint | Descripción |
|---|---|---|
| generador_respuestas | GET /q1/{zona_id} | Conteo de edificios |
| generador_respuestas | GET /q2/{zona_id} | Área promedio y total |
| generador_respuestas | GET /q3/{zona_id} | Densidad por km² |
| generador_respuestas | GET /q4/{zona_a}/{zona_b} | Comparación entre zonas |
| generador_respuestas | GET /q5/{zona_id} | Distribución de confianza |
| servicio_cache | GET /consulta/q1/{zona_id} | Q1 con caché |
| almacenamiento_metricas | GET /metricas | Ver métricas del sistema |
| almacenamiento_metricas | DELETE /metricas/limpiar | Limpiar métricas |

### Zonas disponibles
| ID | Nombre | 
|---|---|
| Z1 | Providencia |
| Z2 | Las Condes |
| Z3 | Maipú |
| Z4 | Santiago Centro |
| Z5 | Pudahuel |

### Integrantes
- Rafael Ostertag
- Valentina Aguirre