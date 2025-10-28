BAILEYS-M7D
مكتبة TypeScript/JavaScript لواجهة واتساب ويب
<div align="center">
<img src="https://i.postimg.cc/6qq8tgrg/file.jpg" alt="صورة العنوان" width="100%"/>
</div>
<!-- شارات -->
<p align="center">
<a href="https://www.google.com/search?q=https://github.com/MenuM7D/BAILEYS-M7D/releases"><img src="https://img.shields.io/github/v/release/MenuM7D/BAILEYS-M7D?include_prereleases&sort=semver" alt="أحدث إصدار"/></a>
<a href="https://www.google.com/search?q=https://github.com/MenuM7D/BAILEYS-M7D/search%3Ftype%3Dcode"><img src="https://img.shields.io/github/languages/code-size/MenuM7D/BAILEYS-M7D" alt="حجم الكود"/></a>
<a href="https://www.google.com/search?q=https://github.com/MenuM7D/BAILEYS-M7D/blob/main/LICENSE"><img src="https://img.shields.io/github/license/MenuM7D/BAILEYS-M7D" alt="الترخيص"/></a>
<a href="https://www.google.com/search?q=https://github.com/MenuM7D/BAILEYS-M7D/stargazers"><img src="https://img.shields.io/github/stars/MenuM7D/BAILEYS-M7D" alt="النجوم"/></a>
<a href="https://www.google.com/search?q=https://github.com/MenuM7D/BAILEYS-M7D/network/members"><img src="https://img.shields.io/github/forks/MenuM7D/BAILEYS-M7D" alt="التفريعات"/></a>
</p>
<!-- إحصائيات GitHub -->
<div align="center">
<img src="https://www.google.com/search?q=https://github-readme-stats.vercel.app/api%3Fusername%3DMenuM7D%26show_icons%3Dtrue%26theme%3Dradical%26locale%3Dar" alt="إحصائيات GitHub"/>
</div>
📱 تواصل معي
| المنصة | الرابط |
|---|---|
| 💬 واتساب | اضغط هنا للتواصل |
| 🌐 جميع المنصات | m7d-platforms.vercel.app |
| 📢 قناة واتساب | انضم الآن |
🌟 نبذة عن المشروع
هذه المكتبة هي نسخة معدلة ومطورة من مكتبة Baileys الأصلية، تم تطويرها وصيانتها بواسطة M7D-DEV.
📌 ملاحظات مهمة
 * ✅ هذه المكتبة نسخة معدلة من github.com/BrunoSobrino/Baileys-Itsukichann
 * ⚠️ هذه المكتبة غير مرتبطة أو معتمدة من قبل واتساب
 * 🚫 يُمنع استخدام المكتبة في الرسائل المزعجة أو التتبع أو الرسائل الجماعية غير المرغوبة
 * ⚖️ المطورون غير مسؤولين عن أي إساءة استخدام للمكتبة
 * 📜 يجب استخدام المكتبة وفقاً لشروط خدمة واتساب
🚀 المميزات الرئيسية
⚡ الأداء
 * لا تحتاج إلى Selenium أو متصفح للتواصل مع واتساب ويب
 * يتم الاتصال مباشرة باستخدام WebSocket
 * توفير نصف جيجابايت من الذاكرة العشوائية
 * دعم متعدد الأجهزة ونسخة الويب من واتساب
🔧 سهولة الاستخدام
 * واجهة برمجية بسيطة ومرنة
 * دعم كامل لـ TypeScript
 * توثيق شامل باللغة العربية
 * أمثلة عملية جاهزة للاستخدام
🎯 الوظائف المدعومة
 * إرسال جميع أنواع الرسائل (نصوص، صور، فيديو، صوت، وثائق...)
 * إدارة المجموعات بشكل كامل
 * القصص والحالات
 * استطلاعات الرأي والأزرار التفاعلية
 * الرسائل المشفرة end-to-end
 * وأكثر بكثير...
📥 التثبيت
باستخدام npm:
npm install git+[https://github.com/MenuM7D/BAILEYS-M7D.git](https://github.com/MenuM7D/BAILEYS-M7D.git)

باستخدام yarn:
yarn add git+[https://github.com/MenuM7D/BAILEYS-M7D.git](https://github.com/MenuM7D/BAILEYS-M7D.git)

إضافة إلى package.json:
{
  "dependencies": {
    "@whiskeysockets/baileys": "git+[https://github.com/MenuM7D/BAILEYS-M7D.git](https://github.com/MenuM7D/BAILEYS-M7D.git)"
  }
}

الاستيراد في الكود:
import makeWASocket from '@whiskeysockets/baileys'

🎓 مثال بسيط للبدء
import makeWASocket, { DisconnectReason, useMultiFileAuthState } from '@whiskeysockets/baileys'
import { Boom } from '@hapi/boom'

async function connectToWhatsApp() {
    // إعداد حالة المصادقة (Auth State)
    const { state, saveCreds } = await useMultiFileAuthState('./auth_info_baileys')

    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true // طباعة رمز QR في الطرفية
    })

    // معالجة تحديثات الاتصال
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update

        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error as Boom)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('الاتصال مغلق، سبب:', lastDisconnect.error, '، إعادة الاتصال:', shouldReconnect)

            if(shouldReconnect) {
                connectToWhatsApp() // إعادة الاتصال
            }
        } else if(connection === 'open') {
            console.log('تم فتح الاتصال بنجاح!')
        }
    })

    // معالجة الرسائل الواردة
    sock.ev.on('messages.upsert', async ({ messages }) => {
        for (const m of messages) {
            console.log('رسالة جديدة:', JSON.stringify(m, undefined, 2))

            // مثال بسيط للرد
            console.log('الرد على:', m.key.remoteJid)
            await sock.sendMessage(m.key.remoteJid, { text: 'مرحباً! هذا بوت يعمل بمكتبة Baileys-M7D' })
        }
    })

    // حفظ بيانات الجلسة عند التحديث
    sock.ev.on('creds.update', saveCreds)
}

