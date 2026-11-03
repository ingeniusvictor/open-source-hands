# ORBI × LUMIA — Plan maestro de evolución gestual y visión multimodal
**Código de continuidad:** LUMIA-GESTURE-ROADMAP-01  
**Fecha de registro:** 2026-11-03  
**Estado:** PLANIFICADO / CONGELADO PARA FUTURA EJECUCIÓN. No iniciado.  
**Repositorio de resguardo y laboratorio:** https://github.com/ingeniusvictor/open-source-hands  
**Origen del código:** https://github.com/abouchedidxcode/open-source-hands  
**Commit original preservado al copiar:** dd79fb7b3a9d2e9868e433611e6b8737dfca3936  
**Integración prevista:** LUMIA, dentro de ORBI Ecosystem.  

> **Decisión clave:** Este repositorio es una copia independiente de referencia y un futuro laboratorio. No es el repositorio de producción de LUMIA. La prioridad ahora es conservar el código y las decisiones. No comenzar implementación ni alterar su motor original hasta cumplir las fases previas y establecer una línea base verificada.

## 1. Objetivo y visión de producto

Transformar progresivamente a LUMIA desde asistente conversacional con avatar 3D a una interfaz multimodal capaz de **recordar, observar, interpretar movimientos y responder con voz, expresiones y animaciones**, manteniendo un modo de uso exclusivamente por texto o voz.

Las futuras capacidades buscadas incluyen:
- Reconocimiento contextual de gestos deliberados: saludo, señalar, palma abierta, confirmaciones y movimientos de dos manos.
- Comandos sin contacto mediante webcam: seleccionar, desplazarse, ampliar, detener, activar demostraciones.
- Respuesta expresiva del avatar VRM: mirada, asentimiento, saludo, señalamiento, gestos corporales y sincronización voz-visemas.
- Relación entre lo que observa y lo conversado: por ejemplo, reconocer un saludo cuando el usuario decide activar la cámara y responder apropiadamente.
- Investigación posterior y responsable de reconocimiento de **Lengua de Señas Chilena (LSCh)** con participantes signantes y recursos de calidad. No afirmar traducción completa sin evidencia.
- Potencial reutilización opcional del motor de gestos en proyectos ORBI, sin obligar a los juegos ni a LUMIA a depender de él.

**Resultado esperado a largo plazo:** una interacción natural, accesible y con control explícito sobre cuándo LUMIA puede usar la cámara. Este documento no declara ninguna de esas capacidades como ya implementada.

## 2. Secuencia obligatoria: tres evoluciones

| Orden | Evolución | Propósito | Requisito de salida antes de avanzar |
|---|---|---|---|
| 01 | LUMIA MEMORY — Memoria persistente | Recordar contexto útil entre sesiones, con consentimiento, límites y controles | Persistencia fiable, edición/borrado, respaldo y pruebas de privacidad |
| 02 | LUMIA VISION — Visión contextual | Capturar imágenes autorizadas, comprender escenas, objetos y texto | Encendido/apagado explícito, fallbacks, pruebas con cámaras reales y regresión de conversación |
| 03 | LUMIA GESTURE — Interacción gestual | Interpretar posturas, trayectorias y gestos; conectarlos con el avatar y el diálogo | Se inicia solo tras estabilizar las anteriores; se libera por subfases |

**No invertir el orden acordado.** Se puede estudiar documentación del kit y mantener este respaldo, pero no fusionar la tercera evolución antes de que memoria y visión estén listas. La visión de objetos/texto y el reconocimiento gestual son capacidades separadas que pueden compartir la webcam y el administrador de permisos.

## 3. Estado de referencia conocido al crear este plan

