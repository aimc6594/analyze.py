# Análisis de HTML y CSS

Este script en Python está diseñado para analizar archivos HTML y CSS, y combinar los resultados para identificar y clasificar los elementos activos e inactivos en una página web. El objetivo principal es proporcionar una visión clara de las etiquetas HTML, clases CSS, IDs y selectores de etiqueta utilizados, así como determinar cuáles están en uso y cuáles no. Los resultados se guardan en archivos JSON para su posterior análisis.

## Funcionalidades

1. **Análisis de HTML (`analyze_html`):**
   - **Entrada:** Ruta del archivo HTML.
   - **Salida:** Conjuntos de etiquetas HTML, clases y IDs encontrados en el archivo.
   - **Descripción:** Lee el archivo HTML y utiliza BeautifulSoup para extraer todas las etiquetas, clases y IDs presentes. Retorna estos datos para su posterior procesamiento.

2. **Análisis de CSS (`analyze_css`):**
   - **Entrada:** Ruta de la carpeta que contiene archivos CSS.
   - **Salida:** Diccionario con clases, IDs y selectores de etiquetas CSS, junto con los archivos CSS donde aparecen.
   - **Descripción:** Recorre todos los archivos CSS en la carpeta especificada, identifica las clases, IDs y selectores de etiquetas, y almacena los archivos CSS donde se encuentran.

3. **Combinación de Resultados (`combine_results`):**
   - **Entrada:** Datos de etiquetas, clases y IDs extraídos del HTML y CSS.
   - **Salida:** Dos diccionarios: `active_data` y `inactive_data`.
   - **Descripción:** Combina los resultados del análisis de HTML y CSS para identificar qué clases, IDs y etiquetas están activamente en uso y cuáles no. Separa los datos en activos (presentes en ambos análisis) e inactivos (presentes en CSS pero no en HTML y viceversa).

4. **Guardado en JSON (`save_to_json`):**
   - **Entrada:** Datos a guardar y nombre del archivo.
   - **Salida:** Archivo JSON con los datos guardados.
   - **Descripción:** Guarda los datos combinados en archivos JSON, uno para los datos activos y otro para los inactivos.

## Requisitos

- Python 3.x
- Bibliotecas: `beautifulsoup4`, `lxml`

## Uso

1. Coloca el archivo HTML a analizar en el directorio raíz del proyecto con el nombre `index.html`.
2. Coloca los archivos CSS en una carpeta llamada `css` dentro del directorio raíz.
3. Ejecuta el script con el comando:

   ```bash
   python analyze.py
   ```

4. Los resultados se guardarán en dos archivos JSON: `active.json` e `inactive.json`.

## Ejemplo de Resultados

- **`active.json`**: Contiene elementos HTML y selectores CSS que están en uso en la página.
- **`inactive.json`**: Contiene elementos HTML y selectores CSS que no están en uso en la página.