// تشغيل البوت
connectToWhatsApp()

📚 التوثيق التفصيلي
🔐 الاتصال والمصادقة
الاتصال برمز QR
import makeWASocket, { Browsers } from '@whiskeysockets/baileys'

const sock = makeWASocket({
    browser: Browsers.ubuntu('My App'), // تحديد نوع المتصفح
    printQRInTerminal: true // عرض رمز QR في الطرفية
})

// بعد تشغيل الكود، ستظهر رمز QR في الطرفية، قم بمسحه من تطبيق واتساب في هاتفك!

الاتصال برمز الاقتران
> ⚠️ ملاحظة: رمز الاقتران ليس Mobile API، إنه طريقة للاتصال بواتساب ويب بدون رمز QR
> 
import makeWASocket from '@whiskeysockets/baileys'

const sock = makeWASocket({
    printQRInTerminal: false // يجب أن يكون false لاستخدام رمز الاقتران
})

if (!sock.authState.creds.registered) {
    const number = '201220864180' // رقم الهاتف بدون + أو ()
    const code = await sock.requestPairingCode(number)
    console.log('رمز الاقتران:', code)
}

حفظ واستعادة الجلسات
import makeWASocket, { useMultiFileAuthState } from '@whiskeysockets/baileys'

const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')

const sock = makeWASocket({ auth: state })

// حفظ بيانات الاعتماد عند التحديث
sock.ev.on('creds.update', saveCreds)

استقبال السجل الكامل
 * قم بتعيين syncFullHistory إلى true
 * بشكل افتراضي، تستخدم Baileys إعدادات متصفح Chrome. إذا كنت ترغب في محاكاة اتصال سطح المكتب (واستقبال المزيد من سجل الرسائل)، أضف إعدادات المتصفح هذه:
<!-- end list -->
const suki = makeWASocket({
    // ...otherOpts,
    // يمكن استخدام Windows, Ubuntu هنا أيضاً
    browser: Browsers.macOS('Desktop'),
    syncFullHistory: true // تفعيل مزامنة السجل الكامل
})

ملاحظات هامة حول إعدادات الاتصال
التخزين المؤقت لبيانات المجموعات (Cache)
 * إذا كنت تستخدم Baileys للمجموعات، نوصي بتعيين cachedGroupMetadata في إعدادات الاتصال، تحتاج إلى تنفيذ ذاكرة تخزين مؤقت مثل هذا:
<!-- end list -->
import NodeCache from 'node-cache' // يجب تثبيت مكتبة node-cache

const groupCache = new NodeCache({stdTTL: 5 * 60, useClones: false}) // ذاكرة تخزين مؤقت للمجموعات

const suki = makeWASocket({
    cachedGroupMetadata: async (jid) => groupCache.get(jid)
})

suki.ev.on('groups.update', async ([event]) => {
    const metadata = await suki.groupMetadata(event.id)
    groupCache.set(event.id, metadata)
})

suki.ev.on('group-participants.update', async (event) => {
    const metadata = await suki.groupMetadata(event.id)
    groupCache.set(event.id, metadata)
})

تحسين نظام إعادة المحاولة وفك تشفير أصوات الاستطلاع
 * إذا كنت ترغب في تحسين إرسال الرسائل، وإعادة المحاولة عند حدوث خطأ، وفك تشفير أصوات الاستطلاع، فأنت بحاجة إلى مخزن (store) وتعيين getMessage في إعدادات الاتصال:
<!-- end list -->
const suki = makeWASocket({
    getMessage: async (key) => await getMessageFromStore(key) // دالة لجلب الرسالة من المخزن
})

استقبال الإشعارات في تطبيق واتساب
 * إذا كنت ترغب في استقبال الإشعارات في تطبيق واتساب، قم بتعيين markOnlineOnConnect إلى false:
<!-- end list -->
const suki = makeWASocket({
    markOnlineOnConnect: false // عدم إظهار "متصل الآن"
})

📨 إرسال الرسائل
الرسائل النصية
await sock.sendMessage(jid, { text: 'مرحباً بك في Baileys-M7D!' })

الاقتباس من رسالة
await sock.sendMessage(jid, { text: 'هذه رسالة مقتبسة' }, { quoted: message })

الإشارة للمستخدمين
await sock.sendMessage(
    jid,
    {
        text: '@201220864180 مرحباً!',
        mentions: ['201220864180@s.whatsapp.net'] // تمرير الـ JID المراد الإشارة إليه
    }
)

إعادة توجيه الرسائل
 * يجب أن يكون لديك كائن الرسالة (message)، يمكن جلبه من المخزن أو استخدام كائن رسالة واردة.
<!-- end list -->
const msg = getMessageFromStore() // قم بتنفيذ هذا من جانبك
await suki.sendMessage(jid, { forward: msg, force: true }) // واتساب سيعيد توجيه الرسالة!

رسائل الموقع
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

