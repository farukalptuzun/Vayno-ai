# Vayno AI LLM

## 📋 Proje Amacı ve Genel Bakış

Vayno AI LLM, kişiselleştirilmiş fitness ve beslenme asistanı sunan, Claude 3.5 Sonnet ve OpenAI GPT-4 tabanlı, modüler ve ölçeklenebilir bir yapay zeka katmanıdır. Kullanıcıya özel yemek planı, egzersiz programı, yiyecek analizi, su takibi, akıllı bildirimler ve dinamik plan güncellemeleri gibi fonksiyonlar sunar. Proje, FastAPI ile geliştirilmiş olup, modern API mimarisi ve mikroservis yaklaşımıyla tasarlanmıştır.

## ✨ Özellikler

- 🍽️ **Kişiselleştirilmiş Yemek Planı Oluşturma**
- 🥗 **Mutfakta Bulunan Malzemelerden Yemek Tarifi Oluşturma**
- 🖼️ **AI Destekli Görsel Üretimi** (OpenAI DALL-E / Google Gemini)
- 🏋️ **Kişiye Özel Egzersiz Programı**
- 🔍 **Yiyecek Analizi ve Günlük Takip**
- 💧 **Su Tüketimi Takibi ve Hedef Belirleme**
- 💬 **AI Chat Asistanı (Claude 3.5 Sonnet/OpenAI GPT-4)**
- 📊 **Dinamik Plan Güncellemeleri ve Optimizasyon**
  - Full plan update (tüm planı yeniden oluşturma)
  - Partial plan update (sadece belirli öğünleri güncelleme)
- 🔔 **Akıllı Bildirimler ve Hatırlatıcılar**
- 📈 **İlerleme ve Hedef Takibi**
- 🤖 **Çoklu LLM Sağlayıcı Desteği (Claude/OpenAI)**
- 🎤 **Ses İşleme Entegrasyonu** (Whisper - harici GPU sunucusu)

## 🏗️ Mimari ve Teknoloji Stack

### Temel Bileşenler

- **FastAPI**: RESTful API ve servis yönetimi
- **MongoDB Atlas**: Kullanıcı context'i, plan verileri, geçmiş kayıtları
- **Claude 3.5 Sonnet & OpenAI GPT-4**: LLM tabanlı öneri ve analizler
- **Docker & Docker Compose**: Kolay dağıtım ve izole çalışma ortamı
- **Pytest**: Otomatik testler ve entegrasyon
- **Motor (Async MongoDB Driver)**: Asenkron MongoDB işlemleri
- **Google Gemini**: Görsel analizi için (opsiyonel)

### Harici Servisler ve Entegrasyonlar

#### 🎤 Whisper (Ses İşleme)
- **Konum**: Ayrı bir GPU sunucusunda çalışıyor
- **Kullanım**: Ses kayıtlarını metne dönüştürme
- **Bağlantı**: HTTP/HTTPS üzerinden API çağrısı ile entegre
- **Not**: Bu servis, chatbot uygulaması tarafından kullanılmak üzere harici olarak host edilir

#### 🔗 Ngrok (Canlı Yayın)
- **Amaç**: Yerel geliştirme ortamını canlı sunucuya bağlama
- **Kullanım**: `ngrok` ile localhost:8000'i public URL'e expose etme
- **Endpoint**: Canlı chatbot uygulaması bu URL üzerinden API'ye bağlanır

#### 💬 Chatbot Entegrasyonu
- **Bağlantı**: Başka bir projeden canlı olarak bağlanıyor
- **Protokol**: RESTful API üzerinden HTTP/HTTPS
- **Endpoint'ler**: `/api/chat/*` endpoint'leri chatbot tarafından kullanılıyor
- **Context Yönetimi**: MongoDB üzerinden kullanıcı context'i paylaşılıyor

## 🔄 Sistem Mimarisi ve Veri Akışı

