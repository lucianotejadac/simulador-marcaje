# Simulador de Marcaje de Radiofármacos de ⁹⁹ᵐTc

Simulador interactivo del marcaje de radiofármacos de tecnecio-99m, desarrollado como material de apoyo para exposición en el Departamento de Tecnología Médica, Universidad de Chile.

Reproduce, de forma manipulable y a escala molecular, lo que ocurre dentro del vial cuando se marca un kit frío con eluido del generador: la llegada del pertecnetato, la radiólisis del agua, la reducción del Tc(VII) con Sn²⁺ y la unión al ligando, ya sea por quelación o por adsorción. El usuario arrastra las especies con el puntero y las reacciones ocurren por contacto, con un motor de física 2D que corre íntegramente en el navegador.

> ⚠️ Herramienta docente y de divulgación. El modelo es cualitativo: ilustra mecanismos y errores de procedimiento, no calcula rendimientos radioquímicos ni sustituye los protocolos de radiofarmacia.

## Acceso en línea

👉 https://lucianotejadac.github.io/simulador-marcaje/

No requiere instalación. El HTML es autocontenido y no llama a ningún CDN, así que funciona incluso sin conexión una vez cargado. Basta un navegador moderno con soporte de SVG y eventos de puntero.

## Radiofármacos incluidos

Los cinco parten de la misma reducción con Sn²⁺ y se diferencian en el ligando y en sus cuidados.

| Radiofármaco | Uso clínico | Particularidad del marcaje |
| --- | --- | --- |
| ⁹⁹ᵐTc-MDP | Cintigrama óseo | quelato P–C–P |
| ⁹⁹ᵐTc-Sestamibi | Perfusión miocárdica | hay que hervirlo 10 min |
| ⁹⁹ᵐTc-EC | Renograma | kit de dos frascos |
| ⁹⁹ᵐTc-nanocoloide de SAH | Ganglio centinela | partícula de 5–80 nm |
| ⁹⁹ᵐTc-MAA | Perfusión pulmonar | partícula de 10–90 µm |

## Recorrido

Tronco común a todos los kits:

1. **Llega el tecnecio.** El eluido entra y disuelve el liofilizado. El Tc viene como TcO₄⁻, en estado +7, encerrado por cuatro oxígenos y sin capacidad de unirse a nada.
2. **La radiólisis.** La radiación del propio ⁹⁹ᵐTc rompe el agua y aparece un radical ·OH que oxida lo primero que toca, incluido el marcaje ya formado. El antioxidante del kit se sacrifica y el complejo queda intacto: se ve por qué el kit lo exige.
3. **Junta los electrones.** Hacen falta 6 e⁻, así que el usuario arrastra tres Sn²⁺ hasta reunirlos.
4. **Junta el tecnecio.** Cada Tc⁷⁺ pide 3 e⁻.
5. **La reducción.** Tc(VII) pasa a Tc⁴⁺, la especie capaz de unirse al ligando.
6. **El error: entra aire.** El O₂ compite por el Sn²⁺ y deja el marcaje sin reductor.

Después, cada radiofármaco tiene su kit, su forma de unión, su paso extra y un error característico:

| Radiofármaco | Unión | Paso extra | Error modelado |
| --- | --- | --- | --- |
| ⁹⁹ᵐTc-MDP | quelación P–C–P | — | entrada de aire |
| ⁹⁹ᵐTc-Sestamibi | primero Tc-citrato, luego transquelación a los 6 MIBI | calor, 10 min | no calentar |
| ⁹⁹ᵐTc-EC | quelación N₂S₂ | premezclar los dos frascos | saltarse el orden |
| ⁹⁹ᵐTc-nanocoloide | adsorción, no quelato | reposo | dejarlo pasado |
| ⁹⁹ᵐTc-MAA | adsorción en el agregado | cuidados de manejo | agitar fuerte |

Ecuaciones que acompañan las etapas:

