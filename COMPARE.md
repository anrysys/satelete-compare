# Сравнение кандидатов для системы управления сателлитами

### Кандидат 1: **WordPress Multisite (Headless) + WPGraphQL + FaustWP + Next.js**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      WordPress Multisite Network        │
│   (Централизованное управление сайтами) │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│         MySQL Database                   │
│  (Shared tables + site-specific tables) │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Kubernetes Cluster (k3s/k8s)       │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │Satellite││
│  │Next.js#1│  │Next.js#2│  │Next.js#3││
│  │(Faust.js)│  │(Faust.js)│  │(Faust.js)││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **Mature ecosystem** с тысячами готовых плагинов и тем
- **Built-in multisite** нативная поддержка множественных сайтов
- **WPGraphQL** для современного API подхода
- **FaustWP Plugin** для seamless headless интеграции
- **Familiar admin interface** знакомый всем интерфейс

**Технический стек:**

- **Backend**: WordPress 6+ с Multisite
- **Database**: MySQL/MariaDB с shared структурой
- **Frontend**: Next.js 15+ с Faust.js framework
- **API**: WPGraphQL + REST API
- **Deploy**: Docker + Kubernetes
- **Cache**: Redis + Cloudflare

**🗄️ Multi-tenancy стратегия:**

🔵 **Одна общая база данных** с логическим разделением:

- ✅ **Shared Database, Shared Schema** архитектура
- ✅ WordPress Multisite использует префиксы таблиц (`wp_1_posts`, `wp_2_posts`, etc.)
- ✅ Общие таблицы: `wp_blogs`, `wp_users`, `wp_site`, `wp_sitemeta`
- ✅ Индивидуальные таблицы для каждого сайта: `wp_{blog_id}_posts`, `wp_{blog_id}_options`
- ❌ Ограниченная изоляция данных (все в одной БД)
- ❌ Невозможность независимого масштабирования отдельных сателлитов
- ❌ Риск cascading failures при проблемах с БД

**Преимущества:**

- ✅ Огромная экосистема плагинов и тем
- ✅ Знакомый интерфейс для контент-менеджеров
- ✅ Встроенная multi-tenancy с Multisite
- ✅ SEO-оптимизация из коробки (Yoast, RankMath)
- ✅ Большое сообщество разработчиков
- ✅ Проверенная временем стабильность
- ✅ Отличная интеграция с существующими WP сайтами
- ✅ Превосходная локализация - встроенная поддержка многоязычности, тысячи переводов, WPML/Polylang плагины
- ✅ Максимальная расширяемость - 60,000+ плагинов, hooks/filters система, custom post types, shortcodes

**Недостатки:**

- ❌ Legacy код и технический долг
- ❌ Проблемы с производительностью при больших нагрузках
- ❌ Сложности с современными DevOps практиками
- ❌ Уязвимости безопасности (частые обновления)
- ❌ PHP не всегда подходит для современной разработки
- ❌ Полное отсутствие gRPC
- ❌ Монолитная MySQL БД - отсутствие распределенного хранения, ограниченное горизонтальное масштабирование
- ❌ Устаревающая архитектура - Legacy PHP код, устаревшие практики, медленное принятие современных стандартов
- ❌ Слабая DDoS защита - монолитная архитектура уязвима, зависит от внешних решений (Cloudflare)

**Обязательства по безопасности и обновлениям:**

🔒 **Критически важные обновления:**

- WordPress Core обновления каждые 2-4 недели (автоматические минорные патчи)
- Плагины требуют еженедельного мониторинга (особенно WooCommerce, Yoast SEO)
- Темы обновляются по мере выхода security patches
- PHP версия должна обновляться каждые 6-12 месяцев для поддержки безопасности

🛡️ **Постоянный мониторинг:**

- Wordfence/Sucuri для real-time защиты от атак
- Ежедневные бэкапы всех сайтов мультисети
- Еженедельный аудит плагинов на CVE уязвимости
- Мониторинг failed login attempts и suspicious activity

📋 **Обязательный график обновлений:**

- **Немедленно**: Critical security patches (0-24 часа)
- **Еженедельно**: Плагины и темы с security fixes
- **Ежемесячно**: Minor WordPress Core updates
- **Ежеквартально**: Major WordPress версии (после тестирования на staging)
- **Раз в полгода**: PHP/MySQL обновления

⚠️ **Риски при несоблюдении:**

- Высокий риск взлома через известные уязвимости
- Возможность заражения malware всей мультисети
- SEO штрафы за compromised content
- Потеря данных при успешных атаках

📊 **GDPR Compliance:**

🟡 **Частичная поддержка** - требует дополнительной настройки:

- ✅ WordPress Core: встроенные Privacy Tools (с версии 4.9.6)
- ✅ Автоматическое удаление персональных данных по запросу
- ✅ Export персональных данных в машиночитаемом формате
- ✅ Cookie consent через плагины (Cookie Notice, Complianz)
- ❌ Плагины требуют индивидуальной проверки на GDPR compliance
- ❌ Многие legacy плагины не соответствуют GDPR
- ❌ Необходима настройка data retention политик вручную
- ❌ Tracking и аналитика требуют дополнительных privacy плагинов

🔧 **Требуемые доработки:**
- Настройка GDPR-compliant cookie banners
- Аудит всех плагинов на соответствие privacy требованиям
- Настройка data processing agreements с третьими сторонами
- Документирование data flow и storage policies

**Максимальная масштабируемость:** 🚧 **50-200 сайтов** (ограничено архитектурой MySQL и WordPress Core)

**DDoS устойчивость:** 🛡️ **Низкая** - максимум 1-5 Gbps атак с Cloudflare, без него - критично уязвим

**Сроки разработки:** ⏰ **2-4 недели** - быстрый старт благодаря готовым решениям, настройка Multisite + headless

**Сроки MVP/PoC:**

- 🚀 **PoC**: 3-5 дней - развертывание с готовыми темами и плагинами
- 📦 **MVP**: 1-2 недели - базовая функциональность с 5-10 сателлитами
- 🚀 **Выкат производства**: 2-4 недели - полная настройка, оптимизация, мониторинг

**Лицензирование:** 📜 **GPL v2/v3** - бесплатный open source, но требует открытия модификаций

---

