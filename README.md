# Vayno AI LLM

## Proje Amacı ve Genel Bakış

Vayno AI LLM, kişiselleştirilmiş fitness ve beslenme asistanı sunan, Claude 3.5 Sonnet ve OpenAI GPT-4 tabanlı, modüler ve ölçeklenebilir bir yapay zeka katmanıdır. Kullanıcıya özel yemek planı, egzersiz programı, yiyecek analizi, su takibi, akıllı bildirimler ve dinamik plan güncellemeleri gibi fonksiyonlar sunar. Proje, FastAPI ile geliştirilmiş olup, modern API mimarisi ve mikroservis yaklaşımıyla tasarlanmıştır.

## Özellikler

- 🍽️ **Kişiselleştirilmiş Yemek Planı Oluşturma**
- 🥗 **Mutfakta Bulunan Malzemelerden Yemek Tarifi Oluşturma**
- 🏋️ **Kişiye Özel Egzersiz Programı**
- 🔍 **Yiyecek Analizi ve Günlük Takip**
- 💧 **Su Tüketimi Takibi ve Hedef Belirleme**
- 💬 **AI Chat Asistanı (Claude 3.5 Sonnet/OpenAI GPT-4)**
- 📊 **Dinamik Plan Güncellemeleri ve Optimizasyon**
- 🔔 **Akıllı Bildirimler ve Hatırlatıcılar**
- 📈 **İlerleme ve Hedef Takibi**
- 🤖 **Çoklu LLM Sağlayıcı Desteği (Claude/OpenAI)**

## Mimarinin Temel Bileşenleri

- **FastAPI**: RESTful API ve servis yönetimi
- **Redis**: Kullanıcı durumu, önbellek ve hızlı veri erişimi
- **Claude 3.5 Sonnet & OpenAI GPT-4**: LLM tabanlı öneri ve analizler
- **Docker**: Kolay dağıtım ve izole çalışma ortamı
- **Pytest**: Otomatik testler ve entegrasyon
- 

## Ana Modüller ve API Endpointleri

### 1. Yemek Planı Modülü
- `POST /api/meal-plan/generate` : Kişiye özel yemek planı oluşturur
- `GET /api/meal-plan/health` : Servis sağlık durumu
- 

### 2. Malzemelerden Yemek Tarifi ve Görsel Oluşturma Modülü
- `POST /api/recipes/generate` : Kişinin elinde bulunan malzemelere göre yemek tarifi oluşturur
- `POST /api/recipes/generate_with_image` : Kişinin elinde bulunana malzemelerden yemek tarifi ve görsel üretir.
- `POST /api/recipes/favorite/{recipe_id} ` : Oluşan yemeği favorilerine ekleyebilir.
- `GET /api/recipe/favorite/me` : Kullanının favorilerine aldığı yemekler gösterilir
- `GET /api/recipes/health` : Servis sağlık durumu
- 

### 3. Egzersiz Planı Modülü
- `POST /api/workout-plan/generate` : Kişiye özel egzersiz planı oluşturur
- `GET /api/workout-plan/health` : Servis sağlık durumu

### 4. Yiyecek Analizi Modülü
- `POST /api/food-analysis/analyze` : Yiyecek girdisi analiz eder
- `POST /api/food-analysis/log-meal` : Öğün kaydeder ve analiz eder
- `GET /api/food-analysis/daily-summary/{user_id}` : Günlük özet
- `GET /api/food-analysis/health` : Servis sağlık durumu

### 5. Su Takibi Modülü
- `POST /api/water-tracking/log` : Su tüketimi kaydı
- `POST /api/water-tracking/log-bulk` : Toplu su kaydı
- `GET /api/water-tracking/daily-summary/{user_id}` : Günlük özet
- `POST /api/water-tracking/set-goal` : Günlük hedef belirleme
- `GET /api/water-tracking/health` : Servis sağlık durumu

### 6. AI Chat Modülü
- `POST /api/chat/message` : AI asistan ile sohbet
- `POST /api/chat/suggestions` : Proaktif öneriler
- `POST /api/chat/motivation` : Motivasyon mesajı
- `GET /api/chat/history/{user_id}` : Sohbet geçmişi
- `GET /api/chat/health` : Servis sağlık durumu

### 7. Plan Güncellemeleri Modülü
- `POST /api/plan-updates/analyze` : Plan performans analizi
- `POST /api/plan-updates/update/meal-plan` : Yemek planı güncelle
- `POST /api/plan-updates/update/workout-plan` : Egzersiz planı güncelle
- `GET /api/plan-updates/check-updates/{user_id}` : Otomatik güncelleme kontrolü
- `POST /api/plan-updates/optimize` : Optimizasyon önerileri
- `GET /api/plan-updates/health` : Servis sağlık durumu

### 8. Bildirimler Modülü
- `POST /api/notifications/create` : Bildirim oluştur
- `POST /api/notifications/schedule-recurring` : Tekrarlayan bildirim planla
- `GET /api/notifications/user/{user_id}` : Kullanıcı bildirimleri
- `POST /api/notifications/send-immediate` : Anında bildirim gönder
- `POST /api/notifications/smart-generate` : Akıllı bildirim üret
- `GET /api/notifications/health` : Servis sağlık durumu

## Testler

Tüm modüller için kapsamlı testler mevcuttur. Testleri çalıştırmak için:

```bash
source venv/bin/activate
pytest
```
veya Docker ortamında:
```bash
docker-compose exec app pytest
```

## Bağımlılıklar

`requirements.txt` içeriği:

```
fastapi==0.104.1
uvicorn[standard]==0.24.0
pydantic==2.5.0
python-multipart==0.0.6
python-dotenv==1.0.0
redis==5.0.1
openai==1.3.7
anthropic>=0.20.0
httpx==0.25.2
pytest==7.4.3
pytest-asyncio==0.21.1
aiofiles==23.2.1
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
pydantic-settings==2.1.0
```

## Katkı ve Geliştirme

- Kod stili: [PEP8](https://peps.python.org/pep-0008/), `black` ile otomatik formatlama
- Testler: `pytest`, `pytest-asyncio`
- Geliştirici bağımlılıkları için: `pip install -e .[dev]`

## Sıkça Sorulan Sorular

- **Claude API anahtarı zorunlu mu?**
  - Evet, Claude (Anthropic) API anahtarı olmadan LLM fonksiyonları çalışmaz.
- **Redis olmadan çalışır mı?**
  - Redis bağlantısı yoksa otomatik olarak in-memory fallback kullanılır (test amaçlı).

