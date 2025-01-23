# Conexión base de datos relacional

- ## ¿Qué es JDBC?
JDBC (Java Database Connectivity) es una herramienta que permite que un programa desarrollado en Java pueda interactuar directamente con una base de datos. Su objetivo principal es facilitar esa comunicación de manera eficiente y estructurada, sin importar el tipo de base de datos que se esté utilizando. Gracias a JDBC, se pueden realizar operaciones como guardar información, consultar datos, actualizarlos o eliminarlos, todo de forma sencilla y ordenada.

---

- ## ¿Cuáles son sus componentes?

### 1. **Driver JDBC:**
  
Este es el primer elemento esencial, ya que funciona como un intérprete o traductor que permite que el programa en Java pueda comunicarse con la base de datos. Cada tipo de base de datos (por ejemplo, MySQL, PostgreSQL, Oracle, etc.) tiene su propio driver, que traduce las instrucciones en un lenguaje que esa base de datos pueda entender.

  
### 2. **Conexión:**

La conexión es como un canal o un enlace directo que se establece entre el programa y la base de datos. Es lo que permite enviar y recibir información entre ambas partes. Sin esta conexión, no sería posible la comunicación.



### 3. **Sentencias:**

Una vez que se establece la conexión, se pueden enviar instrucciones a la base de datos. Estas instrucciones, conocidas como sentencias, pueden ser de distintos tipos:

- **Consultas (SELECT):** Para leer datos de la base de datos.
- **Inserciones (INSERT):** Para agregar nuevos datos.
- **Actualizaciones (UPDATE):** Para modificar información existente.
- **Eliminaciones (DELETE):** Para borrar datos.

### 4. **Resultados:**

Cuando se ejecutan las sentencias, la base de datos devuelve una respuesta. Por ejemplo, si se realiza una consulta, el resultado será un conjunto de datos que se puede procesar y utilizar en el programa. En otros casos, como al agregar o modificar datos, el resultado puede ser simplemente una confirmación de que la operación se realizó con éxito.

---

- ## Librerías de Scala que permiten conectarse a una base de datos relacional
  
| **Aspecto**                  | **Slick**                                                                                          | **Doobie**                                                                                         | **JDBC**                                                                                           |
|-------------------------------|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| **¿Qué hace?**                | Facilita el trabajo con bases de datos al usar objetos en lugar de escribir consultas complejas. | Permite trabajar directamente con consultas SQL dentro del programa.                             | Proporciona una API estándar para conectar, consultar y trabajar con bases de datos relacionales. |
| **Cómo funciona**             | Convierte los datos de la base de datos en objetos que pueden usarse fácilmente en el código.    | Trabaja con consultas SQL que el programador escribe manualmente para más control.               | Usa clases estándar de Java (`DriverManager`, `Connection`, `Statement`) para realizar operaciones SQL. |
| **Facilidad de uso**          | Es más fácil de aprender porque automatiza muchos procesos y simplifica el manejo de datos.      | Requiere conocimientos más avanzados, ya que todo se hace de manera manual con SQL.               | Es directo, pero requiere más código y cuidado manual para manejar las conexiones y consultas.    |
| **Control sobre las consultas**| Ofrece una forma simplificada, aunque permite escribir consultas SQL si es necesario.           | Da control completo sobre las consultas porque todo se basa en SQL escrito directamente.          | Da control completo sobre las consultas porque se trabajan manualmente con SQL.                  |
| **Resultados**                | Genera objetos automáticamente basados en los datos de la base de datos.                        | Requiere que el programador defina cómo manejar los datos que se obtienen.                        | Los resultados son manejados directamente mediante la clase `ResultSet`.                          |
| **Gestión de conexiones**     | Maneja automáticamente las conexiones a la base de datos.                                       | Da más control al programador sobre las conexiones, pero requiere más atención.                   | El programador debe gestionar manualmente las conexiones y asegurarse de cerrarlas.               |
| **Curva de aprendizaje**      | Más sencilla, ideal para quienes buscan una herramienta fácil de usar.                          | Más técnica, adecuada para quienes tienen experiencia con SQL y programación funcional.            | Muy técnica, ya que requiere conocimientos detallados de SQL y manejo manual de recursos.         |
| **Casos ideales**             | Útil para proyectos que necesitan facilidad y rapidez en el manejo de bases de datos.           | Ideal para proyectos donde se necesita control detallado y se trabaja mucho con consultas SQL.     | Adecuado para proyectos simples o cuando se requiere acceso directo y completo a SQL.             |


