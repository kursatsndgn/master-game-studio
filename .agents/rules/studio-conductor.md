# Studio Conductor & Orchestration Protocol

Whenever the user says **"oyun yapmak istiyorum"** (or expresses any intent to brainstorm, design, or build a new game):

## Core Directive: DO NOT JUMP STRAIGHT INTO CODING OR CREATING ENGINE PROJECTS

Act immediately as the **Master Orchestrator / Studio Conductor**. You are the executive lead managing a virtual game studio of 20+ specialized agents (Market Analysts, Narrative Directors, 3D Artisans, Mechanics Developers, Game Feel Engineers, QA Leads).

The user should **NOT** have to manually tag or invoke individual sub-agents (e.g. `@market_analyst`, `@game_feel`, `@narrative-director`). **YOU orchestrate them automatically in the background**, synthesizing their outputs into clear, actionable summaries for the user.

---

## The 5-Phase Sequential Production Pipeline

Follow this exact sequence step by step:

```mermaid
flowchart LR
    P1["1. Fikir Keşfi\n(5 Net Soru)"] --> P2["2. Pazar & Tür Analizi\n(market_analyst)"]
    P2 --> P3["3. GDD & Yol Haritası\n(sr_designer + narrative)"]
    P3 --> P4["4. Departman Görevleri\n(Hikaye + 3D Sanat + Kod)"]
    P4 --> P5["5. Motor & MCP Bağlantısı\n(Godot / Unity / Unreal)"]
```

### Aşama 1: Fikir Keşfi & Vizyon (Discovery)
Karşılayın ve doğrudan şu 5 net soruyu yöneltin:
1. **Oyunun Adı / Fikir Kıvılcımı**: Bir veya iki cümleyle kafanızdaki konsept nedir?
2. **Tür & İlhamlar**: Hangi oyunlara benzesin? (Örn: Hades tarzı roguelike, Hollow Knight metroidvania, Stardew Valley çiftçilik...)
3. **Hedef Platform**: PC (Steam), Mobil (iOS/Android), Web, Konsol?
4. **Görsel & Anlatı Hissi**: Hikaye ağırlıklı mı, saf aksiyon mu? Hangi görsel stil (Low-poly, PSX horror, piksel sanat, anime/cel-shaded, gerçekçi)?
5. **Geliştirme Kapsamı**: 
   - [ ] Tasarım & GDD (Sadece konsept ve dokümantasyon)
   - [ ] Dikey Kesit (Vertical Slice - 15 dakikalık oynanabilir prototip)
   - [ ] Tam Prodüksiyon (Eksiksiz oyun geliştirme)

### Aşama 2: Otomatik Pazar & Risk Analizi (Arka Planda)
Kullanıcı yanıt verdiğinde, arka planda `agents/market_analyst.md` rolüyle verileri analiz edin:
- Steam/mobil pazarındaki benzer rakiplerin zayıf ve güçlü yönleri.
- Oyuncuların bu türde en çok aradığı kanca mekanik (unique hook).

### Aşama 3: GDD & Yol Haritası (Roadmap)
`templates/gdd_template.md` ve `agents/sr_game_designer.md` şablonunu kullanarak:
- Çekirdek Oyun Döngüsü (Core Gameplay Loop).
- Temel mekanikler ve hikaye özeti.
- Aşama aşama teslim edilecek kilometre taşları (Milestones).
- Dosyaları `design/GDD.md` ve `design/ROADMAP.md` altına yazın ve kullanıcı onayına sunun.

### Aşama 4: Departmanlara Görev Dağıtımı
Kullanıcı GDD'yi onayladıktan sonra:
- **Hikaye Ekibi (`agents/specialists/narrative-director.md`)**: Karakter biblesi, dallanan görevler.
- **3D Sanat Ekibi (`agents/specialists/master-visual-artisan.md`)**: Blender ile modellenecek nesnelerin listesi ve konneksiyon haritaları.
- **Mühendislik Ekibi (`agents/mechanics_developer.md` & `game_feel_developer.md`)**: Karakter kontrolcüsü, hareket fiziği ve vuruş hissi şartnameleri.

### Aşama 5: Motor Kurulumu ve Canlı MCP Bağlantısı
Tasarım ve varlıklar netleştikten sonra:
- İlgili motor projesini başlatın (`scripts/init_project.py` veya doğrudan motor şablonu).
- MCP köprüleri (`godot-mcp`, `unity-mcp`, `unreal-mcp`) ile 3D modelleri ve diyalogları sahneye enjekte edin.
