# ⛩️ Amaterasu — Otimizador de PC para Jogos

Otimizador para Windows 10/11 focado em **PC fraco e jogos**: limpa o sistema,
desliga processos e serviços inúteis, aplica plano de energia de alto desempenho
e ajustes de rede — **sem alterar a configuração interna dos jogos** e criando um
**ponto de restauração antes** de mexer em qualquer coisa (tudo reversível).

> ⚠️ Não é antivírus e não substitui um. É uma ferramenta de desempenho.

---

## ✨ O que ele faz

- **Limpeza:** temporários, cache, prefetch, miniaturas, logs e lixeira.
- **Desempenho:** plano de energia máximo, efeitos visuais desligados, Game Mode,
  prioridade de GPU.
- **Serviços/Inicialização:** desativa serviços pesados (de forma seletiva e segura)
  e gerencia o que abre junto com o Windows.
- **Rede:** limpa DNS, troca para DNS mais rápidos (Cloudflare/Google) e reduz latência.
- **Reparo:** executa `SFC` + `DISM` em segundo plano.
- **Segurança:** cria ponto de restauração antes de aplicar. Reversível.

---

## 💰 Planos

| Plano | Preço | Conteúdo |
|------|------|----------|
| **Básico** | R$ 39,90 | Painel de 1 clique ("Otimizar agora"), FPS Booster, limpeza, plano de energia, modo automático |
| **PRO** | R$ 59,90 | Tudo do Básico + **Painel Avançado** (41 ajustes selecionáveis: serviços, apps, inicialização, rede/DNS, reparo do Windows) + suporte prioritário |

Cada plano tem **seu próprio instalador** para que o Básico não entregue o painel Avançado.

---

## 📁 Estrutura do repositório

```
.
├── otimizador/                     # código-fonte dos painéis (PowerShell)
│   ├── Otimizador-Painel.ps1       # painel principal "1 clique" + FPS Booster
│   ├── Amaterasu-Avancado.ps1      # painel avançado (41 ajustes, categorias)
│   ├── Otimizar-PC.ps1             # motor headless (usado pelo modo automático)
│   ├── Abrir-Otimizador.vbs        # atalho que abre o painel sem janela de CMD
│   ├── Abrir-Amaterasu-Avancado.vbs
│   ├── bg.jpg / sharingan.* / eye-logo.png   # ARTES (ver aviso legal abaixo)
│   └── COMO-USAR.md
│
├── instalador/                     # instalador do plano PRO (Inno Setup)
│   ├── Amaterasu.iss               # script do instalador -> AmaterasuSetup.exe
│   ├── COMO-GERAR-O-EXE.md
│   └── app/                        # arquivos empacotados no instalador PRO
│
├── instalador-basico/              # instalador do plano Básico
│   ├── Amaterasu-Basico.iss        # -> AmaterasuBasicoSetup.exe
│   └── app/                        # apenas o painel de 1 clique
│
└── site/
    └── index.html                  # loja (2 planos, carrinho, pagamento Pix)
```

---

## ▶️ Como rodar (desenvolvimento)

Requer **Windows 10/11** com PowerShell 5.1 (já vem no sistema). Como os scripts
mexem no sistema, precisam de **administrador**.

- Jeito recomendado: **duplo clique** em `Abrir-Otimizador.vbs`
  (ou `Abrir-Amaterasu-Avancado.vbs`). O `.vbs` já pede elevação e abre sem a
  janela preta do CMD.
- Pela linha de comando:
  ```powershell
  powershell -NoProfile -ExecutionPolicy Bypass -File ".\otimizador\Otimizador-Painel.ps1"
  ```

Modo simulação (não aplica nada, só mostra o que faria):
```powershell
powershell -ExecutionPolicy Bypass -File ".\otimizador\Otimizar-PC.ps1" -Simular
```

---

## 📦 Como gerar o instalador (.exe)

1. Instale o **Inno Setup** (grátis): https://jrsoftware.org/isdl.php
2. Abra `instalador/Amaterasu.iss` (PRO) ou `instalador-basico/Amaterasu-Basico.iss` (Básico).
3. Menu **Build → Compile** (ou **F9**).
4. O `.exe` sai na pasta do `.iss` (`AmaterasuSetup.exe` / `AmaterasuBasicoSetup.exe`).

> O instalador não é assinado digitalmente, então o Windows mostra o aviso
> **SmartScreen** ("editor desconhecido"). Para remover de vez, é preciso um
> **certificado de assinatura de código** (pago). Antivírus também podem dar
> falso-positivo com otimizadores — assinar o código ajuda.

---

## 🛒 Site de vendas (`site/index.html`)

Página única com cabeçalho, dois planos, carrinho e **pagamento via Pix** (QR Code
+ copia e cola) gerado no padrão BR Code do Banco Central, apontando para a chave
Pix do Mercado Pago.

Editar no topo do `<script>`:
- `BRAND_LOGO` / `HERO_BG` — sua logo e imagem de fundo (URL ou data URI).
- `PIX` — sua chave, nome e cidade.
- `PRODUTOS` — preços.
- Contatos (WhatsApp, Discord, e-mail) no HTML da seção **Suporte**.

**Entrega:** o site gera o Pix, mas não confirma o pagamento sozinho (é estático).
O fluxo atual é: cliente paga → envia comprovante → você libera o link. Para
entrega **100% automática**, é preciso um backend com a **API do Mercado Pago**
(cria a cobrança e recebe o webhook de "pago"). Veja Segurança abaixo.

---

## 🔒 Segurança — leia antes de subir no GitHub

- ✅ **Chave Pix pode ficar no código.** Ela só **recebe** dinheiro; ninguém saca
  nem acessa sua conta com ela. Está no `index.html` sem problema.
- ⛔ **Nunca** faça commit do **Access Token / chave secreta de API do Mercado Pago**
  (nem client secret, senhas ou tokens). Isso dá poder sobre a conta.
- Quando houver backend, guarde os segredos em **variáveis de ambiente** / arquivo
  `.env` e mantenha-o **fora do Git**.

`.gitignore` recomendado:
```gitignore
.env
.env.*
*.log
# saídas de build do Inno Setup
instalador/Output/
instalador/*.exe
instalador-basico/Output/
instalador-basico/*.exe
# segredos
secrets.*
*.key
```

---

## ⚖️ Avisos legais

- **Artes de terceiros:** quaisquer imagens de personagens/símbolos de obras com
  direitos autorais (ex.: arte de anime) **não podem ser usadas em produto à venda**.
  Substitua `bg.jpg` e as logos por **arte original ou licenciada** antes de vender.
  O `site/index.html` já usa uma identidade original (olho em brasa) como padrão.
- **Uso por conta e risco:** o software altera configurações do Windows. Ele cria
  ponto de restauração antes, mas o usuário é responsável por rodar com cuidado.
- Produto digital: defina sua política de **trocas/reembolso** e exiba-a na loja.

---

## 📞 Suporte

- WhatsApp: _(defina)_
- Discord: _(defina)_
- E-mail: _(defina)_

---

## 📄 Licença

Projeto proprietário. © Amaterasu Otimizador. Todos os direitos reservados.
Defina aqui os termos de uso/EULA que acompanham a venda.
