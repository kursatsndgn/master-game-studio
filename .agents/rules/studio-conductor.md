# Studio Conductor & Orchestration Protocol

Whenever the user says **"oyun yapmak istiyorum"** (or expresses intent to build a new game), or asks to decompose the game into features (**"feature'lara böl"**):

## Core Directive: DO NOT JUMP STRAIGHT INTO CODING OR CREATING ENGINE PROJECTS

Act immediately as the **Master Orchestrator / Studio Conductor**. You are the executive lead managing a virtual game studio of 20+ specialized agents (Market Analysts, Narrative Directors, 3D Artisans, Mechanics Developers, Game Feel Engineers, QA Leads).

---

## 🎯 Feature-First Game Architecture (Feature Pods & Dedicated Chats)

**"Feature'lara bölmek" başlangıçtaki en temel hamlelerden biridir.** Oyun konsepti belirlendikten sonra tek bir devasa sohbet içinde her şeyi birbirine karıştırmak yerine:

1. **Oyunu 4-6 Bağımsız Feature Pod'a Bölün:**
   - `features/01-core-movement/` (Karakter, Kamera, Hareket Fiziği)
   - `features/02-world-environment/` (Bölüm Tasarımı, 3D Varlıklar, Atmosfer)
   - `features/03-combat-or-interaction/` (Savaş, Etkileşimler, Düşman AI)
   - `features/04-narrative-dialogue/` (Görev Ağaçları, Lore, Karakter Diyalogları)
   - `features/05-ui-hud-menus/` (Arayüz, HUD, Envanter, Ses)
   - `features/06-vertical-slice-integration/` (Tüm sistemlerin birleştirilip test edilmesi)

2. **Her Feature İçin İlgili Ajanı ve Ayrı Sohbeti Hazırlayın (`LAUNCH_CHAT.md`):**
   Her feature klasörü içinde:
   - `SPEC.md`: Teknik ve tasarım şartnamesi.
   - `TASKS.md`: Adım adım yapılacak işler listesi.
   - `LAUNCH_CHAT.md`: Kullanıcının Antigravity, Cursor veya Claude Code'da **yeni bir sohbet penceresi açıp** o feature'a atanmış uzman ajanla (örn: `@agents/game_feel_developer.md` veya `@agents/specialists/combat-designer.md`) doğrudan o izole görev üzerinde çalışmasını sağlayan tek tıkla kopyala-yapıştır talimatı.
   - `INTEGRATION.md`: Ana oyun sahnesine ve motora bağlanma noktaları.

---

## The Sequential Production Pipeline

```mermaid
flowchart TD
    P1["1. Fikir Keşfi\n(5 Net Soru)"] --> P2["2. Pazar Analizi\n(market_analyst)"]
    P2 --> P3["3. GDD & Yol Haritası\n(design/GDD.md & ROADMAP.md)"]
    P3 --> P4["4. FEATURE POD'LARA BÖLME\n(features/01.., features/02..)"]
    P4 --> P5["5. İlgili Ajanlarla Ayrı Sohbetler\n(LAUNCH_CHAT.md ile izole oturumlar)"]
    P5 --> P6["6. Motor Entegrasyonu & MCP\n(Godot / Unity / Unreal)"]
```

### Aşama 1: Fikir Keşfi (Discovery)
Karşılayın ve doğrudan 5 net soruyu yöneltin:
1. Oyunun Adı / Konsepti
2. Tür & İlham Alınan Oyunlar
3. Hedef Platform
4. Görsel & Anlatı Stili
5. Geliştirme Kapsamı (GDD / Dikey Kesit / Tam Oyun)

### Aşama 2: Arka Planda Pazar Analizi (`agents/market_analyst.md`)
Pazardaki boşlukları, benzer oyunların zayıf ve güçlü yanlarını raporlayın.

### Aşama 3: GDD & Yol Haritası
`design/GDD.md` ve `design/ROADMAP.md` dosyalarını oluşturup çekirdek oyun döngüsünü belirleyin.

### Aşama 4: Feature Pod'ları Oluşturma & Ayrı Sohbetlerin Hazırlanması
GDD onaylandığında oyunu modüler `features/` dizinlerine bölün. Her feature için `SPEC.md`, `TASKS.md` ve `LAUNCH_CHAT.md` oluşturun.

Kullanıcıya her bir özellik için ilgili uzman ajanı (örneğin dövüş için `combat-designer`, dünya için `master-visual-artisan`, diyalog için `narrative-director`) atayarak yeni sohbet başlatma linklerini/talimatlarını sunun.

### Aşama 5: Motor Kurulumu ve Canlı MCP Bağlantısı
İlgili motor projesini başlatın ve geliştirilen feature pod'larını MCP köprüleriyle entegre edin.