رسائل الموقع المباشر
await suki.sendMessage(
    jid,
    {
        location: {
            degreesLatitude: 24.121231,
            degreesLongitude: 55.1121221
        },
        live: true // تفعيل الموقع المباشر
    }
)

رسائل الاتصال
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
            contacts: [{ vcard }] // مصفوفة من كائنات الـ vCard
        }
    }
)

رسائل التفاعل
await sock.sendMessage(
    jid,
    {
        react: {
            text: '❤️', // استخدم سلسلة فارغة لإزالة التفاعل
            key: message.key // مفتاح الرسالة للتفاعل معها
        }
    }
)

تثبيت الرسالة
 * تحتاج إلى تمرير key الخاص بالرسالة.
 * الوقت يمكن أن يكون: 24 ساعة (86400 ثانية)، 7 أيام (604800 ثانية)، 30 يوم (2592000 ثانية).
<!-- end list -->
await suki.sendMessage(
    jid,
    {
        pin: {
            type: 1, // 2 للإزالة
            time: 86400, // المدة بالثواني
            key: Key
        }
    }
)

رسائل "الاحتفاظ" (Keep Message)
await suki.sendMessage(
    jid,
    {
        keep: {
            key: Key,
            type: 1 // أو 2
        }
    }
)

رسائل الاستطلاع
await sock.sendMessage(
    jid,
    {
        poll: {
            name: 'ما رأيك في Baileys-M7D؟',
            values: ['ممتاز', 'جيد', 'يحتاج تحسين'],
            selectableCount: 1, // عدد الخيارات الممكن تحديدها
            toAnnouncementGroup: false // أو true للمجموعات الإعلانية
        }
    }
)

رسائل نتيجة الاستطلاع
await suki.sendMessage(
    jid,
    {
        pollResult: {
            name: 'مرحباً', // اسم الاستطلاع
            values: [
               ['خيار 1', 1000], // [اسم الخيار, عدد الأصوات]
               ['خيار 2', 2000]
           ]
        }
    }
)

رسائل المكالمات (Call)
await suki.sendMessage(
    jid,
    {
        call: {
            name: 'مرحباً',
            type: 1 // 1 للصوت، 2 للفيديو
        }
    }
)

رسائل الحدث (Event)
await suki.sendMessage(
    jid,
    {
        event: {
            isCanceled: false,
            name: 'عطلة معاً!',
            description: 'من يريد المجيء؟',
            location: {
                degreesLatitude: 24.121231,
                degreesLongitude: 55.1121221,
                name: 'اسم الموقع'
            },
            call: 'audio', // أو 'video'
            startTime: Date.now(),
            endTime: Date.now() + 3600000,
            extraGuestsAllowed: true
        }
    }
)

رسائل الطلب (Order)
await suki.sendMessage(
    jid,
    {
        order: {
            orderId: '574xxx',
            thumbnail: 'your_thumbnail_buffer_or_url',
            itemCount: 3,
            status: 'INQUIRY', // INQUIRY || ACCEPTED || DECLINED
            surface: 'CATALOG', // CATALOG || CART
            message: 'وصف الطلب',
            orderTitle: "عنوان الطلب",
            sellerJid: 'seller_jid@s.whatsapp.net',
            token: 'your_token',
            totalAmount1000: 10000, // المبلغ بـ 1000 ضعف (10.000 = 10 EGP)
            totalCurrencyCode: 'EGP'
        }
    }
)

رسائل المنتج (Product)
await suki.sendMessage(
    jid,
    {
        product: {
            productImage: { url: 'your_url' }, // أو buffer
            productId: 'your_id',
            title: 'عنوان المنتج',
            description: 'وصف المنتج',
            currencyCode: 'EGP',
            priceAmount1000: 50000, // المبلغ بـ 1000 ضعف
            retailerId: 'your_reid',
            url: 'your_url',
            productImageCount: 1,
        },
       businessOwnerJid: 'business_jid@s.whatsapp.net'
    }
)

رسائل الدفع (Payment)
await suki.sendMessage(
    jid,
    {
        payment: {
            note: 'مرحباً!',
            currency: 'EGP',
            amount: '10000',
            from: '628xxxx@s.whatsapp.net',
        }
    }
)

رسائل دعوة الدفع
await suki.sendMessage(
    jid,
    {
        paymentInvite: {
            type: 1, // 1 || 2 || 3
            expiry: 0
        }
    }
)

رسائل دعوة المسؤول (للقنوات)
await suki.sendMessage(
    jid,
    {
        adminInvite: {
            jid: '123xxx@newsletter',
            name: 'اسم القناة',
            caption: 'من فضلك كن مسؤولاً في قناتي',
            expiration: 86400,
            jpegThumbnail: Buffer // اختياري
        }
    }
)

رسائل دعوة المجموعة
await suki.sendMessage(
    jid,
    {
        groupInvite: {
            jid: '123xxx@g.us',
            name: 'اسم المجموعة',
            caption: 'من فضلك انضم لمجموعتي',
            code: 'code_invite',
            expiration: 86400,
            jpegThumbnail: Buffer, // اختياري
        }
    }
)

رسائل حزمة الملصقات
await suki.sendMessage(
    jid,
    {
        stickerPack: {
            name: 'مرحباً',
            publisher: 'By M7D',
            description: 'وصف',
            cover: Buffer, // Buffer صورة الغلاف
            stickers: [{
                sticker: { url: '[https://example.com/1234.webp](https://example.com/1234.webp)' },
                emojis: ['❤'], // اختياري
            },
            {
                sticker: Buffer, // Buffer الملصق
                emojis: ['👍'], // اختياري
            }]
        }
    }
)

