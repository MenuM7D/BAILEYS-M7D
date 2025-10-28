<div align='center'>

# BAILEYS-M7D
## مكتبة TypeScript/JavaScript لواجهة واتساب ويب

<img src="https://i.postimg.cc/6qq8tgrg/file.jpg" alt="صورة العنوان" width="100%"/>

<br/>

<!-- شارات -->
<p>
  <img src="https://img.shields.io/github/v/release/MenuM7D/BAILEYS-M7D?include_prereleases&sort=semver" alt="أحدث إصدار"/>
  <img src="https://img.shields.io/github/languages/code-size/MenuM7D/BAILEYS-M7D" alt="حجم الكود"/>
  <img src="https://img.shields.io/github/license/MenuM7D/BAILEYS-M7D" alt="الترخيص"/>
  <img src="https://img.shields.io/github/stars/MenuM7D/BAILEYS-M7D" alt="النجوم"/>
  <img src="https://img.shields.io/github/forks/MenuM7D/BAILEYS-M7D" alt="التفريعات"/>
</p>

<!-- إحصائيات GitHub -->
<p>
  <img src="https://github-readme-stats.vercel.app/api?username=MenuM7D&show_icons=true&theme=radical" alt="إحصائيات GitHub"/>
</p>

</div>

---

## 📱 تواصل معي

<div align="center">