```
Na⁹⁹ᵐTcO₄ · eluido del generador
Tc(H₂O)ₙ + MDP  →  ⁹⁹ᵐTc-MDP
Tc + citrato    →  Tc-citrato
Tc + 6 MIBI     →  [⁹⁹ᵐTc(MIBI)₆]⁺
Tc + EC         →  [⁹⁹ᵐTcO(EC)]⁻
```

## Componentes del vial modelados

Cada especie tiene rótulo y función visible en pantalla: TcO₄⁻ (pertecnetato), Tc⁴⁺ reducido, Sn²⁺ reductor y Sn⁴⁺ gastado, ascorbato como antioxidante, O₂, el radical ·OH de la radiólisis, citrato transquelante y su intermedio Tc-citrato, cisteína estabilizante, manitol y glucosa como lioprotectores, tampón que fija el pH, albúmina libre, NaCl para la isotonicidad, Cu⁺ acompañando a los MIBI, y los ligandos MDP, MIBI, EC, nanocoloide y MAA con sus complejos. Al empujar cualquier especie contra otra, el simulador evalúa si la reacción procede o si la rechaza.

## Controles

| Acción | Cómo |
| --- | --- |
| Avanzar | clic o barra espaciadora |
| Retroceder | ← o botón Atrás |
| Mover especies y provocar reacciones | arrastrar con mouse o dedo |
| Volver al menú | botón Radiofármacos |
| Empezar de nuevo | botón Reiniciar |

En el pie se llevan contadores en vivo de TcO₄⁻ libre, Tc⁴⁺, complejo formado, Sn²⁺ disponible y O₂ cuando corresponde.

## Stack técnico

| Capa | Tecnología |
| --- | --- |
| UI | HTML5 + CSS3 con custom properties y tipografía fluida |
| Escena | SVG 1600×900 con gradientes y capas de partículas, rótulos y efectos |
| Lógica | JavaScript ES6 vanilla, sin transpilación |
| Física | Planck.js 1.4.2 embebida en el propio archivo |
| Tipografías | fuentes del sistema |

Un solo archivo HTML, sin npm, sin bundler, sin servidor y sin dependencias externas en tiempo de ejecución.

## Limitaciones conocidas

El modelo es cualitativo y pedagógico: representa mecanismos, no cinética. No hay constantes de velocidad, ni cálculo de pureza radioquímica, ni decaimiento del ⁹⁹ᵐTc en el tiempo. Las cantidades en pantalla son unas pocas partículas representativas, no concentraciones reales, y los tamaños relativos están exagerados para que la escena sea legible. Los pasos extra (calor, reposo, premezcla) se resuelven como eventos discretos, no como procesos con temperatura y tiempo variables. Los errores modelados son los más didácticos de cada kit y no agotan las causas de un marcaje fallido.

## Roadmap

- [ ] Más radiofármacos de ⁹⁹ᵐTc (HMPAO, DTPA, MAG3, pirofosfato).
- [ ] Etapa de control de calidad: pureza radioquímica y cromatografía.
- [ ] Modo evaluación, con puntaje por errores cometidos.
- [ ] Resumen exportable de la secuencia realizada.
- [ ] Revisión de accesibilidad y navegación completa por teclado.
- [ ] Versión en inglés.

## Cómo citar

> Tejada Castro, L. (2026). Simulador de marcaje de radiofármacos de ⁹⁹ᵐTc [Software]. Departamento de Tecnología Médica, Universidad de Chile.

## Autor

Luciano Tejada Castro

lucianotejada@uchile.cl

Departamento de Tecnología Médica · Facultad de Medicina · Universidad de Chile

## Licencia

© 2026 Luciano Tejada Castro. Todos los derechos reservados. Obra protegida por la Ley N° 17.336 sobre Propiedad Intelectual de Chile. Cualquier reproducción, adaptación o comunicación pública distinta del uso personal requiere autorización escrita del autor.

Este proyecto incorpora Planck.js 1.4.2, © 2025 Erin Catto y Ali Shakiba, distribuido bajo licencia MIT. Ese componente se rige por su propia licencia y no queda comprendido en la reserva de derechos anterior.
