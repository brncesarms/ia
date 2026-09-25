---
title: "Benchmark Modelos MoE - GEEKOM A7 MAX"
date_created: 2026-09-08
author: "Bruno César"
privacy: public
tags:
  - publico
  - ia
  - ollama
  - benchmark
  - moe
  - geekom
---

# 📊 Benchmark Modelos MoE — GEEKOM A7 MAX

Resultados de benchmark comparativo entre modelos Mistura de Especialistas (MoE) rodando local no **GEEKOM A7 MAX** (Ryzen 9 7940HS, 16 threads, 46 GiB RAM, **iGPU Radeon 780M via Vulkan**).

> ✅ **Todos os modelos rodam 100% na iGPU** (verificado via `ollama ps` em 08/09).

## 📦 Modelos Testados

| # | Modelo | Tamanho | Processador | Tags |
|---|--------|:-------:|:-----------:|------|
| 🥇 | `deepseek-coder-v2:16b` | 8.9 GB | 100% GPU | Código + Raciocínio (7.7B experts, 16B total) |
| 🥈 | `recall704/qwen:7b-moe-q4_k_m` | 4.8 GB | 100% GPU | Qwen MoE quantizado Q4_K_M |
| 🥉 | `sam860/olmoe-1b-7b-0924` | 3.0 GB | 100% GPU | OlMoE 1B-7B (1B ativo de 7B) |

> 💡 **Contexto MoE:** modelos Mistura de Especialistas ativam apenas uma fração dos parâmetros por token. Ex.: o `deepseek-coder-v2:16b` tem 16B total mas só ~2.4B ativos por token — maior eficiência computacional.

## 🖥️ Hardware (GEEKOM)

- **CPU:** AMD Ryzen 9 7940HS (8 cores / 16 threads)
- **RAM:** 46 GiB (44 GiB disponíveis)
- **GPU:** Radeon 780M (iGPU integrada, sem NVIDIA)
- **Ollama:** v0.33.3

## 📊 Resultados por Teste

### 🧪 Geração de Código (Python — fatorial recursivo)

| Modelo | Tempo | Tokens/s |
|--------|:-----:|:--------:|
| DeepSeek-Coder-V2:16b | 14.3s | 23.84 |
| OlMoE-1B-7B | 3.9s | **57.25** 🚀 |
| Qwen-7B-MoE | 15.5s | 12.28 |

### 🧪 Raciocínio Lógico (DeepSeek apenas)

| Modelo | Tempo | Tokens/s |
|--------|:-----:|:--------:|
| DeepSeek-Coder-V2:16b | 16.0s | 30.60 |

### 🧪 Explicação Técnica — Redes TCP/UDP (DeepSeek)

| Modelo | Tempo | Tokens/s |
|--------|:-----:|:--------:|
| DeepSeek-Coder-V2:16b | 25.0s | 39.36 |

### 🧪 Resumo de Texto (DeepSeek)

| Modelo | Tempo | Tokens/s |
|--------|:-----:|:--------:|
| DeepSeek-Coder-V2:16b | 7.8s | 39.33 |

### 🧪 Debug de Código Bash (DeepSeek)

| Modelo | Tempo | Tokens/s |
|--------|:-----:|:--------:|
| DeepSeek-Coder-V2:16b | 6.9s | 31.26 |

---

## 📈 Análise Comparativa

| Métrica | DeepSeek-16B | Qwen-7B-MoE | OlMoE-1B-7B |
|---------|:------------:|:-----------:|:-----------:|
| Tamanho | 8.9 GB | 4.8 GB | 3.0 GB |
| Velocidade média | ~32 tok/s | ~12 tok/s | **~57 tok/s** |
| Qualidade código | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Qualidade raciocínio | ⭐⭐⭐⭐⭐ | N/A | N/A |
| Uso de RAM (estimado) | ~9 GB | ~5 GB | ~3 GB |

### 🚀 Granite3-MoE (IBM) — medição 08/09 (reto, via api/generate)

> Adicionados hoje: `granite3-moe:1b` (A1B, 1.3B total) e `granite3-moe:3b` (A1B, 3.4B total). Ambos **MoE**, Q4_K_M, **100% GPU**, ctx 4096.

| Modelo | Alocado | ctx | tok/s (geração real) |
|--------|:-------:|:---:|:--------------------:|
| `granite3-moe:1b` | GPU | 4096 | **179.6** 🚀🚀 |
| `granite3-moe:3b` | 2.3 GB GPU | 4096 | **109.2** 🚀 |

