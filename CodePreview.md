
```python
import os
import json
from bs4 import BeautifulSoup

def analyze_html(html_file_path):
    print(f"Analizando el archivo HTML: {html_file_path}")
    
    with open(html_file_path, 'r', encoding='utf-8') as file:
        soup = BeautifulSoup(file, 'lxml')

    etiquetas = set()
    clases = set()
    ids = set()

    for tag in soup.find_all(True):
        etiquetas.add(tag.name)
        for cls in tag.get('class', []):
            clases.add(cls)
        if tag.has_attr('id'):
            ids.add(tag['id'])

    print(f"Análisis de HTML completado: {len(etiquetas)} etiquetas, {len(clases)} clases, {len(ids)} IDs encontrados.")
    
    return etiquetas, clases, ids

def analyze_css(css_folder_path):
    print(f"Analizando archivos CSS en la carpeta: {css_folder_path}")
    
    css_data = {
        'classes': {},
        'ids': {},
        'tags': {}
    }

    for css_file in os.listdir(css_folder_path):
        if css_file.endswith('.css'):
            print(f"Analizando archivo CSS: {css_file}")
            with open(os.path.join(css_folder_path, css_file), 'r', encoding='utf-8') as file:
                for line in file:
                    line = line.strip()
                    if line.startswith('.'):
                        class_name = line.split('{')[0].strip().lstrip('.')
                        if class_name:
                            if class_name not in css_data['classes']:
                                css_data['classes'][class_name] = set()
                            css_data['classes'][class_name].add(css_file)
                    elif line.startswith('#'):
                        id_name = line.split('{')[0].strip().lstrip('#')
                        if id_name:
                            if id_name not in css_data['ids']:
                                css_data['ids'][id_name] = set()
                            css_data['ids'][id_name].add(css_file)
                    else:
                        # Compruebe los selectores de etiquetas
                        tag_selector = line.split('{')[0].strip()
                        if tag_selector and not tag_selector.startswith('.') and not tag_selector.startswith('#'):
                            if tag_selector not in css_data['tags']:
                                css_data['tags'][tag_selector] = set()
                            css_data['tags'][tag_selector].add(css_file)

    # Convierte conjuntos en listas para garantizar la compatibilidad con JSON
    for selector_type in css_data:
        for selector in css_data[selector_type]:
            css_data[selector_type][selector] = list(css_data[selector_type][selector])

    print("Análisis de CSS completado.")
    
    return css_data

def combine_results(etiquetas_html, clases_html, ids_html, css_data):
    print("Combinando resultados de HTML y CSS...")
    
    combined_classes = set(clases_html) | set(css_data['classes'].keys())
    combined_ids = set(ids_html) | set(css_data['ids'].keys())
    combined_tags = set(etiquetas_html) | set(css_data['tags'].keys())

    active_data = {
        'etiquetas': list(etiquetas_html),
        'clases': {cls: css_data['classes'][cls] for cls in clases_html if cls in css_data['classes']},
        'ids': {id_: css_data['ids'][id_] for id_ in ids_html if id_ in css_data['ids']},
        'modificadores': {tag: css_data['tags'][tag] for tag in etiquetas_html if tag in css_data['tags']}
    }

    inactive_data = {
        'etiquetas': [],
        'clases': {cls: css_data['classes'][cls] for cls in combined_classes if cls not in clases_html},
        'ids': {id_: css_data['ids'][id_] for id_ in combined_ids if id_ not in ids_html},
        'modificadores': {tag: css_data['tags'][tag] for tag in combined_tags if tag not in etiquetas_html}
    }

    print("Combinación de resultados completada.")
    
    return active_data, inactive_data

def save_to_json(data, filename):
    print(f"Guardando datos en {filename}...")
    with open(filename, 'w', encoding='utf-8') as file:
        json.dump(data, file, indent=2)
    print(f"Datos guardados en {filename}.")

def main():
    html_file_path = 'index.html'
    css_folder_path = 'css'

    etiquetas_html, clases_html, ids_html = analyze_html(html_file_path)
    css_data = analyze_css(css_folder_path)

    # Combinar resultados de HTML y CSS
    active_data, inactive_data = combine_results(etiquetas_html, clases_html, ids_html, css_data)

    save_to_json(active_data, 'active.json')
    save_to_json(inactive_data, 'inactive.json')


    print("Proceso completado.")

if __name__ == '__main__':
    main()
```
