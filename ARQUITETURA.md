---

# 🏛️ 4 PILARES DA POO - REMANEXO

Resumo breve e direto para estudar e apresentar os conceitos de POO presentes no código.

---

## 📍 Onde cada conceito é INVOCADO (onde o professor pergunta)

| Pilar | Onde está | Quando é invocado |
|-------|-----------|-------------------|
| **Abstração** | `backend/models/transacao.py` (linhas 1-19) | Quando `ReceitaModel` e `DespesaModel` herdam de `TransacaoModel` — são OBRIGADAS implementar `calcular_impacto_saldo()` |
| **Herança** | `ReceitaModel(TransacaoModel)` | `super().categorizar()` chama método da classe pai; herda campos `descricao`, `valor`, `data` |
| **Encapsulamento** | `backend/database.py` (UsuarioModel, ContaModel) | Quando `conta.saldo = valor` usa setter (linha 137 em dashboard.py) |
| **Polimorfismo** | `backend/routes/dashboard.py` (linhas 133-135) | Loop: `for tx in todas_transacoes: saldo_total += tx.calcular_impacto_saldo()` — sem if/else |

---

## 1️⃣ ABSTRAÇÃO — o contrato obrigatório

**Arquivo:** `backend/models/transacao.py` linhas 1-19

classe base define o que é obrigatório. subclasses não podem ignorar.

```python
class Transacao(ABC):
    @abstractmethod
    def calcular_impacto_saldo(self):
        pass
```

**invocado quando:** `Receita` herda de `Transacao` → Python força implementar `calcular_impacto_saldo()`

apresente assim: "classe abstrata garante contrato: toda transação deve saber seu impacto no saldo"

---

## 2️⃣ HERANÇA — reutilizar estrutura, especializar comportamento

**Arquivo:** `backend/database.py` linhas 135-165

```python
class ReceitaModel(TransacaoModel):  # ← herança
    def calcular_impacto_saldo(self):
        return self.valor if self.status == 'ativa' else 0

class DespesaModel(TransacaoModel):  # ← herança
    def calcular_impacto_saldo(self):
        return -self.valor if self.status == 'ativa' else 0
```

**invocado quando:** 
- `super().categorizar()` em `backend/models/transacao.py` (chama método da classe pai)
- herança de campos comuns: `descricao`, `valor`, `data`, `status`

apresente assim: "receita e despesa herdam tudo igual, mas cada uma sabe seu jeito"

---

## 3️⃣ ENCAPSULAMENTO — dados protegidos sob controle

**Arquivo:** `backend/database.py` (UsuarioModel, ContaModel, InvocaçãoModel)

setter garante validação centralizada:

```python
# em ContaModel:
conta.saldo = 5000      # ✓ seguro — passa por setter
# nunca: conta._saldo = 5000  ✗ quebra lógica

# em UsuarioModel (senha nunca plain text):
usuario.definir_senha("abc123")  # usa Werkzeug hash
usuario.verificar_senha("abc123")
```

**invocado quando:**
- `backend/routes/dashboard.py` linha 137: `conta.saldo = saldo_total` (setter)
- `backend/routes/nexo.py` linha 250: `conta.saldo = valor` (setter)
- `if self.status == 'ativa'` dentro de `calcular_impacto_saldo()` — status é verificado internamente

apresente assim: "saldo não é acessado direto; setter valida mudanças"

---

## 4️⃣ POLIMORFISMO — mesmo método, respostas diferentes

**Arquivo:** `backend/routes/dashboard.py` linhas 133-135

```python
saldo_total = 0
for tx in todas_transacoes:
    if tx.status == 'ativa':
        saldo_total += tx.calcular_impacto_saldo()  # ← polimorfismo aqui
conta.saldo = saldo_total
```

**invocado quando:**
- `tx` pode ser `ReceitaModel` (retorna +valor) ou `DespesaModel` (retorna -valor)
- **sem if/else** — Python descobre automaticamente qual método chamar
- cada objeto responde de forma apropriada ao mesmo método

apresente assim: "não perguntamos 'que tipo é você?' — só chamamos `calcular_impacto_saldo()` e cada um responde certo"

-