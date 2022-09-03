# 用 zsh + zim + p10k 美化 Windows Terminal 的 WSL

1. 到市集下載安裝 Windows Terminal
2. 用系統管理員身分開啟 **命令提示字元** 或 **Windows PowerShell**
3. 輸入 `wsl --install` 安裝 WSL
    > [使用 WSL 在 Windows 上安裝 Linux](https://docs.microsoft.com/zh-tw/windows/wsl/install)
    >
    > 安裝 WSL 的條件是 Windows 10 2004 版和更新版本， (組建 19041 和更新版本) 或Windows 11，且電腦要支援虛擬化功能（Virtualization Technology）。
    >
    > 以我的電腦主機板是 Asus 的來說，要先進入 BIOS 介面，開啟一個叫做 VMX 的功能。
4. WSL 安裝好之後會需要重開機，開機後應該會自動開啟 WSL，然後會需要輸入 Username 跟 Password（不用跟 Windows 的一樣）
5. 再來就可以用 Windows Terminal 開啟 WSL，來下載 zsh、zim 跟 p10k 了
6. 輸入 `sudo apt install zsh` 安裝 zsh
   > 用 `apt install` 安裝 package 之前都建議先執行 `sudo apt update && sudo apt upgrade` 來更新套件
7. 輸入 `chsh -s $(which zsh)` 來把 zsh 設成預設 Shell
8. 輸入 `curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh` 或 `wget -nv -O - https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh` 安裝 Zim，根據他們的 GitHub 的說明，比起 oh-my-zsh 之類的其他工具效能快一倍。
    > [zimfw - Speed](https://github.com/zimfw/zimfw/wiki/Speed)
9. 在 `~/.zimrc` 新增 `zmodule romkatv/powerlevel10k --use degit` 後，執行 `zimfw install`（安裝完 Zim 之後可能需要先重新載入視窗）
10. 接下來會有 p10k wizard 問你一些問題，只要跟著回答就可以幫你設定好，如果沒有跳出來也可以輸入 `p10k configure` 來啟動

參考來源：

1. [使用 WSL 在 Windows 上安裝 Linux](https://docs.microsoft.com/zh-tw/windows/wsl/install)
2. [zimfw](https://github.com/zimfw/zimfw)
3. [powerlevel10k](https://dwye.dev/post/zsh-zim-powerlevel10k/)