```
┌─────────────────────────────────────────────────────────────┐
│                    Chatbot Uygulaması                        │
│                  (Başka bir proje - Canlı)                   │
└────────────────────┬────────────────────────────────────────┘
                     │ HTTP/HTTPS (Ngrok URL)
                     │
┌────────────────────▼────────────────────────────────────────┐
│              Vayno AI LLM (Bu Proje)                        │
│  ┌────────────────────────────────────────────────────┐    │
│  │  FastAPI Server (Port: 8000)                       │    │
│  │  - Meal Plan Generation                            │    │
│  │  - Workout Plan Generation                         │    │
│  │  - Chat Processing                                 │    │
│  │  - Food Analysis                                   │    │
│  │  - Plan Updates (Full & Partial)                   │    │
│  └────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │  LLM Layer                                         │    │
│  │  - Claude 3.5 Sonnet (Primary)                     │    │
│  │  - OpenAI GPT-4 (Alternative)                      │    │
│  │  - Google Gemini (Image Analysis)                  │    │
│  └────────────────────────────────────────────────────┘    │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│              MongoDB Atlas (Cloud)                          │
│  - User Contexts                                            │
│  - Meal Plans                                               │
│  - Workout Plans                                            │
│  - Conversation History                                     │
│  - Progress Data                                            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│         Whisper Server (Harici GPU Sunucusu)                │
│              - Speech-to-Text Processing                    │
└─────────────────────────────────────────────────────────────┘
```

## 📦 Kurulum

### 1. Gerekli Bağımlılıklar

- Python 3.11+
- MongoDB Atlas hesabı (Cloud) veya lokal MongoDB
- Claude API anahtarı (zorunlu)
- OpenAI API anahtarı (isteğe bağlı - görsel üretimi için)
- Google Gemini API anahtarı (isteğe bağlı - görsel analizi için)
- Docker & Docker Compose (opsiyonel)

### 2. Hızlı Başlangıç (Geliştirici Modu)

```bash
# Proje dizinine girin
git clone <repo-url>
cd vayno-ai

# Virtual environment oluşturun
python3.11 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Bağımlılıkları yükleyin
pip install -r requirements.txt

# Ortam değişkenlerini ayarlayın
cp .env.example .env
# .env dosyasını düzenleyin ve API anahtarlarınızı girin

# Sunucuyu başlatın
python run.py
```

### 3. Docker ile Çalıştırma

```bash
# .env dosyasını oluşturun ve düzenleyin
cp .env.example .env

# Container'ları build edip başlatın
docker-compose up --build

# Arka planda çalıştırmak için
docker-compose up -d --build

# Logları görmek için
docker-compose logs -f

# Container'ı durdurmak için
docker-compose down
```

