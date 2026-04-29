# 🎯 REMANEXO - Sistema Financeiro com POO

> Sistema de gestão financeira com **Open Finance Simulado** desenvolvido em **Python com Flask e SQLite**
>
> Demonstração prática dos 4 pilares da **Programação Orientada a Objetos** em um ambiente funcional e real


## ✨ Próposito do Projeto

Remanexo é um **projeto educacional avançado** que implementa um sistema financeiro completo enquanto demonstra os conceitos fundamentais de POO:

- ✅ **Abstração** → Classe abstrata `Transacao`
- ✅ **Herança** → `Receita` e `Despesa` herdam de `Transacao`
- ✅ **Encapsulamento** → Atributos privados em `Conta` com getters/setters
- ✅ **Polimorfismo** → Padrão State no `Nexo` com comportamentos distintos

---

## 🚀 Início Rápido

### Requisitos
- Python 3.8+
- pip

### Instalação

```bash
# Clone ou acesse o diretório
cd Projeto-Remanexo

# Crie um ambiente virtual
python -m venv venv

# Ative o ambiente
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows

# Instale as dependências
pip install -r requirements.txt

# Execute
python run.py
```

Acesse **http://localhost:5000** no navegador.

---

## 🔐 Credenciais Demo

```
Email: demo@remanexo.com
Senha: 123456
```

> A conta demo vem com saldo inicial de R$ 5.000,00 e plano premium

---

## 📊 Arquitetura POO

### 1. ABSTRAÇÃO
```python
# models/transacao.py
class Transacao(ABC):
    @abstractmethod
    def calcular_impacto_saldo(self):
        pass
```
Define contrato que toda transação deve cumprir.

### 2. HERANÇA
```python
class Receita(Transacao):
    def calcular_impacto_saldo(self):
        return self.valor  # soma

class Despesa(Transacao):
    def calcular_impacto_saldo(self):
        return -self.valor  # subtrai
```
Comportamentos especializados em subclasses.

### 3. ENCAPSULAMENTO
```python
# models/conta.py
class Conta:
    @property
    def saldo(self):
        return self._saldo
    
    @saldo.setter
    def saldo(self, novo_saldo):
        # validação aqui — dados protegidos
        if novo_saldo < 0:
            raise ValueError("saldo não pode ser negativo")
        self._saldo = novo_saldo
```
Atributos privados com acesso controlado.

### 4. POLIMORFISMO (Padrão State)
```python
# models/nexo.py
class EstadoNexo(ABC):
    @abstractmethod
    def sincronizar(self, fila):
        pass

class NexoAtivo(EstadoNexo):
    def sincronizar(self, fila):
        # processa normalmente
        
class NexoInstavel(EstadoNexo):
    def sincronizar(self, fila):
        # segura fila em cache
```
Cada estado implementa seu próprio comportamento.

---

## 📋 Funcionalidades (RF01-RF10)

| RF | Funcionalidade | Status |
|----|---|---|
| RF01 | Autenticação email + senha (werkzeug hash) | ✅ |
| RF02 | Open Finance simulado (Nexo 3 estados) | ✅ |
| RF03 | Dashboard saldo consolidado tempo real | ✅ |
| RF04 | Lixeira de transações (restaurar) | ✅ |
| RF05 | Metas com barra de progresso visual | ✅ |
| RF06 | Categorização automática por palavras-chave | ✅ |
| RF07 | Importação transações via CSV | ✅ |
| RF08 | Exportação relatório PDF | ⏳ Futuro |
| RF09 | Notificações gasto excessivo (>80%) | ✅ |
| RF10 | Conciliação manual sem API | ✅ |

---

## 📁 Estrutura do Projeto

```
Projeto-Remanexo/
├── backend/                     # 🔧 Código do servidor
│   ├── models/
│   │   ├── transacao.py         # Abstração + Herança
│   │   ├── nexo.py              # Polimorfismo (State Pattern)
│   │   ├── conta.py             # Encapsulamento
│   │   ├── meta.py, usuario.py, assinatura.py
│   │   └── __init__.py
│   ├── routes/
│   │   ├── dashboard.py         # RF01, RF03, RF09
│   │   ├── transacoes.py        # RF04, RF06, RF07, RF10
│   │   ├── metas.py             # RF05
│   │   ├── nexo.py              # RF02
│   │   └── __init__.py
│   ├── templates/               # 🎨 Templates HTML
│   │   ├── base.html, dashboard.html
│   │   ├── transacoes.html, metas.html, nexo.html
│   │   └── login.html, cadastro.html
│   ├── static/                  # 📁 CSS/JS customizado
│   ├── database.py              # SQLAlchemy models
│   ├── app.py                   # Factory Flask
│   └── __init__.py
├── frontend/                    # 📱 Frontend (futuro)
├── database/                    # 🗄️ Scripts de banco
├── docs/                        # 📚 Documentação extra
├── run.py                       # Ponto de entrada
├── requirements.txt
├── start.bat / start.sh         # Scripts de inicialização
├── README.md, INSTALL.md, ARQUITETURA.md, CHECKLIST.md
└── ... arquivos adicionais
```

---

## 🎨 Design Visual

- **Paleta**: Azul escuro (#1a2e4a), Verde (#28a745), Vermelho (#dc3545)
- **Framework**: Bootstrap 5 via CDN
- **Layout**: Responsivo e moderno

---

## 🧪 Testando os Conceitos POO

### Teste Polimorfismo
1. Vá para Transações
2. Adicione receita de R$ 100 + despesa de R$ 30
3. Veja como cada uma impacta o saldo diferentemente

### Teste Padrão State
1. Acesse Nexo → Open Finance
2. Mude estado para "Instável"
3. Clique "Sincronizar" e observe comportamento diferente

### Teste Encapsulamento
- Tente acessar `conta._saldo` diretamente (protegido)

### Teste Categorização
- Adicione: "Uber 15km" → categoriza como "transporte"
- Adicione: "Supermercado" → categoriza como "alimentação"

---

## 💻 Tecnologias

- **Backend**: Python 3.8+, Flask 2.3.3
- **Banco**: SQLite com SQLAlchemy
- **Frontend**: HTML5 + Bootstrap 5 + JavaScript vanilla
- **Auth**: Werkzeug (hash seguro)
- **Sessions**: Flask-Session


---

## 🔮 Próximas Versões

- [ ] Gráficos avançados
- [ ] Integração real com Open Finance
- [ ] App Mobile (React Native)
- [ ] Machine Learning para previsões
- [ ] API REST pública

---

## 📄 Licença

MIT - Sinta-se livre para usar, aprender e compartilhar!

---

**Remanexo** © 2026
