# 🧠 Arquitetura — PDV System

## 📌 Visão geral

O PDV System é uma aplicação distribuída composta por frontend em Vue.js e backend em Laravel, comunicando-se via API REST.

A arquitetura foi projetada para suportar operações de venda em tempo real, controle de estoque e gestão multi-loja. 

---

## 🎯 Objetivos arquiteturais

* suportar operações rápidas de venda
* garantir consistência de estoque
* permitir múltiplos pontos de venda
* separar frontend e backend
* escalar para múltiplos usuários simultâneos

---

## 🧩 Arquitetura do sistema

### 🖥️ Frontend (PDV)

* interface de operação de caixa
* gerenciamento de estado com Pinia
* persistência local (carrinho, PDV ativo)
* comunicação com API via Axios

---

### 🛠️ Backend (API + Admin)

* API REST versionada (/api/v1)
* autenticação via Sanctum
* painel administrativo com Filament
* controle de usuários e permissões

---

## 🏗️ Arquitetura em camadas

### Camada de apresentação

* interface PDV (Vue)
* painel administrativo

---

### Camada de aplicação

* processamento de vendas
* gerenciamento de pedidos
* controle de clientes

---

### Camada de domínio

* Order (pedido)
* Product (produto)
* Pdv (ponto de venda)
* Customer (cliente)
* StockMovement (movimentação)

---

### Camada de dados

* persistência de pedidos
* controle de estoque por PDV
* histórico de movimentações

---

## 📦 Controle de estoque

O sistema utiliza modelagem baseada em pivot:

* tabela `pdv_product` armazena estoque por PDV
* movimentações registradas em `stock_movements`
* venda gera saída automática
* cancelamento gera entrada automática

---

## 🔄 Fluxo de venda

1. operador seleciona produtos
2. sistema calcula subtotal e total
3. pedido é enviado para API
4. backend cria order e order_items
5. estoque é reduzido
6. movimentação é registrada

---

## 🔁 Reversão de venda

* cancelamento de pedido
* reposição automática de estoque
* criação de movimentação inversa

---

## 🔐 Autenticação e acesso

* login via API (/login)
* token via Sanctum
* controle por tipo de usuário:

  * admin
  * owner
  * seller

---

## 🏬 Multi-PDV

* cada PDV representa uma unidade de venda
* produtos vinculados a PDVs
* estoque independente por unidade
* usuários associados a PDVs

---

## 📊 Dashboard administrativo

* vendas agregadas
* status de pedidos
* produtos com baixo estoque
* histórico operacional

---

## ⚙️ Processamento e consistência

* transações no backend para vendas
* validação de estoque antes da venda
* logs de movimentação

---

## 📈 Escalabilidade

A arquitetura permite evolução para:

* múltiplos caixas simultâneos
* integração com meios de pagamento
* emissão fiscal
* relatórios financeiros avançados

---

## 💡 Decisões arquiteturais

* frontend desacoplado para flexibilidade
* API REST como núcleo da aplicação
* estoque baseado em pivot
* controle de consistência via backend

---

## 🧾 Conclusão

O PDV System foi projetado para suportar operações comerciais reais, com foco em desempenho, consistência e escalabilidade. A arquitetura permite evolução para ambientes de produção com múltiplos pontos de venda.
