
Source file changed.
{
"cells":[
0:{
"cell_type":"code"
"execution_count":10
"id":"d35fe810-09fc-4e16-8ac5-58e1d1f025a7"
"metadata":{}
"outputs":[
0:{
"name":"stderr"
"output_type":"stream"
"text":[
0:"[nltk_data] Downloading package punkt to
"
1:"[nltk_data]     C:\Users\User\AppData\Roaming\nltk_data...
"
2:"[nltk_data]   Package punkt is already up-to-date!
"
]
}
]
"source":[
0:"import os
"
1:"import nltk
"
2:"import ssl
"
3:"import streamlit as st
"
4:"import random
"
5:"from sklearn.feature_extraction.text import TfidfVectorizer
"
6:"from sklearn.linear_model import LogisticRegression
"
7:"
"
8:"ssl._create_default_https_context = ssl._create_unverified_context
"
9:"nltk.data.path.append(os.path.abspath("nltk_data"))
"
10:"nltk.download('punkt')
"
11:"
"
12:"intents = [
"
13:"    {
"
14:"        "tag": "greeting",
"
15:"        "patterns": ["Hi", "Hello", "Hey", "How are you", "What's up"],
"
16:"        "responses": ["Hi there", "Hello", "Hey", "I'm fine, thank you", "Nothing much"]
"
17:"    },
"
18:"    {
"
19:"        "tag": "goodbye",
"
20:"        "patterns": ["Bye", "See you later", "Goodbye", "Take care"],
"
21:"        "responses": ["Goodbye", "See you later", "Take care"]
"
22:"    },
"
23:"    {
"
24:"        "tag": "thanks",
"
25:"        "patterns": ["Thank you", "Thanks", "Thanks a lot", "I appreciate it"],
"
26:"        "responses": ["You're welcome", "No problem", "Glad I could help"]
"
27:"    },
"
28:"    {
"
29:"        "tag": "about",
"
30:"        "patterns": ["What can you do", "Who are you", "What are you", "What is your purpose"],
"
31:"        "responses": ["I am a chatbot", "My purpose is to assist you", "I can answer questions and provide assistance"]
"
32:"    },
"
33:"    {
"
34:"        "tag": "help",
"
35:"        "patterns": ["Help", "I need help", "Can you help me", "What should I do"],
"
36:"        "responses": ["Sure, what do you need help with?", "I'm here to help. What's the problem?", "How can I assist you?"]
"
37:"    },
"
38:"    {
"
39:"        "tag": "age",
"
40:"        "patterns": ["How old are you", "What's your age"],
"
41:"        "responses": ["I don't have an age. I'm a chatbot.", "I was just born in the digital world.", "Age is just a number for me."]
"
42:"    },
"
43:"    {
"
44:"        "tag": "weather",
"
45:"        "patterns": ["What's the weather like", "How's the weather today"],
"
46:"        "responses": ["I'm sorry, I cannot provide real-time weather information.", "You can check the weather on a weather app or website."]
"
47:"    },
"
48:"    {
"
49:"        "tag": "budget",
"
50:"        "patterns": ["How can I make a budget", "What's a good budgeting strategy", "How do I create a budget"],
"
51:"        "responses": ["To make a budget, start by tracking your income and expenses. Then, allocate your income towards essential expenses like rent, food, and bills. Next, allocate some of your income towards savings and debt repayment. Finally, allocate the remainder of your income towards discretionary expenses like entertainment and hobbies.", "A good budgeting strategy is to use the 50/30/20 rule. This means allocating 50% of your income towards essential expenses, 30% towards discretionary expenses, and 20% towards savings and debt repayment.", "To create a budget, start by setting financial goals for yourself. Then, track your income and expenses for a few months to get a sense of where your money is going. Next, create a budget by allocating your income towards essential expenses, savings and debt repayment, and discretionary expenses."]
"
52:"    },
"
53:"    {
"
54:"        "tag": "credit_score",
"
55:"        "patterns": ["What is a credit score", "How do I check my credit score", "How can I improve my credit score"],
"
56:"        "responses": ["A credit score is a number that represents your creditworthiness. It is based on your credit history and is used by lenders to determine whether or not to lend you money. The higher your credit score, the more likely you are to be approved for credit.", "You can check your credit score for free on several websites such as Credit Karma and Credit Sesame."]
"
57:"    }
"
58:"]
"
59:"
"
60:"vectorizer = TfidfVectorizer()
"
61:"clf = LogisticRegression(random_state=0, max_iter=10000)
"
62:"tags = []
"
63:"patterns = []
"
64:"for intent in intents:
"
65:"    for pattern in intent['patterns']:
"
66:"        tags.append(intent['tag'])
"
67:"        patterns.append(pattern)
"
68:"
"
69:"x = vectorizer.fit_transform(patterns)
"
70:"y = tags
"
71:"clf.fit(x,y)
"
72:"
"
73:"def chatbot(input_text):
"
74:"    input_text = vectorizer.transform([input_text])
"
75:"    tag = clf.predict(input_text)
"
76:"    for intent in intents:
"
77:"        if intent['tag'] == tag:
"
78:"            response = random.choice(intent['responses'])
"
79:"            return response
"
80:"
"
81:"counter = 0
"
82:"
"
83:"def main():
"
84:"    global counter
"
85:"    st.title('Chatbot')
"
86:"    st.write("Welcome to the chatbot. Please type a message and press Enter to start the conversation.")
"
87:"
"
88:"    counter+=1
"
89:"    user_input = st.text_input("You:", key=f"user_input_{counter}")
"
90:"    if user_input:
"
91:"        response = chatbot(user_input)
"
92:"        st.text_area("Chatbot:", value=response, height=100, max_chars=None, key=f"chatbot_response_{counter}")
"
93:"
"
94:"        if response.lower() in ['goodbye', 'bye']:
"
95:"            st.write("Thank you for chatting with me. Have a great day!")
"
96:"            st.stop()
"
97:"
"
98:"    if __name__ == '__main__':
"
99:"        main()"
]
}
1:{
"cell_type":"code"
"execution_count":NULL
"id":"075f9317"
"metadata":{}
"outputs":[]
"source":[]
}
]
"metadata":{
"kernelspec":{
"display_name":"Python 3 (ipykernel)"
"language":"python"
"name":"python3"
}
"language_info":{
"codemirror_mode":{
"name":"ipython"
"version":3
}
"file_extension":".py"
"mimetype":"text/x-python"
"name":"python"
"nbconvert_exporter":"python"
"pygments_lexer":"ipython3"
"version":"3.11.5"
}
}
"nbformat":4
"nbformat_minor":5
}
Made with Streamlit
