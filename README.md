# Orbital-Framework-2

## Primeros pasos
Primero descarga el .zip, para poder continuar con los pasos.

## Descripción
Este es instructivo para el framework modular realizado en Python para la predicción, evaluación geométrica y radiométrica de oportunidades de adquisición de imágenes multiespectrales sobre Zonas de Interés (AOI). Usando la propagación SGP4, geometría solar de alta precisión y análisis de riesgo de conocido como *sun glint*.

## Proceso de Instalación
1. Creación de un entorno virtual:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```

  * Entorno virtual en Windows:
    ```bash
    venv\Scripts\activate
    ```


2. Instalar dependencias:
   ```bash
   pip install -r requirements.txt
   ```

3. Es recomendable configurar como funete de respaldo a Space-Track.org para las TLE. ya que CelesTrak es la fuente primaria, esta opción es opcional, sin embargo para un mejor funcionamiento realiza está configuración. Si CelesTrak falla,
   el framework intenta Space-Track automáticamente. (Para crea una cuenta gratis debes registrarte en https://www.space-track.org/).

   **Importante: Recuerda NUNCA compartir tu usuario y contraseña con nadie.** Recierda que se pierden si cierras la terminal, en dado caso hay que volver a escribirlas antes de correr el script otra vez.

   Los comandos en Windows dependen de que terminal uses sea: **PowerShell**
   ó **cmd (Símbolo del sistema)**, por ello debes de fijarte en el simbolo de tu ventana: si dice `PS C:\...>` es
   PowerShell; si dice solo `C:\...>` es cmd.

   Para **PowerShell** (símbolo `PS D:\...>`) usa lo siguiente:
   ```powershell
   $env:SPACETRACK_USER = "tu_usuario"
   $env:SPACETRACK_PASSWORD = "tu_contraseña"
   python test_spacetrack_connection.py
   ```

   Para **cmd / Símbolo del sistema** (símbolo `D:\...>` sin "PS") usa lo siguiente:
   ```cmd
   set SPACETRACK_USER=tu_usuario
   set SPACETRACK_PASSWORD=tu_contraseña
   python test_spacetrack_connection.py
   ```

   Para **Linux/macOS** (bash/zsh) usa lo sigueinte:
   ```bash
   export SPACETRACK_USER=tu_usuario
   export SPACETRACK_PASSWORD=tu_contraseña
   python test_spacetrack_connection.py
   ```

   En los tres casos: escribe las dos líneas de credenciales primero, presiona
   Enter después de cada una, toma en cuenta que no hay confirmación visual, y luego corre el script **en esa misma ventana**, sin cerrarla. Usa
   `test_spacetrack_connection.py` para verificar la conexión antes de correr
   `main.py` con el respaldo activado.

4. Ejecutar el programa principal:
   ```bash
   python main.py
   ```

## Estructura del proyecto
```
main.py                 # Orquestación multi-satélite y exportación a Excel
diagnostico.py          # Script de diagnóstico manual de un solo satélite/AOI
config/
  config_settings.yaml   # Umbrales geométricos, pesos de scoring, criterios críticos
  sensors.yaml           # Parámetros de Landsat-8/9 OLI y Sentinel-2A/B MSI
src/
  tle_downloader.py       # Descarga de TLE real desde CelesTrak
  tle_loader.py           # Validación de checksum y carga en SGP4
  orbit_propagator.py     # Propagación SGP4 vectorizada más la transformaciones de marco
  coordinate_transformations.py  # TEME/ITRS/WGS-84/ENU (escalar y vectorizado)
  sensor_coverage.py       # Off-nadir, distancia de superficie, cobertura de AOI
  acquisition_windows.py   # Detección de ventanas de adquisición continuas
  solar_geometry.py        # SZA, elevación solar, azimut, iluminación
  glint_analysis.py        # Ángulo y riesgo de sun-glint especular
  radiometric_feasibility.py  # Sombra, saturación, nubosidad
  scoring.py                # Puntuación ponderada y clasificación de factibilidad
  catalog_validation.py     # Validación opcional contra un CSV de catálogo real
  excel_exporter.py         # Generación del reporte .xlsx multi-hoja
test/
  test_framework.py        # Pruebas unitarias (TLE, geometría solar, scoring)
data/
  input/                   # catalog_validation.csv para validación
  output/                  # Excel y log de ejecución generados
```
