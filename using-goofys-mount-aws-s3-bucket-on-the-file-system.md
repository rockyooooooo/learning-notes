# 用 goofys 把 AWS S3 Bucket mount 在 File System 上

[https://github.com/kahing/goofys](https://github.com/kahing/goofys)

```bash
# linux
wget https://github.com/kahing/goofys/releases/latest/download/goofys
# macOS
brew cask install osxfuse
brew install goofys
```

1. linux 直接下載作者準備好的 bin 檔
2. 到 AWS 上取得 credential（不確定是不是一定要用 root user）
    1. 右上角 allenliao
    2. Security Credentials
    3. Access keys (access key ID and secret access key)
    4. 創建新的 key 或是用舊的 key
3. 建立資料夾 `~/.aws`
4. 在 `~/.aws` 建立 `credentials`
    
    ```bash
    [default]
    aws_access_key_id = AKID1234567890
    aws_secret_access_key = MY-SECRET-KEY
    ```
    
5. 執行 `./goofys <bucket_name> <mount_point>`

如果要 unmount 的話，執行 `umount <mount_point>` 就可以了