# ORBI/LUMIA — GESTURE PRE-00: verificación autónoma del kit

**Estado:** investigación / contrato de regresión, NO integración. Mantener intacta la decisión principal de `docs/PLAN-LUMIA-GESTURE.md`: MEMORY → VISION → GESTURE.

## Qué certifica este PR

- Instala dependencias declaradas en `package-lock.json` dentro del runner descartable.
- Ejecuta `npm test`, `npm run typecheck` y `npm run build` en Windows y Linux.
- Los tests pueden utilizar `src/kit/synthetic.ts`, sin permiso de webcam, usuario o imágenes reales.
- No cambia motores de LUMIA, ni mezcla repositorios ni incorpora código gestual a `orbi-jarvis-agent-lab`.
- **No certifica** precisión real de gestos, latencia, cámara o memoria de LUMIA.

## Hallazgos de compatibilidad que debemos respetar

1. El kit documenta 20 poses, 16 movimientos, 5 gestos de dos manos. Su `GestureEngine` recibe `HandInput[]`, por lo que puede ser alimentado sin su dueño de cámara.
2. El `createHands().startCamera()` y `HandTracker.start()` originales llaman `getUserMedia()`. **No** incluir ese arranque dentro de LUMIA: LUMIA ya dispone de `holdCamera()` y `releaseCamera()` compartidos.
3. LUMIA ya tiene seguimiento de manos, pinza, cursor y arrastre en `src/lib/hands.ts`. Reemplazarlo podría romper controles de puntero, manos izquierda/derecha o cámara.
4. `fromMediaPipe` del kit usa por defecto `mirror:true` y `flipLabel:true`, mientras que LUMIA mantiene `SWAP_HANDEDNESS=false` con su build actual. Un adaptador tiene que respetar la orientación que *se verifica en pantalla*, no un valor fijo.
5. El motor de gestos acepta landmarks 3D, mientras la API pública actual de LUMIA expone predominantemente coordenadas 2D. El adaptador debe recibir la salida de MediaPipe antes de perder datos de profundidad y handedness.
6. La copia del kit declara `@mediapipe/tasks-vision@1.1.0`; LUMIA declara `^1.0.1`. Mantener la dependencia actual de LUMIA hasta realizar pruebas específicas de compatibilidad.
7. No instalar un segundo loop `requestAnimationFrame` de 60 FPS con inferencia nueva por encima de la webcam si ello degrada 2B, Whisper, Kokoro o el avatar. Planificar presupuesto de CPU/GPU y caída a modo solo voz.
8. El código tiene licencia MIT y MediaPipe usa Apache 2.0. Conservar las atribuciones, licencia y avisos de terceros.

## Próximo gate (no habilitado)

Solo después de certificar VISION-04A detección facial real y VISION-04B SFace laboratorio físico, y verificar los requisitos de MEMORY/VISION del plan original:

- GESTURE-01 laboratorio de gestos con webcam física (saludo, señalar, pinza).
- GESTURE-02 adaptador de landmarks y eventos **sin** `startCamera()`, `getUserMedia()` ni acciones sensibles.
- GESTURE-03 mapa reversible a animaciones solo cuando los resultados y los clips del avatar se validen.
- GESTURE-04 interacción contextual con voz, con autorización y cancelación siempre disponibles.

**Regla de seguridad:** pruebas en PR borrador, rollback simple, main intacta, no efectos sobre el 2B nativo de LUMIA.