رسالة مشاركة رقم الهاتف
await suki.sendMessage(
    jid,
    {
        sharePhoneNumber: {}
    }
)

رسالة طلب رقم الهاتف
await suki.sendMessage(
    jid,
    {
        requestPhoneNumber: {}
    }
)

رسائل رد الأزرار
// رد على قائمة
await suki.sendMessage(
    jid,
    {
        buttonReply: {
            name: 'مرحباً',
            description: 'وصف',
            rowId: 'ID'
       },
       type: 'list'
    }
)
// رد عادي
await suki.sendMessage(
    jid,
    {
        buttonReply: {
            displayText: 'مرحباً',
            id: 'ID'
       },
       type: 'plain'
    }
)

رسائل الأزرار
await suki.sendMessage(
    jid,
    {
        text: 'هذه رسالة أزرار!',
        // image: buffer or // image: { url: url } إذا أردت استخدام صور
        caption: 'تعليق', // استخدم هذا إذا كنت تستخدم صورة أو فيديو
        footer: 'تذييل الرسالة',
        buttons: [{
            buttonId: 'Id1',
            buttonText: {
                 displayText: 'الزر 1'
              }
          },
          {
            buttonId: 'Id2',
            buttonText: {
                 displayText: 'الزر 2'
              }
         }]
    }
)

رسائل القوائم
// تعمل فقط في الدردشات الخاصة
await suki.sendMessage(
    jid,
    {
        text: 'هذه رسالة قائمة!',
        footer: 'تذييل',
        title: 'عنوان القائمة العريض',
        buttonText: 'النص المطلوب على زر عرض القائمة',
        sections: [
           {
         	title: 'القسم 1',
         	rows: [{
                title: 'خيار 1',
                rowId: 'option1'
             },
 	        {
                title: 'خيار 2',
                rowId: 'option2',
                description: 'هذا وصف للخيار 2'
           }]
       },
       {
       	title: 'القسم 2',
       	rows: [{
               title: 'خيار 3',
               rowId: 'option3'
           }]
       }]
    }
)

رسائل قائمة المنتجات
// تعمل فقط في الدردشات الخاصة
await suki.sendMessage(
    jid,
    {
        text: 'هذه قائمة منتجات!',
        footer: 'تذييل',
        title: 'عنوان القائمة',
        buttonText: 'نص زر عرض القائمة',
        productList: [{
            title: 'عنوان القسم',
            products: [
               { productId: '1234' },
               { productId: '5678' }
            ]
        }],
        businessOwnerJid: '628xxx@s.whatsapp.net',
        thumbnail: '[https://example.com/jdbenkksjs.jpg](https://example.com/jdbenkksjs.jpg)' // أو buffer
    }
)

رسائل البطاقات (Cards)
await suki.sendMessage(
    jid,
    {
        text: 'نص الرسالة',
        title: 'عنوان الرسالة',
        subtile: 'عنوان فرعي',
        footer: 'تذييل',
        cards: [
           {
              image: { url: '[https://example.com/jdbenkksjs.jpg](https://example.com/jdbenkksjs.jpg)' }, // أو buffer
              title: 'عنوان البطاقة',
              body: 'نص البطاقة',
              footer: 'تذييل البطاقة',
              buttons: [
                  {
                      name: 'quick_reply',
                      buttonParamsJson: JSON.stringify({
                         display_text: 'نص الزر',
                         id: 'ID'
                      })
                  },
                  {
                      name: 'cta_url',
                      buttonParamsJson: JSON.stringify({
                         display_text: 'نص الزر',
                         url: '[https://www.example.com](https://www.example.com)'
                      })
                  }
              ]
           }
        ]
    }
)

رسائل القوالب (Template)
>  * (لم تعد تعمل)
> 
await suki.sendMessage(
    jid,
    {
       text: 'هذه رسالة قالب!',
       footer: 'تذييل',
       templateButtons: [{
           index: 1,
           urlButton: {
                displayText: 'تابعني',
                url: '[https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w)'
             },
         },
         {
            index: 2,
            callButton: {
                displayText: 'اتصل بي!',
                phoneNumber: '201220864180'
            },
        },
        {
           index: 3,
           quickReplyButton: {
                displayText: 'هذا رد سريع، مثل الأزرار العادية!',
                id: 'id-like-buttons-message'
            },
       }]
    }
)

رسائل الأزرار التفاعلية
await suki.sendMessage(
    jid,
    {
        text: 'هذه رسالة تفاعلية!',
        title: 'عنوان',
        subtitle: 'عنوان فرعي',
        footer: 'تذييل',
        interactiveButtons: [
            {
                name: 'quick_reply',
                buttonParamsJson: JSON.stringify({
                    display_text: 'اضغطني!',
                    id: 'your_id'
                })
            },
            {
                name: 'cta_url',
                buttonParamsJson: JSON.stringify({
                    display_text: 'تابعني',
                    url: '[https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w)',
                    merchant_url: '[https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w)'
                })
            },
            {
                name: 'cta_copy',
                buttonParamsJson: JSON.stringify({
                    display_text: 'انسخ الرابط!',
                    copy_code: '[https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w](https://whatsapp.com/channel/0029ValNLOS7IUYNlsgu9X3w)'
                })
            },
            {
                name: 'cta_call',
                buttonParamsJson: JSON.stringify({
                    display_text: 'اتصل بي!',
                    phone_number: '201220864180'
                })
            },
            // ... (يمكن إضافة المزيد من أنواع الأزرار التفاعلية هنا)
        ]
    }
)

