# Estándares de calidad aplicados al proyecto

Este documento define los estándares de calidad que el equipo y los workflows de IA deben seguir.

## Referencia principal

Ver especificación completa: **`.ai/standards/quality-attributes.md`**

El ecosistema de prompts utiliza como referencia:
- **ISO/IEC 25010:2011** — Modelo de calidad del producto software
- **WCAG 2.1** — Accesibilidad web (nivel AA objetivo)
- **OWASP Top 10** — Seguridad
- **Clean Code / SOLID** — Principios de diseño
- **Conventional Commits** — Trazabilidad

## Aplicación en este proyecto

### Functional Suitability (Idoneidad funcional)

✓ **Criterios de aceptación explícitos** en cada HU  
✓ Validación V1/V2 contra criterios  
✓ Evidencia verificable (tests)

### Security (Seguridad)

✓ Validación de entrada en backend  
✓ AuthN/AuthZ según roles  
✓ Consultas parametrizadas (ORM)  
✓ No exponer secretos ni stack traces  
✓ HTTPS en producción

### Usability (Usabilidad)

✓ Estados claros: loading, success, error, empty  
✓ Feedback inmediato  
✓ Errores específicos por campo  
✓ Accesibilidad WCAG 2.1 AA cuando aplica  
✓ Responsive design

### Performance Efficiency

✓ Consultas optimizadas (índices cuando necesario)  
✓ Lazy loading en frontend  
✓ Bundle size razonable  
✓ Solo optimizar si HU lo requiere o hay degradación

### Maintainability (Mantenibilidad)

✓ Cambio mínimo por HU  
✓ Reutilización de componentes/servicios  
✓ No refactorización general innecesaria  
✓ Tests unitarios + integración  
✓ Documentación en `docs/dev-history.md`

### Reliability (Fiabilidad)

✓ Manejo robusto de errores  
✓ Transacciones cuando aplica  
✓ Tests E2E del flujo completo  
✓ Validación de casos de error

### Compatibility (Compatibilidad)

✓ No romper funcionalidad existente  
✓ Contratos respetados (Backend ↔ Frontend)  
✓ Backward compatibility en APIs públicas  
✓ Protección de estructuras críticas

## Métricas objetivo

| Métrica | Objetivo | Medición |
|---|---|---|
| Acceptance Score | ≥ 90% | Criterios cumplidos / verificables |
| Test Coverage | ≥ 80% | Jest/Vitest |
| Defect Rate | < 10% | Errores detectados post-implementación / HUs |
| Security Issues | 0 críticos | Análisis manual + workflows |
| Accessibility (WCAG AA) | 100% en funcionalidad pública | axe-core / manual |

## Procesos de validación

### Implementación de HU

1. **V1 (especialistas):** Validación contra criterios de aceptación
2. **V2 (especialistas):** Tests ejecutados + corrección de incumplimientos V1
3. **Integrador:** Validación E2E con evidencia

### Code review (manual)

- Seguridad
- Arquitectura
- Clean Code
- Tests suficientes

### CI/CD

- Linter
- Tests automatizados
- Build exitoso
- Security scan (opcional)

## Evolución de estándares

Este documento debe actualizarse cuando:
1. Se adopten nuevos estándares (ej. agregar OWASP ASVS)
2. Cambien objetivos de métricas
3. Se agreguen validaciones CI/CD
4. El equipo defina nuevas convenciones

---

**Última revisión:** 2026-09-14  
**Responsable:** [Definir según proyecto]  
**Referencia técnica completa:** `.ai/standards/quality-attributes.md`
