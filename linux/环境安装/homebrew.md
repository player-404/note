参考：[**https://github.com/homebrew/install#uninstall-homebrew**](https://github.com/homebrew/install#uninstall-homebrew)

* 打开  [**https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh**](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh) 另存为 install.sh
* 终端运行 /bin/bash install.sh
  * 443错误
    * 运行`git config --global --unset http.proxy ` 和 `git config --global --unset https.proxy`且 DNS 改为 8.8.8.8
  * 其他错误
    * 卸载重装:
      * 打开 https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh 保存为uninstall.sh
      * 运行 /bin/basg uninstall.sh

