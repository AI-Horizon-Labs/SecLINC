# SecLINC: Classificador Automatizado de Incidentes de Cibersegurança com LLMs e Engenharia de Prompting baseado no NIST

Um sistema avançado de classificação automática de incidentes de segurança cibernética utilizando modelos de linguagem (LLM) e técnicas de prompting progressivo, baseado nos padrões NIST.

## 📋 Visão Geral

Este sistema processa descrições de incidentes de segurança e os classifica automaticamente em 12 categorias NIST padronizadas, utilizando diferentes técnicas de prompting e modelos de linguagem tanto locais quanto via API.

### Categorias NIST Suportadas

- **CAT1**: Account Compromise (Comprometimento de Conta)
- **CAT2**: Malware (Software Malicioso)
- **CAT3**: Denial of Service Attack (Ataque de Negação de Serviço)
- **CAT4**: Data Leak (Vazamento de Dados)
- **CAT5**: Vulnerability Exploitation (Exploração de Vulnerabilidades)
- **CAT6**: Insider Abuse (Abuso Interno)
- **CAT7**: Social Engineering (Engenharia Social)
- **CAT8**: Physical Incident (Incidente Físico)
- **CAT9**: Unauthorized Modification (Modificação Não Autorizada)
- **CAT10**: Misuse of Resources (Uso Indevido de Recursos)
- **CAT11**: Third-Party Issues (Problemas de Terceiros)
- **CAT12**: Intrusion Attempt (Tentativa de Intrusão)

## 🚀 Funcionalidades

- **Múltiplas Técnicas de Prompting**:
  - Progressive Hint Prompting (PHP)
  - Progressive Rectification Prompting (PRP)
  - Self-Hint Prompting (SHP)
  - Hypothesis Testing Prompting (HTP)

- **Suporte a Múltiplos Modelos**:
  - APIs externas (OpenAI, etc.)
  - Modelos locais via Hugging Face

- **Análise de Qualidade**:
  - Cálculo de ROUGE Score
  - Métricas de confiabilidade
  - Monitoramento de tokens

- **Formatos de Saída**:
  - CSV
  - JSON
  - XLSX

## 📦 Instalação

1.  **Clone o repositório:**

    ```bash
    git clone https://github.com/AI-Horizon-Labs/SecLINC.git
    cd SecLINC
    ```

2.  **Crie um ambiente virtual (recomendado):**

    ```bash
    python3 -m venv .venv
    source .venv/bin/activate  # Linux/macOS
    .venv\Scripts\activate  # Windows
    ```

3.  **Instale as dependências:**

    ```bash
    pip install -r requirements.txt
    ```

### Dependências

```bash
pip install -r requirements.txt
```



#### Principais bibliotecas:
- `pandas`: Manipulação de dados
- `openai`: Integração com APIs de LLM
- `transformers`: Modelos Hugging Face
- `rouge-score`: Métricas de avaliação
- `psutil`: Monitoramento de recursos
- `tiktoken`: Contagem de tokens
- `tqdm`: Barras de progresso

### Configuração

1. Crie um arquivo `config.json` no diretório raiz:

```json
{
  "ativos": ["openai", "local"],
  "hugginface_api_key": "sua_chave_huggingface",
  "openai": {
    "provider": "openai",
    "modelo": "gpt-4",
    "api_key": "sua_chave_openai",
    "uri": "https://api.openai.com/v1",
    "usar_prompt_local": false
  },
  "local": {
    "provider": "local",
    "modelo": "microsoft/DeepSeek-R1",
    "usar_prompt_local": true,
    "config_model": "trust_remote_code=True,torch_dtype=float16"
  }
}
```

## 🔧 Uso

### Sintaxe Básica

```bash
python main.py <diretorio> --colunas <colunas> --provider <provider> [opções]
```

### Exemplos de Uso

#### Processamento Básico com Progressive Hints Prompting
```bash
python main.py ./dados --colunas incident_description --provider openai --limite_hint 3
```

#### Análise Avançada com Hypothesis Testing Prompting
```bash
python main.py ./dados --colunas incident_description severity --provider local --modo htp --limite_hint 5 --formato json
```

#### Processamento com Self-Hint Prompting
```bash
python main.py ./dados --colunas description type --provider openai --modo shp --limite_rouge 0.85 --formato xlsx
```

#### Processamento com Progressive Rectification Prompting
```bash
python main.py ./dados --colunas description type --provider openai --modo prp --limite_rouge 0.85 --formato xlsx
```