- Existe una línea de LUMIA conversacional con avatar VRM y estados de interfaz como escuchar, pensar, hablar y reposo; ya se han realizado pruebas de sincronización de voz y visemas.
- Se trabaja en voz local, reconocimiento de habla, respuesta textual, movimientos de ojos y cabeza, animaciones de cuerpo y gestos del avatar.
- Existe una referencia de versión estable de LUMIA cuyo estado concreto debe volverse a comprobar al retomar. **No asumir que el avatar, rutas de archivos, ramas, dependencias ni despliegues actuales serán idénticos a los de noviembre de 2026.**
- Este repositorio copiado contiene el kit TypeScript para manos, ejemplos, pruebas, documentación y licencia. No contiene por sí mismo el sistema de memoria ni un traductor de lenguas de señas.
- La copia original fue importada a GitHub con historial conservado. El repositorio propio no está registrado como fork oficial; para actualizarlo más adelante habrá que comparar explícitamente con el upstream.

### Decisiones inmutables hasta nueva revisión
1. No modificar directamente los componentes estables de LUMIA para experimentar.
2. No reemplazar el pipeline de voz, visemas ni animaciones por una solución experimental.
3. Mantener siempre alternativa de voz, teclado y mouse, incluso sin cámara.
4. Trabajar en incrementos pequeños con pruebas y puntos de restauración.
5. No deducir ni guardar identidad de personas a partir del vídeo sin necesidad y autorización.
6. Tratar lengua de señas como problema lingüístico autónomo; la clasificación de gestos aislados es solo un primer paso.

## 4. Qué ofrece realmente Open Source Hands

**Rutas que conviene estudiar al retomar:**
- src/kit/: biblioteca de seguimiento y clasificación de manos.
- src/kit/hands.ts: createHands(), inicio/parada de cámara, inyección de manos sintéticas y frames.
- src/kit/tracker.ts: MediaPipe Hand Landmarker, flujo de webcam y fallback de GPU a CPU.
- src/kit/engine.ts: eventos de poses y movimientos y configuración de umbrales.
- src/kit/poses.ts, motions.ts, features.ts: descriptores de mano, poses y trayectorias.
- src/kit/synthetic.ts: pruebas sin cámara mediante manos simuladas.
- src/kit/draw.ts: visualización de landmarks.
- src/kit/index.ts: API pública.
- src/lab/ y lab.html: laboratorio para umbrales, tasa de acierto y fiabilidad.
- templates/game/: aplicación Three.js de referencia, con controles alternativos y test automatizado.
- AGENTS.md, docs/MAKING-A-GAME.md, templates/game/README.md: guías técnicas.
- LICENSE, THIRD_PARTY_NOTICES.md: permisos, atribuciones y dependencias.

**Capacidad documentada:** unas 20 poses, 16 movimientos, 5 gestos con dos manos y valores continuos como posición, fuerza de pinza y velocidad. El núcleo interpreta landmarks de MediaPipe y no entrega entendimiento semántico universal de escenas, emociones, personas ni lenguaje de señas.

**Dependencias de referencia del upstream:** @mediapipe/tasks-vision fijado en versión 1.1.0, su WASM y el modelo hand_landmarker.task. Antes de integrarlo confirmar compatibilidad de versiones, tamaño de recursos, licencias, seguridad y cambios del upstream; no actualizar sin pruebas.

**Licencias:** código de este kit bajo MIT; componentes de MediaPipe conforme a sus avisos Apache-2.0 y demás atribuciones. Conservar avisos en cualquier distribución o integración.

## 5. Arquitectura objetivo — desacoplada y reversible

Flujo conceptual:

1. **Gestor de permisos y fuentes**: webcam bajo petición del usuario; iniciar/detener, mostrar estado, detectar ausencia/denegación.
2. **Captura compartida**: un flujo de cámara, con presupuestos de FPS/resolución y pausa cuando no hay demanda; evitar que visión y gestos abran cámaras paralelas.
3. **Adaptadores de percepción**:
   - Vision Adapter: objetos, OCR/escena y contexto visual conforme a la segunda evolución.
   - Hand Tracker Adapter: landmarks de una/dos manos a partir de MediaPipe.
   - Futuro Face/Pose Adapter: solo si hace falta para contexto gestual o investigación de señas; evaluar requisitos y licencias.
