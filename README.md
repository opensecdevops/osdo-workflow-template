# 🛡️ OSDO Workflow Template

**Open Security DevOps (OSDO) Framework** - El framework completo, opinado y listo para usar para seguridad DevOps. **Este es el componente FRAMEWORK** - Orquestador de seguridad con best practices preconfiguradas.

**Relacionado:** [osdo-actions](https://github.com/opensecdevops/osdo-actions) (acciones granulares) • [osdo-workflows](https://github.com/opensecdevops/osdo-workflows) (workflows modulares)

---

## 📚 Tabla de Contenidos

1. [Características](#características-principales)
2. [Arquitectura](#arquitectura)
3. [Quick Start](#quick-start)
4. [Uso en Otros Proyectos](#uso-en-otros-proyectos)
5. [Configuración](#configuración-avanzada)
6. [Herramientas Incluidas](#herramientas-de-seguridad-incluidas)
7. [Acciones Disponibles](#referencia-rápida-de-acciones)
8. [Ejemplos](#ejemplos-por-lenguaje)
9. [Documentación](#documentación)

---

## 🚀 Características Principales

- ✅ **Framework Opinado** - Best practices preconfiguradas
- ✅ **Todo Incluido** - Seguridad, tests, compliance y reportes
- ✅ **Multi-lenguaje** - Node.js, Python, Java, Go, PHP
- ✅ **Composite Actions** - Acciones modulares y reutilizables
- ✅ **Reporting Completo** - Reportes en múltiples formatos
- ✅ **Seguridad Integral** - SCA, SAST, secretos, containers
- ✅ **Quality Gates** - Umbrales configurables
- ✅ **SARIF Compatible** - Integración con GitHub Security tab
- ✅ **Optimizado** - Caché inteligente para rendimiento

---

## 🏗️ Arquitectura

### Los 3 Pilares de OSDO

```
┌───────────────────────────────────────────────┐
│  Tu Proyecto / Organización                   │
└────────────┬────────────────────────────────┬─┘
             │                                │
    ┌────────▼────────────┐          ┌────────▼──────────┐
    │  OSDO Framework     │          │  OSDO Actions     │
    │  (Este repo)        │          │  (Granular)       │
    │  - Opinado          │          │  - Independiente  │
    │  - Todo incluido    │          │  - Customizable   │
    │  - Composite actions│          │  - Versionado     │
    └────────┬────────────┘          └──────────────────┘
             │
    ┌────────▼──────────┐
    │  OSDO Workflows   │
    │  (Modulares)      │
    │  - Scenarios      │
    │  - Reutilizable   │
    └───────────────────┘
```

### Estructura del Repositorio

```
osdo-workflow-template/
├── .github/
│   ├── workflows/
│   │   ├── osdo-framework.yml       # Workflow principal reutilizable
│   │   ├── osdo-complete-workflow.yml # Ejemplo completo
│   │   └── publish-release.yml      # Publica nuevas versiones
│   │
│   └── actions/
│       ├── setup-osdo/              # Setup del entorno
│       ├── test-quality/            # Tests y quality
│       ├── security-scan/           # Análisis de seguridad
│       ├── build-security/          # Build seguro + SBOM
│       ├── security-quality-gate/   # Quality gates
│       └── compliance-report/       # Reportes
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── INTEGRATION_GUIDE.md
│   ├── CONTRIBUTING.md
│   └── ...
│
└── .osdo/
    └── config.yml
```

---

## 🚀 Quick Start

### Para Proyecto Nuevo (2 minutos)

```yaml
# .github/workflows/osdo.yml
name: OSDO Framework

on: [push, pull_request]

jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      node-version: '22'
      test-coverage-threshold: '80'
```

**Eso es todo.** El framework hace:
- ✅ Tests automáticos
- ✅ Análisis de seguridad
- ✅ Validación de compliance
- ✅ Generación de reportes

---

## 🔧 Uso en Otros Proyectos

### Opción 1: Framework Completo (Recomendado)

Para máxima facilidad:

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      # Lenguaje
      node-version: '22'
      
      # Configuración
      test-coverage-threshold: '85'
      enable-sca: true
      enable-sast: true
      enable-container-scan: false
      
      # Comportamiento
      fail-on-high-security: true
```

**Cuándo usar:** Proyecto nuevo, quieres best practices, mínima configuración.

---

### Opción 2: Workflows Modulares

Para flexibilidad media:

```yaml
jobs:
  security:
    uses: opensecdevops/osdo-workflows/.github/workflows/security-only.yml@main

  quality:
    uses: opensecdevops/osdo-workflows/.github/workflows/quality-gates.yml@main
```

**Cuándo usar:** Tienes CI/CD existente, quieres agregar componentes específicos.

---

### Opción 3: Actions Granulares

Para máximo control:

```yaml
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: opensecdevops/osdo-actions/actions/osdo-sca@osdo-sca/v1.0.0
        with:
          enable-npm: true
          
      - uses: opensecdevops/osdo-actions/actions/osdo-sast@osdo-sast/v1.0.0
        with:
          enable-semgrep: true
```

**Cuándo usar:** Máximo control, arquitectura custom, integración específica.

---

## ⚙️ Configuración Avanzada

### Inputs Disponibles

| Input | Default | Descripción |
|-------|---------|-------------|
| `node-version` | `22` | Versión de Node.js a usar |
| `python-version` | `3.11` | Versión de Python a usar |
| `java-version` | `17` | Versión de Java a usar |
| `test-coverage-threshold` | `80` | Umbral mínimo de cobertura |
| `enable-sca` | `true` | Habilitar Software Composition Analysis |
| `enable-sast` | `true` | Habilitar Static Application Security Testing |
| `enable-secrets` | `true` | Habilitar detección de secretos |
| `enable-container-scan` | `false` | Habilitar escaneo de contenedores |
| `enable-build-security` | `true` | Habilitar build seguro y SBOM |
| `enable-quality-gate` | `true` | Habilitar Security Quality Gate |
| `fail-on-high-security` | `true` | Fallar si encuentra vulnerabilidades altas |
| `sbom-format` | `both` | Formato SBOM: `json`, `xml`, o `both` |

### Archivo de Configuración

```yaml
# .osdo/config.yml
test:
  coverage:
    minimum: 85

security:
  quality_gates:
    critical: 0
    high: 5
    medium: 20

reporting:
  formats:
    - markdown
    - html
```

---

## 🔍 Herramientas de Seguridad Incluidas

### Software Composition Analysis (SCA)
- **npm audit** + audit-ci
- **Safety** (Python)
- **OWASP Dependency-Check** (Java)
- **Govulncheck** (Go)
- **PHP Security Checker** (PHP/Composer)

### Detección de Secretos
- **TruffleHog** - Detección de secretos en Git
- **Gitleaks** - Detección de credenciales

### Static Application Security Testing (SAST)
- **Semgrep** - Multi-lenguaje (JS, Python, Java, Go, PHP)
- **Bandit** - Python
- **Gosec** - Go
- **SpotBugs + Find-Sec-Bugs** - Java
- **PHPStan** - PHP

### Análisis de Contenedores
- **Trivy** - Vulnerabilidades en imágenes Docker
- **Hadolint** - Linting de Dockerfiles

---

## 📌 Referencia Rápida de Acciones

| Acción | Versión | Descripción |
|--------|---------|-------------|
| `setup-osdo` | `@setup-osdo/v1.0.0` | Setup del entorno OSDO |
| `test-quality` | `@test-quality/v1.0.0` | Tests y quality checks |
| `security-scan` | `@security-scan/v1.0.0` | Análisis de seguridad integral |
| `build-security` | `@build-security/v1.0.0` | Build seguro + SBOM |
| `security-quality-gate` | `@security-quality-gate/v1.0.0` | Quality gate de seguridad |
| `compliance-report` | `@compliance-report/v1.0.0` | Generación de reportes |

---

## 📖 Ejemplos por Lenguaje

### Node.js

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      node-version: '22'
      test-coverage-threshold: '85'
      enable-sca: true
```

### Python

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      python-version: '3.11'
      test-coverage-threshold: '80'
      enable-sast: true
```

### Java (Maven)

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      java-version: '17'
      build-command: 'mvn clean install'
      test-coverage-threshold: '80'
```

### Go

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      go-version: '1.21'
      test-coverage-threshold: '75'
```

### PHP

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      php-version: '8.2'
      test-coverage-threshold: '80'
```

### Con Docker

```yaml
jobs:
  osdo:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    with:
      node-version: '22'
      enable-container-scan: true
      enable-build-security: true
```

---

## 📊 Outputs del Framework

```yaml
compliance-status:          # PASS, WARNING, FAIL
security-score:             # 0-100
test-coverage:              # Porcentaje
vulnerabilities-found:      # Número
quality-gate-status:        # PASSED, WARNING, FAILED
build-verification:         # VERIFIED, WARNING, SKIPPED
```

---

## 📚 Documentación

### Inicio Rápido
- [ARCHITECTURE.md](../ARCHITECTURE.md) - Arquitectura de 3 repositorios
- [INTEGRATION_GUIDE.md](../INTEGRATION_GUIDE.md) - Guía de integración
- [CONTRIBUTING.md](../CONTRIBUTING.md) - Guía de contribución

### Documentación Interna
- [docs/QUICK_START.md](docs/QUICK_START.md) - Inicio rápido detallado
- [docs/PUBLISHING_GUIDE.md](docs/PUBLISHING_GUIDE.md) - Publicar nuevas versiones
- [docs/cache-optimization.md](docs/cache-optimization.md) - Optimización de caché

### Documentación Relacionada
- [osdo-actions README](https://github.com/opensecdevops/osdo-actions/blob/main/README.md)
- [osdo-workflows README](https://github.com/opensecdevops/osdo-workflows/blob/main/README.md)

---

## 🤝 Cómo Contribuir

1. Lee [CONTRIBUTING.md](../CONTRIBUTING.md)
2. Fork el repositorio
3. Crea rama: `feature/tu-feature`
4. Commit: `git commit -m "feat: describe cambio"`
5. Push: `git push origin feature/tu-feature`
6. Abre Pull Request

---

## 📞 Soporte

- **Issues:** [Reporta un bug](../issues)
- **Discussions:** [Haz una pregunta](../discussions)
- **Documentación:** [Ver docs](../INTEGRATION_GUIDE.md)

---

## 📄 Licencia

MIT License - Ver [LICENSE](LICENSE) para detalles

---

## 🎯 Roadmap

- [ ] Soporte para DAST
- [ ] Dashboard unificado
- [ ] Policy as Code
- [ ] ML-based vulnerability detection
- [ ] Enterprise integrations

---

**Made with ❤️ by the OpenSecDevOps Community**

🔗 **Comunidad:** [OpenSecDevOps](https://opensecdevops.org)