رسائل (Status Mentions)
const jidat = [
    '123451679@g.us',
    '62xxxxxxx@s.whatsapp.net'
]
// نص
await suki.sendStatusMentions(
    {
      text: 'مرحباً بالجميع :3',
      font: 2, // اختياري (1-6)
      textColor: 'FF0000', // اختياري
      backgroundColor: '#000000' // اختياري
    },
    jids // بحد أقصى 5 منشنات
)

// صورة
await suki.sendStatusMentions(
    {
      Image: { url: '[https://example.com/ruriooe.jpg](https://example.com/ruriooe.jpg)' }, // أو buffer
      caption: 'مرحباً بالجميع :3' // اختياري
    },
    jids
)

رسائل المتجر (Shop)
await suki.sendMessage(
    jid,
    {
       text: 'نص',
       title: 'عنوان',
       subtitle: 'عنوان فرعي',
       footer: 'تذييل',
       shop: {
          surface: 1, // 2 | 3 | 4
          id: '[https://example.com](https://example.com)'
       },
       viewOnce: true
    }
)

رسائل المجموعة (Collection)
await suki.sendMessage(
    jid,
    {
       text: 'نص',
       title: 'عنوان',
       subtitle: 'عنوان فرعي',
       footer: 'تذييل',
       collection: {
          bizJid: 'jid',
          id: '[https://example.com](https://example.com)',
          version: 1
       },
       viewOnce: true
    }
)

ميزة أيقونة الذكاء الاصطناعي (AI)
await suki.sendMessage(
    jid,
    {
        text: 'مرحباً'
    }, {
    ai: true // أضف خاصية ai واجعلها true لتفعيل أيقونة الذكاء الاصطناعي
    }
)

إرسال رسائل مع معاينة الروابط
 * بشكل افتراضي، لا يقوم واتساب بإنشاء معاينات للروابط المرسلة من الويب.
 * لتمكين هذه الميزة، أضف link-preview-js كمكتبة تابعة لمشروعك باستخدام yarn add link-preview-js.
 * إرسال الرابط:
<!-- end list -->
await suki.sendMessage(
    jid,
    {
        text: 'مرحباً، تم إرسال هذا باستخدام [https://github.com/MenuM7D/BAILEYS-M7D](https://github.com/MenuM7D/BAILEYS-M7D)'
    }
)

🎬 الوسائط المتعددة
إرسال الصور
await sock.sendMessage(
    jid,
    {
        image: { url: './image.jpg' },
        caption: 'صورة جميلة 📸'
    }
)

إرسال الفيديو
await sock.sendMessage(
    jid,
    {
        video: { url: './video.mp4' },
        caption: 'شاهد هذا الفيديو 🎥'
    }
)

إرسال الصوت
await sock.sendMessage(
    jid,
    {
        audio: { url: './audio.mp3' },
        mimetype: 'audio/mp4',
        ptt: true // رسالة صوتية (بصمة)
    }
)

إرسال صور GIF
 * واتساب لا يدعم ملفات .gif، لذلك نرسلها كفيديو .mp4 مع تفعيل gifPlayback.
<!-- end list -->
import * as fs from 'fs' // يجب استيراد fs لقراءة الملفات

await suki.sendMessage(
    jid,
    {
        video: fs.readFileSync('Media/ma_gif.mp4'),
        caption: 'مرحباً',
        gifPlayback: true
    }
)

إرسال فيديو PTV (دائري)
 * (رسائل الفيديو الدائرية الفورية)
<!-- end list -->
await suki.sendMessage(
    id,
    {
        video: {
            url: './Media/ma_gif.mp4'
        },
        ptv: true // تفعيل ميزة PTV (Personalized Video)
    }
)

الملصقات
await sock.sendMessage(
    jid,
    {
        sticker: { url: './sticker.webp' }
    }
)

الألبومات (عدة وسائط)
 * (إرسال عدة صور وفيديوهات في رسالة واحدة)
<!-- end list -->
await suki.sendMessage(
    id,
    {
        album: [{
        	image: {
        		url: '[https://example.com/itsukichan.jpg](https://example.com/itsukichan.jpg)'
        	},
        	caption: 'مرحباً'
        }, {
        	image: Buffer,
        	caption: 'مرحباً 2'
        }, {
        	video: {
        		url: '[https://example.com/itsukichan.mp4](https://example.com/itsukichan.mp4)'
        	},
        	caption: 'مرحباً 3'
        }]
    }
)

رسائل المشاهدة مرة واحدة
await sock.sendMessage(
    jid,
    {
        image: { url: './image.jpg' },
        viewOnce: true,
        caption: 'شاهد هذه الصورة مرة واحدة فقط!'
    }
)

الصور المصغرة للوسائط
 * للصور والملصقات، يمكن إنشاء الصورة المصغرة تلقائياً إذا أضفت jimp أو sharp كمكتبة تابعة (yarn add jimp).
 * للصور المصغرة للفيديو، يجب أن يكون ffmpeg مثبتاً على نظامك.
✍️ تعديل ومعالجة الرسائل
تعديل الرسائل
const msg = await sock.sendMessage(jid, { text: 'نص أصلي' })

// تعديل الرسالة
await sock.sendMessage(jid, {
    text: 'نص معدل',
    edit: msg.key // مفتاح الرسالة المراد تعديلها
})

