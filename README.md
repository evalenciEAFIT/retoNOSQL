# reto Estructuras de Datos (NOSQL)

# Motor de Datos NoSQL

## Descripción

Este proyecto implementa un motor de datos NoSQL básico en C++, que permite gestionar documentos identificados por un `id_document` único (por ejemplo, `"ABC123"`). Cada documento puede almacenar múltiples campos clave-valor, como `nombre`, `edad`, y `programa`, representando un sistema de almacenamiento de datos flexible y eficiente. Utiliza una tabla hash para optimizar las operaciones de acceso, inserción, modificación y eliminación de registros.

### Ejemplo de Documento

Cada documento almacenado en el motor de datos NoSQL tiene la siguiente estructura:

```plaintext
{id_document: "ABC123", nombre: "Edison", edad: 52, programa: "Ingenieria de Sistemas"}
```

## Funcionalidades CRUD

Este sistema permite las siguientes operaciones:

- **INSERT_FIELD**: Inserta un campo nuevo en un documento o crea un documento si no existe.
- **UPDATE_FIELD**: Actualiza el valor de un campo en un documento existente.
- **GET_FIELD**: Recupera el valor de un campo específico dentro de un documento.
- **DELETE_FIELD**: Elimina un campo específico de un documento.
- **LIST_DOCUMENT**: Lista todos los campos y valores de un documento.
- **LIST_ALL**: Lista todos los documentos almacenados con sus respectivos campos y valores.

## Estructura del Proyecto

- `MotorDatosNoSQL.cpp`: Implementación de la clase `MotorDatosNoSQL`, donde se manejan todos los métodos CRUD.
- `main.cpp`: Punto de entrada del programa, incluye pruebas y comandos de ejemplo.
- `README.md`: Documentación detallada del proyecto.
- `Makefile` (opcional): Permite compilar el proyecto y ejecutar el programa mediante el comando `make`.

## Ejecución del Proyecto

1. **Compilación (para C++):**
   ```bash
   g++ main.cpp MotorDatosNoSQL.cpp -o motor_datos_nosql
   ```

2. **Ejecución:**
   ```bash
   ./motor_datos_nosql
   ```

## Ejemplos de Uso de los Comandos

A continuación se presentan ejemplos de cada comando.

### Insertar un Campo en un Documento Nuevo

```plaintext
Comando:
INSERT_FIELD "ABC123" "nombre" "Edison"
Descripción: Inserta el campo "nombre" con el valor "Edison" en el documento con ID "ABC123".
```

### Insertar Más Campos en el Mismo Documento

```plaintext
Comando:
INSERT_FIELD "ABC123" "edad" "52"
INSERT_FIELD "ABC123" "programa" "Ingenieria de Sistemas"
Descripción: Inserta los campos "edad" y "programa" en el documento con ID "ABC123".
```

### Listar Todos los Campos de un Documento

```plaintext
Comando:
LIST_DOCUMENT "ABC123"
Descripción: Muestra todos los campos y valores del documento con ID "ABC123".
Salida esperada:
Documento "ABC123":
  Campo: nombre | Valor: Edison
  Campo: edad | Valor: 52
  Campo: programa | Valor: Ingenieria de Sistemas
```

### Actualizar el Valor de un Campo

```plaintext
Comando:
UPDATE_FIELD "ABC123" "nombre" "Edison Garcia"
Descripción: Actualiza el campo "nombre" en el documento con ID "ABC123" al nuevo valor "Edison Garcia".
```

### Obtener el Valor de un Campo

```plaintext
Comando:
GET_FIELD "ABC123" "programa"
Descripción: Recupera el valor del campo "programa" en el documento con ID "ABC123".
Salida esperada:
Valor: Ingenieria de Sistemas
```

### Eliminar un Campo

```plaintext
Comando:
DELETE_FIELD "ABC123" "edad"
Descripción: Elimina el campo "edad" del documento con ID "ABC123".
```

### Listar Todos los Documentos y Sus Campos

```plaintext
Comando:
LIST_ALL
Descripción: Muestra todos los documentos en la base de datos con sus respectivos campos y valores.
Salida esperada:
Documento "ABC123":
  Campo: nombre | Valor: Edison Garcia
  Campo: programa | Valor: Ingenieria de Sistemas
```