### Кандидат 2: **Payload CMS Multi-Tenant (Headless) + Next.js**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      Payload CMS Admin Panel            │
│   (Управление сателлитами и контентом)  │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│         MongoDB Database                 │
│  (Сателлиты, Страницы, Медиа, SEO)     │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Kubernetes Cluster (k3s/k8s)       │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │Satellite││
│  │Next.js#1│  │Next.js#2│  │Next.js#3││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **TypeScript-first** современная архитектура
- **Code-based configuration** конфигурация через код
- **Built-in authentication** с гибкими ролями
- **Modern admin UI** красивый и быстрый интерфейс
- **Full-stack TypeScript** единообразие во всем стеке

**Технический стек:**

- **Backend**: Payload CMS (Node.js/TypeScript)
- **Database**: MongoDB с Mongoose ODM
- **Frontend**: Next.js 15+ с App Router
- **API**: REST + GraphQL автогенерация
- **Deploy**: Docker + Kubernetes
- **Storage**: Local/S3/CloudStorage

**🗄️ Multi-tenancy стратегия:**

🔵 **Одна общая база данных** с document-level isolation:

- ✅ **Shared Database, Shared Schema** с tenant field
- ✅ MongoDB collections с полем `tenant_id` для каждого документа
- ✅ Payload CMS hooks для автоматической фильтрации по tenant
- ✅ Гибкая схема данных благодаря NoSQL природе
- ✅ Code-first подход к tenant configuration
- ❌ Потенциальные проблемы с производительностью при росте данных
- ❌ Сложность в реализации tenant-specific индексов
- ❌ MongoDB sharding ограничения для multi-tenancy

**Преимущества:**

- ✅ Современная TypeScript архитектура
- ✅ Отличный developer experience
- ✅ Быстрая разработка и прототипирование
- ✅ Гибкая система ролей и разрешений
- ✅ Автоматическая генерация API
- ✅ Встроенная поддержка файлов и медиа
- ✅ Хорошая документация
- ✅ Хорошая расширяемость - hooks система, custom fields, plugins architecture, TypeScript поддержка
- ✅ Современная архитектура - активное развитие, TypeScript-first подход, следование современным стандартам

**Недостатки:**

- ❌ Относительно молодая экосистема
- ❌ Меньше готовых плагинов по сравнению с WP
- ❌ Требует знания TypeScript/Node.js
- ❌ Меньше специалистов на рынке
- ❌ Нет такой большой community поддержки
- ❌ Отсутствие встроенной поддержки gRPC
- ❌ MongoDB ограничения - не является распределенной БД по умолчанию, ограниченное шардирование
- ❌ Ограниченная локализация - базовая поддержка i18n, требует кастомной настройки для сложных случаев

**Обязательства по безопасности и обновлениям:**

🔒 **Современная архитектура безопасности:**

- Payload CMS обновления каждые 4-6 недель (активная разработка)
- Node.js обновления каждые 3-6 месяцев для LTS версий
- TypeScript обновления по мере выхода новых версий
- MongoDB security patches применяются ежемесячно

🛡️ **Встроенная защита:**

- JWT токены с автоматической ротацией
- Built-in RBAC система с детальными правами
- MongoDB Atlas с encryption at rest/transit
- Автоматическое санитизирование данных

📋 **Обязательный график обновлений:**

- **Еженедельно**: npm audit для проверки уязвимостей в dependencies
- **Ежемесячно**: Payload CMS updates
- **Ежеквартально**: Node.js LTS обновления
- **По мере выхода**: Critical security patches для MongoDB
- **Раз в полгода**: Major версии (после regression testing)

⚠️ **Риски TypeScript экосистемы:**

- Быстрая эволюция может привести к breaking changes
- Dependency vulnerabilities в npm пакетах
- Требует постоянного мониторинга security advisories
- Меньше security tooling по сравнению с PHP экосистемой

📊 **GDPR Compliance:**

🟢 **Хорошая поддержка** - современная архитектура с privacy-first подходом:

- ✅ Built-in data encryption и secure authentication
- ✅ Гранулярные права доступа через RBAC систему
- ✅ Автоматическое логирование всех data access operations
- ✅ TypeScript type safety для data handling
- ✅ Configurable data retention policies через код
- ❌ Нет built-in cookie consent management
- ❌ Privacy policy generator требует кастомной реализации
- ❌ Data export/import tools требуют разработки

🔧 **Требуемые доработки:**
- Разработка GDPR-compliant API endpoints для data requests
- Интеграция с cookie consent решениями
- Настройка automated data purging workflows
- Документирование data processing procedures

**Максимальная масштабируемость:** 🔄 **100-500 сайтов** (ограничено MongoDB и отсутствием распределенной архитектуры)

**DDoS устойчивость:** 🛡️ **Умеренная** - до 10-20 Gbps с правильной Kubernetes настройкой + load balancing

**Сроки разработки:** ⏰ **4-8 недель** - разработка TypeScript архитектуры, настройка multi-tenancy, API интеграции

**Сроки MVP/PoC:**

- 🚀 **PoC**: 1-2 недели - создание базовой архитектуры с TypeScript
- 📦 **MVP**: 3-4 недели - полнофункциональная система с админкой
- 🚀 **Выкат производства**: 4-8 недель - тестирование, оптимизация, безопасность

**Лицензирование:** 📜 **MIT License** - полностью бесплатный, можно использовать коммерчески без ограничений

---

### Кандидат 3: **Strapi Headless CMS + Next.js Multi-Tenant**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      Strapi Admin Panel                  │
│   (Управление сателлитами и контентом)  │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│         PostgreSQL Database              │
│  (Multi-tenant с tenant_id разделением) │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Kubernetes Cluster (k3s/k8s)       │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │Satellite││
│  │Next.js#1│  │Next.js#2│  │Next.js#3││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **Enterprise-ready** архитектура с поддержкой RBAC
- **Мощный Content Types Builder** для гибкой структуры контента
- **Multi-environment** поддержка (dev/staging/prod)
- **Plugin ecosystem** с готовыми интеграциями
- **GraphQL + REST API** для максимальной гибкости

**Технический стек:**

- **Backend**: Strapi v4 (Node.js)
- **Database**: PostgreSQL с row-level security
- **Frontend**: Next.js 15+ с App Router
- **API**: GraphQL + REST
- **Deploy**: Docker + Kubernetes
- **CDN**: Cloudflare/AWS CloudFront

**🗄️ Multi-tenancy стратегия:**

