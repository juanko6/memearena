# Aplicación web: MemeArena

## Descripción general:

MemeArena es una plataforma donde los usuarios registrados pueden subir memes (imágenes o frases cortas) y participar en duelos 1 vs 1 generados automáticamente por el sistema. el sistema empareja memes de categorias similares o tematicas relacionadas para crear batallas cada cierto tiempo Los usuarios votan por el meme que consideran mejor en cada batalla, y los memes ganadores suman puntos para el ranking. La aplicación permite gestionar perfiles de usuario, memes, votaciones y rankings, con una interfaz gamificada estilo torneo.

---

## Parte 1: Requerimientos / Casos de uso

### Usuarios sin autenticación

1. Un usuario no registrado debe poder ver las batallas activas en curso con los dos memes enfrentados.
2. Un usuario no registrado debe poder ver el ranking de memes más exitosos por victorias.
3. Un usuario no registrado debe poder ver detalles de un meme, incluyendo título, categoría, contexto, autor, historial de victorias/derrotas.
4. Un usuario no registrado debe poder ver el historial de batallas finalizadas.
5. Un usuario no registrado debe poder ver el feed de los memes publicados por los otros usuarios

### Usuarios autenticados

3. Un usuario debe poder **registrarse, hacer login y logout** en la aplicación.
4. Un usuario debe poder **editar su perfil** (nickname, avatar, bio) y darse de baja.
5. Un usuario autenticado debe poder **subir un meme** con título, imagen, categoría y contexto.
6. Un usuario autenticado debe poder **editar o eliminar sus propios memes**.
7. Un usuario autenticado debe poder **votar en batallas activas** eligiendo uno de los dos memes enfrentados.
8. Un usuario autenticado puede **ver su historial de votos y resultados de duelos**.


### Gestión automática de batallas (Sistema)

9. El sistema debe **crear batallas automáticamente** cada determinado tiempo (ej: cada 6 horas).
10. El sistema debe **seleccionar memes para batalla basándose en categoría similar** o palabras clave en el contexto.
11. El sistema debe **finalizar automáticamente las batallas después de un tiempo** determinado (ej: 60 horas).
12. El sistema debe **actualizar contadores de victorias/derrotas** de los memes según resultados.
13. El sistema debe **evitar emparejar el mismo meme** en batallas consecutivas.
14. La aplicación debe **mostrar resultados en tiempo real** durante las batallas activas.
15. El sistema debe **prevenir votos duplicados del mismo usuario en la misma batalla**.

### Ranking y gamificación

16. La aplicación debe **mostrar un ranking de usuarios** según la cantidad de votos recibidos en sus memes.
17. Se pueden **otorgar medallas o logros** simples por participación (opcional).

---

## Parte 2: Modelo de datos (entidades principales)

erDiagram
    USUARIOS {
        string id_user
        string nickname
        string email
        string password
        string avatar
        string bio
        date fecha_registro
    }
    
    MEMES {
        string id_meme
        string id_usuario
        string titulo
        string imagen
        string categoria
        string contexto
        date fecha_subida
        number victorias
        number derrotas
        boolean activo
        date ultima_batalla
    }

    BATALLAS {
        string id_batalla
        string meme1_id
        string meme2_id
        date fecha_inicio
        date fecha_fin
        number votos_meme1
        number votos_meme2
        string ganador_id
        string estado
        string criterio_emparejamiento
    }

    VOTOS_BATALLA {
        string id_voto
        string id_batalla
        string id_usuario
        string meme_elegido_id
        date fecha_voto
    }

    USUARIOS ||--|{ MEMES : "crea"
    USUARIOS ||--|{ VOTOS_BATALLA : "vota_en"
    MEMES ||--|{ BATALLAS : "participa_como_meme1"
    MEMES ||--|{ BATALLAS : "participa_como_meme2"
    BATALLAS ||--|{ VOTOS_BATALLA : "recibe"