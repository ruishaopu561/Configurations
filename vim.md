#
##
 
## 问题
1.libcmake 下载不下来  
解决：  
直接手动下载下来，放到目录```~/.vim/bundle/YouCompleteMe/third_party/ycmd/clang_archives```下。

2.安装到最后报错:
```zsh
ERROR: msbuild or xbuild is required to build Omnisharp.
```
解决:  
```
sudo apt-get install mono-complete
npm install xbuild
```