🔵 **Одна общая база данных** с row-level security:

- ✅ **Shared Database, Shared Schema** с tenant_id разделением
- ✅ PostgreSQL row-level security (RLS) для изоляции данных
- ✅ Strapi plugins для tenant management
- ✅ SQL-based querying с automatic tenant filtering
- ✅ Enterprise-уровень изоляции данных
- ✅ Поддержка read-реплик для масштабирования чтения
- ❌ Одна точка отказа для всех сателлитов
- ❌ Сложность в миграции отдельных tenant'ов

**Преимущества:**

- ✅ Очень гибкая система ролей и разрешений
- ✅ Мощные возможности кастомизации контента
- ✅ Большое сообщество и экосистема плагинов
- ✅ Отличная документация и поддержка
- ✅ Встроенная поддержка многоязычности
- ✅ Advanced media management с трансформациями
- ✅ PostgreSQL база - поддержка репликации и read-реплик для улучшенной масштабируемости
- ✅ Отличная расширяемость - plugin marketplace, custom controllers, middleware, lifecycle hooks
- ✅ Актуальная платформа - регулярные обновления, современные практики, enterprise поддержка

**Недостатки:**

- ❌ Более сложная настройка по сравнению с Payload
- ❌ Может быть избыточен для простых сайтов
- ❌ Требует больше ресурсов сервера
- ❌ Отсутствие встроенного распределенного хранения - PostgreSQL не является natively distributed БД

**Обязательства по безопасности и обновлениям:**

🔒 **Enterprise-уровень безопасности:**

- Strapi обновления каждые 4-8 недель (стабильные релизы)
- PostgreSQL патчи применяются ежемесячно
- Node.js security updates каждые 2-3 месяца
- Plugin ecosystem с регулярными security audits

🛡️ **Комплексная защита:**

- Built-in RBAC с fine-grained permissions
- PostgreSQL row-level security для multi-tenancy
- Content Security Policy headers из коробки
- Автоматическое rate limiting и DDoS protection

📋 **Обязательный график обновлений:**

- **Еженедельно**: npm/yarn audit для dependencies
- **Ежемесячно**: Strapi minor updates и PostgreSQL patches
- **Ежеквартально**: Major Strapi releases (после staging тестов)
- **Раз в полгода**: Node.js LTS upgrades
- **По мере выхода**: Critical security fixes (0-48 часов)

⚠️ **Enterprise требования:**

- Обязательная настройка WAF (Web Application Firewall)
- Регулярные penetration tests (ежеквартально)
- Compliance audits для GDPR/SOC2 если требуется
- 24/7 мониторинг с alerting при подозрительной активности

📊 **GDPR Compliance:**

🟢 **Отличная поддержка** - enterprise-ready с встроенными privacy features:

- ✅ Built-in data audit trails и access logging
- ✅ Автоматическая data anonymization через plugins
- ✅ PostgreSQL row-level security для data isolation
- ✅ GDPR plugin marketplace с готовыми решениями
- ✅ API endpoints для data export/delete requests
- ✅ Configurable data retention policies
- ✅ Cookie consent integration через plugins (CookieHub, Complianz)
- ❌ Privacy impact assessments требуют manual documentation

🔧 **Enterprise GDPR features:**
- Data Processing Agreements templates
- Automated breach notification workflows
- Data mapping и flow documentation tools
- Integration с external privacy management platforms

**Максимальная масштабируемость:** 📈 **200-800 сайтов** (лучше чем Payload благодаря PostgreSQL, но ограничено отсутствием gRPC)

**DDoS устойчивость:** 🛡️ **Хорошая** - до 50-100 Gbps с enterprise настройками, автомасштабирование в Kubernetes

**Сроки разработки:** ⏰ **6-10 недель** - настройка enterprise функций, создание multi-tenant архитектуры, PostgreSQL оптимизация

**Сроки MVP/PoC:**

- 🚀 **PoC**: 2-3 недели - базовая установка с несколькими content types
- 📦 **MVP**: 4-6 недель - enterprise функции, роли, мультисайт поддержка
- 🚀 **Выкат производства**: 6-10 недель - performance tuning, security audit, monitoring

**Лицензирование:** 📜 **MIT License** + **Enterprise Edition** ($$$) - Community Edition бесплатен, Enterprise с расширенными возможностями

---

### Кандидат 4: **Sanity Headless CMS + Next.js Multi-Tenant**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      Sanity Studio                       │
│   (Real-time контент редактирование)    │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│         Sanity Cloud Database            │
│  (Document-based с живой синхронизацией)│
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Edge Functions (Vercel/Netlify)    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │Satellite││
│  │Next.js#1│  │Next.js#2│  │Next.js#3││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **Real-time collaboration** в режиме реального времени
- **Structured content** с мощной схемой данных
- **GROQ query language** для сложных запросов
- **Built-in CDN** и глобальное распределение
- **Serverless-first** архитектура

**Технический стек:**

- **Backend**: Sanity Cloud (Managed)
- **Database**: Sanity's document database
- **Frontend**: Next.js 15+ с ISR/SSG
- **API**: GROQ + GraphQL
- **Deploy**: Vercel/Netlify Edge Functions
- **Storage**: Sanity Assets с автооптимизацией

**🗄️ Multi-tenancy стратегия:**

🟠 **Гибридная архитектура** - один проект, datasets isolation:

- ✅ **Shared Project, Multiple Datasets** подход
- ✅ Каждый сателлит = отдельный dataset в одном Sanity проекте
- ✅ Programmatic dataset management через Sanity API
- ✅ Cross-dataset queries для shared content
- ✅ Automatic edge caching для каждого dataset
- ✅ Independent content schemas по dataset
- ⚠️ Ограничения Sanity Cloud на количество datasets
- ❌ Vendor lock-in на Sanity управление datasets

**Преимущества:**

- ✅ Невероятно быстрая разработка
- ✅ Real-time updates без WebSockets
- ✅ Excellent developer experience
- ✅ Автоматическая оптимизация изображений
- ✅ Встроенная CDN с edge caching
- ✅ Масштабируется автоматически
- ✅ Отличная интеграция с Vercel/Netlify
- ✅ Глобальная распределенная архитектура - автоматическое распределение данных по edge locations
- ✅ Продвинутая локализация - мощная поддержка интернационализации через Sanity's localization system
- ✅ Умеренная расширяемость - custom input components, document actions, plugins, но ограничена cloud-платформой
- ✅ Передовые технологии - cutting-edge решения, serverless-first, постоянные инновации

