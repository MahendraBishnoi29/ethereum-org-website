---
title: دسترسی به داده‌ها
description: مروری بر مشکلات و راه حل های مربوط به در دسترس بودن داده ها در اتریوم
lang: fa
---

«اعتماد نکن، تایید کن» یک اصل رایج در اتریوم است. ایده این است که گره شما می‌تواند به طور مستقل صحت اطلاعاتی را که دریافت می‌کند با اجرای تمام تراکنش‌های موجود در بلوک‌هایی که از همتایان دریافت می‌کنند تأیید کند تا اطمینان حاصل شود که تغییرات پیشنهادی دقیقاً با تغییرات محاسبه‌شده مستقل توسط گره مطابقت دارند. این بدان معناست که گره‌ها نباید به صادق بودن فرستندگان بلوک اعتماد کنند. در صورت عدم وجود داده، این امکان پذیر نیست.

**در دسترس بودن داده** به اطمینان کاربر از اینکه داده های مورد نیاز برای تأیید یک بلوک واقعاً در دسترس همه شرکت کنندگان شبکه است، اشاره دارد. برای گره‌های کامل در لایه 1 اتریوم، این کار نسبتاً ساده است. گره کامل یک کپی از تمام داده‌های هر بلوک را دانلود می‌کند - داده‌ها _باید_ در دسترس باشند تا امکان دانلود وجود داشته باشد. بلوکی با داده های از دست رفته به جای اضافه شدن به بلاکچین، دور انداخته می شود. این "در دسترس بودن داده های زنجیره ای" است و یکی از ویژگی های بلاک چین های یکپارچه است. گره های کامل را نمی توان فریب داد تا تراکنش های نامعتبر را بپذیرند زیرا آنها هر تراکنش را برای خود دانلود و اجرا می کنند. با این حال، برای بلاک چین‌های مدولار، رول‌‌آپ های لایه 2 و کلاینت های سبک، چشم‌انداز در دسترس بودن داده‌ها پیچیده‌تر است و به برخی روش‌های تأیید پیچیده‌تر نیاز دارد.

## پیش‌نیازها {#prerequisites}

شما باید درک خوبی از [اصول بلاک چین](/developers/docs/intro-to-ethereum/)، به خصوص [مکانیسم های اجماع](/developers/docs/consensus-mechanisms/) داشته باشید. این صفحه همچنین فرض می‌کند که خواننده با [بلوک‌ها](/developers/docs/blocks/)، [تراکنش ها](/developers/docs/transactions/)، [گره‌ها](/developers/docs/nodes-and-clients/)، [راه‌حل‌های مقیاس‌بندی](/developers/docs/scaling/) و سایر موضوعات مرتبط آشنا است.

## مشکل در دسترس بودن داده ها {#the-data-availability-problem}

مشکل در دسترس بودن داده ها این است که باید به کل شبکه ثابت شود که شکل خلاصه شده برخی از داده های تراکنش که به بلاک چین اضافه می شود، واقعاً مجموعه ای از تراکنش های معتبر را نشان می دهد، اما بدون نیاز به همه گره ها برای دانلود همه داده ها. داده‌های کامل تراکنش برای تأیید مستقل بلوک‌ها ضروری است، اما نیاز به تمام گره‌ها برای دانلود همه داده‌های تراکنش، مانعی برای مقیاس‌پذیری است. هدف راه‌حل‌های مشکل در دسترس بودن داده، ارائه تضمین‌های کافی مبنی بر این است که داده‌های کامل تراکنش برای تأیید در دسترس شرکت‌کنندگانی از شبکه قرار گرفته است که داده‌ها را برای خود دانلود و ذخیره نمی‌کنند.

[گره‌های سبک](/developers/docs/nodes-and-clients/light-clients) و [رول‌‌آپ های لایه 2](/developers/docs/scaling) نمونه‌های مهمی از شرکت‌کنندگان در شبکه هستند که به تضمین‌های قوی در دسترس بودن داده نیاز دارند اما نمی‌توانند داده‌های تراکنش را برای خود دانلود و پردازش کنند. اجتناب از دانلود داده‌های تراکنش چیزی است که گره‌های سبک را سبک می‌کند و به رول‌‌آپ ها امکان می‌دهد راه‌حل‌های مقیاس‌پذیری مؤثری باشند.