| المنصة | الرابط |
|--------|--------|
| 💬 واتساب | [اضغط هنا للتواصل](https://wa.me/201220864180) |
| 🌐 جميع المنصات | [m7d-platforms.vercel.app](https://m7d-platforms.vercel.app) |
| 📢 قناة واتساب | [انضم الآن](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w) |

</div>

---

## 🌟 نبذة عن المشروع

هذه المكتبة هي **نسخة معدلة ومطورة** من مكتبة Baileys الأصلية، تم تطويرها وصيانتها بواسطة **M7D-DEV**.

### 📌 ملاحظات مهمة

- ✅ هذه المكتبة نسخة معدلة من [github.com/BrunoSobrino/Baileys-Itsukichann](https://github.com/BrunoSobrino/Baileys-Itsukichann)
- ⚠️ هذه المكتبة **غير مرتبطة** أو **معتمدة** من قبل واتساب
- 🚫 يُمنع استخدام المكتبة في الرسائل المزعجة أو التتبع أو الرسائل الجماعية غير المرغوبة
- ⚖️ المطورون **غير مسؤولين** عن أي إساءة استخدام للمكتبة
- 📜 يجب استخدام المكتبة وفقاً لشروط خدمة واتساب

---

## 🚀 المميزات الرئيسية

### ⚡ الأداء
- **لا تحتاج إلى Selenium** أو متصفح للتواصل مع واتساب ويب
- يتم الاتصال مباشرة باستخدام **WebSocket**
- توفير **نصف جيجابايت** من الذاكرة العشوائية
- دعم **متعدد الأجهزة** ونسخة الويب من واتساب

### 🔧 سهولة الاستخدام
- واجهة برمجية بسيطة ومرنة
- دعم كامل لـ TypeScript
- توثيق شامل باللغة العربية
- أمثلة عملية جاهزة للاستخدام

### 🎯 الوظائف المدعومة
- إرسال جميع أنواع الرسائل (نصوص، صور، فيديو، صوت، وثائق...)
- إدارة المجموعات بشكل كامل
- القصص والحالات
- استطلاعات الرأي والأزرار التفاعلية
- الرسائل المشفرة end-to-end
- وأكثر بكثير...

---

## 📥 التثبيت

### باستخدام npm:
```bash
npm install git+https://github.com/MenuM7D/BAILEYS-M7D.git
```

### باستخدام yarn:
```bash
yarn add git+https://github.com/MenuM7D/BAILEYS-M7D.git
```

### إضافة إلى package.json:
```json
{
  "dependencies": {
    "@whiskeysockets/baileys": "git+https://github.com/MenuM7D/BAILEYS-M7D.git"
  }
}
```

### الاستيراد في الكود:
```typescript
import makeWASocket from '@whiskeysockets/baileys'
```

---

## 🎓 مثال بسيط للبدء

```typescript
import makeWASocket, { DisconnectReason, useMultiFileAuthState } from '@whiskeysockets/baileys'
import { Boom } from '@hapi/boom'

async function connectToWhatsApp() {
    const { state, saveCreds } = await useMultiFileAuthState('./auth_info_baileys')
    
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true
    })
    
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error as Boom)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('الاتصال مغلق، سبب:', lastDisconnect.error, '، إعادة الاتصال:', shouldReconnect)
            
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('تم فتح الاتصال بنجاح!')
        }
    })
    
    sock.ev.on('messages.upsert', async ({ messages }) => {
        for (const m of messages) {
            console.log('رسالة جديدة:', JSON.stringify(m, undefined, 2))
            
            console.log('الرد على:', m.key.remoteJid)
            await sock.sendMessage(m.key.remoteJid!, { text: 'مرحباً! هذا بوت يعمل بمكتبة Baileys-M7D' })
        }
    })
    
    // حفظ بيانات الجلسة عند التحديث
    sock.ev.on('creds.update', saveCreds)
}

// تشغيل البوت
connectToWhatsApp()
```

---

## 📚 الفهرس

### 🔐 الاتصال والمصادقة
- [الاتصال برمز QR](#الاتصال-برمز-qr)
- [الاتصال برمز الاقتران](#الاتصال-برمز-الاقتران)
- [حفظ واستعادة الجلسات](#حفظ-واستعادة-الجلسات)
- [استقبال السجل الكامل](#استقبال-السجل-الكامل)

### 📨 إرسال الرسائل
- [الرسائل النصية](#الرسائل-النصية)
- [الرسائل مع الاقتباس](#الاقتباس-من-رسالة)
- [المنشن والإشارة للمستخدمين](#الإشارة-للمستخدمين)
- [رسائل الموقع](#رسائل-الموقع)
- [رسائل الاتصال](#رسائل-الاتصال)
- [رسائل التفاعل (Reactions)](#رسائل-التفاعل)
- [الاستطلاعات](#رسائل-الاستطلاع)
- [الأزرار التفاعلية](#الأزرار-التفاعلية)
- [الرسائل الإعلامية](#الرسائل-الإعلامية)

### 🎬 الوسائط المتعددة
- [إرسال الصور](#إرسال-الصور)
- [إرسال الفيديو](#إرسال-الفيديو)
- [إرسال الصوت](#إرسال-الصوت)
- [إرسال المستندات](#إرسال-المستندات)
- [الملصقات (Stickers)](#الملصقات)
- [الألبومات](#الألبومات)
- [رسائل المشاهدة مرة واحدة](#رسائل-المشاهدة-مرة-واحدة)

### 👥 إدارة المجموعات
- [إنشاء مجموعة](#إنشاء-مجموعة)
- [إضافة/إزالة الأعضاء](#إضافة-وإزالة-الأعضاء)
- [ترقية/تخفيض المسؤولين](#ترقية-وتخفيض-المسؤولين)
- [تغيير الإعدادات](#تغيير-إعدادات-المجموعة)
- [الحصول على رابط الدعوة](#الحصول-على-رابط-الدعوة)
- [الانضمام للمجموعات](#الانضمام-للمجموعات)

### ⚙️ الإعدادات والخصوصية
- [حظر/إلغاء حظر المستخدمين](#حظر-المستخدمين)
- [إعدادات الخصوصية](#إعدادات-الخصوصية)
- [تغيير الملف الشخصي](#تغيير-الملف-الشخصي)
- [صورة الملف الشخصي](#صورة-الملف-الشخصي)

### 🛠️ متقدم
- [معالجة الأحداث](#معالجة-الأحداث)
- [تخزين البيانات](#تخزين-البيانات)
- [تحميل الوسائط](#تحميل-الوسائط)
- [تعديل الرسائل](#تعديل-الرسائل)
- [حذف الرسائل](#حذف-الرسائل)

---

## 🔐 الاتصال والمصادقة

### الاتصال برمز QR

```typescript
import makeWASocket, { Browsers } from '@whiskeysockets/baileys'

const sock = makeWASocket({
    browser: Browsers.ubuntu('My App'),
    printQRInTerminal: true
})
```

بعد تشغيل الكود، ستظهر رمز QR في الطرفية، قم بمسحه من تطبيق واتساب في هاتفك!

### الاتصال برمز الاقتران

> ⚠️ **ملاحظة:** رمز الاقتران ليس Mobile API، إنه طريقة للاتصال بواتساب ويب بدون رمز QR

```typescript
import makeWASocket from '@whiskeysockets/baileys'

const sock = makeWASocket({
    printQRInTerminal: false // يجب أن يكون false
})

if (!sock.authState.creds.registered) {
    const number = '201220864180' // رقم الهاتف بدون + أو ()
    const code = await sock.requestPairingCode(number)
    console.log('رمز الاقتران:', code)
}
```

### حفظ واستعادة الجلسات

```typescript
import makeWASocket, { useMultiFileAuthState } from '@whiskeysockets/baileys'

const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')

const sock = makeWASocket({ auth: state })

// حفظ بيانات الاعتماد عند التحديث
sock.ev.on('creds.update', saveCreds)
```

---

## 📨 إرسال الرسائل

### الرسائل النصية

```typescript
await sock.sendMessage(jid, { text: 'مرحباً بك في Baileys-M7D!' })
```

### الاقتباس من رسالة

```typescript
await sock.sendMessage(jid, { text: 'هذه رسالة مقتبسة' }, { quoted: message })
```

### الإشارة للمستخدمين

```typescript
await sock.sendMessage(
    jid,
    {
        text: '@201220864180 مرحباً!',
        mentions: ['201220864180@s.whatsapp.net']
    }
)
```

### رسائل الموقع

```typescript
await sock.sendMessage(
    jid, 
    {
        location: {
            degreesLatitude: 30.0444,
            degreesLongitude: 31.2357,
            name: 'القاهرة، مصر'
        }
    }
)
```

### رسائل الاتصال

```typescript
const vcard = 'BEGIN:VCARD\n'
            + 'VERSION:3.0\n' 
            + 'FN:M7D DEV\n'
            + 'ORG:M7D Development\n'
            + 'TEL;type=CELL;type=VOICE;waid=201220864180:+20 122 086 4180\n'
            + 'END:VCARD'

await sock.sendMessage(
    jid,
    { 
        contacts: { 
            displayName: 'M7D DEV', 
            contacts: [{ vcard }] 
        }
    }
)
```

### رسائل التفاعل

```typescript
await sock.sendMessage(
    jid,
    {
        react: {
            text: '❤️', // استخدم سلسلة فارغة لإزالة التفاعل
            key: message.key
        }
    }
)
```

### رسائل الاستطلاع

```typescript
await sock.sendMessage(
    jid,
    {
        poll: {
            name: 'ما رأيك في Baileys-M7D؟',
            values: ['ممتاز', 'جيد', 'يحتاج تحسين'],
            selectableCount: 1
        }
    }
)
```

---

## 🎬 الوسائط المتعددة

### إرسال الصور

```typescript
await sock.sendMessage(
    jid, 
    { 
        image: { url: './image.jpg' },
        caption: 'صورة جميلة 📸'
    }
)
```

### إرسال الفيديو

```typescript
await sock.sendMessage(
    jid, 
    { 
        video: { url: './video.mp4' },
        caption: 'شاهد هذا الفيديو 🎥'
    }
)
```

### إرسال الصوت

```typescript
await sock.sendMessage(
    jid, 
    {
        audio: { url: './audio.mp3' },
        mimetype: 'audio/mp4',
        ptt: true // رسالة صوتية
    }
)
```

### الملصقات

```typescript
await sock.sendMessage(
    jid,
    {
        sticker: { url: './sticker.webp' }
    }
)
```

### رسائل المشاهدة مرة واحدة

```typescript
await sock.sendMessage(
    jid, 
    { 
        image: { url: './image.jpg' },
        viewOnce: true,
        caption: 'شاهد هذه الصورة مرة واحدة فقط!'
    }
)
```

---

## 👥 إدارة المجموعات

### إنشاء مجموعة

```typescript
const group = await sock.groupCreate('مجموعة رائعة', ['1234@s.whatsapp.net', '5678@s.whatsapp.net'])
console.log('تم إنشاء المجموعة برقم:', group.gid)
await sock.sendMessage(group.id, { text: 'مرحباً بالجميع! 👋' })
```

### إضافة وإزالة الأعضاء

```typescript
// إضافة أعضاء
await sock.groupParticipantsUpdate(
    jid, 
    ['1234@s.whatsapp.net', '5678@s.whatsapp.net'],
    'add'
)

// إزالة أعضاء
await sock.groupParticipantsUpdate(
    jid, 
    ['1234@s.whatsapp.net'],
    'remove'
)
```

### ترقية وتخفيض المسؤولين

```typescript
// ترقية إلى مسؤول
await sock.groupParticipantsUpdate(jid, ['1234@s.whatsapp.net'], 'promote')

// تخفيض من مسؤول
await sock.groupParticipantsUpdate(jid, ['1234@s.whatsapp.net'], 'demote')
```

### تغيير إعدادات المجموعة

```typescript
// تغيير اسم المجموعة
await sock.groupUpdateSubject(jid, 'اسم جديد للمجموعة')

// تغيير الوصف
await sock.groupUpdateDescription(jid, 'وصف جديد للمجموعة')

// السماح للمسؤولين فقط بالإرسال
await sock.groupSettingUpdate(jid, 'announcement')

// السماح للجميع بالإرسال
await sock.groupSettingUpdate(jid, 'not_announcement')
```

### الحصول على رابط الدعوة

```typescript
const code = await sock.groupInviteCode(jid)
const link = 'https://chat.whatsapp.com/' + code
console.log('رابط الدعوة:', link)
```

### الانضمام للمجموعات

```typescript
const code = 'ABC123XYZ' // رمز الدعوة فقط بدون الرابط
const response = await sock.groupAcceptInvite(code)
console.log('تم الانضمام إلى المجموعة:', response)
```

---

## ⚙️ الإعدادات والخصوصية

### حظر المستخدمين

```typescript
// حظر مستخدم
await sock.updateBlockStatus(jid, 'block')

// إلغاء الحظر
await sock.updateBlockStatus(jid, 'unblock')
```

### إعدادات الخصوصية

```typescript
// تحديث خصوصية آخر ظهور
await sock.updateLastSeenPrivacy('all') // 'contacts' | 'contact_blacklist' | 'none'

// تحديث خصوصية صورة الملف الشخصي
await sock.updateProfilePicturePrivacy('contacts')

// تحديث خصوصية الحالة
await sock.updateStatusPrivacy('contacts')

// تحديث خصوصية إضافة المجموعات
await sock.updateGroupsAddPrivacy('contacts')
```

### تغيير الملف الشخصي

```typescript
// تغيير الاسم
await sock.updateProfileName('M7D Developer')

// تغيير الحالة
await sock.updateProfileStatus('مطور عربي محترف 💻')
```

### صورة الملف الشخصي

```typescript
// تغيير الصورة
await sock.updateProfilePicture(jid, { url: './profile.jpg' })

// حذف الصورة
await sock.removeProfilePicture(jid)
```

---

## 🛠️ متقدم

### معالجة الأحداث

```typescript
// رسائل جديدة
sock.ev.on('messages.upsert', async ({ messages }) => {
    for (const msg of messages) {
        console.log('رسالة جديدة:', msg)
    }
})

// تحديث الاتصال
sock.ev.on('connection.update', (update) => {
    console.log('تحديث الاتصال:', update)
})

// تحديث المجموعات
sock.ev.on('groups.update', (updates) => {
    console.log('تحديث المجموعات:', updates)
})

// تحديث الجهات
sock.ev.on('contacts.upsert', (contacts) => {
    console.log('جهات جديدة:', contacts)
})
```

### تخزين البيانات

```typescript
import { makeInMemoryStore } from '@whiskeysockets/baileys'

const store = makeInMemoryStore({})

// قراءة من ملف
store.readFromFile('./baileys_store.json')

// حفظ كل 10 ثواني
setInterval(() => {
    store.writeToFile('./baileys_store.json')
}, 10_000)

// ربط المخزن بالسوكت
store.bind(sock.ev)
```

### تحميل الوسائط

```typescript
import { downloadMediaMessage } from '@whiskeysockets/baileys'
import { createWriteStream } from 'fs'

sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0]
    
    if (!msg.message) return
    
    if (msg.message.imageMessage) {
        const stream = await downloadMediaMessage(
            msg,
            'stream',
            {},
            { 
                logger,
                reuploadRequest: sock.updateMediaMessage
            }
        )
        
        const writeStream = createWriteStream('./downloaded-image.jpg')
        stream.pipe(writeStream)
    }
})
```

### تعديل الرسائل

```typescript
const msg = await sock.sendMessage(jid, { text: 'نص أصلي' })

// تعديل الرسالة
await sock.sendMessage(jid, {
    text: 'نص معدل',
    edit: msg.key
})
```

### حذف الرسائل

```typescript
const msg = await sock.sendMessage(jid, { text: 'سيتم حذف هذه الرسالة' })

// حذف للجميع
await sock.sendMessage(jid, { delete: msg.key })
```

---

## 📖 روابط مفيدة

- [مستودع GitHub](https://github.com/MenuM7D/BAILEYS-M7D)
- [التوثيق الكامل](https://guide.whiskeysockets.io/)
- [الأمثلة العملية](https://github.com/MenuM7D/BAILEYS-M7D/tree/main/Example)

---

## 🙏 شكر وتقدير

شكراً لـ:
- [@pokearaujo](https://github.com/pokearaujo/multidevice) - لتوثيق نظام الأجهزة المتعددة
- [@Sigalor](https://github.com/sigalor/whatsapp-web-reveng) - لتوثيق واتساب ويب
- [@Rhymen](https://github.com/Rhymen/go-whatsapp/) - للتطبيق بلغة Go
- [@BrunoSobrino](https://github.com/BrunoSobrino/Baileys-Itsukichann) - للنسخة الأصلية المعدلة

---

## 📄 الترخيص

هذا المشروع مرخص تحت رخصة MIT - انظر ملف [LICENSE](LICENSE) للمزيد من التفاصيل.

---

## 💬 الدعم والمساعدة

إذا كنت بحاجة لمساعدة أو لديك استفسار:

<div align="center">

### تواصل معي مباشرة

[![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/201220864180)
[![Website](https://img.shields.io/badge/Website-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://m7d-platforms.vercel.app)
[![Channel](https://img.shields.io/badge/Channel-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w)

</div>

---

<div align="center">

### صُنع بـ ❤️ بواسطة M7D-DEV

**BAILEYS-M7D - مكتبة واتساب احترافية للمطورين العرب**

</div>