**Недостатки:**

- ❌ Vendor lock-in на Sanity Cloud
- ❌ Дороже при больших объемах данных
- ❌ Ограничения в кастомизации backend логики
- ❌ Изучение GROQ требует времени
- ❌ Проприетарная распределенная система - нет контроля над архитектурой БД

**Обязательства по безопасности и обновлениям:**

🔒 **Cloud-native безопасность:**

- Sanity Cloud автоматически обновляется (managed service)
- Zero-downtime deployments для всех security patches
- Next.js framework updates каждые 2-4 недели
- GROQ API не требует manual security maintenance

🛡️ **Enterprise-уровень защиты:**

- SOC 2 Type II compliance из коробки
- GDPR compliance с data residency options
- Автоматическое backup и disaster recovery
- End-to-end encryption для всех данных
- Built-in DDoS protection через global CDN

📋 **Минимальные требования к обновлениям:**

- **Автоматически**: Sanity Cloud infrastructure updates
- **Ежемесячно**: Next.js и frontend dependencies
- **Ежеквартально**: Sanity Studio updates
- **По необходимости**: Schema migrations и GROQ query optimization
- **Никогда**: Database management (полностью managed)

⚠️ **Риски vendor lock-in:**

- Зависимость от Sanity Cloud availability (99.9% SLA)
- Возможное повышение цен при scale-up
- Ограниченная кастомизация security политик
- Необходимость migration плана при смене provider
- Compliance зависит от Sanity политик, не под вашим контролем

📊 **GDPR Compliance:**

🟢 **Превосходная поддержка** - managed service с полным compliance:

- ✅ GDPR compliance из коробки (EU data residency)
- ✅ Автоматическое data processing agreements (DPA)
- ✅ Built-in data export в portable форматах
- ✅ Right to be forgotten через API
- ✅ Audit trails для всех data access operations
- ✅ Cookie-less tracking options
- ✅ Privacy-first architecture по умолчанию
- ✅ Data breach notification procedures включены в SLA

🔧 **Managed GDPR features:**
- Automated data retention и purging
- Privacy policy generator и consent management
- Data processing impact assessments support
- Regular compliance audits (Sanity ответственность)

**Максимальная масштабируемость:** 🚀 **500-2000 сайтов** (благодаря Cloud-native архитектуре, но дорого для больших объемов)

**DDoS устойчивость:** 🛡️ **Отличная** - до 200+ Gbps благодаря глобальной CDN и автомасштабированию Sanity Cloud

**Сроки разработки:** ⏰ **3-6 недель** - быстрая настройка благодаря SaaS решению, готовые шаблоны и интеграции

**Сроки MVP/PoC:**

- 🚀 **PoC**: 2-3 дня - настройка Sanity Studio и базовых схем
- 📦 **MVP**: 1-2 недели - создание сателлитов с Sanity Cloud интеграцией
- 🚀 **Выкат производства**: 3-6 недель - масштабирование, оптимизация, мониторинг

**Лицензирование:** 💰 **Freemium SaaS** - бесплатный план до лимитов, далее платные подписки ($99-$949/мес)

---

### Кандидат 5: **Directus Headless CMS + Next.js Multi-Tenant**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      Directus Admin App                  │
│   (SQL Database как CMS интерфейс)      │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│         Any SQL Database                 │
│  (MySQL/PostgreSQL/SQLite/MSSQL)       │
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Kubernetes/Docker Swarm            │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │Satellite││
│  │Next.js#1│  │Next.js#2│  │Next.js#3││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **Database-agnostic** - работает с любой SQL базой
- **No vendor lock-in** - полностью open source
- **Auto-generated API** из существующих таблиц
- **Powerful admin interface** с гибкими настройками
- **Enterprise features** в бесплатной версии

**Технический стек:**

- **Backend**: Directus (Node.js/PHP доступен)
- **Database**: PostgreSQL/MySQL/любая SQL
- **Frontend**: Next.js 15+ с статической генерацией
- **API**: GraphQL + REST автогенерация
- **Deploy**: Docker + любая платформа
- **Files**: S3/Local/CloudStorage

**🗄️ Multi-tenancy стратегия:**

🟢 **Гибкая архитектура** - несколько вариантов реализации:

**Вариант 1: Shared Database + Tenant Field**
- ✅ Одна БД с полем `tenant_id` в каждой таблице
- ✅ Directus hooks для automatic tenant filtering
- ✅ Простота управления и бэкапов

**Вариант 2: Separate Databases**
- ✅ Отдельная БД для каждого сателлита
- ✅ Полная изоляция данных
- ✅ Independent scaling и backups
- ✅ Multiple Directus instances или single instance с database switching

**Вариант 3: Database Schemas (PostgreSQL)**
- ✅ Один PostgreSQL server с отдельными schemas
- ✅ Row-level security для дополнительной защиты
- ✅ Shared resources, isolated data

**Преимущества:**

- ✅ Полная свобода от вендоров
- ✅ Работает с существующими базами данных
- ✅ Автоматическая генерация API из схемы БД
- ✅ Мощные возможности фильтрации и поиска
- ✅ Встроенная система пользователей и ролей
- ✅ Workflow automation встроен
- ✅ Отличная производительность
- ✅ Database-agnostic - поддержка любых SQL БД включая кластерные решения
- ✅ Хорошая расширяемость - extensions система, custom endpoints, hooks, interfaces customization
- ✅ Современная платформа - активная разработка, modern tech stack, регулярные релизы

**Недостатки:**

- ❌ Требует больше DevOps знаний для деплоя
- ❌ Меньше готовых интеграций по сравнению со Strapi
- ❌ Документация менее обширная
- ❌ Сообщество меньше чем у WordPress/Strapi
- ❌ Отсутствие встроенного распределенного хранения - зависит от выбранной SQL БД
- ❌ Базовая локализация - требует дополнительной настройки для полной многоязычной поддержки

**Обязательства по безопасности и обновлениям:**

🔒 **Гибкая security архитектура:**

- Directus обновления каждые 6-8 недель (стабильный релизный цикл)
- SQL Database security patches по графику производителя БД
- Node.js LTS updates каждые 6 месяцев
- Extensions и custom endpoints требуют индивидуального аудита