## Código Esqueleto

A continuación se muestra un código esqueleto en C++ que puedes utilizar como base para desarrollar el motor de datos NoSQL.

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

class MotorDatosNoSQL {
private:
    // Tabla hash donde cada documento está identificado por su id_document
    std::unordered_map<std::string, std::unordered_map<std::string, std::string>> tablaDocumentos;

public:
    // Inserta un campo en un documento. Si el documento no existe, se crea.
    void insertField(const std::string& id_document, const std::string& field_key, const std::string& field_value) {
        auto& document = tablaDocumentos[id_document];
        if (document.find(field_key) != document.end()) {
            std::cout << "Error: El campo ya existe en el documento." << std::endl;
            return;
        }
        document[field_key] = field_value;
        std::cout << "Campo insertado en el documento." << std::endl;
    }

    // Actualiza el valor de un campo en un documento existente.
    void updateField(const std::string& id_document, const std::string& field_key, const std::string& new_value) {
        auto it = tablaDocumentos.find(id_document);
        if (it == tablaDocumentos.end() || it->second.find(field_key) == it->second.end()) {
            std::cout << "Error: Documento o campo no encontrado." << std::endl;
            return;
        }
        it->second[field_key] = new_value;
        std::cout << "Campo actualizado en el documento." << std::endl;
    }

    // Recupera el valor de un campo en un documento específico.
    void getField(const std::string& id_document, const std::string& field_key) const {
        auto doc_it = tablaDocumentos.find(id_document);
        if (doc_it != tablaDocumentos.end()) {
            auto field_it = doc_it->second.find(field_key);
            if (field_it != doc_it->second.end()) {
                std::cout << "Valor: " << field_it->second << std::endl;
                return;
            }
        }
        std::cout << "Error: Documento o campo no encontrado." << std::endl;
    }

    // Elimina un campo específico de un documento.
    void deleteField(const std::string& id_document, const std::string& field_key) {
        auto doc_it = tablaDocumentos.find(id_document);
        if (doc_it != tablaDocumentos.end() && doc_it->second.erase(field_key)) {
            std::cout << "Campo eliminado del documento." << std::endl;
        } else {
            std::cout << "Error: Documento o campo no encontrado." << std::endl;
        }
    }

    // Lista todos los campos de un documento.
    void listDocument(const std::string& id_document) const {
        auto doc_it = tablaDocumentos.find(id_document);
        if (doc_it != tablaDocumentos.end()) {
            std::cout << "Documento "" << id_document << "":" << std::endl;
            for (const auto& [field_key, field_value] : doc_it->second) {
                std::cout << "  Campo: " << field_key << " | Valor: " << field_value << std::endl;
            }
        } else {
            std::cout << "Error: Documento no encontrado." << std::endl;
        }
    }

    // Lista todos los documentos y sus campos.
    void listAll() const {
        if (tablaDocumentos.empty()) {
            std::cout << "No hay documentos." << std::endl;
            return;
        }
        for (const auto& [id_document, fields] : tablaDocumentos) {
            std::cout << "Documento "" << id_document << "":" << std::endl;
            for (const auto& [field_key, field_value] : fields) {
                std::cout << "  Campo: " << field_key << " | Valor: " << field_value << std::endl;
            }
        }
    }
};

int main() {
    MotorDatosNoSQL db;

    // Ejemplo de inserción y visualización de documentos
    db.insertField("ABC123", "nombre", "Edison");
    db.insertField("ABC123", "edad", "52");
    db.insertField("ABC123", "programa", "Ingenieria de Sistemas");
    db.listDocument("ABC123");

    return 0;
}
```

## Consideraciones

- **Manejo de errores**: Verifica la existencia del `id_document` y `field_key` antes de realizar operaciones.
- **Formato de entrada**: Asegúrate de que los comandos se ingresen en el formato correcto.
- **Eficiencia**: Este proyecto utiliza una tabla hash para optimizar las operaciones de CRUD.

## Créditos

Proyecto realizado para el curso de estructuras de datos, aplicando técnicas de almacenamiento y manipulación eficiente de datos.

---

Este `README.md` proporciona toda la información necesaria para comprender, ejecutar, y probar el proyecto. Además, incluye un código esqueleto en C++ para guiar el desarrollo del motor de datos NoSQL.
