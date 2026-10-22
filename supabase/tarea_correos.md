# La tarea programada que manda los correos de bienvenida

La página solo guarda el registro con `estado_correo = 'pendiente'`. Una **tarea programada de Claude**, con los conectores de **Supabase** y **Gmail**, manda el correo desde tu Gmail y marca el registro. La página nunca depende de Claude: si la tarea no corre, el registro queda guardado y pendiente.

## 1. Crear la tarea

En `claude.ai/code/routines` (en la app de escritorio: Code, **Programadas**), **Nueva tarea**, **Configurar manualmente**:

- **Nombre:** Correos de bienvenida
- **Prompt:** el de abajo, con el nombre de tu proyecto de Supabase, tu marca y tu producto.
- **Disparador:** **Programación**, cada hora (respaldo) y, con **+**, **API** (aviso inmediato).
- **Configuración avanzada:** tu repositorio de la página y, en **Conectores**, deja solo **Supabase** y **Gmail**.

```
Atiendes la lista de interesados del lanzamiento de la Chamarra Ligera
Girasol de Boutique El Girasol. Usa el conector de Supabase en el
proyecto NOMBRE-DE-TU-PROYECTO y el conector de Gmail.

1. En la tabla interesados, toma los registros con estado_correo
   'pendiente'.
2. Si un correo se ve mal escrito (gmial.com, hotmial.com, un dominio
   que no existe), márcalo 'invalido', escribe el motivo en
   detalle_correo y no le escribas.
3. A los demás, mándales con Gmail un correo de bienvenida corto y
   cálido, de tú: su nombre, la talla que eligió, la fecha del
   lanzamiento (sábado 14 de noviembre, 10:00) y una línea según cuándo
   piensa comprar. Asunto: "¡Ya estás en la lista de la Chamarra Ligera
   Girasol!". Al pie: "Si no quieres recibir más correos, responde baja."
4. Después de mandar cada uno, márcalo 'enviado' y pon la hora en
   enviado_en.
5. Busca en Gmail los avisos de correo no entregado de los últimos
   2 días. Si alguno es de un correo de la lista, márcalo 'rebotado'
   con el motivo.
6. Antes de terminar, revisa si llegaron pendientes mientras trabajabas.
No cambies nada más de la base. Al final, di cuántos enviaste, cuántos
inválidos y cuántos rebotados.
```

## 2. Conectar la página con la tarea

Al agregar el disparador **API**, da clic en **Generar token** y copia la **liga** (Fire URL) y el **token**. El token se ve una sola vez.

Después, en Claude con el conector de Supabase:

```
En mi proyecto NOMBRE-DE-TU-PROYECTO, guarda en la tabla privado.config
la clave liga_tarea con el valor LIGA y la clave token_tarea con el
valor TOKEN. No cambies nada más.
```

Desde ese momento, cada registro nuevo avisa a la tarea (como mucho una vez cada 2 minutos) y el correo llega en uno o dos minutos. La programación de cada hora atiende lo que se haya quedado.

## 3. Comprobar

- En Supabase, **Table Editor**, `interesados`: la columna `estado_correo` cambia de `pendiente` a `enviado`, `invalido` o `rebotado`.
- En la tarea, **Ejecutar ahora** la corre sin esperar.
- Si la tarea se detiene a pedir permiso para mandar el correo, ábrela desde su historial y aprueba. Cada corrida usa el plan de quien la creó.