🛡️ **Database-agnostic защита:**

- Подключение к любой SQL БД с их встроенными security features
- Row-level security (PostgreSQL) или custom access control
- API токены с role-based permissions
- Field-level encryption для sensitive данных

📋 **Обязательный график обновлений:**

- **Еженедельно**: npm security audit для dependencies
- **Ежемесячно**: Database patches (PostgreSQL/MySQL)
- **Каждые 2 месяца**: Directus minor releases
- **Ежеквартально**: Directus major versions и Node.js LTS
- **По мере выхода**: Critical security vulnerabilities (24-48 часов)

⚠️ **DevOps responsibility:**

- Полная ответственность за infrastructure security
- Необходимость настройки firewall, SSL, backups
- Мониторинг и alerting должны быть настроены вручную
- Требует экспертизы по security hardening SQL БД
- Self-hosted означает полную ответственность за compliance

📊 **GDPR Compliance:**

🟡 **Хорошая основа** - требует правильной настройки и конфигурации:

- ✅ Database-agnostic подход позволяет выбрать GDPR-ready БД
- ✅ Granular permissions system для data access control
- ✅ Audit logging всех database operations
- ✅ API endpoints для automated data management
- ✅ Field-level encryption для sensitive data
- ❌ Нет built-in cookie consent management
- ❌ Data retention policies требуют custom implementation
- ❌ Privacy tools требуют разработки или интеграции

🔧 **Требуемая настройка:**
- Конфигурация database-specific GDPR features
- Разработка data export/delete API endpoints
- Интеграция с external privacy management tools
- Настройка automated compliance workflows

**Максимальная масштабируемость:** 📊 **300-1000 сайтов** (отличная база для масштабирования, но нет встроенного распределения)

**DDoS устойчивость:** 🛡️ **Умеренная** - до 20-30 Gbps в зависимости от настроек инфраструктуры и load balancer

**Сроки разработки:** ⏰ **4-7 недель** - настройка multi-tenant архитектуры, интеграция с выбранной SQL БД, кастомизация

**Сроки MVP/PoC:**

- 🚀 **PoC**: 1 неделя - быстрая установка с выбранной SQL БД
- 📦 **MVP**: 3-4 недели - настройка ролей, API, базовое мультисайт решение
- 🚀 **Выкат производства**: 4-7 недель - fine-tuning, безопасность, масштабирование

**Лицензирование:** 📜 **GPL v3** + **Professional ($$$)** - Core бесплатен, Professional лицензия для закрытых проектов

---

### Кандидат 6: **Кастомная система на базе современного стека (gRPC + Микросервисы)**

**Архитектура:**

```
┌─────────────────────────────────────────┐
│      Custom Admin Panel (Next.js)       │
│   (Управление сателлитами и контентом)  │
└───────────────┬─────────────────────────┘
                │ gRPC + GraphQL APIs
                ▼
┌─────────────────────────────────────────┐
│      Microservices (Go/Rust)            │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Content  │  │Sites    │  │Deploy   ││
│  │Service  │  │Service  │  │Service  ││
│  └─────────┘  └─────────┘  └─────────┘│
└───────────────┬─────────────────────────┘
                │ gRPC Communication
                ▼
┌─────────────────────────────────────────┐
│      Distributed Databases              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │  TiKV   │  │CockroachDB│ │ScyllaDB ││
│  │ (Meta)  │  │(Content) │  │(Media) ││
│  └─────────┘  └─────────┘  └─────────┘│
└───────────────┬─────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────┐
│      Kubernetes Cluster                 │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐│
│  │Satellite│  │Satellite│  │  ...    ││
│  │ Site #1 │  │ Site #2 │  │Site #1000││
│  └─────────┘  └─────────┘  └─────────┘│
└─────────────────────────────────────────┘
```

**Ключевые особенности:**

- **gRPC-first архитектура** для максимальной производительности
- **Микросервисная архитектура** для горизонтального масштабирования
- **Distributed databases** с автоматическим шардингом
- **Cloud-native design** для Kubernetes из коробки
- **Enterprise-grade** готовность к нагрузкам 1000+ сайтов

**Технический стек:**

- **Backend**: Go/Rust микросервисы с gRPC
- **Database**: TiKV + CockroachDB + ScyllaDB кластеры
- **Frontend**: Next.js 15+ с TypeScript
- **API**: gRPC + GraphQL + REST
- **Deploy**: Kubernetes + Helm Charts + ArgoCD
- **Cache**: Redis Cluster + CDN

**🗄️ Multi-tenancy стратегия:**

🟢 **Полностью распределенная архитектура** - Database-per-Service + Sharding:

**Микросервисная изоляция:**
- ✅ **Database-per-Service** pattern для каждого микросервиса
- ✅ **TiKV**: Metadata service (сайты, пользователи, конфигурация)
- ✅ **CockroachDB**: Content service с auto-sharding по tenant_id
- ✅ **ScyllaDB**: Media service с distributed storage
- ✅ **Redis Cluster**: Кеширование с namespace isolation

**Динамическое масштабирование:**
- ✅ Automatic tenant sharding на уровне БД
- ✅ Independent scaling для каждого типа данных
- ✅ Geographic data distribution (GDPR compliance)
- ✅ Cross-service transactions через gRPC
- ✅ Service mesh (Istio) для network isolation

**Преимущества:**

- ✅ Максимальная производительность с gRPC (КРИТИЧНО ДЛЯ 1000+ САТЕЛЛИТОВ)
- ✅ Истинно горизонтальное масштабирование
- ✅ Distributed databases для enterprise нагрузок
- ✅ Cloud-native архитектура
- ✅ Полная кастомизация под нужды бизнеса
- ✅ Лучшая безопасность через микросервисы
- ✅ Real-time возможности через gRPC streaming
- ✅ Vendor lock-in отсутствует полностью
- ✅ **Истинно распределенные БД** - TiKV, CockroachDB, ScyllaDB с автоматическим шардингом и репликацией
- ✅ **Полная контроль локализации** - собственная система i18n с поддержкой любых языков и форматов
- ✅ **Неограниченная расширяемость** - собственная архитектура, любые протоколы, custom микросервисы, plugin система
- ✅ **Future-proof архитектура** - новейшие технологии, microservices, cloud-native, без legacy ограничений

**Недостатки:**