4. **Interpretación de eventos**: clasificador de gestos + filtros de estabilidad/umbral + desambiguación de intención; diferenciación entre gesto incidental y comando.
5. **Gestor multimodal de LUMIA**: combina intención de voz/texto y gesto autorizado; aplica prioridades, turnos, estado conversacional y niveles de confianza.
6. **Acciones**: respuestas habladas, estados del avatar, animación VRM, navegación UI o una acción reversible. Confirmación expresa para acciones sensibles.
7. **Observabilidad local**: contadores de rendimiento, errores, eventos anonimizados y pruebas; capturas/registro persistente solo con consentimiento explícito.

**Interfaces sugeridas (contratos, no implementación):**
- GestureEvent: {type, phase, confidence, timestamp, hand?, motion?, source, sessionId}
- MultimodalIntent: {intent, sources, confidence, requiresConfirmation, expiresAt}
- AvatarAction: {animationId, duration?, priority, canInterrupt, fallback}
- CameraState: {permission, active, owner, error?, fps?}

No acoplar la API de MediaPipe directamente a la lógica del agente ni a los archivos de animación VRM. Usar un adaptador que pueda desactivarse con un feature flag: gestureEnabled=false por defecto en la primera integración.

## 6. Plan de trabajo detallado (subfases)

### PRE-00 — Resguardo e inventario (AHORA)
**Objetivo:** conservar el hallazgo sin alterar funcionalidades.
- [x] Localizar proyecto público y confirmar documentación/licencia.
- [x] Copiar el código original al GitHub ingeniusvictor/open-source-hands.
- [x] Confirmar commit de origen dd79fb7b3a9d2e9868e433611e6b8737dfca3936.
- [x] Registrar este plan.
- [ ] Cuando se retome: inventariar ramas/tags, dependencias, superficie de ataque, licencias y cambios de upstream; ejecutar tests de la copia.
- [ ] Confirmar cuál es el repositorio/rama/commit estable de LUMIA en ese momento.
**Salida:** inventario reproducible y rollback definido. No se considerará validación funcional el simple hecho de que GitHub contenga los archivos.

### GATE-01 — Concluir LUMIA MEMORY (PRERREQUISITO)
- [ ] Definir memoria episódica, preferencias duraderas y memoria temporal por separado.
- [ ] Diseñar almacenamiento local/seguro, esquema/versionado, límites de retención y recuperación.
- [ ] Permitir al usuario revisar, corregir, exportar y borrar datos recordados.
- [ ] Evitar guardar información visual o biométrica por defecto.
- [ ] Probar reinicios, corrupción de datos, duplicados, conflictos y recuperación.
- [ ] Certificar que el agente no inventa recuerdos y distingue memoria de inferencia.
**Puerta:** memoria persistente operativa, auditada y estable; no dar por cumplida por la sola existencia de una base de datos.

### GATE-02 — Concluir LUMIA VISION (PRERREQUISITO)
- [ ] Definir acciones visibles «activar cámara», «pausar» y «apagar», y un indicador inequívoco.
- [ ] Implementar flujo de capturas y preguntas sobre imagen/escena/OCR con autorización.
- [ ] Separar procesamiento local de cualquier servicio remoto y solicitar consentimiento específico si hay transferencia.
- [ ] Gestionar iluminación deficiente, cámara ocupada, webcam no disponible y permisos denegados.
- [ ] Evitar identificación facial y vigilancia encubierta; evaluar privacidad de terceros y menores.
- [ ] Medir carga de CPU/GPU/RAM y efecto sobre STT, TTS, avatar 3D y latencia conversacional.
- [ ] Certificar uso continuo, reinicio de permisos y modo solo voz.
**Puerta:** visión estable, compartimentada y con controles de privacidad comprobables.

