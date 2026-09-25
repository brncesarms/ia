---
title: "Ranking Geral de Modelos — GEEKOM"
date_created: 2026-09-08
date_updated: 2026-09-08
tags:
  - ia
  - ollama
  - benchmark
  - ranking
  - geekom
---

# 🏆 Ranking Geral de Modelos — GEEKOM A7 MAX

Consolidado de todos os benchmarks executados na GEEKOM (06/09 a 08/09/2026), todos rodando **100% na iGPU Radeon 780M** via Vulkan (Ollama com `OLLAMA_IGPU_ENABLE=1`, `OLLAMA_VULKAN=1`).

> ✅ **Todos os modelos rodam na iGPU** (verificado via `ollama ps`) — comparação de velocidade é justa entre todos.

## 🥇 Ranking Geral de Velocidade (tok/s)

> 🎯 **Coluna "Atende currículo"** considera as competências do Bruno no ME.md: 🌐 **Redes** (Mikrotik/Ubiquiti) · 🐧 **Linux** · 🪟 **Windows** · 🛠️ **Infra/scripts** · 🩺 **Suporte/diagnóstico**.
> Legenda da nota: 🥇 ótimo · ✅ bom · ⚠️ mediano · ❌ ruim
> 🧩 **Coluna "MoE?":** ✅ = Mixture of Experts · ❌ = Dense. *(Arquitetura confirmada por `ollama show` + documentação oficial.)*
> 📏 **ctx:** contexto máximo do modelo. **Regra (08/09): mínimo 32k** — removidos do ranking os modelos com teto nativo 4k (inviáveis p/ técnico longo).

| # | Modelo | MoE? | Tamanho | Alocado | ctx | tok/s | Atende currículo |
|---|--------|:----:|:-------:|:-------:|:---:|:-----:|------------------|
| 1 | `qwen3-coder:30b` | ✅ | 18 GB | 21 GB | 256k | 33.4–35.5 | 🥇 🐧🪟🛠️ Scripts bash/PowerShell, automação, infra |
| 2 | `laguna-xs-2.1-64k` | ✅ | 20 GB | 20 GB GPU | 32k | 37.7 | ✅ 🌐🐧🪟 Roda 100% GPU a 32k (reabilitado!) · a 64k estoura memória → usá-lo com ctx ≤32k |
| 3 | `qwen3.6:35b-a3b` | ✅ | 23 GB | 22 GB GPU | 262k | 31.6–32.4 | ⚠️ 🤖 **Leitura rápida/chat** (70-80s vs 206s); ❌ **trava na escrita agêntica** (loop no OpenCode >25min) |
| 4 | `deepseek-coder-v2:16b` | ✅ | 8.9 GB | 11–27 GB | **160k** | 32–49.3 | 🥇 🐧🪟🛠️ Código/config + raciocínio técnico (redes); ctx 160k nativo |
| 5 | `gemma4:26b` | ✅ | 18 GB | ~2 GB | 256k | 25.3 | 🥇 🌐🐧🪟🩺 Equilíbrio total — **padrão do dia a dia + OpenCode** |
| 6 | `gpt-oss:20b` | ✅ | 13 GB | 12G | 32k | 20.4 | ✅ 🩺🌐 Suporte/diagnóstico + assuntos técnicos gerais |
| 7 | `qwen2.5-coder:7b` | ❌ | 4.7 GB | 13G | 32k | 12.1–17.7 | ⚠️ 🐧 Scripts simples; limitado p/ assuntos complexos |
| 8 | `recall704/qwen:7b-moe-q4_k_m` | ✅ | 4.8 GB | 21 GB | 32k | ~12 | ❌ Decepcionante — descartado |
| 9 | `qwen3.6:27b` | ❌ | 17 GB | 20G | 256k | 9.4 | ✅ 🌐🧠 Raciocínio profundo (redes/segurança) p/ tarefa única |
| 10 | `qwen3:14b` | ❌ | 9.3 GB | — | 256k | sem bench | ⚠️ Candidato sem benchmark registrado |

> ⚠️ **`laguna-xs-2.1`: atenção ao contexto!** Medido em 08/09: com **32k** ele carrega **100% GPU em 20 GB** (37.7 tok/s) — foi **reabilitado**. O problema ocorre com **64k+** (estoura a memória da iGPU → trashing). **Use `laguna` sempre com ctx ≤32k.**