حذف الرسائل (للجميع)
const msg = await sock.sendMessage(jid, { text: 'سيتم حذف هذه الرسالة' })

// حذف للجميع
await sock.sendMessage(jid, { delete: msg.key })

تحميل الوسائط
import { downloadMediaMessage } from '@whiskeysockets/baileys'
import { createWriteStream } from 'fs'

sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0]

    if (!msg.message) return

    if (msg.message.imageMessage) {
        const stream = await downloadMediaMessage(
            msg,
            'stream', // يمكن أن تكون 'buffer' أيضاً
            {},
            {
                // ... logger,
                // تمرير هذا حتى تتمكن baileys من طلب إعادة تحميل الوسائط المحذوفة
                reuploadRequest: sock.updateMediaMessage
            }
        )

        const writeStream = createWriteStream('./downloaded-image.jpg')
        stream.pipe(writeStream)
    }
})

إعادة رفع الوسائط إلى واتساب
 * يقوم واتساب بإزالة الوسائط القديمة من خوادمه تلقائياً. للوصول إليها، يجب إعادة رفعها.
<!-- end list -->
await suki.updateMediaMessage(msg) // msg هو كائن الرسالة

📞 المكالمات والحالات
رفض المكالمات
 * يمكنك الحصول على callId و callFrom من حدث call.
<!-- end list -->
await suki.rejectCall(callId, callFrom)

إرسال الحالات في الدردشة
 * (مثل: متصل، يكتب...)
 * الحالة تنتهي صلاحيتها بعد حوالي 10 ثواني.
<!-- end list -->
await suki.sendPresenceUpdate('available', jid) // متصل
await suki.sendPresenceUpdate('unavailable', jid) // غير متصل
await suki.sendPresenceUpdate('composing', jid) // يكتب...
await suki.sendPresenceUpdate('recording', jid) // يسجل صوتاً...
await suki.sendPresenceUpdate('paused', jid) // توقف عن الكتابة/التسجيل

قراءة الرسائل (علامة الصح الزرقاء)
 * يجب عليك تمرير key الخاص بالرسالة.
<!-- end list -->
const key: WAMessageKey // تعريف مفتاح الرسالة
// يمكن تمرير مصفوفة من المفاتيح لقراءة عدة رسائل
await suki.readMessages([key])

💬 تعديل الدردشات
أرشفة دردشة
const lastMsgInChat = await getLastMessageInChat(jid) // قم بتنفيذ هذا من جانبك
await suki.chatModify({ archive: true, lastMessages: [lastMsgInChat] }, jid)

كتم/إلغاء كتم دردشة
 * الأوقات المدعومة: 8 ساعات (بالمللي ثانية)، 7 أيام، أو null للإلغاء.
<!-- end list -->
// كتم لمدة 8 ساعات
await suki.chatModify({ mute: 8 * 60 * 60 * 1000 }, jid)
// إلغاء الكتم
await suki.chatModify({ mute: null }, jid)

تحديد دردشة كمقروءة/غير مقروءة
const lastMsgInChat = await getLastMessageInChat(jid) // قم بتنفيذ هذا من جانبك
// تحديد كغير مقروءة
await suki.chatModify({ markRead: false, lastMessages: [lastMsgInChat] }, jid)

حذف رسالة (لنفسي فقط)
await suki.chatModify(
    {
        clear: {
            messages: [
                {
                    id: 'MESSAGE_ID',
                    fromMe: true, // صحيح إذا كانت الرسالة مرسلة من البوت
                    timestamp: '1654823909'
                }
            ]
        }
    },
    jid
)

حذف دردشة
const lastMsgInChat = await getLastMessageInChat(jid) // قم بتنفيذ هذا من جانبك
await suki.chatModify({
        delete: true,
        lastMessages: [
            {
                key: lastMsgInChat.key,
                messageTimestamp: lastMsgInChat.messageTimestamp
            }
        ]
    },
    jid
)

تثبيت/إلغاء تثبيت دردشة
await suki.chatModify({
        pin: true // أو `false` لإلغاء التثبيت
    },
    jid
)

تمييز رسالة بنجمة/إلغاء التمييز
await suki.chatModify({
        star: {
            messages: [
                {
                    id: 'messageID',
                    fromMe: true // أو `false`
                }
            ],
            star: true // - true: تمييز بنجمة false: إلغاء التمييز
        }
    },
    jid
)

الرسائل ذاتية الاختفاء
 * الأوقات المدعومة (بالثواني): 0 (إلغاء)، 86400 (24 ساعة)، 604800 (7 أيام)، 7776000 (90 يوم).
<!-- end list -->
import { WA_DEFAULT_EPHEMERAL } from '@whiskeysockets/baileys'

// تشغيل (7 أيام افتراضي)
await suki.sendMessage(
    jid,
    { disappearingMessagesInChat: WA_DEFAULT_EPHEMERAL }
)

// إيقاف
await suki.sendMessage(
    jid,
    { disappearingMessagesInChat: false }
)

مسح الرسائل
 * (Clear Messages)
<!-- end list -->
await suki.clearMessage(jid, key, timestamps) // يتم تمرير مفتاح الرسالة و الطابع الزمني

🔍 استعلامات المستخدم والبيانات
التحقق من وجود رقم على واتساب
const [result] = await suki.onWhatsApp(jid)
if (result.exists) console.log (`${jid} موجود على واتساب، jid: ${result.jid}`)

