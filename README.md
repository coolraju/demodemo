# Xtream Codes - Add New Stream Manually

## 1. Login to MySQL

```bash
mysql -u root -p
```

```sql
USE xtream_iptvpro;
```

## 2. Check Server ID

```sql
SELECT id, server_name, status
FROM streaming_servers;
```

Example:

```text
Server ID = 1
```

## 3. Verify Stream

```sql
SELECT id, stream_display_name
FROM streams
WHERE id = 2;
```

## 4. Assign Stream to Server

```sql
INSERT INTO streams_sys
(
    server_stream_id,
    stream_id,
    server_id,
    parent_id,
    pid,
    stream_status,
    stream_started,
    current_source,
    on_demand,
    delay_available_at
)
VALUES
(
    2,
    2,
    1,
    NULL,
    NULL,
    0,
    UNIX_TIMESTAMP(),
    '',
    0,
    0
);
```

If the record already exists:

```sql
UPDATE streams_sys
SET
    server_id = 1,
    server_stream_id = 2
WHERE stream_id = 2;
```

## 5. Rebuild Cache

```bash
sudo -u xtreamcodes \
/home/xtreamcodes/iptv_xtream_codes/php/bin/php \
/home/xtreamcodes/iptv_xtream_codes/crons/setup_cache.php
```

## 6. Start Live Checker

```bash
sudo -u xtreamcodes \
/home/xtreamcodes/iptv_xtream_codes/php/bin/php \
/home/xtreamcodes/iptv_xtream_codes/crons/live_checker.php
```

## 7. Verify Stream Started

```sql
SELECT
    stream_id,
    server_id,
    pid,
    stream_status,
    current_source
FROM streams_sys
WHERE stream_id = 2;
```

## 8. Verify FFmpeg

```bash
ps aux | grep ffmpeg
```

## 9. Verify Stream Files

```bash
ls -lah /home/xtreamcodes/iptv_xtream_codes/streams/
```

You should see files like:

```text
2_0.ts
2_.m3u8
2_.pid
2_.progress
```

## 10. Test Playback

```text
http://SERVER_IP:25461/live/USERNAME/PASSWORD/2.ts
```

Example:

```text
http://SERVER_IP:25461/live/USERNAME/PASSWORD/2.ts
```

API Test:

```text
http://SERVER_IP:25461/player_api.php?username=USERNAME&password=PASSWORD
```
