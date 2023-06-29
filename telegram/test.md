### Test server
https://docs.pyrogram.org/topics/test-servers
- in macos Telegram Desktop Shift + Alt and right click on "Add account" and choose "Test server"

### Test Numbers
Beside normal numbers, the test environment allows you to login with reserved test numbers. Valid phone numbers follow the pattern 99966XYYYY, where X is the DC number (1 to 3) and YYYY are random numbers. Users with such numbers always get XXXXX or XXXXXX as the confirmation code (the DC number, repeated five or six times).

!Telegram периодически сбрасывает тестовые номера, поэтому все дальнейшие действия по авторизации и настройке telegram-клиента лучше производить непосредственно перед каждым запуском тестов.

### Test accounts

На ios чтобы добавить тестовый сервер, нужно бысто нажимать на Settings 10 раз, а не медленно, это была моя ошибка.

После добавления в тестового аккаунта 999... именно через iphone а не через desktop, все пошлло лучше, те тест Python запустился(TelegramClient), в телефоне установился пароль, и уже в приложении Python его вводим для работы. Тестовый аккаунт привязан к номеру 8282, ко второму номеру его не получилось привязать. Hash-и и id-и с сайта https://my.telegram.org/auth?to=apps подошли от реального номера телефона ...8288.