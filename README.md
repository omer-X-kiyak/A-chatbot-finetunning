<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Generative Model Training Script</title>
</head>
<body>
    <h1>Generative Model Training Script</h1>
    <p>Bu Python betiği, transformer tabanlı bir dil modelini kullanarak özelleştirilmiş generatif modeller oluşturmayı amaçlar. Aşağıda betiğin ana özellikleri ve işlevleri ayrıntılı olarak açıklanmaktadır:</p>
    
    <h2>Özellikler</h2>
    <ul>
        <li><strong>Model Yükleme ve Hazırlama:</strong> <code>AutoTokenizer</code> ve <code>AutoModelForCausalLM</code> kullanarak transformer tabanlı dil modelleri yüklenir ve hazırlanır. Bu sayede, modelin prompt oluşturma yetenekleri etkinleştirilir.</li>
        <li><strong>Prompt Oluşturma:</strong> Verilen girdi başlığına dayalı olarak prompt oluşturmak için tokenize işlemi ve modelin <code>generate</code> fonksiyonu kullanılır. Bu işlev, modelin belirli bir başlığa uygun metinler üretmesini sağlar.</li>
        <li><strong>PEFT Entegrasyonu:</strong> <code>PEFT</code> (Parameter Efficient Fine-Tuning) kütüphanesi kullanılarak model için LoraConfig ayarlanır ve model PEFT ile eğitilmeye hazırlanır. Bu yöntem, modelin daha az parametre ile verimli bir şekilde eğitilmesine olanak tanır.</li>
        <li><strong>Veri Kümesi Yükleme ve Hazırlama:</strong> <code>datasets</code> kütüphanesi kullanılarak veri kümesi yüklenir, gerekli formatlama işlemleri yapılır ve eğitim ile test veri kümeleri oluşturulur. Bu işlem, modelin eğitimi için gerekli verilerin düzenli bir şekilde hazırlanmasını sağlar.</li>
        <li><strong>Model Eğitimi:</strong> <code>Trainer</code> ve <code>TrainingArguments</code> kullanılarak model eğitimi gerçekleştirilir. Eğitim süreci, modelin belirli görevlerde performansını artırmayı hedefler.</li>
        <li><strong>Sonuçların Kaydedilmesi:</strong> Eğitilen model ve oluşturulan prompt sonuçları kaydedilir ve sıkıştırılır. Bu adım, eğitilen modelin ve üretilen metinlerin daha sonra kullanılabilmesi için saklanmasını sağlar.</li>
        <li><strong>PEFT Modelinin Yüklenmesi:</strong> PEFT modelinin önceden eğitilmiş model ile yüklenmesi