> 💡 **Medição histórica (08/09) — granite3-moe:** os 2 modelos IBM bateram recorde de **tok/s real de geração** na GEEKOM (via `api/generate`: eval_count/eval_duration): **1b → 179.6 tok/s**, **3b → 109.2 tok/s**, ambos 100% GPU. São A1B-class (ativam ~1B/token) — confirma que MoE **A1B leve bate 100+ tok/s** nessa iGPU. ⚠️ **Mas ctx = 4096** (inviabiliza técnico longo) → **removidos do ranking (regra ctx ≥32k)**. O registro fica como dado de benchmark.

### 📊 Benchmark com contexto 32k (08/09)

> Pedido: refazer o benchmark dos 5 primeiros com `num_ctx=32768`. Resultado honesto:

| Modelo | tok/s (ctx 32k) | Alocação | Processador | ctx real | Observação |
|--------|:---------------:|:--------:|:-----------:|:--------:|------------|
| `granite3-moe:1b` | 179.1 | 1.1 GB | 88% GPU | **4096** ⚠️ | Ignora 32k — teto nativo 4096 |
| `granite3-moe:3b` | 109.9 | 2.3 GB | 100% GPU | **4096** ⚠️ | Ignora 32k — teto nativo 4096 |
| `sam860/olmoe-1b-7b-0924` | 114.2 | 3.6 GB | 100% GPU | **4096** ⚠️ | Ignora 32k — teto nativo 4096 |
| `qwen3-coder:30b` | **35.5** | 21 GB | 100% GPU | **32768** ✅ | Aceita 32k; aloc. 24G→21G |
| `laguna-xs-2.1:latest` | **37.7** | 20 GB | 100% GPU | **32768** ✅ | Aceita 32k; roda 100% GPU (reabilitado) |

> ⚠️ **Limitação (motivo da remoção):** `granite3-moe:1b`, `granite3-moe:3b` e `olmoe-1b-7b` têm **contexto máximo nativo de 4096** — mesmo pedindo `num_ctx=32768`, o Ollama mantém 4096. Eles **não suportam 32k por arquitetura** → **eliminados do ranking (regra ctx ≥32k)**. Já `qwen3-coder:30b` e `laguna-xs-2.1` carregam 32k normalmente em 100% GPU (21 GB e 20 GB).
>
> 💡 **Descoberta (08/09):** o `laguna-xs-2.1`, antes aposentado por "rodar em CPU", na verdade **roda 100% GPU em 20 GB com ctx ≤32k** (37.7 tok/s, teste isolado confirmado). O trashing acontecia só com **64k+**. **Reabilitado** — basta usá-lo com ctx ≤32k.

### 🤖 Benchmark `qwen3.6:35b-a3b` — leitura rápida (⚠️ falhou na escrita agêntica) (08/09)

> Pedido: explorar um modelo MoE agêntico que supere o `gemma4:26b` para o OpenCode. Baixado e testado (23 GB, Apache 2.0).

| Critério | Valor | Observação |
|----------|:-----:|------------|
| Arquitetura | **MoE A3B** | qwen35moe — 35.5B total / 3B ativos |
| ctx nativo | **262144** (262k) | ✅ folga enorme p/ requisito 64k do OpenCode |
| Quantização | Q4_K_M | — |
| Alocação | 22 GB | 100% GPU na iGPU |
| tok/s (32k) | **32.4** | vs 25.3 do gemma4 |
| tok/s (64k) | **31.6** | ❌ ainda 100% GPU a 64k! |
| Tool-calling | ✅ **VALIDADO** | Teste: `get_weather`, extraiu `city:"Ariquemes"` corretamente |
| Raciocínio | 77.2% SWE-bench | Superior (gemma4 não pública esse score) |
| Foco | Agêntico (tools+thinking) | Feito para agent/OpenCode |

> 🎯 **Veredito:** no **benchmark isolado** o `qwen3.6:35b-a3b` supera o `gemma4:26b` em tok/s (32.4 vs 25.3) com 262k ctx e raciocínio superior, 100% GPU a 64k (22 GB). **MAS no teste agêntico REAL no OpenCode ele TRAVOU em loop (>25min)** numa tarefa de escrita — o gemma4:26b concluiu em ~4min. **Benchmark de tok/s NÃO prevê comportamento agêntico** → decisão: gemma4 segue campeão do OpenCode; qwen3.6 vira opção de **leitura rápida** (70-80s vs 206s na leitura).

### 📊 Resumo MoE vs Dense

