<h1>Información que cura... muy interesante y me proporciono el dato el equipo de seguridad.</h1>

Lo estoy implementando en mi respaldo de archivos e investigue un poco mas la herramienta, es nativa en Windows usando powershell, les dejo el dato y en el ejemplo de uso un comando que les puede ayudar muchísimo para estos casos:
 
# 📦 Robocopy
Robocopy (Robust File Copy) es una herramienta avanzada de Windows para copiar, sincronizar y respaldar archivos o directorios de forma eficiente, tolerante a errores y optimizada para grandes volúmenes de datos.
Es especialmente útil para:
- respaldos masivos
- sincronización entre servidores
- migraciones de archivos
- replicación de repositorios o proyectos

A diferencia de copy o xcopy, Robocopy ofrece:
- tolerancia a fallos de red
- reintentos automáticos
- multithreading
- logs detallados
- sincronización tipo espejo

# 🎯 Beneficios de usar Robocopy

<table>
    <thead>
        <tr>
            <td><b>Beneficio</b></td>
            <td><b>Descripción</b></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Alta confiabilidad</td>
            <td>Maneja interrupciones de red y reintenta automáticamente</td>
        </tr>
        <tr>
            <td>Copia incremental</td>
            <td>Solo copia archivos nuevos o modificados</td>
        </tr>
        <tr>
            <td>Multithreading</td>
            <td>Copia múltiples archivos en paralelo para mayor velocidad</td>
        </tr>
        <tr>
            <td>Sincronización</td>
            <td>Puede mantener dos carpetas idénticas</td>
        </tr>
        <tr>
            <td>Registro detallado</td>
            <td>Permite generar logs completos de las operaciones</td>
        </tr>
        <tr>
            <td>Reanudación de copia</td>
            <td>Puede continuar copias interrumpidas</td>
        </tr>
        <tr>
            <td>Gran rendimiento</td>
            <td>Muy eficiente con miles o millones de archivos</td>
        </tr>
    </tbody>
</table>

# 📊 Parámetros utilizados

<table>
    <thead>
        <tr>
            <td><b>Parámetro</b></td>
            <td><b>Función</b></td>
            <td><b>Beneficio</b></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>/MIR</td>
            <td>Sincroniza origen y destino como espejo</td>
            <td>Mantiene el destino idéntico al origen</td>
        </tr>
        <tr>
            <td>/E</td>
            <td>Copia subcarpetas incluyendo vacías</td>
            <td>Mantiene estructura completa</td>
        </tr>
        <tr>
            <td>/Z</td>
            <td>Modo reiniciable</td>
            <td>Permite continuar copias interrumpidas</td>
        </tr>
        <tr>
            <td>/ZB</td>
            <td>Intenta modo reiniciable y luego modo backup</td>
            <td>Mayor tolerancia a errores de permisos</td>
        </tr>
        <tr>
            <td>/R:n</td>
            <td>Número de reintentos</td>
            <td>Controla intentos si hay fallos</td>
        </tr>
        <tr>
            <td>/W:n</td>
            <td>Tiempo de espera entre reintentos</td>
            <td>Reduce saturación en errores</td>
        </tr>
        <tr>
            <td>/MT:n</td>
            <td>Copia con múltiples hilos</td>
            <td>Acelera transferencias con muchos archivos</td>
        </tr>
        <tr>
            <td>/FFT</td>
            <td>Ajusta timestamps para NAS/Linux</td>
            <td>Evita recopiados innecesarios</td>
        </tr>
        <tr>
            <td>/TBD</td>
            <td>Espera disponibilidad del recurso de red</td>
            <td>Evita fallos en shares temporales</td>
        </tr>
        <tr>
            <td>/XD</td>
            <td>Excluye directorios específicos</td>
            <td>Evita copiar carpetas innecesarias</td>
        </tr>
        <tr>
            <td>/LOG:file</td>
            <td>Guarda salida en archivo log</td>
            <td>Permite auditoría y monitoreo</td>
        </tr>
        <tr>
            <td>/LOG+</td>
            <td>Agrega información al log existente</td>
            <td>Mantiene historial de ejecuciones</td>
        </tr>
        <tr>
            <td>/TEE</td>
            <td>Muestra salida en consola y log</td>
            <td>Permite monitoreo en tiempo real</td>
        </tr>
        <tr>
            <td>/NP</td>
            <td>No muestra progreso por archivo</td>
            <td>Reduce salida en consola y logs</td>
        </tr>
        <tr>
            <td>/L</td>
            <td>Simula la ejecución</td>
            <td>Permite validar cambios antes de ejecutarlos</td>
        </tr>
    </tbody>
</table>


# 🧪 Ejemplo de uso

```
robocopy "C:\wamp64\www" \\SERVER_DESTINO\Respaldo\Proyectos\wamp64\www" ^
/MIR /ZB /R:3 /W:5 /FFT /TBD /NP /TEE ^
/XD node_modules vendor ^
/MT:32 ^
/LOG+:C:\Logs\Respaldo.txt
```

Este comando realiza:
- sincronización espejo entre origen y destino
- copia multihilo para mayor velocidad
- tolerancia a interrupciones de red
- exclusión de carpetas innecesarias
- registro de operaciones en log
- monitoreo en consola y log simultáneamente

# ⚠️ Consideraciones importantes

<table>
    <thead>
        <tr>
            <th><b>Consideración</b></th>
            <th><b>Explicación</b></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>/MIR</b> elimina archivos en destino</td>
            <td>Si no existen en origen serán eliminados</td>
        </tr>
        <tr>
            <td>No afecta la carpeta origen</td>
            <td>Solo sincroniza el destino</td>
        </tr>
        <tr>
            <td>Puede detenerse con Ctrl + C</td>
            <td>La copia puede reanudarse posteriormente</td>
        </tr>
        <tr>
            <td>Ideal para grandes volúmenes</td>
            <td>Especialmente útil para miles de archivos</td>
        </tr>
    </tbody>
</table>

# 🛠 Buenas prácticas

Antes de ejecutar un espejo completo se recomienda simular:
```
robocopy origen destino /MIR /L
```
Esto permite verificar:
- archivos que se copiarán
- archivos que se eliminarán
- estructura final del destino

# 💡 Conclusión

Robocopy es una herramienta altamente eficiente para copias robustas y sincronización de datos, siendo ampliamente utilizada en administración de sistemas, migraciones de infraestructura y procesos de respaldo automatizado.