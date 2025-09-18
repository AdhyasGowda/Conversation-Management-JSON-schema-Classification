# Conversation-Management-JSON-schema-Classification
This project demonstrates a Conversation Manager that tracks and summarizes conversations periodically and performs JSON-schema extraction on user messages using Groq’s OpenAI-compatible API.

# Key features include:
-Conversation tracking: Collects user and assistant messages.
-Periodic summarization: Automatically summarizes the conversation after a defined number of messages.
-JSON-schema extraction: Extract structured data (e.g., name, email, phone, location, age) from user messages.

# Demo Example
- Run 1: User said: Hi, I'm Adhya. I want help with a Python assignment on queues.
Assistant replied: Sure—what specifically? FIFO/LRU?

- Run 2: User said: How to implement FIFO page replacement?
Assistant replied: You can maintain a queue and ... (explanation)

- Run 3: User said: Can you show a small code snippet?
Assistant replied: Yes, here's a minimal Python example...

- Periodic summarization triggered
Summary:
Adhya is seeking help with a Python assignment on queues and wants to implement FIFO page replacement. The assistant provides an explanation and a minimal Python code snippet.

# Setup Instructions
1. Clone the repository
   git clone https://github.com/yourusername/conversation-manager.git
   cd conversation-manager

2. Install dependencies
   pip install requests

3. Set your Groq API key
   import os
   os.environ['OPENAI_API_KEY'] = "YOUR_REAL_KEY_HERE"
   
# Usage
1. Run the conversation manager
   from conversation_manager import ConversationManager, call_chat_api
   cm = ConversationManager(summary_every_k=3)
   
2. Feed conversation messages
   cm.append_user("Hello, I need help with Python.")
   cm.append_assistant("Sure, what topic specifically?")

3. Trigger periodic summarization
   summarized, summary = cm.apply_periodic_summary(max_words=50)
   if summarized:
      print(summary)

4. JSON Schema extraction
   user_message = "Hi, I'm Adhya, my email is adhya@example.com, phone 9876543210, location Bangalore, age 21."

   json_schema_function = [
    {
        "name": "extract_user_info",
        "description": "Extract name, email, phone, location, age from user message",
        "parameters": {
            "type": "object",
            "properties": {
                "name": {"type": "string"},
                "email": {"type": "string"},
                "phone": {"type": "string"},
                "location": {"type": "string"},
                "age": {"type": "integer"}
            },
            "required": ["name", "email", "phone", "location", "age"]
        }
    }
  ]

  resp = call_chat_api(
    messages=[{"role": "user", "content": user_message}],
    functions=json_schema_function,
    function_call="auto",
    max_tokens=128
   )
print(resp)



