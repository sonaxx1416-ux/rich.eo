# rich.eo

ألوان وصناديق وجداول وأشرطة تقدم في الطرفية، للغة **eo**. مثل مكتبة rich في بايثون، بس بسنتاكس eo.

```eo
import rich

rich.say "[bold red]هلا[/] [green]بالعالم[/] :rocket:"
rich.panel "مرحبا بك في [bold]eo[/]", "ترحيب"
rich.rule "قسم جديد"
```

## التثبيت

```bash
eo get https://github.com/USER/rich.eo
```

أو انسخ `rich.eo` جنب ملفك. تحتاج `eo.exe` الجديد (فيه `write` و `term_width` و `text_width`).

---

## الألوان: `[style]نص[/]`

```eo
rich.say "[bold]عريض[/] [italic]مائل[/] [underline]تحته خط[/] [strike]مشطوب[/] [dim]باهت[/]"
rich.say "[red]أحمر[/] [bright_green]أخضر فاتح[/] [#ff8800]برتقالي[/] [rgb(120,80,255)]بنفسجي[/]"
rich.say "[bold white on red] تنبيه [/]"
rich.say "[bold]عريض [red]وأحمر[/red] ورجع عريض[/]"
```

| تكتب | يطلع |
|---|---|
| `[bold]` `[b]` | عريض |
| `[italic]` `[i]` ، `[underline]` `[u]` ، `[strike]` `[s]` ، `[dim]` ، `[reverse]` ، `[blink]` | |
| `[red]` `[green]` `[yellow]` `[blue]` `[magenta]` `[cyan]` `[white]` `[black]` `[gray]` | ألوان |
| `[bright_red]` ... | فاتحة |
| `[orange]` `[pink]` `[gold]` `[purple]` `[lime]` `[teal]` `[navy]` `[brown]` `[violet]` | ألوان زيادة |
| `[#ff8800]` ، `[rgb(255,136,0)]` ، `[color(208)]` | أي لون |
| `[white on blue]` | خلفية: `on` وبعدها لون |
| `[/]` | يقفل آخر واحد. `[/red]` يقفل `red` بالذات |
| `\[` | قوس عادي (مو لون) |

وفيه إيموجي بالاسم: `:rocket:` 🚀 `:fire:` 🔥 `:check:` ✅ `:x:` ❌ `:warning:` ⚠️ `:tada:` 🎉 `:wave:` 👋 `:star:` ⭐ `:heart:` ❤️ `:bulb:` 💡 `:robot:` 🤖 وغيرها.

النص اللي بين أقواس وهو مو لون يطلع كما هو، فـ `rich.say "[1, 2]"` تطبع `[1, 2]`.

## الطباعة

| | |
|---|---|
| `rich.say "نص"` | يطبع مع الألوان |
| `rich.log "نص"` | `[17:20:05] نص` (بوقتك) |
| `rich.success "تم"` ، `rich.info` ، `rich.warn` ، `rich.error` | ✔ ℹ ⚠ ✖ ملونة |
| `rich.rule "عنوان"` | خط بعرض الطرفية وفي نصه العنوان (`rich.rule` بدون عنوان = خط بس) |
| `rich.markup("[red]x[/]")` | يرجع النص بالألوان بدل ما يطبعه |
| `rich.style("نص", "bold green")` | يلوّن نص كامل |
| `rich.strip("[red]x[/]")` | يشيل الماركب: `"x"` |

## Panel

```eo
rich.panel "السطر الأول\nالسطر الثاني"
rich.panel "تم رفع الملفات", "✅ نجاح", "green"
rich.panel "نص", "عنوان", "cyan", 40       # عرض ثابت
```

```
╭─────── ✅ نجاح ───────╮
│ تم رفع الملفات        │
╰──────────────────────╯
```

## Table

```eo
var t = rich.Table("الأعضاء")
t.column "الاسم", "cyan"
t.column "العمر", "green", "right"
t.column "الحالة", "", "center"
t.row ["علي", 20, "✅"]
t.row ["سارة", 7, "[red]غايب[/]"]
t.show
```

```
╭──────┬──────┬────────╮
│ الاسم │ العمر │ الحالة │
├──────┼──────┼────────┤
│ علي  │   20 │   ✅   │
│ سارة │    7 │  غايب  │
╰──────┴──────┴────────╯
```

- `t.column "اسم", "لون", "left|right|center"`
- `t.row [قيم]` (أي نوع، وتقبل ماركب)
- `t.header_style = "bold cyan"` ، `t.border = "blue"` ، `t.lines = true` (خط بين كل صف)
- `t.render()` يرجع الجدول كنص (تقدر ترسله في ديسكورد داخل ``` ```)

## Progress

```eo
var bar = rich.Progress("تحميل", 100)
for i in range(100) {
    sleep 0.02
    bar.advance
}
```

```
تحميل ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:02
```

`bar.advance` (+1) أو `bar.advance 10`، و `bar.update 50` (يحطه على رقم). لو وقفت قبل 100% نادِ `bar.stop`. وتغيّر `bar.width` و `bar.spec` (اللون).

## Status (علامة تحميل)

```eo
var s = rich.Status("جاري الاتصال...")
for i in range(20) {
    sleep 0.1
    s.tick
}
s.done "اتصلت ✅"
```

كل `s.tick` يدوّر العلامة خطوة (⠋⠙⠹⠸...)، و `s.tick "نص جديد"` يغيّر النص.

## Tree

```eo
var tree = rich.Tree("📁 [bold]المشروع[/]")
var src = tree.add "src"
src.add "main.eo"
src.add "rich.eo"
tree.add "README.md"
tree.show
```

```
📁 المشروع
├── src
│   ├── main.eo
│   └── rich.eo
└── README.md
```

## inspect

```eo
rich.inspect({"name": "eo", "version": 1.9, "ok": true, "tags": ["fast", "simple"]})
```

يطبع القوائم والقواميس بألوان ومرتبة على كذا سطر لو طويلة. و `rich.pretty(x)` ترجعها كنص.

> القاموس لازم بين أقواس: `rich.inspect({...})`. بدونها eo تفهم `{` بداية بلوك.

## أسئلة

```eo
var name = rich.ask("وش اسمك؟")
var city = rich.ask("مدينتك؟", "الرياض")      # القيمة الافتراضية لو ضغط Enter
if rich.confirm("تكمل؟") {
    rich.success "يلا"
}
```

`confirm` تقبل `y` و `yes` و `نعم` و `اي` و `ايه`.

## الألوان تنطفي متى؟

- لو المخرج ملف (`eo main.eo > log.txt`) الألوان تنطفي لحالها.
- `NO_COLOR=1` يطفيها، و `FORCE_COLOR=1` يشغّلها غصب.
- من الكود: `rich.colors(false)` و `rich.colors(true)`.

في ويندوز eo تشغّل الألوان و UTF-8 في الطرفية لحالها.

> العربي: أغلب الطرفيات (ومنها Windows Terminal وطرفية VS Code) ما تعرض العربي من اليمين لليسار، فيطلع مقلوب. المكتبة تحسب العرض صح فالصناديق والجداول تبقى مرتبة.
