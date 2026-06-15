# 💜 Seguras 2.0 — Chatbot contra el ciberacoso (RAG + LLM)

Actividad en equipos · **Procesamiento de Lenguaje Natural**
Maestría en Inteligencia Artificial Aplicada — Tecnológico de Monterrey
Prof. Luis Eduardo Falcón Morales

---

## 📌 Descripción

**Seguras 2.0** es un chatbot conversacional que acompaña y orienta a personas que
enfrentan **ciberacoso o violencia digital** en México. Combina **Recuperación Aumentada
Generativa (RAG)** con un **Modelo de Lenguaje de Gran Tamaño (LLM)** para dar respuestas
empáticas, accionables y **fundamentadas en documentos curados en español**, reduciendo las
alucinaciones típicas de un LLM por sí solo.

El proyecto evoluciona un trabajo previo del equipo, *Seguras1* (un clasificador clásico de
acoso sí/no), que aquí se **reutiliza como una herramienta interna** del chatbot.

## 🧠 Arquitectura

| Componente | Tecnología |
|---|---|
| Detector de acoso (señal interna) | TF-IDF + Regresión Logística |
| Embeddings multilingües | `paraphrase-multilingual-MiniLM-L12-v2` |
| Recuperador (RAG) | FAISS (similitud coseno) |
| LLM | `Qwen2.5-3B-Instruct` cuantizado en **NF4** (estilo QLoRA) |
| Interfaz | Gradio |

```
Mensaje → [Detector] + [RAG: FAISS sobre base de conocimiento] → [Prompt] → [LLM NF4] → Respuesta
```

## 🗂️ Dataset

[`somosnlp-hackathon-2022/Dataset-Acoso-Twitter-Es`](https://huggingface.co/datasets/somosnlp-hackathon-2022/Dataset-Acoso-Twitter-Es)

## ▶️ Cómo ejecutar (Google Colab)

1. Abre `MNA_NLP_actividad_chatbot_LLM_RAG_Seguras.ipynb` en Google Colab.
2. Menú **Entorno de ejecución → Cambiar tipo de entorno → GPU (T4)**.
3. **Entorno de ejecución → Ejecutar todas**.
4. La última celda abre la interfaz de Gradio con un enlace público.

> El LLM se carga en 4-bit NF4, por lo que se requiere GPU. Sin GPU, el cuaderno avisa y
> usa CPU (más lento).

## 📁 Estructura del repositorio

```
.
├── MNA_NLP_actividad_chatbot_LLM_RAG_Seguras.ipynb   # Notebook solución (entregable)
├── README.md
└── .gitignore
```

## ⚠️ Aviso

Seguras es un apoyo **orientativo** y **no sustituye** atención profesional, psicológica,
legal ni a las autoridades. En México: **Línea Mujeres 800 108 4053** (24 h) ·
**Denuncia 079 opción 1** · **Emergencias 911**.

## 👩‍💻 Equipo

- Nombre 1 — Matrícula
- Nombre 2 — Matrícula
- Nombre 3 — Matrícula

**Número de equipo:** ____
