# Esposicion-prog-REGEX

## 1. ¿Qué es una expresión regular? 
Una expresión regular (o regex) es una secuencia de caracteres que define un patrón de 
búsqueda. Se utiliza para buscar, validar o manipular texto de manera eficiente. Por 
ejemplo, puedes usar una expresión regular para verificar si un texto es un correo 
electrónico válido o para extraer números de un texto. 
## 2. ¿Para qué se usan? 
Las expresiones regulares se usan para: 
● Validar datos: Por ejemplo, verificar si un texto es un correo electrónico, una 
contraseña segura o un número de teléfono válido. 
● Buscar patrones: Encontrar palabras, números o combinaciones específicas en un 
texto. 
● Reemplazar texto: Cambiar partes de un texto que coincidan con un patrón. 
● Dividir cadenas: Separar un texto en partes basadas en un patrón. 
## 3. Explica las expresiones regulares con un ejemplo práctico. 
Supongamos que queremos validar si un texto es un correo electrónico válido. Un correo 
electrónico generalmente tiene la forma usuario@dominio.com. Podemos usar la siguiente 
expresión regular: 
```
val regex = Regex("[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}")
```
● [a-zA-Z0-9._%+-]+: Coincide con el nombre del usuario (letras, números, puntos, 
guiones bajos, etc.). 
● @: Coincide con el símbolo "@". 
● [a-zA-Z0-9.-]+: Coincide con el dominio (letras, números, puntos, guiones). 
● \\.: Coincide con el punto antes del dominio de nivel superior. 
● [a-zA-Z]{2,}: Coincide con el dominio de nivel superior (como .com, .es, etc.). 
Aqui un ejemplo en código: 
```
fun main() { 
val email = "usuario@dominio.com" 
val regex = Regex("[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}") 
if (regex.matches(email)) { 
println("El correo es válido.") 
} else { 
println("El correo no es válido.") 
} 
}
```
## 4. Localiza en la práctica del Ahorcado dónde se utiliza una 
expresión regular. 
 
En el juego del Ahorcado, una expresión regular podría usarse para validar que la entrada 
del usuario sea una sola letra y no un número, símbolo o cadena vacía. Por ejemplo la 
función generarPalabras en la clase Palabra: 
``` 
  fun generarPalabras( 
            cantidad: Int, 
            tamanioMin: Int, 
            tamanioMax: Int, 
            idioma: Idioma = Idioma.ES 
        ): MutableSet<Palabra> { 
            val client = HttpClient { 
                install(ContentNegotiation) { 
                    gson() 
                } 
            } 
 
            val palabras = mutableSetOf<Palabra>() // Usamos un conjunto para evitar 
repeticiones 
 
            val url = "https://random-word-api.herokuapp.com/word?number=${cantidad * 
5}&lang=${idioma.codigo}" 
 
            val patron = if (idioma == Idioma.ES) { 
                "^[a-záéíóúüñ]+$" 
            } else { 
                "^[a-z]+$" 
            } 
 
            runBlocking { 
                try { 
                    while (palabras.size < cantidad) { 
                        // Hacemos la solicitud GET 
                        val respuesta: Array<String> = client.get(url).body() 
 
                        // Filtramos las palabras según las condiciones 
                        val filtradas = respuesta 
                            .map { it.trim().lowercase() } // Convertimos a minúsculas 
                            .filter { it.length in tamanioMin..tamanioMax } // Filtramos por tamaño 
                            .filter { it.matches(Regex(patron)) } // Solo letras 
                            .filter { !it.contains(" ") } // Excluye palabras que contengan espacios 
                            .map { Palabra(it) } // Mapeamos a la data class 
 
                        palabras.addAll(filtradas) 
                    } 
                } catch (e: Exception) { 
                    println("Error al obtener las palabras: ${e.message}") 
                } 
            } 
 
            client.close() 
            return palabras.take(cantidad).toMutableSet() 
        } 
    } 
 
}
```
 
En este código tenemos que la expresión regular se encuentra en esta parte del codigo: 
```
val patron = if (idioma == Idioma.ES) { 
    "^[a-záéíóúüñ]+$" 
} else { 
    "^[a-z]+$" 
} 
```
Esta expresión  usa para la implementación de las palabras acentuadas según si el 
parámetro del idioma coincide con el idioma español,en caso contrario su rango de acción 
recae solo en las letras del abecedario  
 
 
## 5. ¿Qué es una función de extensión? 
Una función de extensión es una función que se añade a una clase existente sin modificar 
su código. En Kotlin, puedes extender una clase con nuevas funciones. Por ejemplo, 
puedes añadir una función a la clase List<String> para filtrar elementos usando una 
expresión regular. 
 
6. Función de extensión filtrar para List<String>. 
Vamos a crear una función de extensión llamada filtrar que filtre los elementos de una lista 
de cadenas (List<String>) usando una expresión regular. 
 
```
fun List<String>.filtrar(regex: Regex): List<String> { 
    return this.filter { regex.matches(it) } 
} 
 
fun main() { 
    val lista = listOf("hola", "123", "adios", "abc123", "kotlin") 
    val regex = Regex("[a-zA-Z]+") // Solo letras 
    val resultado = lista.filtrar(regex) 
println(resultado) // Salida: [hola, adios, kotlin] 
}
```
Explicación: 
1. Definimos la función de extensión filtrar para List<String>. 
2. La función recibe una expresión regular (regex) como parámetro. 
3. Usamos el método filter de Kotlin para recorrer la lista y quedarnos solo con los 
elementos que coinciden con la expresión regular. 
4. En el ejemplo, la expresión regular [a-zA-Z]+ filtra las cadenas que contienen solo 
letras.