| Arquitetura | Qtde | Modelos |
|:-----------:|:----:|---------|
| ✅ MoE | 7 | qwen3-coder · laguna · qwen3.6:35b-a3b · deepseek-coder-v2 · gemma4 · gpt-oss · qwen:7b-moe |
| ❌ Dense | 3 | qwen2.5-coder · qwen3.6:27b · qwen3 |

> 🔬 **Nota (08/09):** `granite3-moe:1b`, `granite3-moe:3b` e `olmoe-1b-7b` foram **removidos do ranking** por terem **contexto nativo de 4k** (abaixo do mínimo de 32k) — não atendem usos técnicos longos. Modelos restantes têm **ctx ≥32k**.

### 🎯 Melhores do currículo por especialidade

| Especialidade (ME.md) | 🥇 Modelo principal | 🥈 Alternativa |
|------------------------|:-------------------:|:--------------:|
| 🌐 Redes (Mikrotik/Ubiquiti, roteamento) | `gemma4:26b` | `deepseek-coder-v2:16b` |
| 🐧 Linux (admin/scripts/bash) | `qwen3-coder:30b` | `deepseek-coder-v2:16b` |
| 🪟 Windows (PowerShell/Task Scheduler) | `qwen3-coder:30b` | `deepseek-coder-v2:16b` |
| 🛠️ Infraestrutura (montagem/automação) | `gemma4:26b` | `qwen3-coder:30b` |
| 🩺 Suporte técnico (diagnóstico) | `gemma4:26b` | `gpt-oss:20b` |
| ⚡ Chat/suporte rápido | `laguna-xs-2.1` | `gpt-oss:20b` |

## 📈 Ranking por Qualidade/Eficiência

| Critério | 🥇 Vencedor | Motivo |
|----------|:-----------:|--------|
| 🚀 Velocidade bruta | `laguna-xs-2.1` | 37.7 tok/s (100% GPU a 32k) |
| 👨💻 Melhor p/ código | `qwen3-coder:30b` | 33.4 tok/s, 100% GPU, 256K ctx |
| 🎯 Padrão atual do OpenCode | `gemma4:26b` | MoE A4B esparso, ~2GB VRAM, conclui tarefas agênticas |
| 🧠 Raciocínio/qualidade | `qwen3.6:27b` | 68.9% SWE-bench (mas 9 tok/s) |
| ⚡ Melhor eficiência VRAM | `gemma4:26b` | só ~3,3GB VRAM ativa |
| 💪 Código iGPU (MoE) | `deepseek-coder-v2:16b` | ~32-49 tok/s, ctx 160k |

## 🎯 Conclusões

### 🥇 Campeão para uso agêntico (OpenCode): **`gemma4:26b`** 👑
- **MoE A4B**, ~2 GB VRAM ativa, 256k ctx, tool-calling validado.
- **Teste real no OpenCode (08/09):** conclui tarefas de escrita em ~4min — confiável, segue convenções (emojis + dentro do cofre).
- **Motivo da decisão:** o `qwen3.6:35b-a3b` parecia superior no benchmark isolado (32.4 vs 25.3 tok/s), **mas no teste agêntico REAL travou em loop (>25min)** em tarefa de escrita. Benchmark de tok/s NÃO prevê comportamento agêntico.
- ⚠️ **Lição registrada:** sempre medir velocidade de execução e capacidade de CONCLUIR, não só tok/s (ver `guia-ia-local/tests/resultados-velocidade.md`).

### 🥈 Leitura rápida (alternativa): **`qwen3.6:35b-a3b`**
- Ótimo para **chat/leitura rápida** (70-80s vs 206s do gemma4 na leitura), 262k ctx.
- ⚠️ **NÃO recomendado para escrita agêntica** (trava em loop no OpenCode).

### 🎯 Melhor combinação para o currículo do Bruno
Para **Analista de TI Pleno** (redes + Linux/Windows + infra + suporte), a dupla ideal:

| Fluxo de trabalho | 🥇 Modelo | Por quê |
|-------------------|:---------:|---------|
| 🐧🪟 Criar/editar scripts (bash/PowerShell), automação | `qwen3-coder:30b` | Melhor em código, 33.4 tok/s, rápido o bastante p/ iterar |
| 🌐 Investigar roteamento/redes (Mikrotik/Ubiquiti), suporte | `gemma4:26b` | Equilíbrio técnico + velocidade + eficiência VRAM |
| 🛠️ Diagnóstico e instruções de infra | `gemma4:26b` (padrão) | 256k ctx p/ contextos grandes de rede/logs |
| 💬 Chat/leitura rápida | `qwen3.6:35b-a3b` ou `laguna-xs-2.1` | Leitura veloz; ⚠️ qwen3.6 só p/ leitura |

