# 💜 Seguras 2.0 — Chatbot contra el ciberacoso (RAG + LLM)

Actividad **Procesamiento de Lenguaje Natural**
Maestría en Inteligencia Artificial Aplicada — Tecnológico de Monterrey

## Introducción
El ciberacoso y la violencia digital afectan de forma desproporcionada a mujeres y adolescentes: mensajes hostiles, amenazas, hostigamiento, suplantación de identidad, doxing y difusión de contenido íntimo sin consentimiento. Muchas víctimas no saben qué hacer, a quién acudir ni cómo reunir evidencia, y con frecuencia sienten miedo o vergüenza de pedir ayuda.

Este proyecto extiende un trabajo previo propio realizado para el Bootcamp **Talento Tech** de Inteligencia Artificial del Ministerio de las TIC de Colombia, «Seguras», que era un clasificador clásico (TF-IDF + Regresión Logística) entrenado sobre el dataset Dataset-Acoso-Twitter-Es para decir si un texto es o no ciberacoso. Ese modelo respondía sí/no, pero no orientaba a la persona.

Fuente del Data set: https://huggingface.co/datasets/somosnlp-hackathon-2022/Dataset-Acoso-Twitter-Es

Seguras 2.0 convierte ese detector en una pieza de un chatbot conversacional RAG + LLM

## 📌 Descripción

**Seguras 2.0** es un chatbot conversacional que acompaña y orienta a personas que
enfrentan **ciberacoso o violencia digital** en México. Combina **Recuperación Aumentada
Generativa (RAG)** con un **Modelo de Lenguaje de Gran Tamaño (LLM)** para dar respuestas
empáticas, accionables y **fundamentadas en documentos curados en español**, reduciendo las
alucinaciones típicas de un LLM por sí solo.

El proyecto evoluciona un trabajo previo del equipo, *Seguras1* (un clasificador clásico de
acoso sí/no), que aquí se **reutiliza como una herramienta interna** del chatbot.

## Sobre el RAG
¿Por qué RAG + LLM? Un LLM por sí solo puede alucinar leyes, teléfonos o procedimientos. Con Recuperación Aumentada Generativa (RAG) el modelo responde fundamentado en documentos curados en español (definiciones, pasos de actuación, recursos verificados de México, marco legal), lo que reduce las alucinaciones y personaliza las respuestas a la temática de seguridad frente al ciberacoso.

##  Arquitectura del sistema

```
                         ┌──────────────────────────────────────────────┐
   Mensaje de la         │                 SEGURAS 2.0                    │
   persona  ───────────► │                                                │
                         │  (A) Detector  TF-IDF + LogReg  → señal acoso  │
                         │  (B) Embeddings multilingües (consulta)        │
                         │  (C) FAISS  → recupera top-k documentos (RAG)  │
                         │  (D) Prompt = persona + contexto + señal + hist│
                         │  (E) LLM cuantizado NF4 (estilo QLoRA)         │
                         │        └─► genera respuesta en español         │
                         └──────────────────────────────────────────────┘
                                          │
   Interfaz Gradio  ◄─────────────────────┘  (respuesta + detección + fuentes)
```

**Componentes:**
- **(A) Detector** — el modelo de *Seguras1* reutilizado como *tool* que aporta una señal
  («posible acoso» / «sin señales»).
- **(B) Embeddings** — `paraphrase-multilingual-MiniLM-L12-v2` (entiende español).
- **(C) Recuperador** — índice **FAISS** por similitud coseno sobre la base de conocimiento.
- **(D) Orquestador** — arma el *prompt* con la persona del bot, el contexto recuperado,
  la señal del detector y el historial.
- **(E) LLM** — modelo *instruct* cargado en **4 bits NF4** con **doble cuantización** y
  **cómputo en bfloat16** (la técnica de cuantización vista en la teoría de QLoRA).


## 🗂️ Dataset

[`somosnlp-hackathon-2022/Dataset-Acoso-Twitter-Es`](https://huggingface.co/datasets/somosnlp-hackathon-2022/Dataset-Acoso-Twitter-Es)

## ▶️ Cómo ejecutar (Google Colab)

1. Abre `MNA_NLP_actividad_chatbot_LLM_RAG_Seguras.ipynb` en Google Colab.
2. Menú **Entorno de ejecución → Cambiar tipo de entorno → GPU (T4)**.
3. **Entorno de ejecución → Ejecutar todas**.
4. La última celda abre la interfaz de Gradio con un enlace público.

> El LLM se carga en 4-bit NF4, por lo que se requiere GPU. Sin GPU, el cuaderno avisa y
> usa CPU (más lento).

## Estructura del repositorio

```
.
├── MNA_NLP_actividad_chatbot_LLM_RAG_Seguras.ipynb   # Notebook solución (entregable)
├── README.md
└── .gitignore
```

## Aviso

Seguras es un apoyo **orientativo** y **no sustituye** atención profesional, psicológica,
legal ni a las autoridades. En México: **Línea Mujeres 800 108 4053** (24 h) ·
**Denuncia 079 opción 1** · **Emergencias 911**.

## Siguientes pasos

Si bien el proyecto está enfocado principalmente para un público mexicano, el siguiente gran paso es adicionar al público colombiano, ya que el proyecto se construyó desde un inicio para personas de Colombia, pero al evolucionarse para una entrega de la materia de Procesamiento de Lenguaje Natural del TEC. de Monterrey, se ajustó la información.
Lo más necesario es mejorar las bases de datos para así mejorar la precisión del RAG.

## Conclusiones
Adecuación de las respuestas. Al estar fundamentadas en la base de conocimiento (RAG), las respuestas se mantienen pertinentes a la temática (ciberacoso y violencia digital) y consistentes con los recursos reales de México. El sistema deja de dar una etiqueta seca («acoso / no acoso») y pasa a acompañar y orientar, que es lo que una persona en esa situación realmente necesita.

Valor del RAG frente a un LLM solo. El componente de recuperación reduce las alucinaciones: el LLM ya no improvisa teléfonos o leyes, sino que se apoya en documentos curados. Esto es crítico en un dominio sensible donde un dato incorrecto (un número de ayuda equivocado) podría tener consecuencias reales.

Integración del trabajo previo. El clasificador de Seguras1 no se desechó: se reutilizó como herramienta interna que aporta una señal al chatbot. Así, el proyecto evoluciona de un modelo clásico de ML a un sistema conversacional moderno.

Cuantización (QLoRA). Cargar el LLM en NF4 con doble cuantización y cómputo BF16 permitió ejecutar un modelo instruct potente en una GPU modesta (T4), conectando directamente con la teoría de la sesión.

Limitaciones. (1) El detector se entrenó con tuits, por lo que puede equivocarse con textos de otro estilo; por eso se usa solo como pista. (2) La base de conocimiento es acotada: ampliarla (documentos oficiales, por estado) mejoraría la cobertura. (3) El LLM puede seguir cometiendo errores, de ahí los guardrails del prompt y el aviso de que Seguras no sustituye atención profesional. (4) La temática exige cuidado ético: el bot evita juzgar, no promete confidencialidad absoluta y siempre deriva a recursos humanos.

Trabajo futuro. Hacer fine-tuning del LLM con QLoRA sobre diálogos de orientación, ampliar el RAG con fuentes oficiales actualizadas, y agregar detección de riesgo más fina para escalar a recursos de emergencia cuando sea necesario.


