### install & launch
1. win
    * redis.window.conf文件 maxheap 设置为104857600（100M）
    * > redis-server.exe redis.windows.conf  
        redis-cli.exe -h 127.0.0.1 -p 6379 -a "password"  
        config get xx  
        config set xx xx
