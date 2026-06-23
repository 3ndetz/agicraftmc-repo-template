# Безопасность и секреты

Этот репозиторий — **шаблон**. Из него убраны все приватные данные:

- удалён `.env.production` (оставлен только `.env.example`);
- удалены `forwarding.secret` (секрет модерн-форвардинга Velocity);
- удалены `floodgate/key.pem` (ключи Floodgate — генерируются заново сами);
- значения паролей/секретов/токенов в конфигах заменены на `CHANGEME`;
- домены, имена и прочая специфика заменены на `example.com` / `MyMC`.

## Что нужно создать перед запуском

1. **`.env`** — скопируй из `.env.example` и заполни СВОИМИ значениями:
   - `POSTGRES_PASSWORD`, `JWT_SECRET` — придумай длинные случайные;
   - `RCON_PASSWORD` — длинный случайный;
   - `YOOKASSA_SHOP_ID` / `YOOKASSA_SECRET_KEY` — если нужна оплата (иначе оставь заглушки);
   - домены/URL — свои.

2. **`forwarding.secret`** для Velocity (modern forwarding). Один и тот же секрет
   нужен проксе и всем бэкенд-серверам:
   ```bash
   openssl rand -hex 24 > velocity/forwarding.secret
   cp velocity/forwarding.secret agents/config/forwarding.secret
   # и в config/ остальных серверов, если используешь modern forwarding
   ```
   (рядом лежат `forwarding.secret.example` как напоминание.)

3. **Floodgate** (если используешь GeyserMC/Bedrock) сам сгенерирует `key.pem`
   при первом старте — ничего делать не надо.

4. Пройдись по конфигам и заменли оставшиеся `CHANGEME` на свои значения:
   ```bash
   grep -rn CHANGEME .
   ```

## Не коммить
`.env`, `*/forwarding.secret`, `**/key.pem` и любые реальные секреты уже в `.gitignore`
по смыслу — **не добавляй их в git**. Если случайно закоммитил ключ, ротируй его и
вычисти из истории (`git filter-repo`), а не просто удали следующим коммитом.
