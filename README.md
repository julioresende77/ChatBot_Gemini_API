# ChatBot_Geminini_API
## Imersão Inteligência Artificial 2ª Edição - Alura & Google
### Criando meu próprio chatbot com a Gemini API no Google Colab

### **Instalando o SDK do Google**
!pip install -q -U google-generativeai


### **Importando o SDK do Python**
import google.generativeai as genai

GOOGLE_API_KEY="COLE SUA API KEY"
genai.configure(api_key=GOOGLE_API_KEY)

### **Listar os modelos disponíveis**
for m in genai.list_models():
  if 'generateContent' in m.supported_generation_methods:
    print(m.name)

#### models/gemini-1.0-pro
#### models/gemini-1.0-pro-001
#### models/gemini-1.0-pro-latest
#### models/gemini-1.0-pro-vision-latest
#### models/gemini-1.5-pro-latest
#### models/gemini-pro
#### models/gemini-pro-vision

generation_config = {
    "candidate_count": 1,
    "temperature": 0.5,
}

safety_settings = {
    "HARASSMENT": "BLOCK_NONE",
    "HATE": "BLOCK_NONE",
    "SEXUAL": "BLOCK_NONE",
    "DANGEROUS": "BLOCK_NONE",
}

### **Inicializando o modelo**
model = genai.GenerativeModel(model_name="gemini-1.0-pro",
                              generation_config=generation_config,
                              safety_settings=safety_settings)

response = model.generate_content("Vamos aprender conteúdo sobre IA. Me ensine como se eu fosse uma criança de 9 anos.")
print(response.text)


### **Melhorando a visualização**
import textwrap
from IPython.display import display
from IPython.display import Markdown

def to_markdown(text):
  text = text.replace('•', '  *')
  return Markdown(textwrap.indent(text, '> ', predicate=lambda _: True))

chat = model.start_chat(history=[])

### **Imprimindo o histórico**
for message in chat.history:
  display(to_markdown(f'**{message.role}**: {message.parts[0].text}'))
  print('-------------------------------------------')

user: Qual é a capital do japão?

-------------------------------------------
model: Tóquio

-------------------------------------------
user: Qual é a comida tipica desse pais?

-------------------------------------------
model: Sushi

-------------------------------------------
user: Qual é a população dessa cidade?

-------------------------------------------
model: 37.339.804 (2023)

-------------------------------------------
user: Qual cidade está se referindo?

-------------------------------------------
model: Tóquio

-------------------------------------------

### **Inicializando o ChatBot**
prompt = input("Esperando prompt: ")

while prompt != "fim":
  response = chat.send_message(prompt)
  print("Resposta: ", response.text, "\n")
  prompt = input("Esperando prompt: ")
