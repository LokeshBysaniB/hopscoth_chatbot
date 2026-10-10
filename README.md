# Hopscotch Support Chatbot (Session 1)

A simple customer support chatbot for **Hopscotch**, a premium kids' fashion retailer. It answers questions about returns, refunds and defects using the company's return policy.

This is Session 1 of a learning series. It is built with the raw OpenAI SDK and plain glue code, with no LangChain or other framework.

## What it does

- Answers customer questions using only the rules in [hopscotch_policy.md](hopscotch_policy.md)
- Asks for missing details such as the delivery date, the item's condition or the order value
- Asks for photo evidence when a customer reports a defect
- Passes sensitive cases to a human agent instead of deciding them itself:
  - Child-safety concerns
  - Skin reactions or allergies
  - Orders above INR 5,000
  - Unclear facts, or cases where it's hard to tell a defect from misuse

## How it works

1. A web chat interface is built with **Streamlit**.
2. The return policy is loaded from a file and added to a system prompt.
3. Messages are sent to the **`openai/gpt-4o-mini`** model through **OpenRouter**, using the OpenAI SDK.
4. The full conversation history is kept in the Streamlit session and resent on every turn, because the model itself remembers nothing between calls.

For a visual overview, open [Architecture.html](Architecture.html) in a browser.

## Project structure

| File | Purpose |
|------|---------|
| `app.py` | The Streamlit chatbot app |
| `hopscotch_policy.md` | The return and refund policy the bot answers from |
| `Architecture.html` | Architecture diagram |
| `requirements.txt` | Python dependencies |
| `.env` | Your OpenRouter API key (not committed) |

## Setup

1. **Create and activate a virtual environment**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Add your API key.** Get a key from [openrouter.ai/keys](https://openrouter.ai/keys), then create a `.env` file in the project folder containing:
   ```
   OPENROUTER_API_KEY=your-key-here
   ```

4. **Run the app**
   ```bash
   streamlit run app.py
   ```
   The app opens in your browser, usually at http://localhost:8501.

## Using the app

- Type a question in the chat box at the bottom.
- The sidebar has:
  - **Clear chat** to start a new conversation
  - **Raw history list** to see exactly what is sent to the model on each turn
  - **Example prompts** to try

### Example questions

- "I bought a dress 10 days ago, never worn, tags on — can I return it?"
- "The sole of my son's sneakers peeled off after 3 weeks of school"
- "A button came off and my toddler nearly put it in his mouth"
- "My order was ₹7,200, I want a refund"