- ❌ Требует разработки с нуля (6-12 месяцев)
- ❌ Высокие требования к экспертизе команды
- ❌ Сложность в первоначальной настройке
- ❌ Больше времени на MVP по сравнению с готовыми CMS
- ❌ Требует команду из 3-5 senior разработчиков

**Обязательства по безопасности и обновлениям:**

🔒 **Enterprise-grade security архитектура:**

- Микросервисы обновляются независимо по rolling update стратегии
- Go/Rust компоненты с minimal attack surface
- gRPC TLS mutual authentication между сервисами
- Distributed databases с encryption at rest/transit

🛡️ **Multi-layer security:**

- Zero-trust network architecture между микросервисами
- Container security scanning в CI/CD pipeline
- Runtime security monitoring с SIEM интеграцией
- Automatic failover и circuit breakers
- Service mesh (Istio) для advanced traffic management

📋 **Continuous security operations:**

- **Ежедневно**: Container vulnerability scanning
- **Еженедельно**: Dependency updates для Go/Rust modules
- **Каждые 2 недели**: Rolling updates микросервисов
- **Ежемесячно**: Infrastructure security audits
- **Ежеквартально**: Penetration testing всей архитектуры
- **По мере выхода**: Zero-day exploits patching (0-4 часа)

⚠️ **Advanced security challenges:**

- Требует security experts в команде (DevSecOps)
- Необходимость построения comprehensive monitoring (Prometheus + Grafana + ELK)
- Compliance audits более сложные для distributed systems
- Incident response planning для microservices failures
- Security training для всей команды по cloud-native security

📊 **GDPR Compliance:**

🟢 **Максимальный контроль** - полностью кастомизируемая privacy-by-design архитектура:

- ✅ Privacy-by-design встроен в каждый микросервис
- ✅ Distributed data sovereignty с geographic data isolation
- ✅ Real-time data lineage tracking через all microservices
- ✅ Automated data retention и purging policies
- ✅ Granular consent management на уровне API
- ✅ Cryptographic data anonymization capabilities
- ✅ Built-in data portability и export functions
- ✅ Comprehensive audit trails across distributed systems

🔧 **Advanced GDPR capabilities:**
- Custom privacy impact assessment automation
- Data processing agreements generator
- Automated breach detection и notification
- AI-powered data classification и sensitivity analysis
- Multi-jurisdictional compliance support (CCPA, GDPR, etc.)

**Максимальная масштабируемость:** 🏆 **10,000+ сайтов** (неограниченное горизонтальное масштабирование с распределенными БД и gRPC)

**DDoS устойчивость:** 🛡️ **Максимальная** - 1+ Tbps благодаря микросервисам, auto-scaling, распределенной архитектуре

**Сроки разработки:** ⏰ **12-18 месяцев** - полная разработка с нуля, включая архитектуру, безопасность, масштабирование

**Сроки MVP/PoC:**

- 🚀 **PoC**: 2-3 месяца - базовая gRPC архитектура с простейшими микросервисами
- 📦 **MVP**: 6-9 месяцев - функциональная система с основными возможностями
- 🚀 **Выкат производства**: 12-18 месяцев - enterprise-уровень с полным monitoring и безопасностью

**Лицензирование:** 🔧 **Собственная разработка** - полная свобода выбора лицензии, обычно MIT/Apache 2.0 для компонентов

---

## Сравнительная таблица всех 6 кандидатов