- **API**: [http://localhost:8000](http://localhost:8000)
- **Swagger UI**: [http://localhost:8000/docs](http://localhost:8000/docs)
- **Health Check**: [http://localhost:8000/health](http://localhost:8000/health)

### 4. Ngrok ile Canlı Yayın

```bash
# Ngrok'u yükleyin (macOS)
brew install ngrok

# Ngrok'u başlatın (port 8000'i expose et)
ngrok http 8000

# Ngrok size bir public URL verecek (örn: https://abc123.ngrok.io)
# Bu URL'i chatbot uygulamanızda kullanın
```

**Not**: Ngrok ücretsiz planında URL her yeniden başlatmada değişir. Production için ngrok'un ücretli planını veya kendi domain'inizi kullanın.

### 5. .env Ayarları

`.env` dosyasında olması gereken başlıca değişkenler:

```env
# API Keys (Zorunlu)
CLAUDE_API_KEY=sk-ant-xxx-xxx
SECRET_KEY=super-secret-key-change-in-production

# API Keys (Opsiyonel)
OPENAI_API_KEY=sk-xxx  # Görsel üretimi için
GOOGLE_API_KEY=xxx     # Gemini görsel analizi için

# MongoDB Configuration
MONGODB_URL=mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority
MONGODB_DBNAME=test

# Application Settings
DEBUG=true
LOG_LEVEL=INFO
HOST=0.0.0.0
PORT=8000

# LLM Default Settings
DEFAULT_LLM_PROVIDER=claude
MAX_TOKENS=2000
TEMPERATURE=0.5
```

## 🗂️ Proje Yapısı

```
vayno-ai/
├── src/
│   ├── api/
│   │   ├── main.py                 # FastAPI uygulaması ve route tanımlamaları
│   │   ├── middleware/             # Auth, validation, rate limiting middleware'leri
│   │   └── routes/                 # API endpoint'leri
│   │       ├── meal_plan.py        # Yemek planı oluşturma ve yönetimi
│   │       ├── workout_plan.py     # Egzersiz planı oluşturma
│   │       ├── chat.py             # AI chat endpoint'leri
│   │       ├── food_analysis.py    # Yiyecek analizi
│   │       ├── water_tracking.py   # Su takibi
│   │       ├── plan_updates.py     # Plan güncelleme (full & partial)
│   │       ├── notifications.py    # Bildirim yönetimi
│   │       └── recipe_plan.py      # Tarif oluşturma ve görsel üretimi
│   ├── core/
│   │   ├── llm/                    # LLM client ve prompt yönetimi
│   │   │   ├── llm_client.py       # Claude/OpenAI client wrapper
│   │   │   ├── prompt_manager.py   # Prompt şablonları
│   │   │   └── response_parser.py  # LLM response parsing
│   │   ├── processors/             # İş mantığı katmanı
│   │   │   ├── meal_processor.py
│   │   │   ├── workout_processor.py
│   │   │   ├── chat_processor.py
│   │   │   ├── food_processor.py
│   │   │   ├── plan_updates_processor.py  # Full & partial updates
│   │   │   └── recipe_processor.py
│   │   ├── context/                # Context yönetimi
│   │   │   └── context_manager.py  # MongoDB tabanlı context manager
│   │   └── utils/                  # Yardımcı fonksiyonlar
│   │       ├── calculators.py      # Beslenme ve fitness hesaplamaları
│   │       ├── constraints.py      # Kısıtlama yönetimi
│   │       └── validators.py       # Veri doğrulama
│   ├── config/
│   │   ├── settings.py             # Uygulama ayarları
│   │   └── logging.py              # Logging konfigürasyonu
│   └── data/
│       ├── prompts/                # Prompt şablonları
│       ├── templates/              # Response şablonları
│       └── cache/                  # Cache klasörü
├── tests/                          # Test dosyaları
├── docker/
│   └── Dockerfile                  # Docker image tanımı
├── docker-compose.yml              # Docker Compose konfigürasyonu
├── requirements.txt                # Python bağımlılıkları
├── run.py                          # Uygulama başlatma scripti
└── README.md                       # Bu dosya
```

## 📡 API Endpoint'leri

### 1. Yemek Planı Modülü

#### Full Meal Plan Generation
```http
POST /api/meal-plan/generate
Content-Type: application/json

{
  "user_id": "001_user",
  "profile": {
    "age": 30,
    "gender": "female",
    "weight": 65,
    "height": 165,
    "activity_level": "moderate",
    "goal": "weight_loss"
  },
  "preferences": {
    "diet_type": "vegetarian",
    "allergies": ["nuts"],
    "excluded_foods": ["fish"]
  },
  "duration_weeks": 4
}
```

#### Partial Meal Plan Update
```http
POST /api/plan-updates/update/meal-plan/partial
Content-Type: application/json

{
  "user_id": "001_user",
  "target": {
    "by": "global_index",
    "day_index": 2
  },
  "meals": ["breakfast"],
  "mode": "prompt",
  "prompt": "Bol proteinli kerevizli kahvaltı; toplam ~400 kalori",
  "update_triggers": ["user_request", "partial_update"]
}
```

**Not**: Partial update için `/update/meal-plan/partial` endpoint'ini kullanın. `/update/meal-plan` endpoint'i **full update** yapar.

- `GET /api/meal-plan/health` : Servis sağlık durumu

### 2. Malzemelerden Yemek Tarifi ve Görsel Oluşturma Modülü

- `POST /api/recipes/generate` : Malzemelere göre yemek tarifi oluşturur
- `POST /api/recipes/generate_with_image` : Tarif + görsel üretir (OpenAI DALL-E)
- `POST /api/recipes/favorite/{recipe_id}` : Favorilere ekleme
- `GET /api/recipe/favorite/me` : Kullanıcı favorileri
- `GET /api/recipes/health` : Servis sağlık durumu

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

```http
POST /api/chat/message
Content-Type: application/json

{
  "user_id": "001_user",
  "message": "Bugün ne yemem gerekiyor?",
  "context_type": "meal_planning",
  "user_profile": {
    "name": "Zeynep",
    "goal": "weight_loss"
  }
}
```

- `POST /api/chat/suggestions` : Proaktif öneriler
- `POST /api/chat/motivation` : Motivasyon mesajı
- `GET /api/chat/history/{user_id}` : Sohbet geçmişi
- `GET /api/chat/health` : Servis sağlık durumu

### 7. Plan Güncellemeleri Modülü

#### Full Plan Update
```http
POST /api/plan-updates/update/meal-plan
Content-Type: application/json

{
  "user_id": "001_user",
  "update_triggers": ["user_request", "performance_update"],
  "new_requirements": {
    "constraints": {
      "calorie_adjustment": "reduce_100_calories",
      "macro_rebalancing": "increase_protein"
    }
  }
}
```

#### Partial Plan Update
```http
POST /api/plan-updates/update/meal-plan/partial
Content-Type: application/json

{
  "user_id": "001_user",
  "target": {
    "by": "week_day",
    "day_name": "monday"
  },
  "meals": ["breakfast", "lunch"],
  "mode": "prompt",
  "prompt": "Daha hafif ve protein ağırlıklı öğünler"
}
```

- `POST /api/plan-updates/analyze` : Plan performans analizi
- `POST /api/plan-updates/update/workout-plan` : Egzersiz planı güncelle (full)
- `POST /api/plan-updates/update/workout-plan/partial` : Egzersiz planı güncelle (partial)
- `GET /api/plan-updates/check-updates/{user_id}` : Otomatik güncelleme kontrolü
- `POST /api/plan-updates/optimize` : Optimizasyon önerileri
- `GET /api/plan-updates/update-history/{user_id}` : Güncelleme geçmişi
- `GET /api/plan-updates/health` : Servis sağlık durumu

### 8. Bildirimler Modülü

- `POST /api/notifications/create` : Bildirim oluştur
- `POST /api/notifications/schedule-recurring` : Tekrarlayan bildirim planla
- `GET /api/notifications/user/{user_id}` : Kullanıcı bildirimleri
- `POST /api/notifications/send-immediate` : Anında bildirim gönder
- `POST /api/notifications/smart-generate` : Akıllı bildirim üret
- `GET /api/notifications/health` : Servis sağlık durumu

## 🔧 Nasıl Çalışır?

### 1. Context Yönetimi (MongoDB Tabanlı)

Kullanıcı verileri MongoDB Atlas'da saklanır:
- **Koleksiyon**: `contexts`
- **Unique Index**: `user_id` üzerinde
- **TTL Index**: `last_updated` alanında (24 saat)
- **İçerik**: Kullanıcı profili, mevcut planlar, konuşma geçmişi, ilerleme verileri

```python
# Context örneği
{
  "user_id": "001_user",
  "profile": {...},
  "current_plans": {
    "meal_plans": {
      "plan_id_123": {...},
      "plan_id_456": {...}
    },
    "workout_plans": {...}
  },
  "conversation_history": [...],
  "progress_data": {...}
}
```

### 2. LLM İşleme Akışı

1. **Request Alınır**: API endpoint'i request'i alır
2. **Context Çekilir**: MongoDB'den kullanıcı context'i alınır
3. **Prompt Oluşturulur**: `prompt_manager` ile kullanıcıya özel prompt oluşturulur
4. **LLM'ye Gönderilir**: Claude veya OpenAI'ye istek gönderilir
5. **Response Parse Edilir**: `response_parser` ile JSON çıkarılır
6. **Context Güncellenir**: Yeni veriler MongoDB'ye kaydedilir
7. **Response Döndürülür**: Kullanıcıya formatlanmış response gönderilir

### 3. Plan Güncelleme Mekanizması

#### Full Update:
- Tüm plan yeniden oluşturulur
- LLM mevcut planı analiz eder
- Yeni plan üretilir
- MongoDB'ye kaydedilir
- Context güncellenir

#### Partial Update:
- Sadece belirtilen öğün(ler) güncellenir
- Hedef öğün belirlenir (`day_index`, `meal_type`)
- Prompt ile veya manuel olarak yeni öğün üretilir
- Plan içinde sadece o öğün değiştirilir
- MongoDB'ye kaydedilir

### 4. Chatbot Entegrasyonu

Chatbot uygulaması:
1. Kullanıcı mesajını alır
2. Whisper ile ses kaydını metne dönüştürür (harici GPU sunucusu)
3. Metni bu API'ye gönderir (`/api/chat/message`)
4. API response'u alır
5. Kullanıcıya gösterir

## 🧪 Testler

Tüm modüller için kapsamlı testler mevcuttur:

```bash
# Virtual environment aktif edin
source venv/bin/activate

# Tüm testleri çalıştırın
pytest

# Belirli bir modülü test edin
pytest tests/test_meal_plan.py

# Coverage raporu ile
pytest --cov=src tests/
```

Docker ortamında:
```bash
docker-compose exec app pytest
```

## 📦 Bağımlılıklar

`requirements.txt` içeriği:

```
fastapi==0.104.1
uvicorn[standard]==0.24.0
pydantic==2.5.0
python-multipart==0.0.6
python-dotenv==1.0.0
openai==1.3.7
anthropic>=0.20.0
httpx==0.25.2
pytest==7.4.3
pytest-asyncio==0.21.1
aiofiles==23.2.1
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
pydantic-settings==2.1.0
requests>=2.32.0
motor==3.4.0           # Async MongoDB driver
pymongo==4.6.1         # MongoDB Python client
google-generativeai>=0.3.0  # Google Gemini API
Pillow>=10.0.0         # Image processing
boto3>=1.28.0          # AWS SDK (opsiyonel)
```

## 🐳 Docker

### Dockerfile Yapısı
- **Base Image**: `python:3.11-slim`
- **Working Directory**: `/app`
- **Port**: `8000`
- **Command**: `python run.py`

### Docker Compose
- **Service**: `app` (vayno-ai container)
- **Port Mapping**: `8000:8000`
- **Environment**: `.env` dosyasından yüklenir
- **Health Check**: `/docs` endpoint'i üzerinden

## 🔌 Harici Entegrasyonlar

### Whisper Servisi
- **Konum**: Ayrı GPU sunucusu
- **Protokol**: HTTP/HTTPS
- **Kullanım**: Chatbot uygulaması tarafından doğrudan kullanılıyor
- **Not**: Bu API ile doğrudan entegre değil, chatbot üzerinden geliyor

### Chatbot Uygulaması
- **Bağlantı**: Ngrok URL üzerinden
- **Endpoint'ler**: `/api/chat/*` endpoint'leri kullanılıyor
- **Context**: MongoDB üzerinden paylaşılıyor
- **Real-time**: WebSocket veya HTTP polling ile

## 🔐 Güvenlik

- **API Keys**: `.env` dosyasında saklanır, asla commit edilmez
- **CORS**: Tüm origin'lere açık (production'da kısıtlanmalı)
- **Secret Key**: JWT token'lar için kullanılır
- **MongoDB**: MongoDB Atlas'ın güvenlik özellikleri kullanılır

## 📊 Monitoring ve Logging

- **Health Check**: `/health` endpoint'i servis durumunu kontrol eder
- **Logging**: Python logging modülü kullanılır
- **Log Level**: `INFO` (production'da `WARNING` önerilir)
- **MongoDB Health**: Connection ping ile kontrol edilir
- **LLM Health**: Provider health check ile kontrol edilir

## 🚀 Production Deployment

### Öneriler:
1. **Ngrok Yerine**: Kendi domain'inizi kullanın veya ngrok'un ücretli planını
2. **Environment Variables**: Tüm hassas bilgileri environment variable olarak saklayın
3. **CORS**: Sadece gerekli origin'lere izin verin
4. **Rate Limiting**: Implement edilmiş middleware'i aktif edin
5. **HTTPS**: SSL/TLS sertifikası kullanın
6. **Monitoring**: Application monitoring tools kullanın (Sentry, DataDog, vb.)
7. **Logging**: Centralized logging solution kullanın
8. **Backup**: MongoDB Atlas otomatik backup kullanır

## 🤝 Katkı ve Geliştirme

### Kod Stili
- **PEP8** standartlarına uyun
- **Type Hints** kullanın
- **Docstrings** ekleyin

### Commit Mesajları
- Anlaşılır ve açıklayıcı olun
- Örnek: `feat: Add partial meal plan update endpoint`

### Test Yazma
- Her yeni özellik için test yazın
- Test coverage'ı %80+ tutmaya çalışın

## ❓ Sıkça Sorulan Sorular

### Claude API anahtarı zorunlu mu?
**Evet**, Claude (Anthropic) API anahtarı olmadan LLM fonksiyonları çalışmaz. Bu anahtar `.env` dosyasında `CLAUDE_API_KEY` olarak tanımlanmalıdır.

### MongoDB bağlantısı nasıl yapılır?
MongoDB Atlas cloud üzerinde çalışıyor. Bağlantı string'i `.env` dosyasında `MONGODB_URL` olarak tanımlanmalıdır. Lokal MongoDB kullanmak isterseniz, connection string'i değiştirin.

### Redis kullanılıyor mu?
**Hayır**, Redis projeden kaldırıldı. Tüm context yönetimi MongoDB üzerinden yapılıyor.

### Partial update ile full update arasındaki fark nedir?
- **Full Update** (`/update/meal-plan`): Tüm planı yeniden oluşturur
- **Partial Update** (`/update/meal-plan/partial`): Sadece belirtilen öğün(ler)i günceller

### Ngrok URL'i her seferinde değişiyor, nasıl kalıcı yapabilirim?
Ngrok'un ücretli planını satın alarak veya kendi domain'inizi kullanarak kalıcı URL elde edebilirsiniz.

### Whisper entegrasyonu nasıl çalışıyor?
Whisper ayrı bir GPU sunucusunda çalışıyor ve chatbot uygulaması tarafından doğrudan kullanılıyor. Bu API ile doğrudan entegre değil.

## 📝 Changelog

### v1.0.0
- İlk stabil sürüm
- MongoDB tabanlı context yönetimi
- Redis kaldırıldı
- Partial meal plan update özelliği eklendi
- MongoDB persistence eklendi (plan updates için)
- Full & partial update endpoint'leri ayrıştırıldı



## 👥 Ekip ve İletişim

- **Proje**: Vayno AI LLM
- **Versiyon**: 1.0.0
- **Destek**: tarvinayazılım

---

**Not**: Bu dokümantasyon sürekli güncellenmektedir. Son güncelleme: 2025-10-21
