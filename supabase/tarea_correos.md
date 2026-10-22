# La tarea programada que manda los correos de bienvenida

La página solo guarda el registro con `estado_correo = 'pendiente'`. Una **tarea programada de Claude**, con los conectores de **Supabase** y **Gmail**, manda el correo desde tu Gmail y marca el registro. La página nunca depende de Claude: si la tarea no corre, el registro queda guardado y pendiente.

## 1. Crear la tarea

En `claude.ai/code/routines` (en la app de escritorio: Code, **Programadas**), **Nueva tarea**, **Configurar manualmente**:

- **Nombre:** Correos de bienvenida
- **Prompt:** el de abajo, con tu marca y tu producto si no usas la Chamarra.
- **Activador:** **Programación**, cada hora (respaldo) y, con **+**, **API** (aviso inmediato).
- **Configuración avanzada:** tu repositorio de la página y, en **Conectores**, deja solo **Supabase** y **Gmail**.

```
Usa los conectores de Supabase y Gmail. En mi
proyecto curso-claude, en la tabla interesados, toma
los registros con estado_correo 'pendiente'.
Si un correo se ve mal escrito (gmial.com, un dominio
que no existe), márcalo 'invalido', con el motivo en
detalle_correo, y no le escribas.
A los demás mándales con Gmail un correo corto y
cálido, de tú, con su nombre, su talla y el día del
lanzamiento, sábado 14 de noviembre a las 10:00.
Asunto: ¡Ya estás en la lista de la Chamarra Ligera
Girasol! Márcalo 'enviado' con la hora en enviado_en.
Si Gmail tiene avisos de correo no entregado a uno
de la lista, márcalo 'rebotado'. No cambies nada más
y al final dime cuántos enviaste.
```

## 2. Conectar la página con la tarea

Al agregar el activador **API**, genera su token y copia la **liga** (Fire URL) y el **token**. El token se ve una sola vez.

Después, en Claude con el conector de Supabase:

```
Con el conector de Supabase, en mi proyecto
curso-claude, guarda en la tabla privado.config la
clave liga_tarea con el valor LIGA y la
clave token_tarea con el valor TOKEN.
No cambies nada más ni me repitas el token.
```

Desde ese momento, cada registro nuevo avisa a la tarea (como mucho una vez cada 2 minutos) y el correo llega en uno o dos minutos. La programación de cada hora atiende lo que se haya quedado.

## 3. Comprobar

- En Supabase, **Table Editor**, `interesados`: la columna `estado_correo` cambia de `pendiente` a `enviado`, `invalido` o `rebotado`.
- En la tarea, **Ejecutar ahora** la corre sin esperar.
- Si la tarea se detiene a pedir permiso para mandar el correo, ábrela desde su historial y aprueba. Cada corrida usa el plan de quien la creó.
