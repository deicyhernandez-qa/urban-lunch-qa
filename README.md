# Urban Lunch – Pruebas de Aplicación Móvil

![status](https://img.shields.io/badge/status-completado-2EA043?style=flat-square&labelColor=30363D)
![casos ejecutados](https://img.shields.io/badge/casos%20ejecutados-52-1F6FEB?style=flat-square&labelColor=30363D)
![bugs documentados](https://img.shields.io/badge/bugs%20documentados-7-DA3633?style=flat-square&labelColor=30363D)
![plataforma](https://img.shields.io/badge/plataforma-Android%209.0%20%7C%20API%2028-DB6D28?style=flat-square&labelColor=30363D)
![método](https://img.shields.io/badge/m%C3%A9todo-checklist%20funcional%20%7C%20testing%20m%C3%B3vil-58A6FF?style=flat-square&labelColor=30363D)

Proyecto de testing de aplicación móvil Android realizado como parte del programa de QA de **TripleTen LatAm** (junio 2026). El objetivo fue verificar el flujo completo de la aplicación Urban Lunch — desde la selección del punto de recogida hasta la confirmación del pedido — mediante un checklist funcional ejecutado en un emulador Android con trazabilidad completa en Android Studio y Logcat.

## Contexto de negocio

Urban Lunch es una aplicación móvil que permite a los usuarios elegir platillos de restaurantes cercanos y recibirlos en un punto de recogida. El flujo completo comprende cinco etapas: selección del punto de recogida en el mapa, elección de platillos (lista y detalle), confirmación del pedido, seguimiento en tiempo real, y pantalla de entrega. Cada pantalla involucra elementos interactivos (botones, mapas, temporizadores, indicadores de progreso) cuyo correcto funcionamiento es crítico para que el usuario complete una compra sin fricción.

## Herramientas y entorno

- **Dispositivo:** Emulador Android — Honor-tripleten1 (Android 9.0, API 28)
- **IDE:** Android Studio con Logcat para captura de logs
- **Gestión de bugs:** Jira (QA_SPRINT 6)
- **Documentación:** Excel (checklist funcional)

## Metodología

Las pruebas se diseñaron como un **checklist funcional** que recorre el flujo completo de la aplicación pantalla a pantalla, verificando cada elemento de la interfaz contra los requisitos: existencia de componentes visuales, comportamiento de botones e interacciones, transiciones entre pantallas, mensajes de error, y consistencia del mapa y los temporizadores. Cada caso fallido se documentó en Jira con descripción, pasos de reproducción, resultado actual vs. esperado, capturas del emulador y logs de Logcat como evidencia técnica.

## Resultados generales

| Métrica | Valor |
|---|---|
| Casos de prueba totales | 52 |
| Casos exitosos (Passed) | 45 (86.5%) |
| Casos fallidos (Failed) | 7 (13.5%) |
| Defectos reportados en Jira | 7 (QS6-1 a QS6-7) |

| Sección probada | Casos | Passed | Failed |
|---|---|---|---|
| Selección del punto de recogida | 5 | 5 | 0 |
| Elección de platillos — Lista | 10 | 8 | 2 |
| Elección de platillos — Detalle | 6 | 5 | 1 |
| Confirmación del pedido | 13 | 13 | 0 |
| Seguimiento del pedido | 10 | 7 | 3 |
| El pedido ha sido enviado | 5 | 4 | 1 |
| Notificaciones de error | 2 | 2 | 0 |
| **Total** | **52** | **45** | **7** |

## Hallazgo principal

Los 7 defectos se concentran en dos patrones: **falta de feedback visual** al usuario sobre el estado del pedido (los botones "+" y "-" solo actualizan el contador pero no confirman si el platillo se agregó o eliminó del pedido real), e **información ausente en el mapa y la pantalla de seguimiento** (no se muestran los restaurantes de origen, ni el temporizador de recogida, ni el tiempo de preparación). Estos bugs afectan directamente la confianza del usuario durante el proceso de compra, ya que no puede verificar que sus acciones tuvieron el efecto esperado.

Un hallazgo adicional relevante: el botón "Siguiente" permanece activo aunque no haya platillos seleccionados — en lugar de bloquearse, permite el clic y muestra el mensaje "Elegir cualquier alimento", lo que invierte la lógica de validación esperada.

## Hallazgos por severidad

| Severidad | Cantidad | Criterio aplicado |
|---|---|---|
| 🟠 Alta | 7 | Funcionalidad visible para el usuario que no opera según el requisito: botones sin feedback, información ausente en mapa/seguimiento, estado de botón incorrecto. |

## Detalle de defectos

| Ticket | Sección | Descripción | Resultado actual | Resultado esperado |
|---|---|---|---|---|
| QS6-1 | Lista de platillos | Botón "+" no evidencia que el platillo se agrega al pedido ni en el mapa | Solo actualiza el contador | Agrega el platillo al pedido desde el restaurante más cercano |
| QS6-2 | Lista de platillos | Botón "-" no evidencia que el platillo se elimina del pedido | Solo disminuye el contador | Elimina el platillo de la lista del pedido |
| QS6-3 | Detalle del platillo | Botón "Siguiente" activo aunque no haya platillos seleccionados | Permite clic y muestra "Elegir cualquier alimento" | Debe estar inactivo sin platillos en el pedido |
| QS6-4 | Seguimiento | El mapa no identifica los restaurantes donde se preparan los platillos | Muestra ruta en verde sin nombre de origen | Debe mostrar los restaurantes identificados |
| QS6-5 | Seguimiento | No se muestra el tiempo de preparación del platillo | El campo no aparece en pantalla | Debe mostrar el tiempo de preparación restante |
| QS6-6 | Seguimiento | No hay temporizador del tiempo que queda hasta la recogida | El campo no aparece en pantalla | Debe mostrar el tiempo restante hasta la recogida |
| QS6-7 | Pedido enviado | El punto de recogida en el mapa no es identificable | Aparece un pin sin nombre | Debe mostrar el nombre del punto de recogida seleccionado |

## Estructura del repositorio

```
urban-lunch-qa/
├── README.md
├── .gitignore
├── casos-de-prueba/
│   └── checklist-pruebas-movil.xlsx        # 52 casos: descripción, estado, link a bug, comentarios
├── checklists/
└── evidencias/
    └── capturas-defectos/                  # 2-3 archivos por defecto (imagen + log cuando aplica)
        ├── QS6-N-reporte-jira.png          # Ticket completo en Jira
        ├── QS6-N-evidencia-emulador.png    # Captura del emulador Android
        └── QS6-N-logcat.txt               # Log de Android Studio (QS6-4 a QS6-7)
```

## Stack y herramientas

`Android Studio` `Logcat` `Emulador Android` `Jira` `Excel` `Testing Móvil` `Checklist Funcional` `Android API 28`

---

**Autora:** Deicy Hernández Zamora — QA Tester
[GitHub](https://github.com/deicyhernandez-qa) · [LinkedIn](https://www.linkedin.com/in/deicy-hernandez-zamora)
