# AI Poetry Generator

This Streamlit application generates poems based on user input using OpenAI's GPT-3.5-turbo model. Users can customize the theme, style, length, rhyme scheme, tone, and mood of the poem.

## Features

- **Theme Input:** Enter a theme for the poem.
- **Style Selection:** Choose from various poetry styles such as Shakespearean, Haiku, Free Verse, Sonnet, and Limerick.
- **Stanza Customization:** Select the number of stanzas (applicable for some styles).
- **Length Customization:** Choose the length of the poem (short, medium, long).
- **Rhyme Scheme Customization:** Select a rhyme scheme (ABAB, AABB, ABBA, Free Verse).
- **Tone Customization:** Select the tone of the poem (Happy, Sad, Angry, Reflective, Romantic).
- **Mood Customization:** Select the mood of the poem (Joyful, Melancholic, Excited, Calm, Tense).

## Installation

1. **Clone the repository:**
    ```sh
    git clone https://github.com/yourusername/ai-poetry-generator.git
    cd ai-poetry-generator
    ```

2. **Create a virtual environment and activate it:**
    ```sh
    python -m venv venv
    # On Windows
    .\venv\Scripts\activate
    # On macOS/Linux
    source venv/bin/activate
    ```

3. **Install the required packages:**
    ```sh
    pip install -r requirements.txt
    ```

4. **Run the Streamlit app:**
    ```sh
    streamlit run streamlit_poetry_generator.py
    ```

## Usage

1. Enter a theme for the poem.
2. Select the type of poetry, number of stanzas, length, rhyme scheme, tone, and mood.
3. Click the "Generate Poem" button to generate and display the poem.
4. Use the sidebar to view example themes and generate poems based on them.

## Code

```python
import openai
import streamlit as st

# Replace 'your_openai_api_key' with your actual OpenAI API key
openai.api_key = 'your_openai_api_key'

def generate_poem(theme, style, stanzas, length, rhyme_scheme, tone, mood):
    if style == "Shakespearean":
        prompt = f"Write a {length} poem about {theme} in the style of Shakespeare with {stanzas} stanzas, in a {tone} tone and a {mood} mood, using a {rhyme_scheme} rhyme scheme."
    elif style == "Haiku":
        prompt = f"Write a Haiku about {theme} in a {tone} tone and a {mood} mood."
    elif style == "Free Verse":
        prompt = f"Write a {length} free verse poem about {theme} in a {tone} tone and a {mood} mood."
    else:
        prompt = f"Write a {length} poem about {theme} in the style of {style} with {stanzas} stanzas, in a {tone} tone and a {mood} mood, using a {rhyme_scheme} rhyme scheme."

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a poet."},
            {"role": "user", "content": prompt}
        ],
        max_tokens=300
    )
    poem = response.choices[0].message["content"].strip()
    return poem

# Streamlit interface
st.title("AI Poetry Generator")

theme = st.text_input("Enter a theme for the poem:")
style = st.selectbox("Select the type of poetry", ["Shakespearean", "Haiku", "Free Verse", "Sonnet", "Limerick"])
stanzas = st.slider("Select the number of stanzas (applicable for some styles)", 1, 10, 4)
length = st.selectbox("Select the length of the poem", ["short", "medium", "long"])
rhyme_scheme = st.selectbox("Select the rhyme scheme", ["ABAB", "AABB", "ABBA", "Free Verse"])
tone = st.selectbox("Select the tone", ["Happy", "Sad", "Angry", "Reflective", "Romantic"])
mood = st.selectbox("Select the mood", ["Joyful", "Melancholic", "Excited", "Calm", "Tense"])

if st.button("Generate Poem"):
    poem = generate_poem(theme, style, stanzas, length, rhyme_scheme, tone, mood)
    st.markdown(f"### {style} Poem about {theme}")
    st.markdown(poem.replace('\n', '\n\n'))

# Example themes section
st.sidebar.header("Example Themes")
example_themes = ["Love", "Nature", "War", "Friendship", "Betrayal"]
if st.sidebar.button("Generate Example Theme"):
    import random
    example_theme = random.choice(example_themes)
    poem = generate_poem(example_theme, style, stanzas, length, rhyme_scheme, tone, mood)
    st.sidebar.markdown(f"### Example Theme: {example_theme}")
    st.sidebar.markdown(poem.replace('\n', '\n\n'))
