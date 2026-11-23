# Stack Tecnológica e ADRs — Inbox AI API

**Versão:** 1.0

  

**Projeto:** `inbox-ai-api`

  

**Sugestão de arquivo no repositório:** `docs/ARCHITECTURE.md`

  

## 1. Stack Tecnológica

|**Camada**|**Tecnologia**|**Papel no Projeto**|
|---|---|---|
|**Linguagem**|Python 3.11+|Linguagem principal do projeto, padrão de mercado para IA aplicada e automação.|
|**Framework API**|FastAPI + Uvicorn|Exposição de rotas REST com tipagem nativa, alta performance e documentação Swagger/OpenAPI automática (`/docs`).|
|**Validação**|Pydantic v2|Validação estrita de contratos de entrada/saída e sanitização das respostas da LLM.|
|**Banco de Dados**|SQLite (`sqlite3`)|Persistência relacional leve, zero configuração externa e uso de SQL puro parametrizado.|
|**Inteligência Artificial**|Google Gemini (`google-genai`)|Classificação semântica de intenção, urgência e geração de sugestão de resposta.|
|**Testes**|Pytest + HTTPX|Testes unitários do motor de classificação, validação de schemas e testes de rotas.|
|**Configuração**|`python-dotenv`|Carregamento seguro de variáveis de ambiente (`.env`) fora do controle de versão.|
|**Versionamento**|Git + Conventional Commits|Histórico limpo, rastreável e padronizado (`feat`, `fix`, `test`, `docs`, `chore`).|

## 2. Architecture Decision Records (ADRs)

### ADR-001: Escolha do FastAPI como framework web

- **Status:** Aceito
    
- **Contexto:** O projeto precisa expor uma API REST enxuta, fácil de testar e com documentação clara para demonstração técnica.
    
- **Decisão:** Adotar **FastAPI** em vez de Flask ou Django.
    
- **Consequências:**
    
    - _Positivas:_ Validação automática com Pydantic, _type hints_ nativos do Python e geração instantânea do Swagger UI (`/docs`) sem código extra.
        
    - _Negativas:_ Exige compreensão básica de tipagem estática em Python para aproveitar bem o framework.
        

### ADR-002: SQLite com SQL puro e camada de repositório isolada

- **Status:** Aceito
    
- **Contexto:** O MVP precisa persistir chamados e gerar métricas analíticas (`COUNT`, `GROUP BY`) sem exigir que quem for testar o projeto precise subir containers ou instalar um servidor de banco de dados.
    
- **Decisão:** Utilizar **SQLite** nativo do Python com **SQL puro parametrizado (`?`)**, isolado em `src/repositories/ticket_repo.py` (sem ORM como SQLAlchemy).
    
- **Consequências:**
    
    - _Positivas:_ Setup imediato em qualquer máquina, demonstração explícita de domínio de SQL e proteção contra _SQL Injection_. O isolamento no repositório permite migrar para PostgreSQL no futuro alterando apenas a camada de dados.
        
    - _Negativas:_ SQLite não é indicado para alta concorrência de escrita em produção distribuída (limitação aceita para o escopo de MVP).
        

### ADR-003: Classificação com LLM + Fallback determinístico local

- **Status:** Aceito
    
- **Contexto:** APIs externas de IA podem sofrer indisponibilidade, _rate limit_, latência alta ou simplesmente estar sem chave configurada em ambientes de teste local.
    
- **Decisão:** Encapsular a chamada ao Gemini em `src/services/ai_classifier.py` exigindo saída em JSON estruturado e adicionar um **fallback local baseado em palavras-chave** acionado automaticamente em caso de falha ou ausência de `GEMINI_API_KEY`.
    
- **Consequências:**
    
    - _Positivas:_ 100% de disponibilidade da triagem e transparência operacional via campo `used_fallback: true/false` na resposta.
        
    - _Negativas:_ A precisão semântica do _fallback_ local é inferior à da LLM em frases ambíguas ou irônicas.
        

### ADR-004: Arquitetura em camadas simples (Clean Code sem over-engineering)

- **Status:** Aceito
    
- **Contexto:** Para um microserviço pequeno de portfólio, arquiteturas muito pesadas podem gerar excesso de arquivos e dificultar a leitura, enquanto colocar tudo em um único arquivo `main.py` viola boas práticas de manutenção.
    
- **Decisão:** Adotar separação direta em 4 responsabilidades: **Rotas** (`main.py`), **Contratos** (`schemas.py`), **Serviços/IA** (`services/`) e **Dados/SQL** (`repositories/` e `database.py`).
    
- **Consequências:**
    
    - _Positivas:_ Código fácil de navegar na entrevista, baixo acoplamento e alta testabilidade unitária.
        
    - _Negativas:_ Caso o sistema cresça para múltiplos domínios complexos, será necessário evoluir para uma estrutura modular ou Hexagonal completa.
        

Você pode criar a pasta `docs/`, salvar o BRD em `docs/BRD.md` e este arquivo em `docs/ARCHITECTURE.md`, e subir ambos em um commit dedicado:

  

Bash

```
mkdir -p docs
git add docs/
git commit -m "docs: add BRD, tech stack and architecture decision records"
git push
```

Quando quiser, seguimos para o **Passo 3 (`src/schemas.py`)**.