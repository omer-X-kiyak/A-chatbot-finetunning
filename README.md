# A-chatbot-finetunning
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Generative Model Training Script</title>
</head>
<body>
    <h1>Generative Model Training Script</h1>
    <p>Bu Python betiği, bir dil modelinin eğitimi ve prompt oluşturma sürecini içerir. Transformer tabanlı dil modellerini kullanarak özelleştirilmiş generatif modeller oluşturmayı amaçlar. Betikte kullanılan temel bileşenler ve süreçler aşağıda açıklanmıştır:</p>
    
    <h2>Gereksinimler</h2>
    <p>İlgili Python kütüphanelerini yüklemek için aşağıdaki komutlar kullanılır:</p>
    <pre>
    !pip install bitsandbytes
    !pip install accelerate
    !pip install --upgrade transformers
    !pip install --upgrade peft
    !pip install --upgrade datasets
    </pre>

    <h2>Model ve Tokenizer Yükleme</h2>
    <p>AutoTokenizer ve AutoModelForCausalLM kullanılarak model ve tokenizer yüklenir:</p>
    <pre>
    from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
    import torch
    
    tokenizer = AutoTokenizer.from_pretrained("model_name")
    model = AutoModelForCausalLM.from_pretrained("model_name", device_map="auto")
    </pre>

    <h2>Prompt Oluşturma</h2>
    <p>Girdi başlığına dayalı olarak prompt oluşturmak için tokenize işlemi ve modelin generate fonksiyonu kullanılır:</p>
    <pre>
    txt = """###SYSTEM: Based on INPUT title generate the prompt for generative model

    ###INPUT: Linux Terminal

    ###PROMPT:"""
    tokens = tokenizer(txt, return_tensors="pt")['input_ids'].to("cuda")
    op = model.generate(tokens, max_new_tokens=200)
    print(tokenizer.decode(op[0]))
    </pre>

    <h2>Modelin PEFT ile Hazırlanması</h2>
    <p>PEFT kütüphanesi kullanılarak model için LoraConfig ayarlanır ve model hazırlanır:</p>
    <pre>
    from peft import get_peft_model, LoraConfig, TaskType, prepare_model_for_kbit_training

    model.gradient_checkpointing_enable()
    model = prepare_model_for_kbit_training(model)

    peft_config = LoraConfig(inference_mode=False, r=8, lora_alpha=32, lora_dropout=0.1, peft_type=TaskType.CAUSAL_LM)
    model = get_peft_model(model, peft_config)

    print(model.print_trainable_parameters())
    </pre>

    <h2>Veri Kümesi Yükleme ve Hazırlama</h2>
    <p>Datasets kütüphanesi kullanılarak veri kümesi yüklenir ve gerekli formatlama işlemleri yapılır:</p>
    <pre>
    from datasets import load_dataset

    dataset = load_dataset("fka/awesome-chatgpt-prompts", split="train")
    print(dataset[0].keys())

    dataset = dataset.map(format_dataset)
    print(dataset[0].keys())
    dataset = dataset.remove_columns(['act', "prompt"])
    print(dataset)
    tmp = dataset.train_test_split(test_size=0.1)
    train_dataset = tmp["train"]
    test_dataset = tmp["test"]
    print(train_dataset)
    print(test_dataset)
    </pre>

    <h2>Model Eğitimi</h2>
    <p>Trainer ve TrainingArguments kullanılarak model eğitimi gerçekleştirilir:</p>
    <pre>
    import torch
    from transformers import Trainer, TrainingArguments, DataCollatorForLanguageModeling
    
    if torch.cuda.device_count() > 1:
        model.is_parallelizable = True
        model.model_parallel = True

    data_collator = DataCollatorForLanguageModeling(tokenizer, mlm=False)

    trainer = Trainer(
                        model = model,
                        train_dataset=train_dataset,
                        eval_dataset = test_dataset,
                        tokenizer = tokenizer,
                        data_collator = data_collator,

                        args = TrainingArguments(
                            output_dir="./training",
                            remove_unused_columns=False,
                            per_device_train_batch_size=2,
                            gradient_checkpointing=True,
                            gradient_accumulation_steps=4,
                            max_steps=200,
                            learning_rate=2.5e-5,
                            logging_steps=5,
                            fp16=True,
                            optim="paged_adamw_8bit",
                            save_strategy="steps",
                            save_steps=50,
                            evaluation_strategy="steps",
                            eval_steps=5,
                            do_eval=True,
                            label_names = ["input_ids", "labels", "attention_mask"],
                            report_to = "none",
                    ))
    trainer.train()
    </pre>

    <h2>Sonuçların Kaydedilmesi</h2>
    <p>Eğitilen model ve oluşturulan prompt sonuçları kaydedilir:</p>
    <pre>
    txt = """###SYSTEM: Based on INPUT title generate the prompt for generative model

    ###INPUT: Math Tutor

    ###PROMPT:"""
    tokens = tokenizer(txt, return_tensors="pt")['input_ids'].to("cuda")
    op = model.generate(tokens, max_new_tokens=200)
    output = tokenizer.decode(op[0], skip_special_tokens=True)
    print(output)

    model.save_pretrained("prompt_250_steps", safe_serialization=False)
    !zip -r prompt_250.zip '/content/prompt_250_steps'
    </pre>

    <h2>PEFT Modelinin Yüklenmesi ve Kaydedilmesi</h2>
    <p>PEFT modelinin önceden eğitilmiş model ile yüklenmesi ve kaydedilmesi:</p>
    <pre>
    from peft import PeftModel

    model_path = "/content/prompt_250_steps"

    model = PeftModel.from_pretrained(model, model_path)

    model_ = model.merge_and_unload()
    model_.save_pretrained("merged_model_")
    </pre>
</body>
</html>