### Parâmetros

#### Obrigatórios
- `diretorio`: Caminho para o diretório contendo arquivos CSV/XLS/XLSX
- `--colunas`: Nomes das colunas a serem usadas como prompt
- `--provider`: Provedor a ser utilizado (definido no config.json)

#### Opcionais
- `--limite_hint`: Número máximo de dicas progressivas (padrão: 0)
- `--limite_rouge`: Limite do ROUGE Score para convergência (padrão: 0.9)
- `--formato`: Formato de saída (csv, json, xlsx) (padrão: csv)
- `--modo`: Técnica de prompting (php, prp, shp, htp) (padrão: php)
- `--nist/--no-nist`: Ativar/desativar categorizador NIST (padrão: ativo)

## 🎯 Modos de Operação

### 1. Progressive Hint Prompting (PHP)
Gera dicas progressivas baseadas na resposta anterior até atingir convergência.

### 2. Progressive Rectification Prompting (PRP)
Refina iterativamente a classificação rejeitando categorias incorretas.

### 3. Self-Hint Prompting (SHP)
Utiliza auto-reflexão para melhorar a classificação inicial.

### 4. Hypothesis Testing Prompting (HTP)
Testa hipóteses para cada categoria NIST de forma sistemática.

## 📊 Saída

### Estrutura dos Resultados

```json
{
  "informacoes_das_colunas": "incident_description: Suspicious login attempt",
  "categoria": "CAT1",
  "explicacao": "Account compromise through unauthorized access attempt",
  "rouge": 0.92
}
```

### Logs Automáticos

O sistema gera logs detalhados em `logs/` contendo:
- Tokens de entrada e saída
- Timestamps
- Prompts utilizados
- Respostas geradas

## 🔍 Monitoramento

### Métricas Coletadas
- **Uso de memória**: Monitoramento em tempo real
- **Tempo de execução**: Duração total do processamento
- **Contagem de tokens**: Para controle de custos
- **ROUGE Score**: Medida de convergência

### Exemplo de Saída de Monitoramento
```
Uso de memória no início: 245.67 MB
Progresso: 150/500 linhas processadas (30.00%)
Uso de memória no final: 489.23 MB
Tempo total de execução: 1847.32 segundos
```

## 🛠️ Estrutura do Projeto

```
├── main.py                 # Arquivo principal
├── config.json            # Configurações do sistema
├── logs/                  # Logs de execução
│   └── YYYY-MM-DD_modelo_modo.json
├── dados/                 # Diretório de dados (exemplo)
│   ├── incidents.csv
│   └── security_logs.xlsx
└── resultados_provider_modo.csv  # Resultados gerados
```

## 🔒 Segurança

- As chaves de API são armazenadas no arquivo `config.json`
- Nunca commite o arquivo `config.json` no controle de versão
- Use variáveis de ambiente para chaves sensíveis em produção

## 🐛 Solução de Problemas

### Problemas Comuns

1. **Erro de configuração não encontrada**
   - Verifique se o arquivo `config.json` existe
   - Confirme se o provider está listado em "ativos"

2. **Erro de memória insuficiente**
   - Reduza o `limite_hint`
   - Use modelos menores para processamento local

3. **Erro de API**
   - Verifique as chaves de API
   - Confirme a conectividade com a internet

### Debug

Para depuração, monitore os logs gerados em `logs/` que contêm informações detalhadas sobre cada execução.

## 📈 Performance

### Otimizações Implementadas
- Processamento em lote de arquivos
- Cache de modelos locais
- Monitoramento de recursos
- Interrupção por convergência (ROUGE Score)

### Benchmarks Típicos
- **Processamento**: ~2-5 segundos por incidente
- **Memória**: 200-500 MB dependendo do modelo
- **Precisão**: 85-95% em datasets de teste

## 🤝 Contribuição

1. Faça fork do projeto
2. Crie uma branch para sua feature
3. Implemente suas mudanças
4. Adicione testes se necessário
5. Envie um pull request

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## 🆘 Suporte

Para questões e suporte:
- Abra uma issue no GitHub
- Consulte a documentação dos modelos utilizados
- Verifique os logs de execução para debugging

---

**Nota**: Este sistema foi desenvolvido para análise automatizada de incidentes de segurança e deve ser usado como ferramenta de apoio, não substituindo a análise humana especializada em casos críticos.
