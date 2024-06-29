Generative Model Training Script

Bu Python betiği, transformer tabanlı bir dil modeli kullanarak özelleştirilmiş generatif modeller oluşturmayı sağlar. Aşağıda betiğin detaylı açıklamaları bulunmaktadır:

Kod Açıklamaları

Model Yükleme ve Hazırlama
Transformer tabanlı bir dil modeli ve tokenizer yüklenir. Bu adımda AutoTokenizer ve AutoModelForCausalLM kullanılır.

Prompt Oluşturma
Verilen girdi başlığına dayalı olarak prompt oluşturmak için tokenize işlemi yapılır ve modelin generate fonksiyonu çağrılır.

PEFT Entegrasyonu
PEFT (Parametric Efficient Fine-Tuning) kütüphanesi kullanılarak modelin PEFT için hazırlanması sağlanır. Model, belirtilen LoraConfig ayarları ile yapılandırılır.

Veri Kümesi Yükleme ve Hazırlama
datasets kütüphanesi kullanılarak önceden yüklenmiş bir veri kümesi yüklenir, gerekli formatlama işlemleri yapılır ve eğitim ile test veri kümeleri oluşturulur.

Model Eğitimi
Eğitim için Trainer ve TrainingArguments kullanılarak model eğitimi gerçekleştirilir. Eğitim adımları, hiperparametreler ve kayıt seçenekleri belirtilir.

Sonuçların Kaydedilmesi
Eğitilen model ve oluşturulan prompt sonuçları, belirtilen adımlar ve formatlar doğrultusunda kaydedilir. Ayrıca, eğitim sürecindeki çıktılar sıkıştırılarak arşivlenir.

PEFT Modelinin Yüklenmesi
PEFT ile eğitilen model, belirtilen adımlarla yüklenir ve gerektiğinde yeniden kaydedilir.