| **Критерий**                   | **WordPress Multisite** | **Payload CMS**        | **Strapi**              | **Sanity**               | **Directus**           | **Кастомная система**             |
| ------------------------------ | ----------------------- | ---------------------- | ----------------------- | ------------------------ | ---------------------- | --------------------------------- |
| **Легкость старта**            | ⭐⭐⭐⭐⭐              | ⭐⭐⭐⭐               | ⭐⭐⭐                  | ⭐⭐⭐⭐⭐               | ⭐⭐⭐                 | ⭐                                |
| **Multi-tenancy**              | ⭐⭐⭐⭐⭐              | ⭐⭐⭐⭐               | ⭐⭐⭐                  | ⭐⭐⭐                   | ⭐⭐⭐⭐               | ⭐⭐⭐⭐⭐                        |
| **Performance**                | ⭐⭐⭐                  | ⭐⭐⭐⭐               | ⭐⭐⭐⭐                | ⭐⭐⭐⭐⭐               | ⭐⭐⭐⭐               | ⭐⭐⭐⭐⭐                        |
| **Vendor Lock-in**             | ⭐⭐⭐⭐⭐              | ⭐⭐⭐⭐⭐             | ⭐⭐⭐⭐⭐              | ⭐                       | ⭐⭐⭐⭐⭐             | ⭐⭐⭐⭐⭐                        |
| **Developer Experience**       | ⭐⭐⭐                  | ⭐⭐⭐⭐               | ⭐⭐⭐⭐                | ⭐⭐⭐⭐⭐               | ⭐⭐⭐⭐               | ⭐⭐⭐                            |
| **Ecosystem**                  | ⭐⭐⭐⭐⭐              | ⭐⭐⭐                 | ⭐⭐⭐⭐                | ⭐⭐⭐                   | ⭐⭐                   | ⭐⭐                              |
| **Hosting Cost**               | ⭐⭐⭐⭐                | ⭐⭐⭐⭐               | ⭐⭐⭐                  | ⭐⭐                     | ⭐⭐⭐⭐               | ⭐⭐⭐                            |
| **Enterprise Ready**           | ⭐⭐⭐⭐                | ⭐⭐⭐                 | ⭐⭐⭐⭐⭐              | ⭐⭐⭐⭐                 | ⭐⭐⭐⭐               | ⭐⭐⭐⭐⭐                        |
| **Распределенная БД**          | ❌ Монолит              | ❌ Ограничена          | ⭐⭐ Реплики            | ⭐⭐⭐⭐ Cloud           | ⭐⭐ Гибкая            | ⭐⭐⭐⭐⭐ Native                 |
| **Локализация/i18n**           | ⭐⭐⭐⭐⭐ Отличная     | ⭐⭐ Базовая           | ⭐⭐⭐⭐ Встроена       | ⭐⭐⭐⭐ Продвинутая     | ⭐⭐ Базовая           | ⭐⭐⭐⭐⭐ Полная                 |
| **Расширяемость**              | ⭐⭐⭐⭐⭐ Максимальная | ⭐⭐⭐ Хорошая         | ⭐⭐⭐⭐ Отличная       | ⭐⭐⭐ Умеренная         | ⭐⭐⭐ Хорошая         | ⭐⭐⭐⭐⭐ Неограниченная         |
| **Актуальность/Legacy**        | ❌ Устаревающая         | ⭐⭐⭐⭐ Современная   | ⭐⭐⭐⭐ Актуальная     | ⭐⭐⭐⭐⭐ Передовая     | ⭐⭐⭐⭐ Современная   | ⭐⭐⭐⭐⭐ Future-proof           |
| **DDoS устойчивость**          | ❌ Низкая (1-5 Gbps)    | ⭐⭐ Умеренная (10-20) | ⭐⭐⭐ Хорошая (50-100) | ⭐⭐⭐⭐ Отличная (200+) | ⭐⭐ Умеренная (20-30) | ⭐⭐⭐⭐⭐ Максимальная (1+ Tbps) |
| **Сроки разработки**           | ⭐⭐⭐⭐⭐ 2-4 недели   | ⭐⭐⭐⭐ 4-8 недель    | ⭐⭐⭐ 6-10 недель      | ⭐⭐⭐⭐⭐ 3-6 недель    | ⭐⭐⭐ 4-7 недель      | ⭐ 12-18 месяцев                  |
| **PoC сроки**                  | ⭐⭐⭐⭐⭐ 3-5 дней     | ⭐⭐⭐⭐ 1-2 недели    | ⭐⭐⭐ 2-3 недели       | ⭐⭐⭐⭐⭐ 2-3 дня       | ⭐⭐⭐⭐ 1 неделя      | ⭐⭐ 2-3 месяца                   |
| **MVP сроки**                  | ⭐⭐⭐⭐⭐ 1-2 недели   | ⭐⭐⭐⭐ 3-4 недели    | ⭐⭐⭐ 4-6 недель       | ⭐⭐⭐⭐⭐ 1-2 недели    | ⭐⭐⭐ 3-4 недели      | ⭐ 6-9 месяцев                    |
| **gRPC Support**               | ❌                      | ❌                     | ❌                      | ❌                       | ❌                     | ✅ Встроен                        |
| **Максимальное кол-во сайтов** | 50-200                  | 100-500                | 200-800                 | 500-2000                 | 300-1000               | 10,000+                           |
| **Безопасность и обновления**  | ⭐⭐ Высокие риски      | ⭐⭐⭐ Стандартная     | ⭐⭐⭐⭐ Enterprise     | ⭐⭐⭐⭐⭐ Автоматическая | ⭐⭐⭐ Гибкая          | ⭐⭐⭐⭐⭐ Максимальная          |
| **GDPR Compliance**            | 🟡 Частичная            | 🟢 Хорошая             | 🟢 Отличная             | 🟢 Превосходная          | 🟡 Хорошая основа      | 🟢 Максимальный контроль         |
| **Multi-tenancy стратегия**    | 🔵 Shared DB+Tables    | 🔵 Shared DB+Field     | 🔵 Shared DB+RLS       | 🟠 Datasets            | 🟢 Гибкая             | 🟢 Распределенная               |
| **Лицензирование**             | GPL v2/v3               | MIT                    | MIT/Enterprise          | Freemium SaaS            | GPL v3/Pro             | Собственное                       |

## 📊 Сводка по масштабируемости

### 🔴 Малые проекты (до 100 сайтов)

- **WordPress Multisite**: 50-200 сайтов - подходит для старта, но быстро достигает лимитов
- **Payload CMS**: 100-500 сайтов - хороший выбор для TypeScript команд

### 🟡 Средние проекты (100-1000 сайтов)

- **Strapi**: 200-800 сайтов - надежный выбор для enterprise проектов
- **Directus**: 300-1000 сайтов - отличная гибкость, работает с любыми SQL БД
- **Sanity**: 500-2000 сайтов - дорого, но очень быстро и надежно

### 🟢 Enterprise проекты (1000+ сайтов)

- **Кастомная система**: 10,000+ сайтов - ЕДИНСТВЕННОЕ решение для истинного масштабирования
  - gRPC для высокой производительности
  - Распределенные базы данных
  - Горизонтальное масштабирование без ограничений

## Рекомендации по выбору

### 🚀 По скорости разработки

1. **Для максимальной скорости разработки**: Sanity + Vercel (дорого, но минимальные требования к безопасности)
2. **Для знакомой экосистемы**: WordPress Multisite + FaustWP (низкая стоимость, но высокие затраты на безопасность)
3. **Для современного TypeScript стека**: Payload CMS + Next.js (низкая стоимость, умеренные требования безопасности)

### 🏢 По уровню проектов

4. **Для enterprise проектов**: Strapi + Kubernetes (средняя стоимость, enterprise-уровень безопасности)
5. **Для полной свободы**: Directus + собственная инфраструктура (средняя стоимость, полная ответственность за безопасность)
6. **Для масштабирования 1000+ сателлитов**: Кастомная система (gRPC + микросервисы) + собственная инфраструктура (относительно очень низкая стоимость для дальнейшего сопровождения и расширения, если не учитывать стоимость первоначальной разработки)

### 🔒 По требованиям безопасности

**Низкие требования к безопасности (статический контент, публичная информация):**

- Sanity (автоматическая безопасность, managed service)
- Payload CMS (современная архитектура, но требует настройки)

**Средние требования к безопасности (корпоративные сайты, CRM):**

- Strapi (enterprise-готовая архитектура)
- Directus (гибкая настройка безопасности)

**Высокие требования к безопасности (финансы, здравоохранение, государственные услуги):**

- Кастомная система (полный контроль, микросервисная архитектура)
- ⚠️ WordPress НЕ рекомендуется для критически важных систем

### 🚨 Критические факторы безопасности

1. **WordPress**: Требует постоянного мониторинга и обновлений (еженедельно)
2. **Payload/Strapi/Directus**: Умеренные требования к DevSecOps (ежемесячно)
3. **Sanity**: Минимальные требования (managed security)
4. **Кастомная система**: Максимальные возможности, но требует экспертизы в команде

### 🛡️ По требованиям GDPR compliance

**Минимальные требования к privacy (публичный контент, маркетинговые сайты):**

- Sanity (автоматическая GDPR compliance, managed service)
- WordPress (с дополнительной настройкой и плагинами)