### GESTURE-01 — Laboratorio autónomo y pruebas de gestos (PRIMERA FASE FUTURA)
- [ ] Crear una rama experimental exclusiva; no tocar main de la copia base durante el prototipo.
- [ ] Instalar dependencias de forma reproducible y revisar lockfile/modelos/WASM.
- [ ] Ejecutar npm test, npm run typecheck, npm run build y npm run e2e; documentar los resultados reales.
- [ ] Probar demo y Gesture Lab con cámara real en ASUS; registrar hardware, navegador, resolución, FPS y errores.
- [ ] Seleccionar vocabulario mínimo no ambiguo: saludo, palma abierta, señalar, pinza y aplauso. No empezar con decenas de comandos.
- [ ] Ajustar umbrales, hysteresis, handedness y espejado; comparar distintas distancias, fondos e iluminaciones.
- [ ] Probar entrada sintética, cámara denegada, CPU fallback y desactivación inmediata.
- [ ] Generar una ficha de pruebas con falsos positivos/negativos, latencia y tasas de reconocimiento por gesto.
**Puerta:** demo repetible con webcam y pruebas sintéticas aprobadas; reporte explícito de límites. Sin conexión a LUMIA todavía.

### GESTURE-02 — Adaptador de entrada y convivencia con visión
- [ ] Integrar el kit como módulo opcional sin duplicar cámaras ni alterar código estable.
- [ ] Diseñar gestor compartido de webcam entre VISION y GESTURE, con cierre/liberación segura.
- [ ] Crear un GestureEvent normalizado con estado start/end, confianza y TTL para prevenir repeticiones accidentales.
- [ ] Agregar dead zones, debouncing, cool-down de comandos y cancelación manual.
- [ ] Resolver conflictos: voz tiene prioridad configurable, gestos no interrumpen inadvertidamente respuestas o grabaciones de voz.
- [ ] Probar transiciones escuchar/pensar/hablar/reposo sin congelamientos.
**Puerta:** gestos llegan como eventos no destructivos a un monitor local, sin acciones automáticas sensibles.

### GESTURE-03 — Avatar VRM expresivo y acciones
- [ ] Mapear eventos a un set mínimo: saludo → saludo VRM, confirmación → asentir, señalar → orientar mirada/señalar, aplauso → reacción.
- [ ] Verificar que existan clips compatibles con el esqueleto real de LUMIA antes de prometer animaciones.
- [ ] Implementar prioridades de animación, blending, regreso a reposo y control de interrupciones.
- [ ] Mantener boca/visemas, mirada/parpadeo, expresiones y audio sin regresiones.
- [ ] Evitar movimientos repetitivos por eventos sostenidos; proteger interacciones conversacionales.
- [ ] Demostrar respuesta sin cámara mediante entrada simulada y comandos manuales.
**Puerta:** una demostración completa saludo detectado → intención validada → animación VRM y respuesta verbal, reproducible y reversible.

### GESTURE-04 — Fusión multimodal y UX
- [ ] Combinar voz + gesto + contexto visual con desambiguación: «LUMIA, mira esto» mientras señala.
- [ ] Mantener estado de intención por pocos segundos con expiración y cancelación.
- [ ] Mostrar interpretación tentativa y permitir corregirla por voz/teclado.
- [ ] Documentar comandos accesibles equivalentes y modo de baja carga.
- [ ] Opcional: conectar controles gestuales a ORBI 3D Motion Studio y prototipos de juegos en proyectos separados.
**Puerta:** casos de uso controlados que no creen efectos inesperados; pruebas de calidad de interacción.

### SIGN-01 — Investigación de Lengua de Señas Chilena (EXPLORATORIO, NO PROMETIDO)
- [ ] Delimitar con personas usuarias de LSCh el problema real y vocabulario prioritario; reconocer que LSCh no equivale a español gesticulado.
- [ ] Identificar fuentes de datos, autorización, representatividad, licencias y criterios de consentimiento.
- [ ] Evaluar manos + rostro + postura + movimiento temporal, no solo formas estáticas de dedos.
- [ ] Diseñar corpus y protocolo de anotación validados por personas signantes.
- [ ] Comenzar con un conjunto limitado de signos/expresiones, indicando cobertura y confianza de resultados.
- [ ] Evaluar variación regional, coarticulación, iluminación, oclusiones y señantes distintos de quienes entrenan.
- [ ] Nunca anunciar «traducción de lenguaje de señas» solo por reconocer gestos aislados.
- [ ] Diseñar abstención ante incertidumbre y escalamiento a transcripción/confirmación humana cuando corresponda.
- [ ] Considerar más adelante otras lenguas de señas mediante modelos/corpus separados. No asumir equivalencia entre LSCh, LSV, ASL u otras.
**Puerta:** validación lingüística y de seguridad con signantes reales. Un detector de manos por sí solo NO completa esta fase.

