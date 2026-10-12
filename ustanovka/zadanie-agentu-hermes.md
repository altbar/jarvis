---
description: "Файл-задание для второй половины установки: агент сам записывает на сервер ключ модели и токен бота, поднимает Telegram и проверяет. Перетащить в OpenCode, дописать ключ MiniMax, токен бота и свой Telegram-ID."
---
# Задание агенту: доведи Hermes на сервере до работающего бота

Агент, это задание от меня, владельца сервера. Прочитай целиком и сразу начинай.

## Правило изменилось 12.10

- Старое правило «ключи и токены участник вставляет руками через nano» отменено для любых ключей: модели, бота, Groq, Exa и других. Ключи я даю тебе в этом сообщении, как раньше пароль root. Записываешь их на сервер сам. Общий принцип - https://raw.githubusercontent.com/altbar/jarvis/main/AGENTS.md, раздел 6.
- Свежие правила: https://raw.githubusercontent.com/altbar/jarvis/main/ustanovka/hermes-na-vps.md (шаги 3, 4, 6) и https://raw.githubusercontent.com/altbar/jarvis/main/ustanovka/server-dostup.md. Читал их раньше в этом разговоре - перечитай, они обновлены. Локальная копия базы у меня на диске могла устареть.
- Значения ключа и токена не повторяй в ответах и не пиши в файлы волта. `~/.hermes/.env` целиком не выводи. Токен бота я перевыпущу завтра на аудите.
- Интерактивное не запускай: мастер `hermes setup`, `hermes setup gateway`, nano и чат `hermes` ждут клавиатуру, а окна для ввода у тебя нет. Только команды ниже.

## Что сделать

На сервер ходи как раньше - по ключу, пользователем `agent` (алиас `ssh jarvis`, если он есть). Перед каждым шагом - одна фраза: что делаешь и зачем.

1. Проверь, что Hermes стоит: `hermes --version`. Нет - поставь по шагу 2 урока, сначала `sudo apt-get install -y git libatomic1 ffmpeg ripgrep`.
2. Модель и ключ - эти команды, с моим ключом вместо `МОЙ_КЛЮЧ`:
   ```
   hermes config set ANTHROPIC_BASE_URL https://api.minimax.io/anthropic
   hermes config set ANTHROPIC_API_KEY МОЙ_КЛЮЧ
   hermes config set model.provider anthropic
   hermes config set model.base_url https://api.minimax.io/anthropic
   hermes config set model MiniMax-M3
   hermes config set tts.provider edge
   chmod 600 ~/.hermes/.env
   ```
   Раньше ставил `model.provider custom` или другой адрес - эти команды всё исправят.
3. Проверь модель: `hermes -z "привет, какая у тебя модель?"`. Ответил - дальше. Ошибка - покажи текст и чини, к боту не переходи.
4. Telegram - с моим токеном и ID:
   ```
   hermes config set TELEGRAM_BOT_TOKEN МОЙ_ТОКЕН
   hermes config set TELEGRAM_ALLOWED_USERS МОЙ_ID
   hermes config set TELEGRAM_HOME_CHANNEL МОЙ_ID
   sed -i '/^HERMES_ALLOWED_USERS=/d' ~/.hermes/.env
   sudo loginctl enable-linger agent
   hermes gateway install
   systemctl --user status hermes-gateway --no-pager
   ```
   Список разрешённых - только `TELEGRAM_ALLOWED_USERS`. Под другим именем Hermes его не читает, и бот открыт всем. Проверка: `grep -c '^TELEGRAM_ALLOWED_USERS=.' ~/.hermes/.env` даёт 1.
   Служба должна быть одна. Уже стоит системная (`systemctl status hermes-gateway`) - пользовательскую не ставь, сделай `hermes gateway restart`.
5. Попроси меня написать боту «привет» с телефона. Ответил - `sudo reboot`, через минуту я снова пишу боту. Ответил после перезагрузки - готово.
6. Дальше по уроку - волт (шаг 5), личность (шаг 7), задача по расписанию (шаг 8), когда я скажу.

## Мои данные

Ключ MiniMax (`sk-cp…`), токен бота от @BotFather и мой числовой Telegram-ID - в этом же сообщении, ниже. Чего-то не хватает - спроси одним сообщением.

Начинай с шага 1.