جلب سجل الدردشة
 * تحتاج إلى أقدم رسالة في الدردشة.
<!-- end list -->
const msg = await getOldestMessageInChat(jid) // قم بتنفيذ هذا من جانبك
await suki.fetchMessageHistory(
    50, // الكمية (الحد الأقصى: 50 لكل طلب)
    msg.key,
    msg.messageTimestamp
)
// سيتم استقبال الرسائل في حدث 'messaging.history-set'

جلب الحالة (Status)
const status = await suki.fetchStatus(jid)
console.log('الحالة: ' + status.status)

جلب صورة الملف الشخصي
 * (للأشخاص، المجموعات، والقنوات)
<!-- end list -->
// لصورة بدقة منخفضة
const ppUrl = await suki.profilePictureUrl(jid)
console.log(ppUrl)

جلب ملف الأعمال
 * (مثل الوصف أو الفئة)
<!-- end list -->
const profile = await suki.getBusinessProfile(jid)
console.log('وصف العمل: ' + profile.description + ', الفئة: ' + profile.category)

جلب حالة وجود المستخدم
 * (إذا كان يكتب أو متصل)
<!-- end list -->
// يتم جلب التحديث واستدعاؤه هنا
suki.ev.on('presence.update', console.log)

// طلب تحديثات لدردشة معينة
await suki.presenceSubscribe(jid)

👤 إدارة الملف الشخصي
تغيير الحالة (About)
await suki.updateProfileStatus('مطور عربي محترف 💻')

تغيير الاسم
await suki.updateProfileName('M7D Developer')

تغيير صورة الملف الشخصي
 * (لك أو للمجموعات)
<!-- end list -->
await suki.updateProfilePicture(jid, { url: './new-profile-picture.jpeg' })

إزالة صورة الملف الشخصي
 * (لك أو للمجموعات)
<!-- end list -->
await suki.removeProfilePicture(jid)

👥 إدارة المجموعات
إنشاء مجموعة
const group = await sock.groupCreate('مجموعة رائعة', ['1234@s.whatsapp.net', '5678@s.whatsapp.net'])
console.log('تم إنشاء المجموعة برقم:', group.gid)
await sock.sendMessage(group.id, { text: 'مرحباً بالجميع! 👋' })

إضافة وإزالة الأعضاء
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

ترقية وتخفيض المسؤولين
// ترقية إلى مسؤول
await sock.groupParticipantsUpdate(jid, ['1234@s.whatsapp.net'], 'promote')

// تخفيض من مسؤول
await sock.groupParticipantsUpdate(jid, ['1234@s.whatsapp.net'], 'demote')

تغيير اسم المجموعة
await sock.groupUpdateSubject(jid, 'اسم جديد للمجموعة')

تغيير وصف المجموعة
await sock.groupUpdateDescription(jid, 'وصف جديد للمجموعة')

تغيير إعدادات المجموعة
// السماح للمسؤولين فقط بالإرسال
await sock.groupSettingUpdate(jid, 'announcement')
// السماح للجميع بالإرسال
await sock.groupSettingUpdate(jid, 'not_announcement')
// السماح للجميع بتعديل إعدادات المجموعة
await sock.groupSettingUpdate(jid, 'unlocked')
// السماح للمسؤولين فقط بتعديل إعدادات المجموعة
await sock.groupSettingUpdate(jid, 'locked')

مغادرة المجموعة
await suki.groupLeave(jid)

الحصول على رابط الدعوة
const code = await sock.groupInviteCode(jid)
const link = '[https://chat.whatsapp.com/](https://chat.whatsapp.com/)' + code
console.log('رابط الدعوة:', link)

إلغاء رابط الدعوة
const code = await suki.groupRevokeInvite(jid)
console.log('كود المجموعة الجديد: ' + code)

الانضمام للمجموعات (عبر الكود)
const code = 'ABC123XYZ' // رمز الدعوة فقط بدون الرابط
const response = await sock.groupAcceptInvite(code)
console.log('تم الانضمام إلى المجموعة:', response)

الحصول على معلومات المجموعة عبر الكود
const response = await suki.groupGetInviteInfo(code)
console.log('معلومات المجموعة: ' + response)

جلب بيانات المجموعة (Metadata)
 * (المشاركين، الاسم، الوصف...)
<!-- end list -->
const metadata = await suki.groupMetadata(jid)
console.log(metadata.id + ', العنوان: ' + metadata.subject + ', الوصف: ' + metadata.desc)

الانضمام عبر رسالة دعوة
const response = await suki.groupAcceptInviteV4(jid, groupInviteMessage)
console.log('تم الانضمام إلى: ' + response)

جلب قائمة طلبات الانضمام
const response = await suki.groupRequestParticipantsList(jid)
console.log(response)

قبول/رفض طلبات الانضمام
const response = await suki.groupRequestParticipantsUpdate(
    jid, // jid المجموعة
    ['abcd@s.whatsapp.net', 'efgh@s.whatsapp.net'],
    'approve' // أو 'reject'
)
console.log(response)

جلب بيانات كل المجموعات المشارك بها
const response = await suki.groupFetchAllParticipating()
console.log(response)

تفعيل/إلغاء الرسائل ذاتية الاختفاء للمجموعة
 * (الأوقات بالثواني: 0, 86400, 604800, 7776000)
<!-- end list -->
await suki.groupToggleEphemeral(jid, 86400)

تغيير وضع إضافة الأعضاء
await suki.groupMemberAddMode(
    jid,
    'all_member_add' // أو 'admin_add'
)