### Conclusión

- **Slick** es una buena opción si se busca algo fácil de usar y que automatice tareas comunes al trabajar con bases de datos.
- **Doobie** es mejor si se necesita un control total sobre las consultas SQL y se trabaja con conceptos avanzados de programación funcional.
---

- ## ¿Cómo establecer una conexión a una base de datos relacional (mysql)?

### Paso 1: Crear una base de datos en MySQL
1. Abrir cliente MySQL (puede ser la terminal o un programa como MySQL Workbench).
   
2. Ejecutar el siguiente comando para crear una base de datos:
```sql
   CREATE DATABASE ejemplo_scala;
```

3. Usae la base de datos recién creada:
```sql
USE ejemplo_scala;
```

### Paso 2: Crear una tabla y añadir datos de prueba

1. Crear una tabla llamada usuarios con los siguientes campos:
```sql
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    edad INT NOT NULL
);
```

2. Insertar algunos datos de prueba:
```sql
INSERT INTO usuarios (nombre, edad) VALUES
('Juan', 30),
('Ana', 25),
('Luis', 35);
```

3. Verificar que los datos fueron insertados:
```sql
SELECT * FROM usuarios;
```

### Codigo en SQL

![image](https://github.com/user-attachments/assets/1c993309-1ada-4944-80c5-56e22bfc1ae3)


### Paso 3: Configurar Scala para conectarse a MySQL

1. Agrega la dependencia del conector MySQL en el proyecto, SBT.
```scala
libraryDependencies += "mysql" % "mysql-connector-java" % "8.0.33"
```

2. Crea el archivo de código Scala para establecer la conexión:
```scala
package PruebaTablaSQL
import java.sql.{Connection, DriverManager, ResultSet}

object ConexionMySQL {
  def main(args: Array[String]): Unit = {
    // Configuración de conexión
    val url = "jdbc:mysql://localhost:3306/ejemplo_scala"
    val user = "root"
    val password = "saquisari2005"

    // Establecer la conexión
    var connection: Connection = null
    try {
      connection = DriverManager.getConnection(url, user, password)
      println("¡Conexión exitosa a la base de datos!")
    } catch {
      case e: Exception =>
        e.printStackTrace()
    } finally {
      if (connection != null) connection.close()
    }
  }
}
```
### Mensaje en Consola de Conexion excitosa 
![image](https://github.com/user-attachments/assets/2145ad7e-1818-4272-abad-22e34c7df8d4)


### Paso 4 (Opcional): Consultar datos desde Scala
```scala
package PruebaTablaSQL
import java.sql.{Connection, DriverManager, ResultSet}

object ConexionMySQL {
  def main(args: Array[String]): Unit = {
    // Configuración de conexión
    val url = "jdbc:mysql://localhost:3306/ejemplo_scala"
    val user = "root"
    val password = "saquisari2005" 

    // Establecer la conexión
    var connection: Connection = null
    try {
      connection = DriverManager.getConnection(url, user, password)
      println("¡Conexión exitosa a la base de datos!")

      // Crear y ejecutar la consulta
      val statement = connection.createStatement()
      val resultSet: ResultSet = statement.executeQuery("SELECT * FROM usuarios")

      // Mostrar los datos
      while (resultSet.next()) {
        val id = resultSet.getInt("id")
        val nombre = resultSet.getString("nombre")
        val edad = resultSet.getInt("edad")
        println(s"ID: $id, Nombre: $nombre, Edad: $edad")
      }
    } catch {
      case e: Exception =>
        e.printStackTrace()
    } finally {
      if (connection != null) connection.close()
    }
  }
}

```

### Mensaje en Consola que Muestra la tabla de datos Obtenida 
![image](https://github.com/user-attachments/assets/c2e61464-7d3d-4705-afbf-67a46ea4f264)