در دسترس بودن داده ها همچنین یک نگرانی مهم برای کلاینت های [«بی حالت»](/roadmap/statelessness) اتریوم در آینده است که برای تأیید بلوک ها نیازی به دانلود و ذخیره داده های حالت ندارند. کلاینت های بی حالت هنوز باید مطمئن باشند که داده‌ها _جایی_ در دسترس هستند و به درستی پردازش شده‌اند.

## راه حل های در دسترس بودن داده ها {#data-availability-solutions}

### نمونه‌گیری در دسترس بودن داده (DAS) {#data-availability-sampling}

نمونه‌گیری در دسترس بودن داده (DAS) روشی برای شبکه برای بررسی در دسترس بودن داده‌ها بدون اعمال فشار زیاد بر هر گره منفرد است. هر گره (از جمله گره‌های بدون شرط‌بندی) تعدادی زیرمجموعه کوچک و تصادفی انتخاب شده از کل داده‌ها را دانلود می‌کند. دانلود موفقیت آمیز نمونه ها با اطمینان بالا تأیید می کند که همه داده ها در دسترس هستند. این به کدگذاری پاک کردن داده‌ها متکی است، که یک مجموعه داده معین را با اطلاعات اضافی گسترش می‌دهد (روش انجام این کار به این صورت است که تابعی به نام _چند جمله‌ای_ را بر روی داده‌ها قرار می‌دهد و آن چند جمله‌ای را در نقاط اضافی ارزیابی می‌کند). این اجازه می دهد تا داده های اصلی در صورت لزوم از داده های اضافی بازیابی شوند. نتیجه این ایجاد داده این است که اگر _هیچ_ کدام از داده‌های اصلی در دسترس نباشد، _نیمی_ از داده‌های توسعه‌یافته از دست خواهند رفت! مقدار نمونه های داده دانلود شده توسط هر گره را می توان تنظیم کرد به طوری که _به شدت_ احتمال دارد که حداقل یکی از قطعات داده نمونه برداری شده توسط هر مشتری وجود نداشته باشد _اگر_ کمتر از نیمی از داده ها واقعاً در دسترس باشد.