> 💡 **Sugestão de uso prático:** `gemma4:26b` como padrão do OpenCode, `qwen3-coder:30b` para tarefas de script pesado, `laguna-xs-2.1` para chat rápido (ctx ≤32k) e `qwen3.6:35b-a3b` como alternativa de leitura rápida.

### ⚡ Novos MoE (08/09) — onde se encaixam
- **`qwen3.6:35b-a3b`**: leitura rápida e raciocínio superior (262k ctx); ⚠️ **falhou na execução agêntica do OpenCode** (loop) — não usar p/ escrita agêntica.
- **`deepseek-coder-v2:16b`**: excelente qualidade de código na iGPU (~32-49 tok/s), **ctx nativo 160k** — boa alternativa ao gemma4 para tarefas de código pesadas.
- **`recall704/qwen:7b-moe-q4_k_m`**: descartado para uso prático (12 tok/s, decepcionante) — manter apenas para experimento.
- ❌ **Removidos (ctx 4k < 32k):** `granite3-moe:1b`, `granite3-moe:3b`, `olmoe-1b-7b` — ótimos em tok/s (57-179) mas contexto 4k inviabiliza técnico longo.

> 🎯 **Busca +50 tok/s com contexto útil:** os modelos de 4k (granite/olmoe) estouravam 50+ tok/s mas com ctx 4k. Com **ctx ≥32k**, o mais rápido é `laguna-xs-2.1` (37.7 tok/s) — **esse é o teto prático com contexto útil** na GEEKOM para geração. Prefill rápido (config tuned: Vulkan + flash attention + iGPU enable) garante bom throughput nos modelos grandes de 256k ctx (qwen3-coder/gemma4).

## 📜 Regra: Só Testar Modelos MoE (decidido em 08/09/2026)

> **Regra definida pelo Bruno:** daqui pra frente, **apenas modelos MoE (Mixture of Experts)** serão testados na GEEKOM.

### 💡 Por quê (contexto técnico completo)

> [!IMPORTANT] Resumo honesto
> **MoE NÃO é "melhor" em absoluto — é a melhor escolha para a GEEKOM.** E é o **padrão da indústria** para modelos grandes em 2026. O que torna MoE ideal aqui é a combinação específica do hardware: **muita memória (46G unified) + pouco compute (iGPU integrada)**.

#### A troca fundamental (MoE vs Dense)

| Aspecto | Dense | MoE |
|---------|:-----:|:---:|
| ⚡ Compute por token | Alto (ativa tudo) | **Baixo** (só k/N experts) ✅ |
| 💾 Memória necessária | Igual ao ativo | **Total de TODOS os experts** ❌ |
| 🧠 Capacidade de conhecimento | Limitada ao tamanho | Gigantesca (total) ✅ |
| 🧩 Qualidade por FLOP | Menor | Maior ✅ |
| 🎛️ Estabilidade de treino | Simples | Rotulação precisa de cuidado ⚠️ |

**O ponto crucial** (fonte: Vibe Engines / DIT):
> *"MoE salva compute (k/N), NÃO memória. Você carrega TODOS os experts na memória, mesmo ativando só alguns por token."*

Exemplo: `deepseek-coder-v2:16b` ativa ~2.4B por token, mas carrega os **15.7B completos** (8.9 GB). Um `qwen3.6:27b` denso carrega 27B e ativa 27B.

#### ✅ Por que MoE vence NA GEEKOM
A GEEKOM tem **iGPU Radeon 780M com memória compartilhada (46GB RAM)**:
- 💾 Memória é **abundante** (46G unified) → a fraqueza do MoE (precisa de muita memória) **some/é absorvida**.
- ⚡ Compute é **o gargalo** (iGPU integrada) → a força do MoE (pouco compute/token) é **exatamente o que falta**.
- 🎯 Resultado: capacidade de modelo grande com velocidade de modelo pequeno.

É por isso que o ranking mostra os MoE na frente: qwen3-coder (33.4), laguna (37.7), deepseek (32-49), gemma4 (25.3) — todos bem acima dos dense (qwen3.6: 9.4, qwen2.5-coder: ~15).

#### ⚠️ Quando MoE NÃO é a melhor escolha
- **Dispositivos com pouca memória** (celular, laptop 8GB): denso pequeno vence — não cabe o total dos experts.
- **GPU dedicada grande com sobra de compute**: não há o que economizar no compute.
- **Tarefas muito pequenas/repetitivas**: um denso pequeno basta e é mais simples.

