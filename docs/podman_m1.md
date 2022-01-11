因為 Docker desktop 已經準備要收費了, Mac 上參考還能使用的替代方案, redhat podman 是一個選擇. 一般的 intel architecture 可以選擇使用 Virtualbox 的 VM 當作 docker daemon, 本篇主要想解決 M1 架構的 docker 替代品.

目前來說 Podman 已經支援 M1 Mac 了, 安裝也很簡單是透過 homebrew 去執行. 搞了一天, 遇到比較大的問題在 homebrew 本身的相容性.

發現的方法也滿直觀的, 當你用 `podman machine init` 拉 podman 要用的 VM 時候, 看一下 VM 檔案的 arch, 如果是 `x86_64` 之類的結尾, 很可能你可以透過本篇解決.

乾淨的 M1 機器貌似可以靠 [這篇](https://fantasticsie.medium.com/%E7%A7%BB%E8%BD%89%E5%88%B0-m1-macbook-pro-%E7%9A%84%E9%AB%94%E9%A9%97-c4eca0d2fa1f)

如果你是從舊的 Intel 機器還原來的, 我參考 [這篇](https://github.com/Homebrew/discussions/discussions/417) 討論, homebrew 對於 M1 migration 的解法是在 `/opt/homebrew` 這個路徑另起爐灶,
執行討論串裡面的 script 將會將 M1 compatible 的 homebrew 裝到這個位置. 之後在你的 `.zshrc` (or bash_profile, etc...) 裡面幫參數 $PATH 前面再加上一個 `/opt/homebrew` 提升優先程度.


安裝完了之後重新執行 `brew install podman`, 你會發現大量你曾經裝過的東西又再重裝一次... 這些是 M1 compatible 的套件.
注意 `podman machine init` 的時候裝起來的 VM, 再檢查一次檔名, 應該是 `aarch64`


<br>
這個時候再接著做 `podman machine start` 應該可以順利完成了.  你可以加上 `alias docker=podman` 讓你的生活開心一點.


<br>
嘗試一下跑你所知道的 docker 指令, 大致上應該是相容的, e.g., `docker pull ubuntu`; `docker run -it ubuntu`


---

另外原本復原而來的 brew packages, 討論串裡建議慢慢一個一個搬, 當全部搬完之後理論上 `/usr/local/` 下面跟 brew 相關的資料就都可以殺掉了.

