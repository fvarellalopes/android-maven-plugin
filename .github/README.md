# GitHub Actions para Android Maven Plugin

Este repositório contém workflows do GitHub Actions para automatizar a compilação e empacotamento do Android Maven Plugin.

## Workflows Disponíveis

### 1. Build and Package (`build.yml`)
- **Trigger**: Push para branches `master`/`main`, Pull Requests, ou execução manual
- **Funcionalidades**:
  - Build com Java 11
  - Configura Android SDK automaticamente
  - Executa testes
  - Gera JAR do plugin
  - Faz upload dos artefatos gerados
  - Cria releases automáticos para tags

### 2. Quick Build (`quick-build.yml`)
- **Trigger**: Push para branches de desenvolvimento (exceto `master`/`main`)
- **Funcionalidades**:
  - Build rápido com Java 11
  - Pula testes para desenvolvimento ágil
  - Verifica se o JAR foi criado com sucesso

## Como Usar

### Execução Automática
Os workflows são executados automaticamente quando você:
- Faz push para qualquer branch
- Cria um Pull Request
- Cria uma tag começando com `v` (ex: `v4.6.2`)

### Execução Manual
Você pode executar manualmente através da interface do GitHub:
1. Vá para a aba "Actions" do repositório
2. Selecione o workflow desejado
3. Clique em "Run workflow"

### Baixar Artefatos
Após o build:
1. Vá para a aba "Actions"
2. Clique no workflow executado
3. Na seção "Artifacts", baixe:
   - `android-maven-plugin-jar`: Contém o JAR gerado
   - `test-results`: Contém relatórios de teste

## Requisitos do Projeto

- **Java**: 11
- **Maven**: 3.0.4+
- **Android SDK**: API 28, Build Tools 28.0.3

## Estrutura dos Artefatos

O JAR principal é gerado em:
```
target/android-maven-plugin-{version}.jar
```

## Notas Importantes

- O projeto usa Java 11 como versão padrão
- Testes requerem Android SDK configurado
- Cache do Maven é usado para acelerar builds subsequentes
- Builds de release são criados automaticamente para tags

## Troubleshooting

### Build falha por dependências Android
- Verifique se as versões do Android SDK estão corretas
- O workflow configura automaticamente API 28 e Build Tools 28.0.3

### Testes falhando
- Alguns testes podem requerer emulador Android
- Use `-DskipTests` para builds sem testes se necessário

### Cache do Maven
- O cache é limpo automaticamente se houver problemas
- Cache key é baseado no hash do `pom.xml`