### RELEASE-01 — Piloto opcional, sin afectar LUMIA estable
- [ ] Compilar matriz de requisitos y resultados de pruebas.
- [ ] Ejecutar regresiones de voz/STT/TTS, traducción, avatar, consumo de recursos y reanudación tras errores.
- [ ] Comparar con baseline sin cámara: tiempos de respuesta y estabilidad de varias conversaciones seguidas.
- [ ] Obtener aprobación explícita para habilitar feature flag y registrar plan de rollback.
- [ ] Publicar cambios primero en rama/PR de integración; fusionar solo con evidencia de QA.
**Puerta:** piloto documentado, opt-in y reversible.

## 7. Casos de uso y prioridades

| Prioridad | Caso | Ejemplo | Éxito observable |
|---|---|---|---|
| P0 | Permisos y apagado | Usuario enciende/apaga webcam | Indicador fiel, cámara liberada, sin procesamiento al apagar |
| P0 | Saludo intencional | Persona saluda con la mano | LUMIA responde una vez, sin falsos disparos constantes |
| P0 | Fallback | Cámara no disponible | LUMIA continúa por voz/texto sin errores |
| P1 | Indicar objeto | Usuario señala mientras pregunta | Respuesta conectada al contexto visual con advertencia si no está claro |
| P1 | Aplauso/reacción | Dos manos aplauden | Evento correcto y animación compatible |
| P1 | Señalar en visor | Usuario indica dirección | Avatar señala o mueve mirada sin afectar labios ni voz |
| P2 | Controles manos | Pinza para seleccionar, zoom con ambas manos | Operaciones reversibles y fáciles de cancelar |
| Investigación | LSCh | Signos y secuencias validadas | Métricas de reconocimiento validadas con signantes y limitaciones declaradas |

## 8. Pruebas y métricas — objetivos a acordar, NO resultados medidos

Recolectar antes/después en el mismo hardware y escenario; no fijar compromisos arbitrarios sin línea base.

- **Exactitud funcional:** matriz por gesto de precision/recall, falsos positivos por minuto, confusiones y eventos duplicados.
- **Latencia:** cámara→landmarks→evento→reacción avatar, mediana y p95; separar detección del tiempo de respuesta del LLM y TTS.
- **Rendimiento:** FPS de render VRM y cámara; CPU, GPU, RAM y uso de memoria WASM. Probar voz local concurrente.
- **Robustez:** iluminación, distancia, mano izquierda/derecha, ambos brazos, oclusiones, fondo y cámaras distintas.
- **Sesiones prolongadas:** 20–30 minutos de prueba con varios ciclos cámara ON/OFF, y preguntas rápidas sin bloqueos.
- **Recuperación:** cortar permiso, desconectar cámara, cambiar pestaña, reiniciar app, salir del modo de gestos.
- **Accesibilidad:** todo control importante con equivalente manual o verbal; no exigir elevar brazos durante periodos largos.
- **Privacidad:** ninguna activación silenciosa; revisión de envíos remotos y ausencia de almacenamientos no consentidos.

Registrar evidencia en la rama futura (test plan, resultados, logs sin vídeo personal, capturas autorizadas y versiones del entorno). No confundir pruebas simuladas con pruebas con webcam real.

