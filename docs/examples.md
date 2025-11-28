# Ejemplos de Uso del OSDO Framework

## 1. Proyecto Node.js con React

```yaml
# .github/workflows/ci.yml
name: 🛡️ CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  osdo-security:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    permissions:
      contents: read
      security-events: write
      actions: read
      pull-requests: write
    with:
      node-version: '20'
      test-coverage-threshold: '85'
      enable-container-scan: false
      enable-static-analysis: true
      report-format: 'markdown'
```

## 2. Proyecto Python con FastAPI

```yaml
# .github/workflows/security.yml
name: 🔒 Security Compliance

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 2 * * 1'

jobs:
  osdo-python:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    permissions:
      contents: read
      security-events: write
      actions: read
      pull-requests: write
    with:
      python-version: '3.11'
      node-version: 'false'
      test-coverage-threshold: '90'
      enable-dependency-scan: true
      enable-secrets-scan: true
      enable-static-analysis: true
      enable-build-security: true
      sbom-format: 'both'
      enable-quality-gate: true
      critical-threshold: '0'
      high-threshold: '5'
```

## 3. Proyecto Full-Stack con Docker

```yaml
# .github/workflows/full-stack-security.yml
name: 🛡️ Full-Stack Security Pipeline

on:
  push:
    branches: [ main, develop, staging ]
  pull_request:
    branches: [ main ]

jobs:
  osdo-fullstack:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    permissions:
      contents: read
      security-events: write
      actions: read
      pull-requests: write
    with:
      node-version: '22'
      python-version: '3.11'
      test-coverage-threshold: '80'
      enable-container-scan: true
      enable-dependency-scan: true
      enable-secrets-scan: true
      enable-static-analysis: true
      enable-build-security: true
      sbom-format: 'both'
      enable-quality-gate: true
      critical-threshold: '0'
      report-format: 'html'
```

## 4. Configuración Personalizada

```yaml
# .osdo/config.yml en tu proyecto
test:
  coverage:
    minimum: 85
    exclude:
      - 'tests/**'
      - 'docs/**'

security:
  dependency_scan:
    severity_threshold: 'high'
  
  static_analysis:
    rules:
      - 'security'
      - 'owasp-top-10'
      - 'cwe-top-25'

quality_gates:
  security_score:
    minimum: 90
  compliance:
    required_checks:
      - 'dependency_scan'
      - 'secrets_scan'
      - 'static_analysis'
```

## 5. Workflow con Deployment Gate

```yaml
name: 🚀 Deploy with OSDO Gate

on:
  push:
    branches: [ main ]

jobs:
  osdo-security:
    uses: opensecdevops/osdo-workflow-template/.github/workflows/osdo-framework.yml@main
    permissions:
      contents: read
      security-events: write
      actions: read
      pull-requests: write
    with:
      test-coverage-threshold: '85'
      fail-on-high-security: true

  deploy:
    needs: osdo-security
    if: needs.osdo-security.outputs.compliance-status == 'COMPLIANT'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Production
        run: |
          echo "Deploying with OSDO compliance: ${{ needs.osdo-security.outputs.compliance-status }}"
          echo "Security Score: ${{ needs.osdo-security.outputs.security-score }}"
          # Your deployment steps here
```
