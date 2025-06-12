# 🌍 Planejador de Viagem com IA

Crie roteiros de viagem personalizados com a ajuda de agentes inteligentes (usando CrewAI + LangChain + Streamlit).

---

## 🚀 Demonstração

Acesse o app online: [https://planejador.streamlit.app](https://planejador.streamlit.app) *(link fictício - substitua pelo real após deploy)*

---

## 🧠 Tecnologias utilizadas

* [Streamlit](https://streamlit.io/) — Interface web interativa
* [LangChain](https://www.langchain.com/) — Integração com LLMs
* [CrewAI](https://docs.crewai.com/) — Orquestração de agentes
* [OpenAI GPT-4](https://platform.openai.com/docs/models) — Modelo de linguagem
* [Folium](https://python-visualization.github.io/folium/) — Mapa interativo com localização

---

## 📦 Instalação local

1. Clone o repositório:

```bash
git clone https://github.com/DaviCheru/Planejador-de-Viagem.git
cd Planejador-de-Viagem
```

2. Instale as dependências:

```bash
pip install -r requirements.txt
```

3. Crie um arquivo `.env` ou configure sua chave:

```bash
export OPENAI_API_KEY="sk-...AaUA"
```

4. Execute o app:

```bash
streamlit run planejador_viagem_app.py
```

---

## ☁️ Deploy no Streamlit Cloud

1. Suba o repositório para o GitHub
2. Vá para [streamlit.io/cloud](https://streamlit.io/cloud) e conecte sua conta GitHub
3. Escolha este repositório
4. **Ajuste o campo "Main file path" para:**

```
planejador_viagem_app.py
```

5. Adicione o segredo da OpenAI:

   * Clique em "Edit Secrets"
   * Adicione:

```toml
OPENAI_API_KEY="sk-...AaUA"

---

## 📝 Como usar

1. Insira o destino da viagem
2. Defina o número de dias
3. Informe seus principais interesses (ex: cultura, gastronomia)
4. Clique em "Gerar roteiro"
5. Visualize o itinerário e o mapa, e baixe seu roteiro em .txt

---

## 📄 Licença

Este projeto é de uso livre para fins educacionais e não possui garantia de produção comercial.





