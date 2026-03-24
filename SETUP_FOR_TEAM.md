# Інструкція Для Команди По Postman Native Git

## Призначення

Цей репозиторій є спільним локальним та GitHub-сховищем для наших Postman workspace-ів.

Ми використовуємо:

- один GitHub-репозиторій для всієї команди: [eh_collections](https://github.com/ezem001/eh_collections)
- один локальний Git-репозиторій, який кожен колега клонує собі на комп'ютер
- три окремі корені Postman workspace-ів усередині репозиторію:
  - `workspaces/Demo`
  - `workspaces/Preprod`
  - `workspaces/Stage`

Така структура обрана навмисно. Вона прибирає конфлікти, коли в різних Postman workspace-ах є колекції з однаковими назвами. Наприклад, `Demo` і `Preprod` можуть одночасно мати колекцію `General MIS API`, але через те, що кожен workspace лежить у своїй окремій папці, файли не перезаписують один одного.

## Чому Ми Використовуємо Саме Таку Структуру

Ми не використовуємо одну спільну пласку папку `postman/collections` для всього, тому що:

- у різних workspace-ах часто є колекції з однаковими назвами
- Postman Native Git стає заплутаним, якщо змішувати різні workspace-и в одній локальній папці
- складно зрозуміти, що саме треба комітити й пушити
- колеги можуть випадково закомітити зміни з не того workspace-а

Замість цього ми використовуємо один спільний репозиторій з окремими папками для кожного Postman workspace-а.

Поточна структура така:

```text
eh_collections/
  workspaces/
    Demo/
      .postman/
      postman/
    Preprod/
      .postman/
      postman/
    Stage/
      .postman/
      postman/
```

Кожна папка всередині `workspaces/` підключається тільки до одного Postman workspace-а.

## Що Потрібно Кожному Колезі Перед Початком

Кожен учасник команди повинен мати:

1. Доступ до GitHub-репозиторію [eh_collections](https://github.com/ezem001/eh_collections) з правами запису.
2. Доступ до Postman team і до таких Postman workspace-ів:
   - `Demo`
   - `Preprod`
   - `Stage`
3. Встановлений Postman Desktop App.
4. Встановлений Git.
5. Базову можливість працювати з терміналом.

Необов'язково, але бажано:

- GitHub Desktop, якщо колезі зручніше працювати з Git через інтерфейс
- уже налаштована GitHub-авторизація локально

## Важливі Правила Перед Початком Роботи

Будь ласка, дотримуйтесь цих правил без винятків:

1. Ніколи не запускайте `git init` всередині `workspaces/Demo`, `workspaces/Preprod` або `workspaces/Stage`.
2. Ніколи не створюйте окремий репозиторій усередині папки конкретного workspace-а.
3. Завжди працюйте з одного кореня репозиторію.
4. Завжди підключайте Postman до вже існуючого локального clone, а не до випадково створеної нової папки.
5. Завжди дотримуйтесь такої відповідності:
   - `Demo` -> `workspaces/Demo`
   - `Preprod` -> `workspaces/Preprod`
   - `Stage` -> `workspaces/Stage`
6. Не зберігайте секрети в закомічених файлах.
7. Кожен колега повинен створити й постійно використовувати свою персональну гілку.
8. Назва персональної гілки повинна відповідати імені колеги в погодженому форматі.
9. Не працюйте напряму в `main`.

## Формат Імен Персональних Гілок

Кожен колега один раз створює собі постійну персональну гілку й далі використовує тільки її.

Приклади:

- `Roman_Katamai`
- `Kate_Hubko`
- `Lilia_Besednikova`
- `Katya_Aloshyna`
- `Anastasiia_Afrosimova`

Важливо:

- це не тимчасова feature-branch на одну задачу
- це постійна персональна гілка
- колега щодня продовжує працювати саме в ній
- pull request-и створюються з персональної гілки в `main`

## Крок 1: Склонувати Репозиторій

Оберіть локальну папку для проєкту й виконайте:

```bash
git clone https://github.com/ezem001/eh_collections.git
cd eh_collections
git checkout main
git pull origin main
```

Після цього перевірте, що структура правильна:

```bash
find workspaces -maxdepth 2 | sort
```

Ви повинні побачити папки:

- `workspaces/Demo`
- `workspaces/Preprod`
- `workspaces/Stage`

## Крок 2: Перевірити Доступ До Git

Перед підключенням Postman переконайтеся, що Git працює:

```bash
git remote -v
git status
```

Очікуваний результат:

- `origin` має вказувати на `https://github.com/ezem001/eh_collections.git`
- `git status` не повинен показувати помилок репозиторію

Якщо доступ на push ще не налаштований, виправте це до початку роботи. Інакше Postman зможе створити локальні зміни, але далі команда не зможе нормально їх синхронізувати.

## Крок 3: Створити Свою Персональну Гілку

Перед реальною роботою в Postman кожен колега повинен один раз створити власну персональну гілку.

Приклад для Roman:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Roman_Katamai
git push -u origin Roman_Katamai
```

Приклад для Kate:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Kate_Hubko
git push -u origin Kate_Hubko
```

Приклад для Lilia:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Lilia_Besednikova
git push -u origin Lilia_Besednikova
```

Приклад для Katya:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Katya_Aloshyna
git push -u origin Katya_Aloshyna
```

Приклад для Anastasiia:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Anastasiia_Afrosimova
git push -u origin Anastasiia_Afrosimova
```

Це робиться лише один раз.

Після створення персональної гілки колега надалі працює тільки в ній.

## Крок 4: Відкрити Правильний Postman Workspace

Відкрийте Postman Desktop App і виберіть правильний workspace:

- `Demo`, якщо хочете працювати з `workspaces/Demo`
- `Preprod`, якщо хочете працювати з `workspaces/Preprod`
- `Stage`, якщо хочете працювати з `workspaces/Stage`

Не підключайте неправильну локальну папку до неправильного Postman workspace-а.

Правильна відповідність:

- Postman workspace `Demo` -> локальна папка `workspaces/Demo`
- Postman workspace `Preprod` -> локальна папка `workspaces/Preprod`
- Postman workspace `Stage` -> локальна папка `workspaces/Stage`

## Крок 5: Підключити Postman До Існуючого Локального Репозиторію

У Postman:

1. Відкрийте потрібний workspace, наприклад `Demo`.
2. У лівій панелі відкрийте `Files`.
3. Якщо Postman просить налаштувати локальну кодову базу, оберіть варіант, який означає відкриття або підключення вже існуючої локальної копії.
4. Виберіть корінь репозиторію, який ви склонували собі на комп'ютер.

Приклади:

macOS:

```text
/Users/<your-user>/.../eh_collections
```

Windows:

```text
C:\Users\<your-user>\Projects\eh_collections
```

5. Коли Postman попросить вказати шлях усередині репозиторію, оберіть:

- `/workspaces/Demo` для workspace `Demo`
- `/workspaces/Preprod` для workspace `Preprod`
- `/workspaces/Stage` для workspace `Stage`

6. Підтвердьте підключення.

Важливо:

- корінь репозиторію це саме склонована папка `eh_collections`
- шлях workspace-а це одна з підпапок усередині `workspaces/`
- не підключайте Postman до окремого вкладеного Git-репозиторію
- не робіть окремий clone тільки для одного workspace-а

## Крок 6: Переконатися, Що Postman Пише В Правильну Папку

Після підключення Postman повинен створювати або оновлювати файли тільки у відповідній папці workspace-а.

Приклади:

- `Demo` повинен писати у `workspaces/Demo/...`
- `Preprod` повинен писати у `workspaces/Preprod/...`
- `Stage` повинен писати у `workspaces/Stage/...`

Перевірити можна через термінал:

```bash
git status
```

Якщо ви змінювали щось у `Demo`, то зміни мають бути тільки в `workspaces/Demo`.

Якщо раптом `Demo` починає писати зміни в `workspaces/Preprod` або навпаки, зупиніться й перепідключіть правильну папку в Postman, перш ніж продовжувати.

## Крок 7: Зрозуміти Різницю Між Local View І Cloud View

Postman Native Git зазвичай використовує:

- `Local View` для роботи з файлами локально
- `Cloud View` для хмарного стану в Postman

Практичне правило:

- працюйте з файлами в `Local View`
- використовуйте `Pull from Postman Cloud`, коли треба підтягнути зміни з хмари в локальні файли
- використовуйте `Push to Postman Cloud`, коли хочете явно відправити локальні зміни назад у Postman Cloud

Не потрібно думати, що Git push автоматично оновлює Postman Cloud.
Так само не потрібно думати, що Postman Cloud автоматично створює Git commit.
Це пов'язані, але різні процеси.

## Крок 8: Щоденний Старт Роботи

Щоранку або перед початком роботи робіть так:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Замість `Roman_Katamai` потрібно підставити власну персональну гілку.

Приклади:

- Roman виконує `git checkout Roman_Katamai`
- Kate виконує `git checkout Kate_Hubko`
- Lilia виконує `git checkout Lilia_Besednikova`
- Katya виконує `git checkout Katya_Aloshyna`
- Anastasiia виконує `git checkout Anastasiia_Afrosimova`

Чому це важливо:

- усі починають роботу з актуального стану команди
- кожен залишається у своїй персональній гілці
- зміни ізольовані по людях
- pull request-и в `main` стають передбачуваними
- конфлікти вирішувати простіше

Не працюйте регулярно прямо в `main`.

## Крок 9: Змінювати Лише Той Workspace, Який Потрібен

Під час роботи в Postman:

- відкривайте тільки той workspace, який збираєтесь міняти
- уважно дивіться, який workspace зараз активний
- перевіряйте, що Git-помітні зміни з'являються саме в правильній папці

Приклади:

- якщо ви працюєте в `Demo`, Git-зміни мають бути в `workspaces/Demo`
- якщо ви працюєте в `Preprod`, Git-зміни мають бути в `workspaces/Preprod`
- якщо ви працюєте в `Stage`, Git-зміни мають бути в `workspaces/Stage`

Це найпростіше правило, яке захищає від випадкових крос-workspace комітів.

## Крок 10: Перед Комітом Перевірити Git-Зміни

Завжди перед комітом подивіться статус:

```bash
git status
```

Якщо потрібна деталізація:

```bash
git diff -- workspaces/Demo
git diff -- workspaces/Preprod
git diff -- workspaces/Stage
```

Робіть цю перевірку, тому що Postman може змінювати:

- визначення колекцій
- `.postman/resources.yaml`
- request-файли
- metadata папок
- допоміжні файли всередині структури workspace-а

## Крок 11: Комітити Тільки Той Workspace, Який Ви Змінювали

### Якщо ви змінювали тільки `Demo`

```bash
git add workspaces/Demo
git status
git commit -m "Update Demo workspace"
git push -u origin Roman_Katamai
```

### Якщо ви змінювали тільки `Preprod`

```bash
git add workspaces/Preprod
git status
git commit -m "Update Preprod workspace"
git push -u origin Roman_Katamai
```

### Якщо ви змінювали тільки `Stage`

```bash
git add workspaces/Stage
git status
git commit -m "Update Stage workspace"
git push -u origin Roman_Katamai
```

Замість `Roman_Katamai` потрібно підставити персональну гілку конкретного колеги.

### Якщо ви свідомо змінювали кілька workspace-ів

```bash
git add workspaces/Demo workspaces/Preprod workspaces/Stage
git status
git commit -m "Update Postman workspaces"
git push -u origin Roman_Katamai
```

Знову ж таки, `Roman_Katamai` треба замінити на персональну гілку колеги.

Важливо:

- краще додавати тільки конкретну папку workspace-а, яку ви міняли
- не використовуйте `git add .`, якщо не впевнені на 100%, що треба комітити все

## Крок 12: Створити Pull Request

Після push у вашу персональну гілку:

1. Відкрийте GitHub.
2. Створіть pull request у `main`.
3. Якщо у вас є процес review, попросіть рев'ю.
4. Після погодження змерджіть зміни.

Це найбезпечніший командний workflow, тому що всі бачать, що саме змінилося в конкретних папках Postman workspace-ів.

## Крок 13: Що Робити Після Merge

Після того як pull request змерджений:

```bash
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Замість `Roman_Katamai` підставте власну гілку.

Важливо:

- не видаляйте персональну гілку після merge
- кожен колега використовує свою персональну гілку повторно й надалі

## Готові Команди По Workspace-ах

### Тільки `Demo`

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Demo
git commit -m "Update Demo workspace"
git push -u origin Roman_Katamai
```

### Тільки `Preprod`

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Preprod
git commit -m "Update Preprod workspace"
git push -u origin Roman_Katamai
```

### Тільки `Stage`

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Stage
git commit -m "Update Stage workspace"
git push -u origin Roman_Katamai
```

У всіх прикладах вище замініть `Roman_Katamai` на власну персональну гілку.

## Як Безпечно Підтягувати Останні Командні Зміни

Рекомендований сценарій для кожного колеги:

```bash
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Замініть `Roman_Katamai` на свою персональну гілку.

Якщо з'явилися конфлікти, вирішіть їх уважно й перевірте відповідну папку Postman workspace-а перед продовженням.

## Як Відновитися, Якщо Postman Підключився Не До Тієї Папки

Ознаки проблеми:

- файли з'являються в неправильній папці workspace-а
- зміни з `Demo` опиняються в `Preprod`
- Postman просить ініціалізувати новий репозиторій у вкладеній папці
- Postman показує не той repository або не той path

Що робити:

1. Зупинити внесення змін.
2. Не запускати `git init`.
3. У Postman відключити поточне локальне підключення папки.
4. Підключити заново існуючий локальний clone репозиторію.
5. Обрати правильний шлях workspace-а:
   - `/workspaces/Demo`
   - `/workspaces/Preprod`
   - `/workspaces/Stage`
6. Запустити `git status` і перевірити, що нові зміни тепер з'являються тільки в правильній папці.

## Як Відновитися, Якщо Git Каже, Що Немає Прав На Push

Перевірте remote:

```bash
git remote -v
```

Очікувано має бути:

```text
origin  https://github.com/ezem001/eh_collections.git (fetch)
origin  https://github.com/ezem001/eh_collections.git (push)
```

Якщо push не працює:

1. Переконайтеся, що ваш GitHub-акаунт має write-доступ до репозиторію.
2. Переконайтеся, що локальна Git-авторизація використовує правильний GitHub-акаунт.
3. Повторіть push зі своєї персональної гілки:

```bash
git push -u origin Roman_Katamai
```

Замініть `Roman_Katamai` на власну гілку.

## Як Відновитися, Якщо Postman Підтягнув Зайві Колекції

Якщо раптом з'явилися колекції, що належать до іншого середовища:

1. Перевірте, який Postman workspace зараз відкритий.
2. Перевірте, який локальний folder path зараз підключений.
3. Переконайтеся, що folder path відповідає назві workspace-а.
4. Якщо ні, перепідключіть правильно.
5. Перевірте файл `workspaces/<workspace-name>/.postman/resources.yaml`.

Цей файл часто є головною підказкою, тому що саме він мапить локальні файли workspace-а до Postman Cloud ресурсів для конкретного workspace-а.

## Як Подивитися, Що Саме Змінилося

Корисні команди:

```bash
git status
git diff -- workspaces/Demo
git diff -- workspaces/Preprod
git diff -- workspaces/Stage
```

Щоб побачити тільки імена файлів:

```bash
git diff --name-only
```

Щоб подивитися вже staged-зміни:

```bash
git diff --cached
```

## Рекомендовані Командні Правила

Ми рекомендуємо такі правила для команди:

1. Один постійний персональний branch на одного колегу.
2. Один workspace на один commit, коли це можливо.
3. Зрозумілі commit message-і:
   - `Update Demo workspace`
   - `Add Preprod regression collections`
   - `Fix Stage environment requests`
4. Завжди перевіряти `git status` перед комітом.
5. Ніколи не комітити зміни з workspace-а, який ви не планували змінювати.
6. Робити pull request-и з персональної гілки в `main`.

## Часті Питання

### Чи потрібен окремий GitHub-репозиторій для кожного Postman workspace-а?

Ні.

Ми спеціально використовуємо один GitHub-репозиторій для команди й окремі папки всередині нього:

- `workspaces/Demo`
- `workspaces/Preprod`
- `workspaces/Stage`

### Чи потрібен окремий локальний clone для кожного workspace-а?

Ні.

Потрібен один локальний clone репозиторію, а кожен Postman workspace підключається до свого шляху всередині цього clone.

### Чи треба запускати `git init` всередині папки workspace-а?

Ні.

Ніколи цього не робіть. Це створює вкладені репозиторії й ламає спільну схему роботи.

### Чи варто використовувати `git add .`?

Зазвичай ні.

Краще використовувати:

- `git add workspaces/Demo`
- `git add workspaces/Preprod`
- `git add workspaces/Stage`

Так значно менший ризик випадкового коміту зайвих змін.

### Чи Git push автоматично оновлює Postman Cloud?

Ні.

Git і Postman Cloud пов'язані, але це різні процеси. Для синхронізації саме з Postman Cloud потрібно використовувати відповідні дії всередині Postman.

### Чи Pull from Postman Cloud автоматично створює Git commit?

Ні.

Після того як Postman оновив локальні файли, ви все одно повинні окремо зробити Git commit і Git push.

## Короткий Checklist Для Нового Колеги

1. Склонувати репозиторій.
2. Перевірити, що GitHub write-доступ працює.
3. Створити свою персональну гілку, наприклад `Roman_Katamai`.
4. Відкрити Postman Desktop App.
5. Підключити `Demo` до `/workspaces/Demo`.
6. Підключити `Preprod` до `/workspaces/Preprod`.
7. Підключити `Stage` до `/workspaces/Stage`.
8. Працювати в `Local View`.
9. Перед кожною сесією оновлювати `main`, переходити у свою гілку й вливати в неї `main`.
10. Комітити тільки ту папку workspace-а, яку реально змінювали.
11. Пушити свою персональну гілку й створювати pull request у `main`.

## Нагадування Про Розташування Репозиторію

URL репозиторію:

- [https://github.com/ezem001/eh_collections](https://github.com/ezem001/eh_collections)

Приклади локального clone path:

macOS:

```text
/Users/<your-user>/Projects/eh_collections
```

Windows:

```text
C:\Users\<your-user>\Projects\eh_collections
```

Шляхи workspace-ів усередині репозиторію:

- `/workspaces/Demo`
- `/workspaces/Preprod`
- `/workspaces/Stage`
