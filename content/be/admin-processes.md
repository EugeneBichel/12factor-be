## XII. Працэсы адміністравання
### Запуск задач адміністравання і кіравання як аднаразовых працэсаў


[Працэс фарміравання](./concurrency) - гэта масіў працэсаў, якія выкарыстоўваюцца для выканання звычайнай працы дадатка (напрыклад, апрацоўкі вэб-запытаў) падчас яго працы. Асобна распрацоўшчыкі часта хочуць выконваць аднаразовыя адміністрацыйныя або эксплуатацыйныя задачы для дадатка, такія як:

* Запуск міграцый базы дадзеных (напрыклад, `manage.py migrate` у Django, `rake.db:migrate` у Rails).
* Запуск кансолі (таксама вядомай як [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop) shell) для запуску любога кода або праверкі мадэляў дадатка на адпаведнасць дадзенай базы дадзеных. Большасць моў распрацоўкі прадастаўляюць REPL, запусціўшы інтэрпрэтатар без якіх-небудзь аргументаў (напрыклад, `python` або `perl`) або ў некаторых выпадках маюць асобную каманду (напрыклад, `irb` для Ruby, `rails console` для Rails).
* Запуск аднаразовых скрыптоў, закамічаных у рэпазіторый дадатка (напрыклад, `php scripts/fix_bad_records.php`).

Аднаразовыя працэсы адміністравання павінны выконвацца ў той жа асяроддзі, што і [звычайныя працяглыя працэсы](./processes) дадатка. Яны запускаюцца для [рэлізу](./build-release-run), выкарыстоўваючы тую ж [кодавую базу](./codebase) і [канфігурацыю](./config), што і любы працэс, запушчаны супраць гэтага рэлізу. Код адміністратара павінен пастаўляцца разам з кодам дадатка, каб пазбегнуць праблем з сінхранізацыяй.

Адны і тыя ж метады [ізаляцыі залежнасцяў](./dependencies) павінны выкарыстоўвацца для ўсіх тыпаў працэсаў. Напрыклад, калі вэб-працэс Ruby выкарыстоўвае каманду `bundle exec thin start`, то пры міграцыі базы дадзеных варта выкарыстоўваць `bundle exec rake db:migrate`. Акрамя таго, праграмы на Python, выкарыстоўваючы віртуальнае асяроддзе павінны выкарыстоўваць `bin/python` для запуску вэб-сервера Tornada і любыя адмін працэсы `manage.py`.

Дванаццаці-фактарны дадатак моцна аддае перавагу мовам распрацоўкі, якія працуюць з REPL shell з скрынкі, і якія дазваляюць лёгка запускаць аднаразовыя скрыпты. У лакальным разгортванні распрацоўшчыкі выклікаюць аднаразовыя працэсы адміністравання з дапамогай прамой каманды shell ўнутры каталога дадатка. Для разгорнутага дадатка ў асяроддзі вытворчасці, распрацоўшчыкі могуць выкарыстоўваць ssh або іншы механізм аддаленага выканання каманд, якія працуюць з гэтам асяроддзем выканання для запуску такога працэсу.