import streamlit as st
import pandas as pd
import plotly.express as px

st.set_page_config(page_title="Art Gallery Dashboard", layout="wide")
st.title("🎨 Art Gallery Dashboard")
st.write("Manage and analyze your art collection!")

if "artworks" not in st.session_state:
    st.session_state.artworks = pd.DataFrame([
        {"Title": "Mona Lisa", "Artist": "Leonardo da Vinci", "Year": 1503, "Medium": "Oil", "Period": "Renaissance", "Price": 860000000},
        {"Title": "Starry Night", "Artist": "Vincent van Gogh", "Year": 1889, "Medium": "Oil", "Period": "Post-Impressionism", "Price": 100000000},
        {"Title": "The Persistence of Memory", "Artist": "Salvador Dali", "Year": 1931, "Medium": "Oil", "Period": "Surrealism", "Price": 50000000},
        {"Title": "Girl with a Pearl Earring", "Artist": "Johannes Vermeer", "Year": 1665, "Medium": "Oil", "Period": "Baroque", "Price": 30000000},
        {"Title": "The Scream", "Artist": "Edvard Munch", "Year": 1893, "Medium": "Tempera", "Period": "Expressionism", "Price": 119922500},
    ])

with st.sidebar:
    st.header("Add New Artwork")
    title = st.text_input("Title")
    artist = st.text_input("Artist")
    year = st.number_input("Year", min_value=1000, max_value=2024, value=2000)
    medium = st.text_input("Medium")
    period = st.text_input("Period")
    price = st.number_input("Price ($)", min_value=0, value=1000)
    if st.button("Add Artwork"):
        new_row = {"Title": title, "Artist": artist, "Year": year, "Medium": medium, "Period": period, "Price": price}
        st.session_state.artworks = pd.concat([st.session_state.artworks, pd.DataFrame([new_row])], ignore_index=True)
        st.success("Artwork added!")

search = st.text_input("Search by Artist")
df = st.session_state.artworks
if search:
    df = df[df["Artist"].str.contains(search, case=False)]

st.subheader("Artwork Collection")
st.dataframe(df, use_container_width=True)

col1, col2 = st.columns(2)
with col1:
    fig1 = px.bar(df, x="Title", y="Price", color="Artist", title="Artwork Prices")
    st.plotly_chart(fig1, use_container_width=True)
with col2:
    fig2 = px.pie(df, names="Period", title="Art Periods")
    st.plotly_chart(fig2, use_container_width=True)
