# Task Decomposer — Kullanım Senaryosu

## Senaryo: Geliştirici "bir blog platformu yap" diyor

**Kullanıcı:** `Can` (full-stack developer, 5 yıl deneyim, AI'ya ilk kez büyük bir iş veriyor)

---

### Perde 1 — Vague Request (Sorun)

**Can:** "Bana bir blog platformu yap."

**Normal bir AI'nın tepkisi (task-decomposer olmadan):**
Hemen kod yazmaya başlar, 200 satır sonra işler karışır, kullanıcı "hayır öyle değil" der, context şişer, sonuç hüsran.

---

### Perde 2 — Goal Articulation (Netleştirme)

**AI (task-decomposer aktif):** "Bunu adımlara ayıralım. Önce hedefi netleştirelim:"

> **Hedef:** Admin paneli olan, markdown destekli, kullanıcı kaydı gerektiren bir blog. MVP olacak, 1 haftada bitmeli. Next.js + PostgreSQL kullanıyoruz. Deploy edilebilir olmalı.

**Can'ın cevabı:** "Evet, Next.js 14 + Prisma + PostgreSQL. Authentication için NextAuth. Markdown için react-markdown. MVP'de yorum sistemi olmasın, sadece admin yazsın."

---

### Perde 3 — Dependency Map (Bağımlılık Haritası)

**AI:**

```
Level 1 — Foundation
  1. Next.js projesi kurulumu + Prisma şema tasarımı
  2. PostgreSQL bağlantısı + migration
Level 2 — Authentication
  3. NextAuth entegrasyonu + admin rolü
Level 3 — Core Features
  4. Blog CRUD API (create, edit, publish, delete)
  5. Admin panel UI (listele, yazı yaz, düzenle)
  6. Public blog görüntüleme sayfası
Level 4 — Polish
  7. Markdown render, SEO meta, hata yönetimi
Level 5 — Verify
  8. Testler + deploy hazırlığı
```

> \*\*Doğru sıra bu mı? Bağımlılıkları değiştirmek ister misin?"

**Can:** "Güzel, Levels 1-3 birlikte çalışabilir. Level 4 ayrı oturum."

---

### Perde 4 — Atomic Prompt Cards (Prompt Kartları)

**AI, her biri context window'a sığacak şekilde kartlar oluşturur:**

```markdown
## Prompt 1: Proje kurulumu + Prisma şema

### Context

Boş bir Next.js 14 projesi oluşturulsun. Prisma + PostgreSQL kurulumu yapılsın.

### Requirements

- `create-next-app` ile typescript projesi
- Prisma şeması: User (id, name, email, role), Post (id, title, slug, content, published, createdAt, updatedAt, authorId)
- PostgreSQL connection string `.env`'de
- İlk migration

### Acceptance Criteria

- `npm run dev` çalışıyor
- `npx prisma db push` başarılı
- `prisma studio`'da tablolar görünüyor

## Prompt 2: NextAuth + admin rolü

### Context

Prompt 1'deki projeye NextAuth ekle.

### Requirements

- [... devam eder]
```

> **Toplam 8 prompt kartı** oluşturulur. Her biri tek başına çalıştırılabilir, kendine yetecek context'e sahip.

---

### Perde 5 — Execution Plan (Uygulama Planı)

**Can'a sunulan nihai plan:**

```markdown
## Execution Plan

Session 1 (bugün — ~2 saat)
☐ Prompt 1: Proje kurulumu + Prisma
☐ Prompt 2: NextAuth + admin
☐ Prompt 3: CRUD API
☐ Prompt 4: Admin panel UI
└─ ✔ Doğrula: admin panele gir, yazı yaz, kaydet

Session 2 (yarın — ~1 saat)
☐ CONTEXT.md oluştur → yeni session'a taşı
☐ Prompt 5: Public blog sayfaları
☐ Prompt 6: Markdown + SEO + error handling
☐ Prompt 7: Testler
☐ Prompt 8: Deploy konfigürasyonu
└─ ✔ Doğrula: canlıda aç, yazıyı gör
```

**Can:** "Harika, başlayalım."

---

### Perde 6 — Session Handoff (Context Taşıma)

**Session 1 sonu** — AI otomatik `CONTEXT.md` oluşturur:

```markdown
# Context Handoff — Session 1

## Completed

- [x] Next.js 14 + Prisma kurulumu — `/prisma/schema.prisma`
- [x] PostgreSQL migration çalışıyor
- [x] NextAuth ile email/password giriş + admin rolü
- [x] Blog CRUD API — `/api/posts`
- [x] Admin panel UI — `/admin/*`

## Decisions

- Authentication: Email/password (Google OAuth sonra eklenir)
- Slug: `title`'dan otomatik oluşturulur, unique constraint var
- Role sistemi: User model'inde `role: enum(ADMIN, AUTHOR, READER)`

## Next session

- Prompt 5: Public blog — `app/blog/[slug]/page.tsx`
- Prompt 6: Markdown rendering, SEO metadata, error boundaries
- Prompt 7: Integration tests
- Prompt 8: Vercel deploy config + env vars

## State

- Branch: `feature/blog-platform`
- Migrations: 1 (initial schema)
- Env vars: `DATABASE_URL`, `NEXTAUTH_SECRET`, `NEXTAUTH_URL`
```

---

### Perde 7 — Session 2 (Kaldığı Yerden Devam)

**Can:** "Devam ediyorum, CONTEXT.md'yi ekledim."

**AI:** CONTEXT.md'yi okur, direkt Prompt 5'ten başlar. Geçmişi hatırlamak zorunda kalmaz — her şey CONTEXT.md'de.

---

## Özet

| Aşama     | Task-decomposer olmadan             | Task-decomposer ile            |
| --------- | ----------------------------------- | ------------------------------ |
| Başlangıç | "Blog yap" → AI sağa sola koşar     | Goal articulation ile netleşir |
| Prompt    | Tek devasa prompt → şişer, kaybolur | 8 atomik prompt → her biri net |
| Context   | 2. saatte unutmaya başlar           | CONTEXT.md ile sıfır kayıp     |
| Sonuç     | Yarım kalmış proje, sinir bozukluğu | Adım adım ilerleyen MVP        |

---

## Can'ın yorumu (simüle)

> "İlk seferde 'blog yap' dediğimde AI direkt kod yazmaya başlamıştı, 5 dk sonra 'hayır öyle değil' diye düzeltiyordum. Task-decomposer ile önce ne istediğimi sorguladı, sonra adımlara böldü, her adımda neyi doğrulayacağımı söyledi. 2 oturumda bitirdim, context kaybı yaşamadım."