> 📏 **Método:** `POST /api/generate` (stream=false, num_predict=300, tema redes TCP/UDP), lendo `eval_count`/`eval_duration` — **tok/s real de geração (decode)**, não estimado por caracteres.

### 🏆 Vencedores

- **Melhor qualidade geral:** `deepseek-coder-v2:16b` — melhor em código, raciocínio e respostas técnicas aprofundadas, com velocidade muito respeitável (~32 tok/s).
- **Mais rápido:** `granite3-moe:1b` — **179.6 tok/s** (recorde da GEEKOM até hoje), quase 3,2x o olmoe e ~15x o qwen-moe.
- **Veloz + pouco mais de qualidade:** `granite3-moe:3b` — 109.2 tok/s.
- **Meio-termo:** `recall704/qwen:7b-moe-q4_k_m` — mais lento que o esperado (~12 tok/s), possivelmente pela quantização Q4_K_M em sequência no modo MoE. Qualidade de código boa, mas performance decepcionante.

> ⚠️ **Limitação dos granites:** ctx de só **4096** tokens — insuficiente para logs de rede, scripts longos ou contextos técnicos profundos. Eles brilham em **chat/suporte instantâneo**, enquanto `qwen3-coder:30b` / `gemma4:26b` (256k ctx) seguem soberanos para trabalho técnico pesado.

## 💡 Observações

- ✅ Todos os 3 modelos MoE rodam **100% na iGPU Radeon 780M** (via Vulkan) — verificado com `ollama ps`.
- ✅ O DeepSeek-Coder-V2 (8.9 GB) carrega sem swap — consumo razoável.
- ✅ Todos os 3 modelos cabem simultaneamente na memória (8.9 + 4.8 + 3.0 = 16.7 GB).
- ❌ O modelo Qwen-7B-MoE teve a menor velocidade — contra intuitivo para um MoE menor; possível ineficiência da quantização Q4_K_M no mapeamento da iGPU.
- ⚠️ A arquitetura MoE (ativa só os experts necessários por token) é **especialmente eficiente em iGPU com memória compartilhada** — reduz a VRAM ativa e dá mais sobra para carregar modelos maiores.

## 🧠 Aprendizado: prefill vs decode (geraçao) na 780M

> 💡 **Confusão comum na web:** os "50+ tok/s" citados para a Radeon 780M costumam ser de **prefill** (processamento de prompt), que chega a ~300-530 tok/s. Já a **geraçao (decode)** — o que sentimos como velocidade de digitaçao — é menor. Na GEEKOM com config tuned (Vulkan + flash attention), medimos:
> - Modelos **A1B-class** (ativam ~1B/token): `granite3-moe:1b` **179 tok/s**, `3b` **109 tok/s** (decode real).
> - Modelos densos/médios MoE: ~9-57 tok/s (decode real).
>
> 🎯 **Conclusão:** a GEEKOM tuned **consegue sim passar de 50 tok/s de geraçao**, mas só em modelos **MoE muito leves (A1B)** com contexto curto. Para trabalho técnico de qualidade (256k ctx), os tok/s caem para a faixa de 20-35 — o que já é excelente para uso agêntico. 👇



## 🔗 Fontes

- [Ollama Biblioteca de Modelos](https://ollama.com/library)
- [DeepSeek-Coder-V2 (HuggingFace)](https://huggingface.co/deepseek-ai/DeepSeek-Coder-V2-Instruct)
- [OlMoE (AllenAI)](https://huggingface.co/allenai/olmoe-1b-7b-0125)

### Granite3-MoE (IBM) — fontes adicionadas 08/09
- [Granite3-MoE:1B — Ollama](https://ollama.com/ibm/granite3-moe:1b)
- [Granite3-MoE:3B — Ollama](https://ollama.com/ibm/granite3-moe:3b)
- [Granite 3 MoE (IBM) — HuggingFace](https://huggingface.co/ibm-granite/granite-3.0-3b-a400m-instruct)

---

## 🔗 Notas Relacionadas
- [Ranking Geral de Modelos de Linguagem](02_ranking_geral_modelos_llm.md) — Comparativo abrangente de modelos abertos densos e MoE.
- [Arquitetura RAG Nativo Local](05_arquitetura_rag_nativo_hermes_db.md) — Motor vetorial com sqlite-vec e FastEmbed no GEEKOM.
- [Acesso ao Ollama via Terminal SSH](03_ollama_acesso_terminal_ssh.md) — Execução e testes de inferência remota via linha de comando.
- [Guia Principal de Inteligência Artificial](README.md) — Índice de modelos, benchmarks e arquitetura de IA.