⚙️ الإعدادات والخصوصية
حظر المستخدمين
// حظر مستخدم
await sock.updateBlockStatus(jid, 'block')

// إلغاء الحظر
await sock.updateBlockStatus(jid, 'unblock')

جلب إعدادات الخصوصية
const privacySettings = await suki.fetchPrivacySettings(true)
console.log('إعدادات الخصوصية: ' + privacySettings)

جلب قائمة الحظر
const response = await suki.fetchBlocklist()
console.log(response)

تحديث خصوصية آخر ظهور
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await suki.updateLastSeenPrivacy(value)

تحديث خصوصية الاتصال (Online)
const value = 'all' // 'match_last_seen'
await suki.updateOnlinePrivacy(value)

تحديث خصوصية صورة الملف الشخصي
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await suki.updateProfilePicturePrivacy(value)

تحديث خصوصية الحالة
const value = 'all' // 'contacts' | 'contact_blacklist' | 'none'
await suki.updateStatusPrivacy(value)

تحديث خصوصية قراءة الرسائل (Read Receipts)
const value = 'all' // 'none'
await suki.updateReadReceiptsPrivacy(value)

تحديث خصوصية إضافة المجموعات
const value = 'all' // 'contacts' | 'contact_blacklist'
await suki.updateGroupsAddPrivacy(value)

تحديث الوضع الافتراضي للرسائل ذاتية الاختفاء
const ephemeral = 86400 // (الأوقات بالثواني: 0, 86400, 604800, 7776000)
await suki.updateDefaultDisappearingMode(ephemeral)

📢 القوائم البريدية والقصص
إرسال رسائل للقوائم البريدية والقصص
await suki.sendMessage(
    jid, // jid القائمة البريدية أو status@broadcast للقصص
    {
        image: { url: url },
        caption: caption
    },
    {
        backgroundColor: backgroundColor,
        font: font,
        statusJidList: statusJidList,
        broadcast: true
    }
)

جلب بيانات قائمة بريدية
const bList = await suki.getBroadcastListInfo('1234@broadcast')
console.log (`اسم القائمة: ${bList.name}, المستلمون: ${bList.recipients}`)

🛠️ متقدم
معالجة الأحداث
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

// فك تشفير أصوات الاستطلاع (يحتاج لمخزن - Store)
suki.ev.on("messages.update", async (chatUpdate) => {
    for(const { key, update } of chatUpdate) {
         if(update.pollUpdates && key.fromMe) {
           const pollCreation = await getMessage(key) // تحتاج لوجود مخزن (store)
             if(pollCreation) {
               const pollUpdate = await getAggregateVotesInPollMessage({
                    message: pollCreation,
                    pollUpdates: update.pollUpdates,
                })
               const toCmd = pollUpdate.filter(v => v.voters.length !== 0)[0]?.name
               if (toCmd == undefined) return
               console.log(toCmd)
	        }
        }
    }
})

تخزين البيانات
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

شرح معرفات واتساب (JIDs)
 * id هو معرف واتساب، ويسمى أيضاً jid.
   * للأشخاص: [كود الدولة][رقم الهاتف]@s.whatsapp.net (مثال: 201220864180@s.whatsapp.net)
   * للمجموعات: [رقم]-[رقم]@g.us (مثال: 123456789-123345@g.us)
   * للقوائم البريدية: [timestamp]@broadcast
   * للقصص (Status): status@broadcast
الدوال المساعدة
 * getContentType: يُرجع نوع محتوى أي رسالة.
 * getDevice: يُرجع الجهاز الذي أُرسلت منه الرسالة.
 * makeCacheableSignalKeyStore: يجعل مخزن المصادقة أسرع.
 * downloadContentFromMessage: يقوم بتحميل المحتوى من أي رسالة.
كتابة وظائف مخصصة
 * يمكنك تمكين مستوى debug في المسجل (logger) لرؤية جميع الرسائل التي يرسلها واتساب.
<!-- end list -->
import P from 'pino'

const suki = makeWASocket({
    logger: P({ level: 'debug' }),
})

 * يمكنك بعد ذلك تسجيل رد نداء (callback) لأحداث Websocket معينة:
<!-- end list -->
// لأي رسالة بـ 'tag' يساوي 'edge_routing'
suki.ws.on('CB:edge_routing', (node: BinaryNode) => { /* ... */ })

// لأي رسالة بـ 'tag' يساوي 'edge_routing' و 'id' يساوي 'abcd'
suki.ws.on('CB:edge_routing,id:abcd', (node: BinaryNode) => { /* ... */ })

📖 روابط مفيدة
 * مستودع GitHub
 * التوثيق الكامل
 * الأمثلة العملية
🙏 شكر وتقدير
شكراً لـ:
 * @pokearaujo - لتوثيق نظام الأجهزة المتعددة
 * @Sigalor - لتوثيق واتساب ويب
 * @Rhymen - للتطبيق بلغة Go
 * @BrunoSobrino - للنسخة الأصلية المعدلة
📄 الترخيص
هذا المشروع مرخص تحت رخصة MIT - انظر ملف LICENSE للمزيد من التفاصيل.
💬 الدعم والمساعدة
إذا كنت بحاجة لمساعدة أو لديك استفسار:
<div align="center">
تواصل معي مباشرة
</div>
<div align="center">
صُنع بـ ❤️ بواسطة M7D-DEV
BAILEYS-M7D - مكتبة واتساب احترافية للمطورين العرب
</div>
