# === Setup Inicial ===
# Variável de ambiente da OpenAI
import os
os.environ['OPENAI_API_KEY'] = 'sk-...'

# Instalação de pacotes necessários
# Execute no terminal se ainda não tiver os pacotes:
# pip install streamlit crewai langchain openai folium streamlit-folium pillow

# === Bibliotecas ===
import streamlit as st
from crewai import Agent, Task, Crew
from langchain.chat_models import ChatOpenAI
import folium
from streamlit_folium import st_folium
import requests

# === Função para obter coordenadas ===
def obter_coordenadas(cidade):
    url = f"https://nominatim.openstreetmap.org/search?format=json&q={cidade}"
    resposta = requests.get(url).json()
    if resposta:
        return float(resposta[0]['lat']), float(resposta[0]['lon'])
    return None, None

# === Função principal ===
def planejar_viagem(destino, dias, interesses):
    llm = ChatOpenAI(model="gpt-4", temperature=0.5)

    planejador = Agent(
        role="Planejador de Viagem",
        goal=f"Criar um roteiro personalizado de {dias} dias para {destino}, considerando interesses como {interesses}",
        backstory="Especialista em criar roteiros personalizados para qualquer tipo de viajante.",
        verbose=True,
        llm=llm
    )

    pesquisador = Agent(
        role="Pesquisador de Destinos",
        goal=f"Pesquisar atrações e dicas úteis sobre {destino}, focando em {interesses}",
        backstory="Especialista em buscar dados atualizados sobre turismo e experiências locais.",
        verbose=True,
        llm=llm
    )

    redator = Agent(
        role="Redator de Itinerários",
        goal="Montar um resumo de viagem detalhado e atrativo com base nas informações coletadas",
        backstory="Profissional de turismo focado em apresentar planos de forma clara e encantadora.",
        verbose=True,
        llm=llm
    )

    tarefa1 = Task(
        description=f"Crie um esboço de viagem de {dias} dias para {destino}, com foco em: {interesses}.",
        agent=planejador
    )

    tarefa2 = Task(
        description=f"Com base no esboço, pesquise atrações, pontos turísticos e dicas úteis sobre {destino}, focando em: {interesses}.",
        agent=pesquisador
    )

    tarefa3 = Task(
        description="Com base nas tarefas anteriores, crie um itinerário final completo, claro e atrativo para o viajante.",
        agent=redator
    )

    equipe = Crew(
        agents=[planejador, pesquisador, redator],
        tasks=[tarefa1, tarefa2, tarefa3],
        verbose=True
    )

    resultado = equipe.kickoff()
    return resultado

# === Streamlit App ===
st.set_page_config(page_title="Planejador de Viagem", page_icon="🌍")
st.title("🌍 Planejador de Viagem Personalizada")

st.markdown("Crie um roteiro de viagem sob medida com ajuda de agentes inteligentes! ✈️")

with st.form("formulario_viagem"):
    destino = st.text_input("Destino")
    dias = st.number_input("Número de dias", min_value=1, step=1)
    interesses = st.text_input("Interesses (Ex: cultura, gastronomia, natureza)")
    submitted = st.form_submit_button("Gerar roteiro")

if submitted:
    with st.spinner("🧠 Gerando plano de viagem..."):
        resultado = planejar_viagem(destino, dias, interesses)
        st.success("✅ Roteiro gerado com sucesso!")

        tab1, tab2 = st.tabs(["🧳 Itinerário", "🗺️ Mapa do destino"])

        with tab1:
            st.text_area("Seu roteiro:", resultado, height=400)
            st.download_button(
                label="📥 Baixar roteiro como .txt",
                data=resultado,
                file_name="itinerario_viagem.txt",
                mime="text/plain"
            )

        with tab2:
            lat, lon = obter_coordenadas(destino)
            if lat and lon:
                mapa = folium.Map(location=[lat, lon], zoom_start=12)
                folium.Marker([lat, lon], tooltip=destino).add_to(mapa)
                st_folium(mapa, width=700)
            else:
                st.warning("Localização não encontrada.")
# Planejador-de-Viagem
