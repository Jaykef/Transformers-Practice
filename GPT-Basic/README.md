# GPT (Generative Pretrained Model) Overview

GPT (Generative Pre-trained Transformer) is a type of language model that utilizes the Transformer architecture to generate human-like text. It is trained on a large corpus of text data and can be fine-tuned for specific natural language processing tasks. 

### Using already built GPTs
1. Importing Required Libraries:
   
   To use already built GPTs, you'll need to import the necessary libraries. The popular transformers library by Hugging Face provides pre-trained
   GPT models and utilities for text generation.
   ```
   from transformers import GPT2LMHeadModel, GPT2Tokenizer
   ```

3. Loading the Pre-trained Model and Tokenizer:

   GPT models are pre-trained on massive amounts of text data, making them proficient in generating coherent and contextually relevant text. We can
   load a pre-trained GPT model and its associated tokenizer.

   ```
   model_name = "gpt2"  # Specify the model name, e.g., "gpt2", "gpt2-medium", etc.
   model = GPT2LMHeadModel.from_pretrained(model_name)
   tokenizer = GPT2Tokenizer.from_pretrained(model_name)
   ```
4. Generating Text with GPTs:
   
   Using the pre-trained GPT model, you can generate text by providing a prompt or an initial input sequence. The model predicts the next token in
   the sequence based on the provided context.
   ```
   def generate_text(prompt, max_length=100):
    inputs = tokenizer.encode(prompt, return_tensors="pt")
    outputs = model.generate(inputs, max_length=max_length, num_return_sequences=1)

    generated_text = tokenizer.decode(outputs[0], skip_special_tokens=True)
    return generated_text
   ```

   In the generate_text function, we encode the prompt using the tokenizer, generate text with the model's generate method, and decode the generated
   output into readable text.

5. Fine-tuning GPTs:

   GPT models can also be fine-tuned on specific tasks by training them on domain-specific data or by adding task-specific layers on top of the pre
   trained model. Fine-tuning allows the model to adapt to the specific nuances and requirements of the target task.

   Here's an example of fine-tuning GPT for a text classification task using the transformers library:

   ```
   from transformers import GPT2ForSequenceClassification, GPT2Tokenizer, AdamW

   # Load pre-trained GPT model and tokenizer
   model_name = "gpt2"
   model = GPT2ForSequenceClassification.from_pretrained(model_name)
   tokenizer = GPT2Tokenizer.from_pretrained(model_name)
   
   # Prepare training data
   train_dataset = ...
   train_dataloader = ...
   
   # Fine-tuning setup
   optimizer = AdamW(model.parameters(), lr=2e-5)
   epochs = 5
   
   # Fine-tuning loop
   for epoch in range(epochs):
       for batch in train_dataloader:
           inputs = tokenizer(batch['text'], truncation=True, padding=True, return_tensors="pt")
           labels = batch['labels']
   
           outputs = model(**inputs, labels=labels)
           loss = outputs.loss
   
           loss.backward()
           optimizer.step()
           optimizer.zero_grad()
   ```