**Стандартные требования privacy (бизнес-приложения, пользовательские данные):**

- Strapi (enterprise-готовые GDPR возможности)
- Payload CMS (modern privacy-first архитектура)

**Высокие требования privacy (финансы, медицина, персональные данные):**

- Кастомная система (максимальный контроль, privacy-by-design)
- Directus (гибкая настройка под специфические требования)

**Критические privacy требования (государственные услуги, EU compliance):**

- Кастомная система (полная data sovereignty)
- Sanity с EU data residency (если бюджет позволяет)
- ⚠️ WordPress НЕ рекомендуется для строгих privacy требований

---

## 🗄️ Детальное сравнение стратегий Multi-tenancy

### 🔵 WordPress Multisite - Shared Database + Table Prefixes

```
┌─────────────────────────────────────────┐
│           MySQL Database                │
│                                         │
│  ┌─────────────┐  ┌─────────────┐      │
│  │ wp_users    │  │ wp_blogs    │      │
│  │ wp_site     │  │ wp_sitemeta │      │
│  └─────────────┘  └─────────────┘      │
│                                         │
│  ┌─────────────┐  ┌─────────────┐      │
│  │ wp_1_posts  │  │ wp_2_posts  │      │
│  │ wp_1_options│  │ wp_2_options│      │
│  └─────────────┘  └─────────────┘      │
└─────────────────────────────────────────┘
```

**Каждый сателлит = новый "blog" в multisite сети**
- ✅ Простая настройка из коробки
- ✅ Shared пользователи и роли
- ❌ Невозможность независимого масштабирования
- ❌ Одна точка отказа для всех сайтов

### 🔵 Payload CMS - Shared Database + Tenant Field

```
┌─────────────────────────────────────────┐
│           MongoDB Database              │
│                                         │
│  Collection: pages                      │
│  { _id, title, content, tenant_id: 1 }  │
│  { _id, title, content, tenant_id: 2 }  │
│                                         │
│  Collection: media                      │
│  { _id, filename, url, tenant_id: 1 }   │
│  { _id, filename, url, tenant_id: 2 }   │
└─────────────────────────────────────────┘
```

**Каждый сателлит = уникальный tenant_id в документах**
- ✅ Гибкая схема данных (NoSQL)
- ✅ TypeScript type safety
- ❌ Потенциальные проблемы с performance при росте
- ❌ Сложность в реализации tenant-specific индексов

### 🔵 Strapi - Shared Database + Row Level Security

```
┌─────────────────────────────────────────┐
│         PostgreSQL Database             │
│                                         │
│  Table: articles                        │
│  id | title | content | tenant_id      │
│  1  | "..."  | "..."  | tenant_1       │
│  2  | "..."  | "..."  | tenant_2       │
│                                         │
│  RLS Policy: WHERE tenant_id = current_setting('app.current_tenant')
└─────────────────────────────────────────┘
```

**Каждый сателлит = tenant_id с PostgreSQL RLS**
- ✅ Enterprise-уровень изоляции данных
- ✅ SQL производительность и надежность
- ✅ Built-in backup и репликация
- ❌ Одна БД = одна точка отказа

### 🟠 Sanity - Multiple Datasets

```
┌─────────────────────────────────────────┐
│           Sanity Project                │
│                                         │
│  Dataset: satellite-1                   │
│  ├── _type: page                        │
│  ├── _type: article                     │
│                                         │
│  Dataset: satellite-2                   │
│  ├── _type: page                        │
│  ├── _type: article                     │
└─────────────────────────────────────────┘
```

**Каждый сателлит = отдельный dataset в Sanity**
- ✅ Полная изоляция данных между сайтами
- ✅ Independent schema evolution
- ✅ Cross-dataset queries для shared content
- ❌ Vendor lock-in на Sanity Cloud
- ⚠️ Ограничения на количество datasets

### 🟢 Directus - Гибкая архитектура

**Опция 1: Separate Databases**
```
┌─────────────────┐ ┌─────────────────┐
│ PostgreSQL      │ │ PostgreSQL      │
│ satellite_1     │ │ satellite_2     │
│ ├── pages       │ │ ├── pages       │
│ ├── articles    │ │ ├── articles    │
└─────────────────┘ └─────────────────┘
```

**Опция 2: Database Schemas**
```
┌─────────────────────────────────────────┐
│         PostgreSQL Server               │
│                                         │
│  Schema: satellite_1                    │
│  ├── pages                              │
│  ├── articles                           │
│                                         │
│  Schema: satellite_2                    │
│  ├── pages                              │
│  ├── articles                           │
└─────────────────────────────────────────┘
```

**Каждый сателлит = отдельная БД ИЛИ схема ИЛИ tenant field**
- ✅ Максимальная гибкость в выборе подхода
- ✅ Можно комбинировать стратегии
- ✅ Database-agnostic решение
- ❌ Требует больше DevOps настройки

### 🟢 Кастомная система - Полностью распределенная

```
┌─────────────────────────────────────────┐
│        TiKV Cluster (Metadata)          │
│  tenant_1_config, tenant_2_config      │
└─────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────┐
│      CockroachDB Cluster (Content)      │
│  Auto-sharded по tenant_id             │
│  ├── Region: EU (GDPR tenants)          │
│  ├── Region: US (US tenants)            │
└─────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────┐
│       ScyllaDB Cluster (Media)          │
│  Distributed storage с CDN             │
└─────────────────────────────────────────┘
```

**Каждый сателлит = автоматический sharding в distributed БД**
- ✅ Неограниченное горизонтальное масштабирование
- ✅ Geographic data distribution (GDPR)
- ✅ Fault tolerance на уровне кластеров
- ✅ Independent scaling по типам данных
- ❌ Высокая сложность разработки и поддержки

---

## 💡 Заключение по архитектуре БД

**Для малых проектов (до 100 сайтов):** 
- Любая shared database стратегия подойдет

**Для средних проектов (100-1000 сайтов):**
- Strapi с PostgreSQL RLS - лучший баланс
- Directus с separate schemas - максимальная гибкость

**Для enterprise проектов (1000+ сайтов):**
- Только кастомная система с distributed БД
- Sanity может работать, но очень дорого

**Рекомендация:** Начните с shared database подхода, предусмотрите миграцию на distributed архитектуру при росте.

Completed (3/3) _Document new candidates with specifications_
