
# Pipeline de Segurança - Security Scan

Esta pipeline implementa verificações automatizadas de segurança utilizando GitHub Actions. As verificações cobrem análise estática (SAST), análise de composição de software (SCA) e verificação de segredos no código.

## Visão Geral

A pipeline é composta por três etapas principais:
1. **SAST Scan (Static Application Security Testing):** Utiliza o Semgrep para identificar vulnerabilidades no código-fonte.
2. **SCA Scan (Software Composition Analysis):** Utiliza o Trivy para verificar vulnerabilidades em dependências e bibliotecas de terceiros.
3. **Secret Scanning:** Utiliza o Gitleaks para identificar segredos expostos acidentalmente no código.

## Estrutura do Workflow

### Permissões
A pipeline requer as seguintes permissões:
```yaml
permissions:
  contents: write
  security-events: write
```

### Gatilho
A pipeline é executada em todos os **pushes**:
```yaml
on:
  push:
```

### Etapas

#### **1. SAST Scan**
- Ferramenta: [Semgrep](https://semgrep.dev/)
- Container oficial: `returntocorp/semgrep`
- Relatório gerado: `sast-report.json`

#### **2. SCA Scan**
- Ferramenta: [Trivy](https://github.com/aquasecurity/trivy)
- Tipo de escaneamento: Sistema de arquivos (`fs`)
- Relatório gerado: `sca-report.json`
- Severidades consideradas: CRITICAL e HIGH.

#### **3. Secret Scanning**
- Ferramenta: [Gitleaks](https://github.com/zricethezav/gitleaks)
- Argumentos: `--path=. --verbose --redact`

## Resultados
Os relatórios gerados por cada etapa são salvos como artefatos no GitHub Actions para futura análise:
- **Secrets Scan:** no próprio Log da pipeline
- **SAST Report:** `sast-report.json`
- **SCA Report:** `sca-report.json`
