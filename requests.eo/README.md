# requests.eo

طلبات HTTP بسهولة للغة **eo**، مثل `requests` في بايثون. مكتوبة بـ eo نفسها فوق `http()`.

```eo
import requests

var r = requests.get("https://api.github.com/users/octocat")
print r.status_code, r.json()["name"]
```

انسخ `requests.eo` جنب ملفك، أو: `eo get <رابط المستودع>`.

---

## get

```eo
requests.get(url)
requests.get(url, headers)
requests.get(url, headers, proxy)
```

```eo
var r = requests.get("https://api.example.com/me", {"Authorization": "Bearer TOKEN"})
var r2 = requests.get("https://example.com", {}, "http://127.0.0.1:8080")    # بدون هيدرز، مع بروكسي
```

## باقي الطرق

| | |
|---|---|
| `requests.post(url, body, headers, proxy)` | `body` قاموس/قائمة (ينرسل JSON) أو نص كما هو |
| `requests.put(url, body, headers, proxy)` | |
| `requests.patch(url, body, headers, proxy)` | |
| `requests.delete(url, headers, proxy)` | |
| `requests.head(url, headers, proxy)` | |
| `requests.request(method, url, body, headers, proxy)` | أي طريقة |
| `requests.download(url, path, headers, proxy)` | ينزّل ملف ويحفظه |

كلها اختيارية بعد الرابط. تبي تتخطى وحدة؟ حط `{}` للهيدرز أو `none` للـ body.

## الجواب `r`

| | |
|---|---|
| `r.status_code` | `200` ، `404` ... و `0` لو ما وصل للسيرفر أصلًا |
| `r.ok` | `true` لو الكود من 200 لين 399 |
| `r.reason` | `"OK"` ، `"Not Found"` ... |
| `r.text` | الجواب كنص |
| `r.json()` | الجواب كقاموس أو قائمة |
| `r.content` | البايتات زي ما هي (صور وملفات): `write_file "a.png", r.content` |
| `r.headers` | هيدرز الجواب (أسماؤها حروف صغيرة) |
| `r.header("Content-Type")` | هيدر واحد، بدون ما يفرق كبير وصغير |
| `r.url` ، `r.method` | |
| `r.elapsed` | كم ثانية أخذ الطلب |
| `r.error` | سبب الفشل لو `status_code` = 0 |
| `r.save("file.png")` | يحفظ الجواب في ملف |
| `r.raise_for_status()` | يوقف الكود بخطأ لو فشل (4xx أو 5xx أو ما وصل). داخل `prc` تمسك الخطأ |

المكتبة **ما توقف برنامجك** لو النت طاح أو الرابط غلط. ترجع `r.status_code = 0` و `r.error` فيها السبب.

## أدوات

```eo
# رابط مع بحث (يشفّر & و = والعربي صح)
var link = requests.url("https://api.com/search", {"q": "eo & go", "page": 2})
# → https://api.com/search?q=eo%20%26%20go&page=2

# هيدرز جاهزة
requests.get(url, requests.bearer("TOKEN"))
requests.get(url, requests.basic("user", "pass"))
requests.post(url, {"a": 1}, requests.merge(requests.bearer("TOKEN"), {"X-App": "eo"}))

# فورم بدل JSON
requests.post(url, requests.form({"user": "ali", "pass": "123"}), requests.FORM)

# تغيّر الهيدرز الافتراضية لكل الطلبات
requests.default_headers["User-Agent"] = "my-app/1.0"
```

| | |
|---|---|
| `requests.quote(text)` | يشفّر نص للرابط |
| `requests.base64(text)` | base64 |
| `requests.FORM` ، `requests.JSON` | هيدر `Content-Type` جاهز |

## البروكسي ⚠️

```eo
var r = requests.get("https://example.com", {}, "http://127.0.0.1:8080")
# أو قبل أي طلب:
requests.set_proxy "http://127.0.0.1:8080"
```

eo تقرأ البروكسي **مرة وحدة، أول طلب**، ويبقى لكل البرنامج. فلازم تحطه في **أول طلب** (أو بـ `set_proxy` قبله). لو حاولت تغيّره بعدين، الطلب يرجع `reason = "Proxy Error"` ويطبع تنبيه، بدون ما يوقف البرنامج.

- يقبل `http://ip:port` أو `ip:port`.
- الطلبات لـ `localhost` و `127.0.0.1` ما تمر على البروكسي.

## اللي ما تقدر تسويه (يحتاج تعديل اللغة)

- `timeout` لكل طلب: الحد ثابت 30 ثانية.
- إيقاف التحويلات (redirects): تتبعها لحالها.
- تغيير البروكسي وسط البرنامج، أو `verify=False`.