DAS برای اطمینان از اینکه اپراتورهای رول‌‌آپ داده‌های تراکنش خود را پس از اجرای [دنک‌شاردینگ کامل](/roadmap/danksharding/#what-is-danksharding) در دسترس قرار می‌دهند، استفاده می‌شود. گره های اتریوم به صورت تصادفی از داده های تراکنش ارائه شده در حباب ها با استفاده از طرح افزونگی که در بالا توضیح داده شد نمونه برداری می کنند تا اطمینان حاصل شود که همه داده ها وجود دارند. همین تکنیک همچنین می‌تواند برای اطمینان از اینکه تولیدکنندگان بلوک تمام داده‌های خود را برای ایمن کردن کلاینت های سبک در دسترس قرار می‌دهند، استفاده شود. به طور مشابه، تحت [جداسازی پیشنهاددهنده-سازنده](/roadmap/pbs) بلوک، فقط سازنده بلوک باید کل یک بلوک را پردازش کند - اعتبارسنج های دیگر با استفاده از نمونه گیری در دسترس بودن داده ها را تأیید می کنند.

### کمیته های در دسترس بودن داده ها {#data-availability-committees}

کمیته های در دسترس بودن داده ها (DACها) طرف های مورد اعتمادی هستند که در دسترس بودن داده ها را ارائه می دهند یا آن را تأیید می کنند. DAC ها را می توان به جای [یا در ترکیب با](https://hackmd.io/@vbuterin/sharding_proposal#Why-not-use-just-committees-and-not-DAS) DAS استفاده کرد. ضمانت‌های امنیتی که با کمیته‌ها ارائه می‌شوند به تشکیلات خاص بستگی دارد. برای مثال، اتریوم از زیرمجموعه‌های نمونه‌برداری تصادفی اعتبارسنج ها برای تأیید در دسترس بودن داده‌ها برای گره‌های سبک استفاده می‌کند.

DAC ها نیز توسط برخی از ولیدیوم‌ها استفاده می شوند. DAC مجموعه ای از گره های قابل اعتماد است که نسخه هایی از داده ها را به صورت آفلاین ذخیره می کند. DAC موظف است در صورت بروز اختلاف، داده ها را در دسترس قرار دهد. اعضای DAC همچنین امضاهای آنچین را منتشر می‌کنند تا ثابت کنند که داده‌های مذکور واقعاً در دسترس هستند. برخی ولیدیوم‌ها را با یک سیستم اعتبارسنج اثبات سهام (PoS) جایگزین می کنند. در اینجا، هر کسی می‌تواند اعتبارسنج شود و داده‌ها را خارج از زنجیره ذخیره کند. با این حال، آنها باید یک "مسیر" ارائه کنند که در یک قرارداد هوشمند سپرده می شود. در صورت رفتار مخرب، مانند مخفی نگه داشتن اطلاعات توسط اعتبارسنج، پیوند را می توان کاهش داد. کمیته های در دسترس بودن داده های اثبات سهام به طور قابل توجهی از DAC های معمولی ایمن تر هستند زیرا مستقیماً رفتار صادقانه را تشویق می کنند.

## در دسترس بودن داده ها و گره های سبک {#data-availability-and-light-nodes}

[گره های سبک](/developers/docs/nodes-and-clients/light-clients) باید صحت هدرهای بلوکی را که دریافت می کنند بدون بارگیری داده های بلوک تأیید کنند. هزینه این سَبُکی نیز ناتوانی در تأیید مستقل هدرهای بلوک با اجرای مجدد تراکنش ها به صورت محلی به روشی است که گره های کامل انجام می دهند.

گره های سبک اتریوم به مجموعه های تصادفی 512 اعتبارسنج که به _کمیته همگام سازی_ اختصاص داده شده اند اعتماد دارند. کمیته همگام‌سازی به‌عنوان یک DAC عمل می‌کند که با استفاده از یک امضای رمزنگاری به کلاینت های سبک می‌گوید که داده‌های سر صحیح هستند. هر روز، کمیته همگام سازی رفرش می شود. هر سرصفحه بلوک به گره‌های سیک هشدار می‌دهد که کدام اعتبارسنج ها باید بلوک _بعدی_ را امضا کنند، بنابراین نمی‌توان آنها را فریب داد تا به یک گروه مخرب که وانمود می‌کنند کمیته همگام‌سازی واقعی هستند، اعتماد کنند.

با این حال، چه اتفاقی می‌افتد اگر یک مهاجم به نحوی _موفق_ شود هدر بلوک مخرب را به کلاینت سبک ارسال کند و آنها را متقاعد کند که توسط یک کمیته همگام‌سازی صادقانه امضا شده است؟ در آن صورت، مهاجم می‌تواند تراکنش‌های نامعتبر را شامل شود و کلاینت سبک کورکورانه آنها را می‌پذیرد، زیرا آنها به‌طور مستقل تمام تغییرات حالت خلاصه‌شده در هدر بلوک را بررسی نمی‌کنند. برای محافظت در برابر این، کلاینت سبک می تواند از اثبات های تقلب استفاده کند.

روش کار این اثبات های تقلب به این صورت است که یک گره کامل، با مشاهده یک انتقال حالت نامعتبر که در اطراف شبکه شایعه می شود، می تواند به سرعت یک قطعه کوچک از داده را تولید کند که نشان دهد یک انتقال حالت پیشنهادی احتمالاً نمی تواند از مجموعه معینی از تراکنش ها ناشی شود و آن داده ها را برای همتایان پخش کند. گره‌های سبک می‌توانند آن موارد اثبات تقلب را انتخاب کرده و از آنها برای حذف هدرهای بلوک بد استفاده کنند، و اطمینان حاصل کنند که در زنجیره صادقانه مشابه گره‌های کامل باقی می‌مانند.

این متکی بر گره های کامل است که به داده های تراکنش کامل دسترسی دارند. مهاجمی که یک هدر بلوک بد پخش می‌کند و همچنین نمی‌تواند داده‌های تراکنش را در دسترس قرار دهد، می‌تواند از ایجاد اثبات تقلب توسط گره‌های کامل جلوگیری کند. گره‌های کامل ممکن است بتوانند هشداری درباره یک بلوک بد ارسال کنند، اما نمی‌توانند از هشدار خود با اثبات پشتیبان بگیرند، زیرا داده‌ها برای تولید اثبات در دسترس نبودند!

راه حل این مشکل در دسترس بودن داده ها DAS است. گره های سبک، تکه های تصادفی بسیار کوچکی از داده های حالت کامل را دانلود می کنند و از نمونه ها برای تأیید اینکه مجموعه داده کامل در دسترس است استفاده می کنند. احتمال واقعی فرض نادرست در دسترس بودن کامل داده ها پس از دانلود N قطعه تصادفی را می توان محاسبه کرد ([برای 100 تکه این شانس 10^-30 است](https://dankradfeist.de/ethereum/2019/12/20/data-availability-checks.html)، یعنی فوق‌العاده بعید است).

حتی در این سناریو، حملاتی که تنها چند بایت را در خود نگه می‌دارند، می‌توانند توسط کلاینت هایی که درخواست‌های داده تصادفی می‌کنند مورد توجه قرار نگیرند. کدگذاری پاک کردن این مشکل را با بازسازی قطعات کوچک از دست رفته داده که می تواند برای بررسی تغییرات حالت پیشنهادی مورد استفاده قرار گیرد، برطرف می کند. سپس می‌توان با استفاده از داده‌های بازسازی‌شده، یک اثبات تقلب ایجاد کرد و از پذیرش هدرهای بد توسط گره‌های سبک جلوگیری کرد.

**توجه:** DAS و اثبات تقلب هنوز برای کلاینت های سبک اتریوم اثبات سهام اجرا نشده اند، اما در نقشه راه هستند و به احتمال زیاد به شکل اثبات های مبتنی بر ZK-SNARK هستند. کلاینت های سبک امروزی به شکلی از DAC متکی هستند: آنها هویت کمیته همگام‌سازی را تأیید می‌کنند و سپس به هدرهای بلوک امضا شده‌ای که دریافت می‌کنند اعتماد می‌کنند.

## در دسترس بودن داده ها و رول‌‌آپ های لایه2 {#data-availability-and-layer-2-rollups}

[راه‌حل‌های مقیاس‌بندی لایه2](/layer-2/)، مانند [رول‌‌آپ ها](/glossary/#rollups)، هزینه‌های تراکنش را کاهش می‌دهند و با پردازش تراکنش‌های خارج از زنجیره، توان عملیاتی اتریوم را افزایش می‌دهند. تراکنش‌های رول‌‌آپ فشرده می شوند و به صورت دسته‌ای در اتریوم پست می‌شوند. دسته ها هزاران تراکنش خارج از زنجیره فردی را در یک تراکنش در اتریوم نشان می دهند. این باعث کاهش تراکم در لایه پایه و کاهش هزینه ها برای کاربران می شود.

با این حال، تنها زمانی می‌توان به تراکنش‌های «خلاصه» ارسال‌شده در اتریوم اعتماد کرد که تغییر حالت پیشنهادی به‌طور مستقل تأیید شود که نتیجه اعمال همه تراکنش‌های خارج از زنجیره است. اگر اپراتورهای رول‌‌آپ داده‌های تراکنش را برای این راستی‌آزمایی در دسترس قرار ندهند، ممکن است داده‌های نادرستی را به اتریوم ارسال کنند.

[رول‌آپ های خوش‌بینانه](/developers/docs/scaling/optimistic-rollups/) داده‌های تراکنش فشرده را به اتریوم ارسال می‌کنند و برای مدتی (معمولاً 7 روز) منتظر می‌مانند تا به تأییدکنندگان مستقل اجازه بررسی داده‌ها را بدهد. اگر کسی مشکلی را شناسایی کند، می تواند یک اثبات تقلب ایجاد کند و از آن برای به چالش کشیدن رول‌‌آپ استفاده کند. این باعث می شود زنجیره به عقب برگردد و بلوک نامعتبر را حذف کند. این تنها در صورتی امکان پذیر است که داده ها در دسترس باشند. در حال حاضر، دو راه وجود دارد که رول‌آپ های خوش‌بینانه داده های تراکنش را به L1 ارسال کنند. برخی رول‌‌آپ ها داده‌ها را به‌صورت دائمی به‌عنوان `CALLDATA` در دسترس قرار می‌دهند که به‌طور دائم در زنجیره زندگی می‌کنند. با اجرای EIP-4844، برخی از رول‌آپ ها داده‌های تراکنش خود را به جای ذخیره‌سازی حباب های ارزان‌تر ارسال می‌کنند. این ذخیره سازی دائمی نیست. تأییدکنندگان مستقل باید در عرض 18 روز قبل از حذف داده ها از لایه 1 اتریوم، حباب ها را استعلام کنند و چالش های خود را مطرح کنند. در دسترس بودن داده ها فقط توسط پروتکل اتریوم برای آن پنجره کوتاه ثابت تضمین شده است. پس از آن، مسئولیت سایر موجودات در اکوسیستم اتریوم می شود. هر گره می تواند در دسترس بودن داده ها را با استفاده از DAS تأیید کند، یعنی با دانلود نمونه های کوچک و تصادفی از داده های حباب.

[رول‌آپ های دانش صفر (ZK)](/developers/docs/scaling/zk-rollups) نیازی به ارسال داده‌های تراکنش ندارند زیرا [شواهد اعتبار دانش صفر](/glossary/#zk-proof) صحت انتقال حالت را تضمین می‌کند. با این حال، در دسترس بودن داده ها هنوز یک مشکل است زیرا ما نمی توانیم عملکرد رول‌آپ دانش صفر (یا تعامل با آن) را بدون دسترسی به داده های وضعیت آن تضمین کنیم. به عنوان مثال، اگر اپراتور جزئیاتی را درباره حالت رول‌آپ مخفی کند، کاربران نمی‌توانند موجودی خود را بدانند. همچنین، آنها نمی توانند با استفاده از اطلاعات موجود در یک بلوک جدید اضافه شده، به روز رسانی حالت را انجام دهند.

## در دسترس بودن داده در مقابل قابلیت بازیابی داده ها {#data-availability-vs-data-retrievability}

در دسترس بودن داده ها با قابلیت بازیابی داده ها متفاوت است. در دست داشتن داده ها با قابلیت بازیابی ها متفاوت است. لزوماً به این معنی نیست که داده ها برای همیشه قابل دسترسی هستند.

قابلیت بازیابی داده ها توانایی گره ها برای بازیابی _اطلاعات تاریخی_ از بلاک چین است. این داده تاریخی برای تأیید بلوک های جدید مورد نیاز نیست، فقط برای همگام سازی گره های کامل از بلوک پیدایش یا ارائه درخواست های تاریخی خاص مورد نیاز است.

پروتکل اصلی اتریوم در درجه اول مربوط به در دسترس بودن داده ها است، نه قابلیت بازیابی داده ها. قابلیت بازیابی داده‌ها را می‌توان توسط جمعیت کوچکی از گره‌های بایگانی که توسط اشخاص ثالث اجرا می‌شوند فراهم کرد، یا می‌توان آن را با استفاده از ذخیره‌سازی فایل غیرمتمرکز مانند [شبکه پورتال](https://www.ethportal.net/) در سراسر شبکه توزیع کرد.

## اطلاعات بیشتر {#further-reading}

- [WTF قابلیت دسترسی داده است؟](https://medium.com/blockchain-capital-blog/wtf-is-data-availability-80c2c95ded0f)
- [قابلیت دسترسی داده چیست؟](https://coinmarketcap.com/alexandria/article/what-is-data-availability)
- [دورنمای قابلیت دسترسی داده اتریوم خارج زنجیره](https://blog.celestia.org/ethereum-off-chain-data-availability-landscape/)
- [مقدمه‌ای بر بررسی‌های قابلیت دسترسی داده](https://dankradfeist.de/ethereum/2019/12/20/data-availability-checks.html)
- [شرحی بر شاردینگ + پیشنهاد DAS](https://hackmd.io/@vbuterin/sharding_proposal#ELI5-data-availability-sampling)
- [یادداشتی بر قابلیت دسترسی داده و کدگذاری پاک شدن](https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding#can-an-attacker-not-circumvent-this-scheme-by-releasing-a-full-unavailable-block-but-then-only-releasing-individual-bits-of-data-as-clients-query-for-them)
- [کمیته های در دسترس بودن داده ها.](https://medium.com/starkware/data-availability-e5564c416424)
- [کمیته های در دسترس بودن داده های اثبات سهام.](https://blog.matter-labs.io/zkporter-a-breakthrough-in-l2-scaling-ed5e48842fbf)
- [راه حل هایی برای مشکل بازیابی داده ها](https://notes.ethereum.org/@vbuterin/data_sharding_roadmap#Who-would-store-historical-data-under-sharding)
- [قابلیت دسترسی داده ها یا: رول‌آپ‌ها چطور یاد گرفتند دیگر نگران نباشند و اتریوم را دوست داشته باشند](https://ethereum2077.substack.com/p/data-availability-in-ethereum-rollups) 
