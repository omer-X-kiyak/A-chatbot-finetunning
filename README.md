<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Generative Model Training Script</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
        }
        h1, h2 {
            color: #333;
            border-bottom: 1px solid #333;
            padding-bottom: 5px;
        }
        h3 {
            margin-top: 20px;
            color: #555;
        }
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            border-radius: 5px;
            overflow-x: auto;
        }
        p {
            margin-bottom: 10px;
        }
        code {
            background-color: #f0f0f0;
            padding: 2px 5px;
            border-radius: 3px;
            font-family: Consolas, Monaco, 'Andale Mono', monospace;
        }
        ul {
            list-style-type: disc;
            margin-left: 20px;
        }
    </style>
</head>
<body>
    <h1>Generative Model Training Script</h1>
    <p>Bu Python betiği, transformer tabanlı bir dil modeli kullanarak özelleştirilmiş generatif modeller oluşturmayı sağlar. Aşağıda betiğin detaylı açıklamaları bulunmaktadır:</p>
    
    <h2>Kod Açıklamaları</h2>
    
    <h3>Model Yükleme ve Hazırlama</h3>
    <p>Transformer tabanlı bir dil modeli ve tokenizer yüklenir. Bu adımda <code>AutoTokenizer</code> ve <code>AutoModelForCausalLM</code> kullanılır.</p>
    
    <h3>Prompt Oluşturma</h3>
    <p>Verilen girdi başlığına dayalı olarak prompt oluşturmak için tokenize işlemi yapılır ve modelin <code>generate</code> fonksiyonu çağrılır.</p>
    
    <h3>PEFT Entegrasyonu</h3>
    <p>PEFT (Parametric Efficient Fine-Tuning) kütüphanesi kullanılarak modelin PEFT için hazırlanması sağlanır. Model, belirtilen <code>LoraConfig</code> ayarları ile yapılandırılır.</p>
    
    <h3>Veri Kümesi Yükleme ve Hazırlama</h3>
    <p><code>datasets</code> kütüphanesi kullanılarak önceden yüklenmiş bir veri kümesi yüklenir, gerekli formatlama işlemleri yapılır ve eğitim ile test veri kümeleri oluşturulur.</p>
    
    <h3>Model Eğitimi</h3>
    <p>Eğitim için <code>Trainer</code> ve <code>TrainingArguments</code> kullanılarak model eğitimi gerçekleştirilir. Eğitim adımları, hiperparametreler ve kayıt seçenekleri belirtilir.</p>
    
    <h3>Sonuçların Kaydedilmesi</h3>
    <p>Eğitilen model ve oluşturulan prompt sonuçları, belirtilen adımlar ve formatlar doğrultusunda kaydedilir. Ayrıca, eğitim sürecindeki çıktılar sıkıştırılarak arşivlenir.</p>
    
    <h3>PEFT Modelinin Yüklenmesi</h3>
    <p>PEFT ile eğitilen model, belirtilen adımlarla yüklenir ve gerektiğinde yeniden kaydedilir.</p>
</body>
</html>