#### ▶️ Conclusão da regra
> Regra **tecnicamente correta para o hardware da GEEKOM**: memória é farta, compute é escasso, e MoE otimiza exatamente esse perfil. Decisão sábia. Em 2026, quase todo modelo de fronteira é MoE.

### 🥇 O padrão da indústria em 2026
Praticamente **todo modelo de fronteira usa MoE**: GPT-5/GPT-OSS, DeepSeek V3/V4, Llama 4, Qwen3/3.5, Gemma 4 (26B A4B), Gemini 3, Claude Opus, Mistral Large 3, Grok, Kimi K2. Modelos **densos puros acima de 70B viraram a exceção**.

### 📋 Procedimento ao avaliar modelo novo
1. 🧩 Conferir que é **MoE** (via `ollama show` → architecture, ou doc oficial). Se for Dense → **não avaliar** (a regra manda só MoE).
2. 📦 Baixar no GEEKOM (`ollama pull`).
3. 🧪 Rodar o benchmark (`benchmark-moe.sh` ou `benchmark-modelos.sh`).
4. 🎯 Registrar no benchmark + ranking, marcando a coluna MoE? = ✅.

### 🥉 Modelos Dense já instalados — status
| Modelo Dense | Status |
|--------------|--------|
| `qwen3:14b` | ⚠️ Mantido por ora (sem bench); não será prioridade de teste |
| `qwen3.6:27b` | ⚠️ Mantido pela qualidade de raciocínio (uso pontual, não padrão) |
| `qwen2.5-coder:7b` | ⚠️ Mantido p/ tarefa simples de chat |

> Os 3 Dense continuam instalados, mas **não receberão novos testes/prioridade**. A mesa de testes fica reservada aos MoE.

## 📂 Onde está cada nota

- 📊 Benchmark MoE completo: [IA/benchmark-moe-geekom.md](./benchmark-moe-geekom.md)
- 🧪 Benchmark iGPU (06/09): [`../../guia-ia-local/benchmarks/BENCHMARKS.md`](../../guia-ia-local/benchmarks/BENCHMARKS.md)
- 📈 Histórico de benchmarks: [`../../guia-ia-local/benchmarks/HISTORICO.md`](../../guia-ia-local/benchmarks/HISTORICO.md)

## 🔗 Notas Correlatas

- 🤖 [IA Local — Índice](./README.md)
- 📊 [Benchmark Modelos MoE](./benchmark-moe-geekom.md)

---

## 🔗 Fontes

### Benchmarks (dados locais)
- Benchmark iGPU 06/09: `guia-ia-local/benchmarks/BENCHMARKS.md` + `HISTORICO.md`
- Benchmark MoE 08/09: `t.i/IA/benchmark-moe-geekom.md`

### Arquitetura MoE vs Dense (pesquisa 2026)
- [MoE é o padrão de fronteira em 2026 — Ertas AI](https://www.ertas.ai/blog/mixture-of-experts-in-2026)
- [Arquiteturas de fronteira 2026: MLA, iRoPE, mHC — largo.dev](https://largo.dev/articles/frontier-llm-architectures-2026/)
- [MoE salva compute, não memória — Vibe Engines](https://vibeengines.com/handbook/moe-vs-dense-models)
- [MoE: o trade-off VRAM vs compute — DIT](https://dintechnologies.com/blog/2026-quantization-moe-efficiency-memory-optimization)
- [Quantização e MoE na prática — DIT](https://dintechnologies.com/blog/2026-quantization-moe-efficiency-memory-optimization)
- [Review abrangente de MoE — arXiv 2507.11181](https://arxiv.org/html/2507.11181v2)
- [Dense Training, Sparse Inference — arXiv 2404.05567](https://arxiv.org/abs/2404.05567)

### Especificações oficiais
- [GPT-OSS-20B (MoE 20.9B/3.6B ativos) — OpenAI/NVIDIA](https://build.nvidia.com/openai/gpt-oss-20b)
- [Gemma 4 26B-A4B (MoE 128 experts) — HuggingFace](https://huggingface.co/google/gemma-4-26B-A4B-it)
- [Laguna XS 2.1 (MoE 33B/3B) — Ollama](https://ollama.com/library/laguna-xs-2.1)
- [Qwen3.6 35B-A3B (MoE agêntico, 262k ctx) — Ollama](https://ollama.com/library/qwen3.6:35b-a3b)
- [Ollama Biblioteca de Modelos](https://ollama.com/library)
