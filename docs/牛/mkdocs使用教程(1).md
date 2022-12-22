mkdocs使用教程

1.新建一个项目文件夹，然后再终端命令行中输入 

git clone https://github.com/hasongo/mkdocs.git 克隆仓库 (只有第一次需要克隆，后续再添加文件输入git pull)

2. 到doce文件夹下创建对应的文件夹放入md文件

3. 回到上级目录然后再命令行中输入 git add .

4. 再命令行中输入 git commit -m '改变的提示' 

5. 再命令行中输入 git push 推送到远程仓库

6. 在mkdocs.yml同级目录下输入mkdocs gh-deploy

   返回类似代码成功

   ```python
   Enumerating objects: 7, done.
   Counting objects: 100% (7/7), done.
   Delta compression using up to 16 threads
   Compressing objects: 100% (4/4), done.
   Writing objects: 100% (4/4), 412 bytes | 412.00 KiB/s, done.
   Total 4 (delta 3), reused 0 (delta 0), pack-reused 0
   remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
   To https://github.com/hasongo/mkdocs.git
      5f45257..4fbbf7f  gh-pages -> gh-pages
   INFO     -  Your documentation should shortly be available at: https://hasongo.github.io/mkdocs/
   ```

   