## 9. Riesgos conocidos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Saturación de GPU por VRM + MediaPipe + visión | Compartir capturas; limitar resolución/FPS; medir y ofrecer modo ligero |
| Colisión con micrófono y conversación | Eventos no intrusivos, estados y prioridades, activación opt-in |
| Falsos positivos por gestos casuales | Histeresis, ventana temporal, cooldown, contexto y confirmación |
| Latencia por filtros duplicados | Respetar suavizado One Euro del kit; no apilar filtros innecesarios |
| Reflejo e izquierda/derecha | Seguir coordenadas espejadas propias del kit; pruebas por mano |
| Incompatibilidad modelo/WASM | Fijar versiones de @mediapipe/tasks-vision y assets |
| Exposición de menores/terceros | Consentimiento, no persistir vídeo por defecto, control visible |
| Traducción incorrecta de LSCh | No presentar resultados experimentales como traducción fiable |
| Drift del repositorio original | Comparar upstream, revisar licencia y congelar versión validada |
| Regresión de LUMIA | Feature flag, pruebas, rama aislada y restauración del último commit estable |

## 10. Gobierno de código y mantenimiento

- Repositorio de origen técnico y respaldo: **ingeniusvictor/open-source-hands**.
- Repositorio de integración de producto: el de LUMIA que se confirme al reanudar; no mezclar automáticamente proyectos.
- Mantener la rama main de este repositorio como copia estable + documentación de ORBI; experimentar solo en ramas feature.
- Preferir copiar/integrar src/kit como dependencia adaptada y con licencia, no reescribir ni forkar internamente su lógica sin motivo.
- Antes de publicar en cualquier app ORBI, documentar obligaciones de copyright, avisos MIT/Apache y fuentes de modelos.
- Adoptar la metodología ORBI: diseño → módulo pequeño → pruebas → revisión → freeze; conservar referencias a commits y PR.
- No prometer fecha de lanzamiento ni afirmar funcionalidades completadas si no hay evidencia.
- Ante cambios del upstream, comparar versiones en una rama separada y no hacer un mirror push que sobreescriba documentación ORBI.

## 11. Protocolo de reanudación en una conversación futura

**Frase para recuperar el proyecto:**

> Continuemos el plan LUMIA-GESTURE-ROADMAP-01 documentado en ingeniusvictor/open-source-hands, archivo docs/PLAN-LUMIA-GESTURE.md. Revisa el estado actual del repositorio y de LUMIA. Respeta la secuencia: memoria persistente → visión → gestos. No alteres el baseline estable; primero confirma qué fases ya están certificadas y comienza por la siguiente tarea pendiente.

**Checklist de inicio al retomar:**
1. Abrir este documento y comprobar su versión y fecha.
2. Revisar commits/ramas recientes de LUMIA, arquitectura de voz, visor VRM y pruebas.
3. Verificar gates MEMORY y VISION con evidencia actual, sin asumirlos completos.
4. Comparar upstream y respaldar rama/commit del motor de manos.
5. Escoger solamente una subfase pendiente y preparar plan de QA y rollback.
6. Registrar resultados y actualizar este documento con fecha, commit, PR, tests y siguiente tarea.

## 12. Registro de decisiones y avance

| Fecha | Registro | Estado |
|---|---|---|
| 2026-11-03 | Se localizó Open Source Hands y se verificó que utiliza MediaPipe + gestos en navegador. | Completado |
| 2026-11-03 | Se hizo copia Git en ingeniusvictor/open-source-hands y se verificó el commit main original. | Completado |
| 2026-11-03 | Se acordó convertir la tecnología en la **tercera evolución** de LUMIA, después de MEMORY y VISION. | Decisión de producto |
| 2026-11-03 | Se redacta la planificación y se pospone su implementación. | Documentación |

**Pendiente inmediato al retomar:** confirmar estado actual de LUMIA MEMORY y VISION. **No iniciar GESTURE-01** hasta verificar sus prerequisitos, salvo pruebas de preservación del repositorio aislado.

---
**Documento de continuidad ORBI Ecosystem — plan de trabajo, no evidencia de implementación.**
