# Demo building

## Prerequisites

- conda create -n streamlit python=3.7
- conda activate streamlit
- pip install streamlit pandas altair matplotlib seaborn plotly bokeh scikit-learn vega_datasets

## Run

```bash
streamlit run app.py
# or
streamlit run https://raw.githubusercontent.com/andfanilo/streamlit-lyondatascience-20200422/master/app_final.py
```

## Demo steps

### Part 1 - First steps 

- Create empty `app.py`
- Open terminal with `CTRL + SHIFT + ù`. Run `streamlit run app.py`
- `st.write("Hello World")`. Activate `Run on save`.
  - I like livereload.
  - Streamlit watches file and updates when change or save detected
- `st.markdown(":tada: Streamlit is **super** easy")` 
  - Tip : markdown accepts HTML/CSS `st.markdown(":tada: <span style='color:red;'>Streamlit</span> is **super** easy", unsafe_allow_html=True)`
- Pull an image of Titanic : `st.image("images/titanic.jpg")`.
- Now let's plot, first read Titanic in DF `df = pd.read_csv('data/titanic.jpg')`.
  - We can `st.dataframe(df)` to see our dataframe live.
  - Tip : Style dataframe : `st.dataframe(df.style.background_gradient(cmap="BuGn"))`
- Describe Altair 
- So to build chart : `chart = alt.Chart(df).mark_bar().encode(x="Age", y="count()")` then into `st.altair_chart(chart)`
- Change bar to red `.mark_bar(color="red")`
- If it's hard to remember, wrap with echo to see code `with st.echo: chart = ...`.

Transition to interactive : now this is very static, what if I want to plot the distribution of another column ?

- instead of modifying the script, extract `col = "Age"` and then `col = st.selectbox("...", df.columns)`.
  - put `Name`, I like `Name` :) .

### Part 2 - Interactive

- so widget state is stored inside object directly
- I like file uploader, let's use it. `f = st.file_uploader(..)` f is directly file uploaded. 
  - `None` for now, `st.write(f)` to prove it
- I don't remember exactly how `st.selectbox` works so `st.help("st.selectbox")`, works with any Python object.
- `if f is not None:` run the code. Then change csv files.
- If you remember sidebar, put file_uploader in sidebar.
- Let's put `if st.sidebar.checkbox('Data preview':)` to hide dataframe. 
  - `st.write(df.dtypes)` and `st.write(df.describe())` there.
- say that for now multipage is `if st.sidebar.selectbox`

### Part 3

- First let's refactor a bit to put loading data in a function `def load_data(f): return pd.read_csv(f)`.
  - say Streamlit displays your errors on screen, not in logs, when `pd.read_csv(None)`
- Add `st.warning("CACHE MISS`) in this function.
- Add `@st.cache`. Copy paste the suppress warning `@st.cache(allow_output_mutation=True, suppress_st_warning=True)`. 
- Upload different files again.

### Part 4

- New sidebar checkbox classification `if st.sidebar.checkbox('Classification':)`
- Import utils : `from utils import *`. Replace `load_data`.
- `if st.button("Run Training")`.
  - `clf, confusion_matrix = train_rf(df)` 
  - `st.balloons()`
  - `st.pyplot(confusion_matrix)`

## Resources

- https://pmbaumgartner.github.io/streamlitopedia/ https://github.com/pmbaumgartner/dank-data-explorer https://dank-data-explorer.herokuapp.com/
- https://datasetsformlapp.herokuapp.com/ https://www.youtube.com/watch?v=SIu2VL-RAXc
